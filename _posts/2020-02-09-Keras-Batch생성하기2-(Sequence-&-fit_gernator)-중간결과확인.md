---
layout: post
title: Keras-Batch생성하기2-(Sequence & fit_gernator)-중간결과확인
author: Sunwoo Kim
categories: Frameworks
tags: [Keras]
---

fit_generator를 이용하다가 불편한점이 하나 있었는데 그것은 바로 Segmentation되는 중간결과를 확인하지 못한다는 것이다. model.fit() 혹은 model.train_on_batch()를 사용하게 되면 직접 배치를 반환받아 그 값을 fit(X, Y), train_on_batch(X, Y)와 같은 방식으로 입력 받을 수 있기 때문에, X, Y, model.predict(X)의 값을 반환 받아서 중간중간에 Plotting 해볼 수 있다는 장점이 있는데, fit_generator는 그 안에서만 진행이 되기 때문에 중간중간에 prediction을 해볼 수 없다는 단점이 있습니다.

그래서 중간중간에 결과를 plotting해보기 위해서 github에 있는 keras의 fit_generator에 아주 간단한 코드를 추가하여 결과를 중간에 볼 수 있도록 할 수 있습니다.

[Keras 공식 코드](https://github.com/keras-team/keras/blob/master/keras/engine/training_generator.py) 를 참조해주세요. 실제 저희가 사용하는 부분과 똑같은 부분입니다. 해당 홈페이지에 들어가서 ctrl+F를 누르고 **test_on_batch** 를 검색해주세요. 그러면 test_on_batch부분이 사용된 부분을 찾을 수 있습니다. 그 부분을 집중적으로 보고 코드를 추가할 예정입니다.

364번째 줄에 **while steps_done < steps** 부분이 보이실 겁니다. 이 부분이 이하가 바로 step만큼 validation을 진행하는 부분입니다.  그래서 x,y 값을 부르고, 399번째 줄에서 test_on_batch가 실행되면서 validation batch에 대한 evaluation을 진행하게 됩니다.  그리고 그 부분을 아래와 같이 바꿔줍니다. (Segmentation 작업을 수행하여서 다음과 같이 결과를 plotting 했습니다.) 그리고 파일 상단에 import matplotlib.pyplot as plt코드를 넣어서 matplotlib을 import 해줍니다.

<center><img src="/public/img/keras/02.png" width="90%"></center>

실제 적용하려면 현제 로컬에 저장된 경로에서 찾아야 되는데요 다음과 같은 경로에서 찾아보시면 됩니다.

<center><img src="/public/img/keras/01.png" width="90%"></center>

이 때, 적용해도 안 바뀔수가 있는데, 코드에서 작동되는 keras및 tensorflow가 진짜로 참조한는 py파일이 어떤 파일인지 잘 찾아보셔야 합니다. 가상환경을 사용하시는데, pubilc한 부분의 파일을 바꾸면 가상환경안에 있는 파일이 아니기 때문에 작동하지 않습니다.

이렇게 바꾸고, **Spyder**를 이용하여 코드를 돌리면 다음과 같이 결과를 출력할 수 있습니다.

<center><img src="/public/img/keras/03.png" width="90%"></center>

<center><img src="/public/img/keras/04.png" width="90%"></center>

왼쪽부터 차례대로, 원본이미지, Ground Truth, Predicted Mask 입니다.



혹시몰라. 주변 부분의 코드를 보고 잘 찾아서 추가하시라고 전체코드를 첨부합니다. keras의 training_generator.py에 plotting하는 코드만 첨가한 코드입니다.

```python
"""Part of the training engine related to Python generators of array data.
"""
from __future__ import absolute_import
from __future__ import division
from __future__ import print_function

import warnings
import numpy as np

from .training_utils import iter_sequence_infinite
from .. import backend as K
from ..utils.data_utils import Sequence
from ..utils.data_utils import GeneratorEnqueuer
from ..utils.data_utils import OrderedEnqueuer
from ..utils.generic_utils import Progbar
from ..utils.generic_utils import to_list
from ..utils.generic_utils import unpack_singleton
from .. import callbacks as cbks

import matplotlib.pyplot as plt


def fit_generator(model,
                  generator,
                  steps_per_epoch=None,
                  epochs=1,
                  verbose=1,
                  callbacks=None,
                  validation_data=None,
                  validation_steps=None,
                  class_weight=None,
                  max_queue_size=10,
                  workers=1,
                  use_multiprocessing=False,
                  shuffle=True,
                  initial_epoch=0):
    """See docstring for `Model.fit_generator`."""
    wait_time = 0.01  # in seconds
    epoch = initial_epoch

    do_validation = bool(validation_data)
    model._make_train_function()
    if do_validation:
        model._make_test_function()

    is_sequence = isinstance(generator, Sequence)
    if not is_sequence and use_multiprocessing and workers > 1:
        warnings.warn(
            UserWarning('Using a generator with `use_multiprocessing=True`'
                        ' and multiple workers may duplicate your data.'
                        ' Please consider using the`keras.utils.Sequence'
                        ' class.'))
    if steps_per_epoch is None:
        if is_sequence:
            steps_per_epoch = len(generator)
        else:
            raise ValueError('`steps_per_epoch=None` is only valid for a'
                             ' generator based on the '
                             '`keras.utils.Sequence`'
                             ' class. Please specify `steps_per_epoch` '
                             'or use the `keras.utils.Sequence` class.')

    # python 2 has 'next', 3 has '__next__'
    # avoid any explicit version checks
    val_gen = (hasattr(validation_data, 'next') or
               hasattr(validation_data, '__next__') or
               isinstance(validation_data, Sequence))
    if (val_gen and not isinstance(validation_data, Sequence) and
            not validation_steps):
        raise ValueError('`validation_steps=None` is only valid for a'
                         ' generator based on the `keras.utils.Sequence`'
                         ' class. Please specify `validation_steps` or use'
                         ' the `keras.utils.Sequence` class.')

    # Prepare display labels.
    out_labels = model.metrics_names
    callback_metrics = out_labels + ['val_' + n for n in out_labels]

    # prepare callbacks
    model.history = cbks.History()
    _callbacks = [cbks.BaseLogger(
        stateful_metrics=model.stateful_metric_names)]
    if verbose:
        _callbacks.append(
            cbks.ProgbarLogger(
                count_mode='steps',
                stateful_metrics=model.stateful_metric_names))
    _callbacks += (callbacks or []) + [model.history]
    callbacks = cbks.CallbackList(_callbacks)

    # it's possible to callback a different model than self:
    if hasattr(model, 'callback_model') and model.callback_model:
        callback_model = model.callback_model
    else:
        callback_model = model
    callbacks.set_model(callback_model)
    callbacks.set_params({
        'epochs': epochs,
        'steps': steps_per_epoch,
        'verbose': verbose,
        'do_validation': do_validation,
        'metrics': callback_metrics,
    })
    callbacks.on_train_begin()

    enqueuer = None
    val_enqueuer = None

    try:
        if do_validation:
            if val_gen and workers > 0:
                # Create an Enqueuer that can be reused
                val_data = validation_data
                if isinstance(val_data, Sequence):
                    val_enqueuer = OrderedEnqueuer(
                        val_data,
                        use_multiprocessing=use_multiprocessing)
                    validation_steps = validation_steps or len(val_data)
                else:
                    val_enqueuer = GeneratorEnqueuer(
                        val_data,
                        use_multiprocessing=use_multiprocessing)
                val_enqueuer.start(workers=workers,
                                   max_queue_size=max_queue_size)
                val_enqueuer_gen = val_enqueuer.get()
            elif val_gen:
                val_data = validation_data
                if isinstance(val_data, Sequence):
                    val_enqueuer_gen = iter_sequence_infinite(val_data)
                    validation_steps = validation_steps or len(val_data)
                else:
                    val_enqueuer_gen = val_data
            else:
                # Prepare data for validation
                if len(validation_data) == 2:
                    val_x, val_y = validation_data
                    val_sample_weight = None
                elif len(validation_data) == 3:
                    val_x, val_y, val_sample_weight = validation_data
                else:
                    raise ValueError('`validation_data` should be a tuple '
                                     '`(val_x, val_y, val_sample_weight)` '
                                     'or `(val_x, val_y)`. Found: ' +
                                     str(validation_data))
                val_x, val_y, val_sample_weights = model._standardize_user_data(
                    val_x, val_y, val_sample_weight)
                val_data = val_x + val_y + val_sample_weights
                if model.uses_learning_phase and not isinstance(K.learning_phase(),
                                                                int):
                    val_data += [0.]
                for cbk in callbacks:
                    cbk.validation_data = val_data

        if workers > 0:
            if is_sequence:
                enqueuer = OrderedEnqueuer(
                    generator,
                    use_multiprocessing=use_multiprocessing,
                    shuffle=shuffle)
            else:
                enqueuer = GeneratorEnqueuer(
                    generator,
                    use_multiprocessing=use_multiprocessing,
                    wait_time=wait_time)
            enqueuer.start(workers=workers, max_queue_size=max_queue_size)
            output_generator = enqueuer.get()
        else:
            if is_sequence:
                output_generator = iter_sequence_infinite(generator)
            else:
                output_generator = generator

        callback_model.stop_training = False
        # Construct epoch logs.
        epoch_logs = {}
        while epoch < epochs:
            for m in model.stateful_metric_functions:
                m.reset_states()
            callbacks.on_epoch_begin(epoch)
            steps_done = 0
            batch_index = 0
            while steps_done < steps_per_epoch:
                generator_output = next(output_generator)

                if not hasattr(generator_output, '__len__'):
                    raise ValueError('Output of generator should be '
                                     'a tuple `(x, y, sample_weight)` '
                                     'or `(x, y)`. Found: ' +
                                     str(generator_output))

                if len(generator_output) == 2:
                    x, y = generator_output
                    sample_weight = None
                elif len(generator_output) == 3:
                    x, y, sample_weight = generator_output
                else:
                    raise ValueError('Output of generator should be '
                                     'a tuple `(x, y, sample_weight)` '
                                     'or `(x, y)`. Found: ' +
                                     str(generator_output))
                # build batch logs
                batch_logs = {}
                if x is None or len(x) == 0:
                    # Handle data tensors support when no input given
                    # step-size = 1 for data tensors
                    batch_size = 1
                elif isinstance(x, list):
                    batch_size = x[0].shape[0]
                elif isinstance(x, dict):
                    batch_size = list(x.values())[0].shape[0]
                else:
                    batch_size = x.shape[0]
                batch_logs['batch'] = batch_index
                batch_logs['size'] = batch_size
                callbacks.on_batch_begin(batch_index, batch_logs)

                outs = model.train_on_batch(x, y,
                                            sample_weight=sample_weight,
                                            class_weight=class_weight)

                outs = to_list(outs)
                for l, o in zip(out_labels, outs):
                    batch_logs[l] = o

                callbacks.on_batch_end(batch_index, batch_logs)

                batch_index += 1
                steps_done += 1

                # Epoch finished.
                if steps_done >= steps_per_epoch and do_validation:
                    if val_gen:
                        val_outs = model.evaluate_generator(
                            val_enqueuer_gen,
                            validation_steps,
                            workers=0)
                    else:
                        # No need for try/except because
                        # data has already been validated.
                        val_outs = model.evaluate(
                            val_x, val_y,
                            batch_size=batch_size,
                            sample_weight=val_sample_weights,
                            verbose=0)
                    val_outs = to_list(val_outs)
                    # Same labels assumed.
                    for l, o in zip(out_labels, val_outs):
                        epoch_logs['val_' + l] = o

                if callback_model.stop_training:
                    break

            callbacks.on_epoch_end(epoch, epoch_logs)
            epoch += 1
            if callback_model.stop_training:
                break

    finally:
        try:
            if enqueuer is not None:
                enqueuer.stop()
        finally:
            if val_enqueuer is not None:
                val_enqueuer.stop()

    callbacks.on_train_end()
    return model.history


def evaluate_generator(model, generator,
                       steps=None,
                       max_queue_size=10,
                       workers=1,
                       use_multiprocessing=False,
                       verbose=0):
    """See docstring for `Model.evaluate_generator`."""
    model._make_test_function()

    if hasattr(model, 'metrics'):
        for m in model.stateful_metric_functions:
            m.reset_states()
        stateful_metric_indices = [
            i for i, name in enumerate(model.metrics_names)
            if str(name) in model.stateful_metric_names]
    else:
        stateful_metric_indices = []

    steps_done = 0
    wait_time = 0.01
    outs_per_batch = []
    batch_sizes = []
    is_sequence = isinstance(generator, Sequence)
    if not is_sequence and use_multiprocessing and workers > 1:
        warnings.warn(
            UserWarning('Using a generator with `use_multiprocessing=True`'
                        ' and multiple workers may duplicate your data.'
                        ' Please consider using the`keras.utils.Sequence'
                        ' class.'))
    if steps is None:
        if is_sequence:
            steps = len(generator)
        else:
            raise ValueError('`steps=None` is only valid for a generator'
                             ' based on the `keras.utils.Sequence` class.'
                             ' Please specify `steps` or use the'
                             ' `keras.utils.Sequence` class.')
    enqueuer = None

    try:
        if workers > 0:
            if is_sequence:
                enqueuer = OrderedEnqueuer(
                    generator,
                    use_multiprocessing=use_multiprocessing)
            else:
                enqueuer = GeneratorEnqueuer(
                    generator,
                    use_multiprocessing=use_multiprocessing,
                    wait_time=wait_time)
            enqueuer.start(workers=workers, max_queue_size=max_queue_size)
            output_generator = enqueuer.get()
        else:
            if is_sequence:
                output_generator = iter_sequence_infinite(generator)
            else:
                output_generator = generator

        if verbose == 1:
            progbar = Progbar(target=steps)

        while steps_done < steps:
            generator_output = next(output_generator)
            if not hasattr(generator_output, '__len__'):
                raise ValueError('Output of generator should be a tuple '
                                 '(x, y, sample_weight) '
                                 'or (x, y). Found: ' +
                                 str(generator_output))
            if len(generator_output) == 2:
                x, y = generator_output
                sample_weight = None
            elif len(generator_output) == 3:
                x, y, sample_weight = generator_output
            else:
                raise ValueError('Output of generator should be a tuple '
                                 '(x, y, sample_weight) '
                                 'or (x, y). Found: ' +
                                 str(generator_output))
            outs = model.test_on_batch(x, y, sample_weight=sample_weight)
            outs = to_list(outs)
            outs_per_batch.append(outs)
            if steps_done + 1 >= steps: # 마지막 validation step
                pred_mask = model.predict_on_batch(x)
                plt.subplot(131)
                plt.imshow(x[0]) # batch에서 첫번째 이미지
                plt.subplot(132)
                plt.imshow(y[0].squeeze(), cmap="gray") # 이미지에 대응하는 마스크
                plt.subplot(133)
                plt.imshow(pred_mask[0].squeeze(), cmap="gray") # 예측된 마스크
                plt.show()

            if x is None or len(x) == 0:
                # Handle data tensors support when no input given
                # step-size = 1 for data tensors
                batch_size = 1
            elif isinstance(x, list):
                batch_size = x[0].shape[0]
            elif isinstance(x, dict):
                batch_size = list(x.values())[0].shape[0]
            else:
                batch_size = x.shape[0]
            if batch_size == 0:
                raise ValueError('Received an empty batch. '
                                 'Batches should contain '
                                 'at least one item.')
            steps_done += 1
            batch_sizes.append(batch_size)
            if verbose == 1:
                progbar.update(steps_done)

    finally:
        if enqueuer is not None:
            enqueuer.stop()

    averages = []
    for i in range(len(outs)):
        if i not in stateful_metric_indices:
            averages.append(np.average([out[i] for out in outs_per_batch],
                                       weights=batch_sizes))
        else:
            averages.append(np.float64(outs_per_batch[-1][i]))
    return unpack_singleton(averages)


def predict_generator(model, generator,
                      steps=None,
                      max_queue_size=10,
                      workers=1,
                      use_multiprocessing=False,
                      verbose=0):
    """See docstring for `Model.predict_generator`."""
    model._make_predict_function()

    steps_done = 0
    wait_time = 0.01
    all_outs = []
    is_sequence = isinstance(generator, Sequence)
    if not is_sequence and use_multiprocessing and workers > 1:
        warnings.warn(
            UserWarning('Using a generator with `use_multiprocessing=True`'
                        ' and multiple workers may duplicate your data.'
                        ' Please consider using the`keras.utils.Sequence'
                        ' class.'))
    if steps is None:
        if is_sequence:
            steps = len(generator)
        else:
            raise ValueError('`steps=None` is only valid for a generator'
                             ' based on the `keras.utils.Sequence` class.'
                             ' Please specify `steps` or use the'
                             ' `keras.utils.Sequence` class.')
    enqueuer = None

    try:
        if workers > 0:
            if is_sequence:
                enqueuer = OrderedEnqueuer(
                    generator,
                    use_multiprocessing=use_multiprocessing)
            else:
                enqueuer = GeneratorEnqueuer(
                    generator,
                    use_multiprocessing=use_multiprocessing,
                    wait_time=wait_time)
            enqueuer.start(workers=workers, max_queue_size=max_queue_size)
            output_generator = enqueuer.get()
        else:
            if is_sequence:
                output_generator = iter_sequence_infinite(generator)
            else:
                output_generator = generator

        if verbose == 1:
            progbar = Progbar(target=steps)

        while steps_done < steps:
            generator_output = next(output_generator)
            if isinstance(generator_output, tuple):
                # Compatibility with the generators
                # used for training.
                if len(generator_output) == 2:
                    x, _ = generator_output
                elif len(generator_output) == 3:
                    x, _, _ = generator_output
                else:
                    raise ValueError('Output of generator should be '
                                     'a tuple `(x, y, sample_weight)` '
                                     'or `(x, y)`. Found: ' +
                                     str(generator_output))
            else:
                # Assumes a generator that only
                # yields inputs (not targets and sample weights).
                x = generator_output

            outs = model.predict_on_batch(x)
            outs = to_list(outs)

            if not all_outs:
                for out in outs:
                    all_outs.append([])

            for i, out in enumerate(outs):
                all_outs[i].append(out)
            steps_done += 1
            if verbose == 1:
                progbar.update(steps_done)

    finally:
        if enqueuer is not None:
            enqueuer.stop()

    if len(all_outs) == 1:
        if steps_done == 1:
            return all_outs[0][0]
        else:
            return np.concatenate(all_outs[0])
    if steps_done == 1:
        return [out[0] for out in all_outs]
    else:
        return [np.concatenate(out) for out in all_outs]
```