# Django Custom Migration

django 1.7

### why needs custom migration?
서비스 운영을 하다보면 버전이 바뀌거나, 테이블 Schema 가 바뀌거나, 데이터가 뻑나거나 등등 데이터 마이그레이션이 필요한 순간이 있습니다. 가장 쉬운 방법은 스크립트를 작성해 데이터를 수정하는 코드를 작성하는 것입니다. 하지만 장고 프레임웍에서는 그보다 안정적이며 관리하기 쉬운 방법을 제공합니다. 바로 아래에서 살펴볼 custom migration file을 작성하는 것입니다.

위에서 말했듯 장고에서 제공하는 database migration은 스크립트 작성을 통한 데이터 수정보다 한층 고차원 수준의 migration을 제공합니다. forwards와 backwards 함수를 통해 forward와 backward를 간편하게 구현 해주며, 다양한 옵션들을 통해 선택적 마이그레이션을 가능하게 해줍니다. 이번 포스팅에서는 가장 기본적인 사용법을 살펴보겠습니다.


### make empty migration file

```bash
$ python manage.py makemigrations --empty <app_name>
```


### forwards function
forwards migrations 요구사항 코드 작성

### backwards function
backwards migrations 요구사항 코드 작성


### base code
```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals

from django.db import models, migrations


def forwards(apps, schema_editor):
    pass


def backwards(apps, schema_editor):
    pass


class Migration(migrations.Migration):

    dependencies = [
        ('polls', '0008_auto_20161002_0608'),
    ]

    operations = [
        migrations.RunPython(forwards,
                             backwards)
    ]
```

### migrate commands
```bash
$ python manage.py migrate [app_name]
```

### migrate backwards command
```
$ python manage.py migrate <app_name> <migrations_no>
```





## References
* [Writing database migrations](https://docs.djangoproject.com/en/1.10/howto/writing-migrations/) - django database migrations official document
