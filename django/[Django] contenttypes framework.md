Tags: Django, Python
Permalinks: django-contrib-contenttypes


# [Django] contenttypes framework

부제: `django.contrib.contenttypes` 은 무엇인가?

장고 프로젝트를 생성하고, `settings.py` 파일을 보면 기본값 설정들이 되어있고, `INSTALLED_APPS` 하위에 이미 어떤 장고 어플리케이션들이 선언되어있다. 해당 어플리케이션인들이 프로젝트에 판단하기 위해 역할을 알아볼 것이다. 이 포스트에서는 `django.contrib.contenttypes`에 대해 살펴볼것이다.

## `django.contrib.contenttypes`
네이밍이 `contenttypes` 이여서 HTTP Header의 속성 Content-Type 에 관련된 앱인줄 알았다. 하지만 설명을 읽어보니 전혀 상관없었다.

> Django includes a contenttypes application that can track all of the models installed in your Django-powered project, providing a high-level, generic interface for working with your models.

설명을 보아하니, 장고 프로젝트안에서 선언된 모델을 추적하며, 높은수준의 사용하기 편한 인터페이스를 제공한단다. 목적은 파악되었다. 아직 필요한지 제거해도 되는지 판단이 서지 않아서 조금 더 알아보겠다.

> It's generally a good idea to have the contenttypes framework installed; several of Django's other bundled applications require it:

> The admin application uses it to log the history of each object added or changed through the admin interface.
Django's authentication framework uses it to tie user permissions to specific models.

몇몇 장고의 번들 어플리케이션을 사용할때 `contenttypes framework` 이 필요하단다... (몇몇!!!??? ~~말이여 방구여~~ 그냥 쓰라는 의미로 받아들이자.)
장고 인증 프레임웍과 어드민에 의존 어플리케이션이란다. 더 이상 읽을 필요가 없다. 사용하기로 결정!!

PS. 장고에서 코드를 작성할때, 모델 사용을 참조 형식이 아닌, 문자열 형식으로 선언하고 런타임 환경에서 Lazy Loading 형식으로 구현하는 경우가 많다. 추축이지만 아마 contenttypes framework 때문에 가능한것 같다. 코드를 까서 살펴보면 되지만, 시간이 없다. 조사는 여기까지 그만 짜이찌엔!!
