# django with multiple database

## 배경지식
시스템 규모가 작을때는 일반적으로 하나의 데이터베이스에서 모든 데이터를 관리합니다.(로그같은 특수 목적의 데이터는 제외) 이유는 데이터 양이 많지 않은데 굳이 멀티 데이터베이스를 쓰면서, 관리 포인트를 늘일필요가 없고, 데이터베이스 늘어날때마다 모든면에서 복잡도가 매우 높아집니다.

 하지만 대륙을 넘나 드는 서비스를 한다거나, 단일 데이터베이스에서 감당하기 힘든 데이터가 쌓인다면, 복잡도를 감수하더라도 멀티 데이터베이스를 고려해야할 상황이 생깁니다. 지금 저희 www.mymusictaste.com 서비스가 위에서 언급한 두 가지 상황에 쳐해있고, 해결 방향을 찾아가는 과도기 입니다. 이런 배경을 갖고 개발을 진행하고 있으며, 이번 제가 맡은 업무중 하나는 여러개의 Django app 중 (저희는 서비스 별로 app을 나누고 있습니다.) 하나를 다른 데이터 베이스로 이전하는 작업을 하게 되었습니다.

## django support multiple database ?
 장고는 멀티데이터베이스를 지원합니다.  아주 잘 디자인된 웹프레임웍 답게 멀티데이터 베이스를 Config 설정에서 간단히 지원합니다. 하지만 각 데이터베이스의 통신을 위한 라우팅은 개발자의 몫입니다. 일반적으로 라우팅을 해주지 않는다면,  `default` 데이터베이스가 사용됩니다.
 > The default routing scheme ensures that if a database isn’t specified, all queries fall back to the default database.

### How to route to which Database ?
그렇다면 멀티데이터 베이스에 어떻게 라우팅 할것인가? 위에서도 말했지만 라우팅은 부분은 전부 개발자의 몫이다. 공식문서에 가이드가 있지만 random 을 활용한 example 정도 수준이며,  Google 검색결과 매우 다양한 방식의 구현을 찾아볼수 있다. 모델 수준에서 사용할 `database`를 명시하는 경우도 있고,  라우터를 세세하게 작성하는 경우도 있었다. 나는 첫번째 방법인, 모델 수준에서 사용할 데이터베이스를 명시하는것은 각 모델 수준에서 routing을 관리해야하기 때문에 좋지 않은 디자인으로 생각했고, 두번째 방법을 선택하기로 했다.



## 구현 샘플 코드 및 설명
예제는 순서대로 진행하면 된다. 초기 프로젝트라면 쉽게 구성할 수 있지만, Legacy가 많은 코드라면 고생을 각오해야한다.
말씀 드리지 않아도 아시겠지만, 아래 작성된 코드와 방식은 이해를 위한 샘플이지 best example은 결코 아니다.

### define database setting
데이터 베이스를 생성 후, 커넥션 정보를 셋팅파일에 추가한다.

settings.py

```python
DATABASES = {
    'default': {
        'NAME': 'master',
        'ENGINE': 'django.db.backends.mysql',
        'USER': 'mysql_user',
        'PASSWORD': 'spam',
    },
    'auth_db': {
        'NAME': 'auth_db',
        'ENGINE': 'django.db.backends.mysql',
        'USER': 'mysql_user',
        'PASSWORD': 'swordfish',
    },
}
```

### define routing
커스텀 라우터 모듈을 작성 후 settings.py 에 작성된 라우터를 사용한다고 선언한다.

router.py

```python
class MultiDBRouter(object):
	def db_for_read(self, model, **hints):
		if model._meta.app_label == 'auth':
			return 'auth_db'
		return 'default'
		
	def db_for_write(self, model, **hints):
		if model._meta.app_label == 'auth':
			return 'auth_db'
		return 'default'

    def allow_migrate(self, db, model):
	    if db == 'auth_db':
	        if model._meta.app_label == 'auth':
	            return True
	        else:
	            return False
	    return True
```


settings.py

```python
...
DATABASE_ROUTERS = [
    '<router_path>',    # e.g) snsproject.core.routers.MultiDBRouter
]
```

### grant mysql user
(MySQL 기준) 새로운 database를 생성하고, application 에서 connection을 맺으려 할시 `Access Denied` 에러가 발생할 것이다. 이를 해결 하기 위해 application user에 새로 생성한 database의 접근 권한을 부여하자. 위 작업은 root 권한 혹은 user 권한 변경이 가능한 DB user가 필요하다.

```sql
GRANT ALL ON <database_name>.* TO 'selo'@'%';
```

### synchronizing new database
```bash
> python manage.py migrate --database=auth_db
```

### when migration issue is raised
만약 마이그레이션을 진행시 마이그레이션 이슈가 발생한다면 기존 마이그레이션 파일의 변경이 필요할수도 있다. 장고 마이그레이션은 매우 편리하지만 100퍼센트 완벽하지는 않아보인다. 이러한 부분은 개발자가 풀어야할 숙제이며, 나는 다음과 같은 방법으로 이 문제를 해결하려한다.

1. 에러가 발생한 마이그레이션 파일을 수정한다. 아래는 멀티디비 구현방식에 따라 달라질 수 있다.
2. 분기 추가 후 위 sync명령어 재구동.

```python
from django.db import migrations

def forwards(apps, schema_editor):
    if not schema_editor.connection.alias == 'default':
        return
    # Your migration code goes here

class Migration(migrations.Migration):

    dependencies = [
        # Dependencies to other migrations
    ]

    operations = [
        migrations.RunPython(forwards),
    ]
```

## 마치며
항상 느끼는거지만 장고는 정말 정말 많은 기능을 품고 있다. 멀티 데이터베이스도 간단하게(???) 지원한다. 하지만 어디까지나 장고가 추구하는 디자인과 API 표준을 벗어나지 않는 범위에서 구현된 프로젝트에만 해당되는 이야기다. 그리고 위 포스팅에서는 다루지 않았지만 1.7 이하의 버전을 쓰고 있다면 몇 가지 제약이 존재한다.


### Reference
* [Django mutiple database](https://docs.djangoproject.com/en/1.7/topics/db/multi-db/)
* [Django Data migrations and multiple databases](
https://docs.djangoproject.com/en/1.9/howto/writing-migrations/#data-migrations-and-multiple-databases)
