Date: January 25th, 2018
Tags: python, django, pythonic
Permalink: django-settings-for-multiple-environments


# Dynamically import django settings for multiple environment such as local, dev, beta, production

<img src="data:image/png;base64,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"/>

장고 개발을 하다보면 각 환경을 구분하고, 이에 따라 변경되야 하는 부분들이 존재한다. 예를들면 임포트 되어야하는 미들웨어 혹은 장고 어플리케이션들, 데이터베이스 연결정보 등이 달라진다.
이럴때 각 환경에 따라 코드를 수정하는 작업은 해서는 안되는 작업이며, 언젠가 한번쯤 실수를 할것이고, 비명을 지르게될것이다. ~~"꺄야양야야야야야 ㅅㅂ"~~


## "어떻게 환경을 인식을 할 것인가?"
어플리케이션이 작동하는 환경을 나누어 보자. 아래처럼 환경을 구분하는 것이 정답은 아니며, 개발프로세스나 회사 업무에 따라 다를것이다. 

* `local`: 개발자 개인PC
* `development`: 사내 네트워크 망에서 공유되는 서버를 통해 작동하는 어플리케이션 환경
* `staing`: 외부 네트워크 망에서 작동하는 서버에서 제공되는 어플리케이션 환경. 릴리즈 전 1차 외부 테스트 환경.
* `beta`: `staging`과 동일하지만 `production`과 최대한 일치되는 환경으로 `production` 릴리즈 이전에 최종테스트가 일어나는 환경.
* `production`: 어플리케이션이 실제 서비스 되는 환경.

위와 같이 다양한 환경에서 동일한 코드베이스의 장고 어플리케이션이 동작하게 되며, 각 환경에서 로드되어야하는 모듈이나 설정정보들이 다를것이다. 이렇게 다른점을 적용하기 위해서는 현재 어플리케이션이 작동하는 환경이 `production` 인지 `local` 인지 인식을 해야한다.

현재 작동하는 환경을 인식하기 위한 방법은 다양하지만, 필자가 가장 선호하는 방법은 환경변수 셋팅이다.

`export SELO_ENV=local`

TIP) 개인 개발PC `.bash_profile`와 같은 쉘 부트스트래핑 파일에 구문을 추가해 놓으면 편하다.

```python
import os
CURRENT_APP_ENV = os.getenv('SELO_ENV')
```

## "각 환경별 설정 모듈화 및 디렉토리 구조잡기"
***유연하며***, ***명시적인*** 구조를 만들어보자. "Here we go. Let's get it. Put your hands up!!!"

### 시작이 반: 장고 프로젝트 초기 구조

```bash
##### 장고의 초기 디렉토리 구조 #####
$ tree
.
|____bar
| |______init__.py
| |____settings.py
| |____urls.py
| |____wsgi.py
|____manage.py
```


버전 `1.11` 기준 장고의 프로젝트를 처음 생성하면 위와 같은 디렉토리 구조로 프로젝트가 생성된다. 아주 심플하다. 구조화 없이 프로젝트가 진행되다보면 알아보기 무서운 "괴물"이 될수있다.

### 결과물: 아주 이쁘게 모듈화된 최종 구조

먼저 우리가 갖게 될 최종 구조를 훓어보고 큰 그림을 그려보자. 최종구조는 다음과 같다.

```bash
$ tree
.
|____meta
| |______init__.py
| |____unittest.py
| |____ckeditor.py
| |____ ...
|______init__.py
|____default.py
|____development.py
|____staging.py
|____beta.py
|____prod.py

# 한눈에 보기 편하도록 출력결과물의 순서를 조정했다.
```

~~"설명이 필요없는 한눈에 이해되는 명확한 구조가 아닌가!! 아름답다!! 나만 그런가...?!!"~~

위 최종구조에서 각 모듈(파일)의 역활을 정리해보자.

* `settings/default.py`: 환경에 상관없는 공통적인 설정을 작성한다.
	* 기본 앱. e.g) `'django.contrib.admin'`, `'django.contrib.auth'`
	* 공통 미들웨어
	* `TIMEZONE` 등 서비스에서 통용되는 상수
	
* `settings/<env_name>.py`: `prod.py` 와 같이 각 환경에 의존적인 설정을 작성한다. `<env_name>.py`.
	* 개발환경에서의 디버깅 툴. e.g) [Django Debug Toolbar](http://django-debug-toolbar.readthedocs.io/en/stable/index.html)
	* 데이터베이스 커넥션 정보
	* 환경별 미들웨어. e.g) 베타유저 인증 미들웨어
	* `TOKEN` 과 같은 환경별 상수
	* 모니터링 툴. e.g) `NewRelic`

	
* `settings/meta/<*.py>`: `meta` 폴더 하위에는 특정 라이브러리의 설정이나 특별한 실행 환경에서 실행되는 설정에 대한 부분을 작성한다. 
	* `$ python manage.py test` 처럼 테스트 실행과 같은 특별한 환경에서 필요한 설정 혹은 라이브러리 [`monkey patch`](https://stackoverflow.com/questions/5626193/what-is-monkey-patching)와 같은 작업을 진행한다.
	* 많은 라인(15줄 이상)의 설정이 필요한 라이브러리([`django-ckeditor`](https://django-ckeditor.readthedocs.io/en/latest/) 등). `settings/meta/ckeditor.py`
	* `default.py` 가 읽기 힘들정도로 길어질 때 모듈로 분리해야하는 경우


### Import: 즐거운 코딩하는 시간

##### manage.py
```python
...
# setdefault 함수의 2번째 인자 부분을 알맞게 변경해주자
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "foo.settings.default")
...
```

##### foo/settings/default.py
환경변수를 읽어, 셋팅을 동적으로 로드하자.

```python
import os


try:
    SELO_ENV = os.environ['SELO_ENV']
except KeyError:
    print "SELO_ENV must be set to OS Variable."
    exit(1)
    
if SELO_ENV == 'development':
    from foo.settings.development import *
elif SELO_ENV == 'staging':
    from foo.settings.staging import *
elif SELO_ENV == 'beta':
    from foo.settings.beta import *
elif SELO_ENV == 'prod':
    from foo.settings.beta import *
else:
    print "Given Value is %s. It's invalid environment name." % SELO_ENV
    exit(1)

```

코드가 드럽다. 안이쁘다. 구려보인다. 이쁘게 바꿔보자. ~~코드는 역시 외모지상주의!!~~

```python
try:
    SELO_ENV = os.environ['SELO_ENV']
except KeyError:
    print "SELO_ENV must be set to OS Variable."
    exit(1)
    
try:
    __import__(SELO_ENV)
except ImportError:
    print "Given Value is %s. It's invalid environment name." % SELO_ENV
    exit(1)
else:
    print("Successfully Loaded %s settings." % SELO_ENV)
  
```

위 코드를 간단히 설명하겠다. `__import__` 는 빌트인 함수이며, 동적으로 모듈을 임포트할 수 있게해준다. 절대경로와 상대경로 임포트 모두 제공한다. 코드로 풀어서 설명하자면, `__import__('development')`는 `from development import *`와 동등한 기능을 수행한다. 결론적으로 `development.py` 모듈의 모든 컨텍스트가 참조된다. 설명은 파이썬2 기준이며, 파이썬3에서는 사용해보지 않았다.


## 정리
필자가 참여한 장고 프로젝트에는 신규 프로젝트도 있었고, 몇 년동안 개발 운영 되고 있는 서비스도 있었다. 프로젝트에서 이러한 환경에 따른 설정은 매우 치명적인 부분임에도 불구하고, 무분별하게 관리되는 경우가 있었다. 문제가 안발생하면 다행이지만... 코드를 공유하는 개발자들 사이에서 설정을 변경 후 푸쉬하는 경우로 문제가 발생했던 적이 몇 번 있었다. 특히, 디비커넥션 정보, 디버거 임포트 등과 같이 개발중인 기능에 의존적인 설정들이 예가 될 것이다. 위 방식과 구조를 취한다면 이러한 문제들을 해결할 수 있을 것이다.

PS. 언젠가 정리해야지 막연히 생각하던걸 새로운 회사에서 또 한번(ㅠ?) 장고프로젝트를 진행하게 됬고, 이 참에 정리했다... 개운하구만!! 그럼 이만 뿅!!

오타나 잘못된 부분에 대한 언급은 언제나 감사드립니다.



