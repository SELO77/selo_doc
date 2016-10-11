# Django Custom Migration

django 1.7

### make empty migration file

```
$ python manage.py makemigrations --empty polls
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
    if not schema_editor.connection.alias == 'default':
        return


def backwards(apps, schema_editor):
    if not schema_editor.connection.alias == 'default':
        return


class Migration(migrations.Migration):

    dependencies = [
        ('polls', '0008_auto_20161002_0608'),
    ]

    operations = [
        migrations.RunPython(forwards,
                             backwards)
    ]
```


### migrate backwards command
```
$ python manage.py migrate <app_name> <migrations_no>
```




## References
* [Writing database migrations](https://docs.djangoproject.com/en/1.10/howto/writing-migrations/) - django database migrations official document