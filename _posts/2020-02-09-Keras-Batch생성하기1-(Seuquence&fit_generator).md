layout: post
title: Keras-Batch 생성하기 (Sequence & fit_generator)
author: Sunwoo Kim
categories: Frameworks
tags: [Keras]

---

논문을 보고 모델을 구현할 때 가장 시간이 많이 드는 것은 아무래도 레이어를 쌓아서 모델구조를 만드는것 보다 데이터를 로드하여 배치를 짜는 부분을 만들고, backward하는 부분과 evaluation하는 부분이라고 생각이 듭니다. 여태까지 이미지를 다룰 때 Numpy 라이브러리만을 이용해서 데이터를 다루었었는데요,  Keras를 이용할 때, keras에 존재하는 기능을 이용하여 데이터 로드하는 부분을 구현하니 매우 간편한거 같습니다. 인터넷에서도 많은 분들이 다루어 주셨기 때문에 공부하는데 그 글들을 많이 참고하였습니다. 이번에는 제가 구현한 부분에 대해서 말씀 드리고자 합니다.

첫번째로 케라스의 **keras.utils.Sequence** 를 상속받아 generator를 구현하여 사용하는 방법입니다. 기본적으로 keras.utils.Seuence에 abtstractMethod로 구현되어 있는 부분을 구현하여 사용합니다. 구조는 아래와 같습니다.

```python
class DataGenerator(keras.utils.Sequence): # 클래스 명은 꼭 DataGenerator가 아니여도 됨
    def __init__(self, *args)
    def on_epoch_end(self)
    def __len__(self)
    def __getitem__(self, index)
```

각 기능은 아래와 같습니다.

-  \__init__ : 클래스의 생성자 입니다. 클래스를 생성할 때 인자들을 받아옵니다. 클래스를 생성시  default값이 존재하지 않는이상 반드시 입력을 해주어야 하며, 클래스 생성과 동시에 작동하는 함수입니다.
- on_epoch_end : 한 epoch을 수행한 후에 fit_generator함수 안에서 호출되는 함수입니다.
- \__len__ : 함수 이름에서 볼 수 있듯이, 길이를 호출하는 함수입니다. 그렇기 때문에 return값으로 길이를 반환하게 됩니다. 이 때 사용자가 길이를 자유롭게 설정하여 반화할 수 있지만, 보통, 한 epoch에 존재하는 batch수를 반환하면 됩니다. 왜냐하면 이 길이를 range로 하여 0부터 해당 길이까지의 값이 getitem함수에 사용될 index로 반환되기 때문입니다.
- \__getitem__ : 실질적으로 배치를 반환하는 부분입니다. 제가 작성한 코드를 보고 이해해 보면 좋을것 같습니다.

```python
import os #for accessing the file system of the system
import random
from skimage import io
from skimage.transform import resize
import numpy as np
import keras

# data generator class
class DataGenerator(keras.utils.Sequence):
    def __init__(self, ids, imgs_dir, masks_dir, batch_size=10, img_size=128, n_classes=1, n_channels=3, shuffle=True):
        self.id_names = ids
        self.indexes = np.arange(len(self.id_names))
        self.imgs_dir = imgs_dir
        self.masks_dir = masks_dir
        self.batch_size = batch_size
        self.img_size = img_size
        self.n_classes = n_classes
        self.n_channels = n_channels
        self.shuffle = shuffle
        self.on_epoch_end()

    # for printing the statistics of the function
    def on_epoch_end(self):
        'Updates indexes after each epoch'
        self.indexes = np.arange(len(self.id_names))
        if self.shuffle == True:
            np.random.shuffle(self.indexes)

    def __data_generation__(self, id_name):
        'Generates data containing batch_size samples'
        # Initialization
        img_path = os.path.join(self.imgs_dir, id_name) # 이미지 1개 경로
        mask_path = os.path.join(self.masks_dir, id_name) # 마스크 1개 경로

        img = io.imread(img_path)
        mask = cv2.imread(mask_path)

        # image normalization
        image = image / 255.0
        mask = mask / 255.0

        return image, mask

    def __len__(self):
        "Denotes the number of batches per epoch"
        # self.id_names: 존재하는 총 이미지 개수를 의미합니다.
        # self.batch_size: 배치사이즈를 의미합니다.
        return int(np.floor(len(self.id_names) / self.batch_size))

    def __getitem__(self, index):  # index : batch no.
        # Generate indexes of the batch
        indexes = self.indexes[index * self.batch_size:(index + 1) * self.batch_size]
        batch_ids = [self.id_names[k] for k in indexes]

        imgs = list()
        masks = list()

        for id_name in batch_ids:
            img, mask = self.__data_generation__(id_name)
            imgs.append(img)
            masks.append(np.expand_dims(mask,-1))

        imgs = np.array(imgs)
        masks = np.array(masks)

        return imgs, masks  # return batch
```

구조를 간략하게 말씀드리자면 다음과 같습니다. 클래스를 생성할 때, init이 호출되어 클래스내의 멤버변수를 초기화 합니다.  그리고 len함수에 return되는 길이값을 보시면 이미지개수를 배치수로 나누어, 한 epoch내에 존재할 수 있는 배치수를 길이로 반환하게 됩니다. 그리고 최종적으로 getitem에서 배치를 반환하게 됩니다. 여기서 제가 수행하려는 작업은 image sgementation 작업이여서 이미지와 정답인 마스크를 불러와서 배치로 만드는 작업이 필요한데 해당작업을 data_generation함수에서 수행하였습니다. 

getitem함수를 보시면, 함수 인자에 **index**가 있는 모습을 볼 수 있습니다 이 인덱스는 **range(0, len)** 사이의 인덱스가 차례대로 반환된 값입니다. 그래서 해당 인덱스를 참조하여, 해당 인덱스에 해당하는 이미지와 마스크를 배치사이즈 만큼 불러들여 배치를 생성하게 합니다.

그러면 제가 작성한 train code를 첨가하여 최종적인 설명을 하도록 하겠습니다.

```python
import os  # for accessing the file system of the system
from keras import optimizers
from keras.callbacks import EarlyStopping, ModelCheckpoint, ReduceLROnPlateau
import segmentation_models as sm
from segmentation_models.utils import set_trainable
from dataset import DataGenerator

if __name__ == '__main__':
    # hyperparameter
    image_size = 384
    train_path = './data/train/imgs/'  # 이미지 파일들의 경로
    mask_path = './data/train/masks/'  # 마스크 파일들의 경로
    epochs = 50  # number of time we need to train dataset
    lr = 1e-5
    batch_size = 2  # tarining batch size

    # train path
    train_ids = os.listdir(train_path) #[1.jpg, 2.jpg .....]
    # Validation Data Size
    val_data_size = 69  # size of set of images used for the validation

    valid_ids = train_ids[:val_data_size] #[1.jpg, 2.jpg, ... 69.jpg]
    train_ids = train_ids[val_data_size:] #[70.jpg, 71.jpg, ... n.jpg]
	
    # train, validation Datagenerator 클래스를  각각 생성합니다.
    train_gen = DataGenerator(train_ids, train_path, mask_path, img_size=image_size, batch_size=batch_size)
    valid_gen = DataGenerator(valid_ids, train_path, mask_path, img_size=image_size, batch_size=batch_size)
    # 여기서 ids, train_path, mask_path는 os.path.join(id, train_path)이런 식으로 경로로 결합하여, 최종적인 이미지의 경로가 됩니다. 이 경로는 앞서 구현한 DataGenerator클래스에서 이미지와 마스크를 불러들이는데 사용됩니다.

    print("total training batches: ", len(train_gen))
    print("total validaton batches: ", len(valid_gen))
    train_steps = len(train_ids) // batch_size
    valid_steps = len(valid_ids) // batch_size

    BACKBONE = 'resnet34'
    #preprocess_input = sm.get_preprocessing(BACKBONE)

    # define model
    model = sm.Unet(BACKBONE, encoder_weights='imagenet')

    optimizer = optimizers.Adam(lr=lr, decay=1e-5)
    model.compile(
        optimizer=optimizer,
        loss=sm.losses.bce_dice_loss,
        metrics=[sm.metrics.iou_score],
    )

    # fit model
    set_trainable(model)
    model.fit_generator(generator=train_gen, validation_data=valid_gen,
                        steps_per_epoch=train_steps, validation_steps=valid_steps,
                        epochs=epochs)

```

코드는 위와 같습니다. 즉, train하는 부분에서 image와 label의 경로들의 리스트를 만들어 주고 그 값을 Datagernator의 인자로 넣어서, Datagernator에서는 그 경로를 참조하여 이미지와 레이블을 불러온 후, 배치로 반환하는 역할을 하게 코드를 작성합니다. 그리고 그 DataGenerator 객테를 fit_generator 함수에 인자로 넣으면 훈련이 진행되게 됩니다.