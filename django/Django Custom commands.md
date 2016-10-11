# Django Custom commands


## Project Tree
```
polls/
    __init__.py
    models.py
    management/
        __init__.py
        commands/
            __init__.py
            _private.py
            closepoll.py
    tests.py
    views.py
```

## BaseCode
```python
# -*- coding: utf-8 -*-
# filename : django_custom_commmand.py
author = "SELO77"


from django.core.management.base import BaseCommand


class Command(BaseCommand):
    
    def handle(self, *args, **options):
    	print "run django_custom_command."
    	pass
```

```bash
$ python manage.py django_custom_command
> run django_custom_command
```