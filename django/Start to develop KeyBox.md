# Start to develop KeyBox

Keybox 란 어떤 용어나 개념을 한문장으로 요약해 학습하기 위한 웹 어플리케이션.

PS. 아래 내용은 개인 정리를 위해 작성된 포스트이므로 많은 부분이 생략되어있습니다. 궁금한 부부에 대한 질문은 댓글을 남겨주시면 시간되는대로 답변드리겠습니다.

## 파이썬 그리고 장고 개발환경 셋팅
pyenv, virtualenv 를 활용한 개발환경 가상환경 셋팅. 한 피씨에서 하나의 특정 어플리케이션만 개발한다면 가상환경이 필요하지 않을지도 모릅니다. 하지만 저와 같이 여러 파이썬 프로젝트를 함께 진행한다면 가상 환경 셋팅은 모든 파이썬 개발의 필수적인 시작 과정입니다.

### Python 가상환경 셋팅 (pyenv, virtualenv)
```bash
$ pyenv global 3.5.0
$ python -V
Python 3.5.0
$ mkdir keybox
$ cd keybox
$ virtualenv venv
Using base prefix '/Users/selochanlee/.pyenv/versions/3.5.0'
New python executable in /Users/selochanlee/Documents/github/keybox/venv/bin/python3.5
Also creating executable in /Users/selochanlee/Documents/github/keybox/venv/bin/python
Installing setuptools, pip, wheel...done.
```
저는 한 PC에서 여러 버전의 파이썬, 특히 2.7.8 과 3.5.0, 을 사용합니다. 이러한 문제를 쉽게 해결해주는 pyenv tool을 사용해 위와 같이 개발환경 셋팅을 합니다. 위와 같이 pc의 python version을 3.5.0으로 셋팅 후 프로젝트의 독립적인 개발환경을 위해 `virtualenv <원하는 가상환경 이름>`를 이용해 가상환경을 만듭니다. 


### install django framework
```bash
$ source venv/bin/activate # 가상 환경 실행
(venv) $ # 실행 후 Command창 prefix가 붙음, (<virtualenv_name>)$
(venv) $ pip install -U django
Collecting django
  Downloading Django-1.10.2-py2.py3-none-any.whl (6.8MB)
    100% |████████████████████████████████| 6.8MB 204kB/s
Installing collected packages: django
Successfully installed django-1.10.2 

(venv) $ pip list
Django (1.10.2)
pip (8.1.2)
setuptools (28.5.0)
wheel (0.30.0a0)
```

`pip install`를 이용하여 django 최신 버전을 설치합니다. 정상적으로 설치가 됬는지 `pip list` 명령어를 이용해 확인합니다. 


## 장고 프로젝트 생성
위의 내용을 모두 진행하셨다면, 파이썬 기반의 장고 웹프레임웍을 사용하요 개발할 수 있는 환경이 끝나셨습니다. 약을 좀 팔자면 명령어 한줄로 웹 어플리케이션을 생성하실수 있습니다. 무슨 말인지는 아래 내용을 보시게 되면 이해가 될겁니다.

### 장고 프로젝트 생성
```bash
(venv) $ django-admin help
Type 'django-admin help <subcommand>' for help on a specific subcommand.

Available subcommands:

[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    runserver
    sendtestemail
    shell
    showmigrations
    sqlflush
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    test
    testserver
```
장고는 메인 커맨드 `django-admin`에 어떤 명령어가 있는지 살펴보았습니다. 아래에서 3번째 줄을 보면 우리가 필요한 명령어가 보이실 겁니다. 그럼 만들어 봅시다.

(커멘드 창이 바뀌었는데요. 옮겨 적는게 귀찮아져서.... 제 실제 커맨드를 복사 붙여넣기 한 부분이니 이해해주시기 바랍니다. 처음부터 그냥 이리할걸....)

```bash
(venv)  ✝  ~/Documents/github/keybox  django-admin startproject keybox
(venv)  ✝  ~/Documents/github/keybox  ls
keybox venv
```

위의 과정을 진행하시면 kyebox라는 디렉토리가 생성된것을 보실수 있습니다. 그럼 같이 keybox 디렉토리에는 무엇이 들어있고, 무엇을 할수있는지 같이 살펴보겠습니다.

### manage.py
```bash
(venv)  ✝  ~/Documents/github/keybox  cd keybox
(venv)  ✝  ~/Documents/github/keybox/keybox  ls
keybox    manage.py
(venv)  ✝  ~/Documents/github/keybox/keybox  python manage.py runserver
Performing system checks...

System check identified no issues (0 silenced).

You have 13 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

October 15, 2016 - 06:22:08
Django version 1.10.2, using settings 'keybox.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

`cd keybox` 키박스 디렉토리로 들어가 `ls` 폴더에 들어있는 파일을 리스트 해보니 또 다른 `keybox` 폴더와 `manage.py`라는 파이썬 파일이 보입니다. `manage.py`파일은 장고 어플리케이션의 모든 명령을 통솔합니다. 더 쉽게 이해해보자면 리모컨이라 생각하시면됩니다. `manage.py` 라는 장고 어플리케이션 리모콘에는 여러가지 버튼이 있습니다. 그 중 저는 `runserver`라는 버튼을 눌렀고, 버튼에 해당하는 어플리케이션 기능이 작동했습니다. 

그러면 제가 작동시킨 기능은 어떤 기능인지 확인해보겠습니다. 브라우저를 열고 `http://127.0.0.1:8000/`를 들어가시면 웹 어플리케이션이 작동하시는 것을 확인하실수 있습니다.

사실 개인 일일 해커톤으로 최대한 빠르게 어플리케이션을 개발하려는 계획 이었는데, 이렇게 글을 쓰다보니 저도 모르게 너무 말이 많아젔네요. 오늘은 여기서 마무리 하고 다음편은 할 수 있을지 없을지 모르지만 ... 하게 된다면 본격 장고 어플리케이션 개발기가 되겠군요. 그럼 진짜 이만 ㅃㅃ