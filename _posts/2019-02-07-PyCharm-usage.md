---
layout: post
title: PyCharm 사용법
author: YouWon
categories: References
tags: [usage]
---

PyCharm은 Jetbrains 사에서 제작 및 배포하는 **유료** 프로그램이다.

---

## 설치

PyCharm 홈페이지에서 설치 파일을 다운받는다. 
[Windows](https://www.jetbrains.com/pycharm/download/#section=windows), [Mac](https://www.jetbrains.com/pycharm/download/#section=mac), [Linux](https://www.jetbrains.com/pycharm/download/#section=linux)

유료 버전을 구매했거나 학생 인증이 가능하다면, Professional 버전을 다운받도록 한다.

### Settings

설치 시 다음 창을 볼 수 있다. 해당 컴퓨터에 설치한 적이 있으면 설정 파일 위치를 지정하고, 아니면 만다.


<center><img src="/public/img/PyCharm/2019-02-07-PyCharm-usage/01.PNG" width="70%"></center>

필자는 Darcula로 지정했고, 왼쪽 아래의 `Skip Remaining and Set Defaults` 버튼을 누른다. 본인이 추가 설정하고 싶은 부분이 있으면 이후 설정에서 마음대로 바꾸면 된다.

![02_install](/public/img/PyCharm/2019-02-07-PyCharm-usage/02.PNG)

설정을 완료하면 아래와 같은 화면을 볼 수 있다. 오른쪽 아래의 `Configure` > `Settings` 를 클릭한다.

![03_install](/public/img/PyCharm/2019-02-07-PyCharm-usage/03.PNG)

정확히는 `Settings for New Projects`라는 대화창을 볼 수 있다. 이는 새 프로젝트를 만들 때 적용되는 **기본 설정**이다. 새로운 설정을 만들고 싶다면 `Default` 설정을 복제(Duplicate)한 뒤 새 설정에서 바꾸도록 한다.

![04_settings](/public/img/PyCharm/2019-02-07-PyCharm-usage/04.PNG)

설정에서 `Appearance & Behavior` > `Appearance`에서, `Theme`를 `Darcula` 또는 다른 것으로 지정할 수 있다. 아래의 `Use Custom Font`는 메뉴 등의 폰트를 해당 폰트로 지정할 수 있다.  
참고로, 코드의 폰트는 `Editor` > `Font`에서 지정한다. 이 두 가지 역시 구분하도록 한다. 기본값을 `Monospaced`이다.

![05_settings](/public/img/PyCharm/2019-02-07-PyCharm-usage/05.PNG)

`Keymap`에서는 단축키를 지정할 수 있다. PyCharm의 기본 단축키는 타 프로그램과 좀 다른 부분이 많아 필자는 일부를 바꿨다.  
변경하고 싶은 단축키를 찾아서 더블클릭 또는 우클릭하면 기존에 지정되어 있는 단축키를 삭제하고 새 단축키를 지정할 수 있다. 이때 겹친다면 기존 단축키를 남겨둘지 제거할지 선택할 수 있다.

![06_settings](/public/img/PyCharm/2019-02-07-PyCharm-usage/06.PNG)

추천하는 변경할 단축키는 다음과 같다. 

Menu | 변경 전 | 변경 후
-------- | -------- | --------
Execute selection in console | Alt + Shift + E | Ctrl + Enter
Edit > Find > Replace | Ctrl + H | Ctrl + R
Refactor > Rename | Shift + F6 | F2
Other > Terminal | | Alt + T
Other > Python Console | | Alt + 8
Other > SciView | | Alt + 0

필자의 경우 나머지 설정은 그대로 두는 편이나, `Ctrl + Enter`로 바꿀 때는 다른 곳에 할당된 것을 지운다(Already assigned 경고창에서 Leave 대신 Remove를 선택). 안 그러면 선택한 부분이 Python Console(대화형)에서 실행되지 않는다.

![07_settings](/public/img/PyCharm/2019-02-07-PyCharm-usage/07.PNG)

위 그림에서 기본 Python Interpreter 파일(python.exe)를 설정한다. 새 프로젝트를 생성 시 Configure Python Interpreter라는 경고가 보이면서 코드 실행이 안 되면 인터프리터가 설정되지 않은 것이다. 컴퓨터에 설치된 파이썬 파일을 찾아 설정하자.

![08_settings](/public/img/PyCharm/2019-02-07-PyCharm-usage/08.PNG)

`Show All...`을 클릭하면 처음에는 빈 창이 보인다. `+`를 눌러서 원하는 환경을 추가한다. 기존의 것을 추가하거나, 새로운 가상환경(virtualenv 또는 conda)를 즉석에서 생성 가능하다.  
이렇게 만든 가상환경은 해당 프로젝트에서만 쓰거나(기본 설정), 아래쪽의 `Make available to all projects`를 체크하여 다른 프로젝트에서도 해당 인터프리터를 택할 수 있도록 정할 수도 있다.

![09_settings](/public/img/PyCharm/2019-02-07-PyCharm-usage/09.PNG)

PyCharm에서 코드 실행을 대화형으로 하면 Python Console에 자꾸 `Special Variables`라는 창이 뜨는 것을 볼 수 있다. 보통 쓸 일이 없는데 기본으로 표시되는 것이므로, `Build, Execution, Deployment` > `Console`에서 `Show console variable by default` 체크를 해제한다.

해당 설정을 마쳤으면 첫 화면에서 `Create New Project`를 클릭한다.

![10_new_projects](/public/img/PyCharm/2019-02-07-PyCharm-usage/10.PNG)

프로젝트 이름은 기본적으로 Untitled 이므로 바꿔주고, 아래쪽의 Project Interpreter를 설정해 둔다. 미리 설정했다면 목록이 보일 것이고, 아니라면 새로 생성하거나 `python.exe` 위치를 찾아 지정해준다.

### Sync Settings

시작 화면에서 `Configure` > `Settings Repository...`, 또는 프로젝트 생성 후 `File` > `Settings Repository...` 를 클릭하면 지금까지 설정한 설정들을 git repository에 저장할 수 있다. git을 알고 있다면, Merge, Overwrite Local, Overwrite Remote의 뜻을 알 것이라 믿는다. git repository에 저장하면 컴퓨터를 옮겨도 동일한 설정을 쉽게 지정할 수 있다.

<center><img src="/public/img/PyCharm/2019-02-07-PyCharm-usage/11.PNG" width="80%"></center>

이를 지정하려면 Personal Access Token이 필요하다. [여기](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)를 참조한다.

<center><img src="/public/img/PyCharm/2019-02-07-PyCharm-usage/12.PNG" width="70%"></center>

등록이 완료되면 Merge, Overwrite Local(git에 저장된 내용을 local로 덮어씀), Overwrite Remote(현재 local 설정을 인터넷에 덮어씀) 중 하나를 선택해 설정을 동기화할 수 있다.

여기까지 초기 설정이 끝났다(원하는 부분만 진행해도 좋다). 이제 PyCharm 프로젝트 화면을 살펴보도록 하자.

---

## Project 창



<center><img src="/public/img/PyCharm/2019-02-07-PyCharm-usage/13.PNG" width="120%"></center>

---

## References

[공식 홈페이지](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)에서 더 자세한 사용법을 찾아볼 수 있다.