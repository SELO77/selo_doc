# Introduction tutorial - Graphene and Django

### requirements
* [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)
* [sqllite3](https://www.sqlite.org/cli.html)


## Set up the Django project
##### setup environment
```shell
# Make project environment for isolation
▶ pyenv virtualenv 3.6.1 graphene-cookbook

# List virtualenvs installed
▶ pyenv virtualenvs 

# Activate the virtual environment
▶ pyenv activate graphene-cookbook

# Install Libraries
▶ pip install django==2.0
▶ pip install graphene-django=2.0.0

```


##### initialize project
```shell
▶ django-admin startproject cookbook
▶ cd cookbook
▶ django-admin.py startapp ingredients
▶ ls
.           ..          cookbook    ingredients manage.py

# migrate basic models such as user, user group, user session
▶ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying sessions.0001_initial... OK
```


##### check the database migrated
```
# RUN sqllite CLI
▶ sqlite3 db.sqlite3

# List names and files of attached databases
sqlite> .databases
main: /Users/selo/Projects/graphene-cookbook/cookbook/db.sqlite3

# List names of tables
sqlite> .tables
auth_group                  auth_user_user_permissions
auth_group_permissions      django_admin_log
auth_permission             django_content_type
auth_user                   django_migrations
auth_user_groups            django_session
```

##### Defining a few models
```
/graphene-cookbook/cookbook/ingredients/models.py
from django.db import models


class Category(models.Model):
    name = models.CharField(max_length=100)

    def __str__(self):
        return self.name


class Ingredient(models.Model):
    name = models.CharField(max_length=100)
    notes = models.TextField()
    category = models.ForeignKey(Category, related_name='ingredients',
    on_delete=models.CASCADE)

    def __str__(self):
        return self.name

```

새로 생성한 `ingredient` 앱을 설치된 앱 목록에 추가합니다.

```python
SERVICE_APPS = [
    'ingredients',
]

INSTALLED_APPS = [
    ...
] + SERVICE_APPS
```

새롭게 정의한 `Category`, `Ingredient` 모델을 Database에 적용합니다.

```
▶ python manage.py makemigrations
Migrations for 'ingredients':
  ingredients/migrations/0001_initial.py
    - Create model Category
    - Create model Ingredient
▶ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, ingredients, sessions
Running migrations:
  Applying ingredients.0001_initial... OK
```

##### setup test data
아래 JSON 형식의 문자열를 이용하여 `/ingredients/fixtures/ingredients.json` 파일을 만듭니다.

```json
[{"model": "ingredients.category", "pk": 1, "fields": {"name": "Dairy"}}, {"model": "ingredients.category", "pk": 2, "fields": {"name": "Meat"}}, {"model": "ingredients.ingredient", "pk": 1, "fields": {"name": "Eggs", "notes": "Good old eggs", "category": 1}}, {"model": "ingredients.ingredient", "pk": 2, "fields": {"name": "Milk", "notes": "Comes from a cow", "category": 1}}, {"model": "ingredients.ingredient", "pk": 3, "fields": {"name": "Beef", "notes": "Much like milk, this comes from a cow", "category": 2}}, {"model": "ingredients.ingredient", "pk": 4, "fields": {"name": "Chicken", "notes": "Definitely doesn't come from a cow", "category": 2}}]
```

위에서 만들었던 fixture 파일을 DB에 로드합니다.

```shell
▶ python manage.py loaddata ingredients
Installed 6 object(s) from 1 fixture(s)
```



## Hello GraphQL - Schema and Object Types
ingredients/schema.py

```
import graphene

from graphene_django.types import DjangoObjectType

from cookbook.ingredients.models import Category, Ingredient


class CategoryType(DjangoObjectType):
    class Meta:
        model = Category


class IngredientType(DjangoObjectType):
    class Meta:
        model = Ingredient


class Query(object):
    all_categories = graphene.List(CategoryType)
    all_ingredients = graphene.List(IngredientType)

    def resolve_all_categories(self, info, **kwargs):
        return Category.objects.all()

    def resolve_all_ingredients(self, info, **kwargs):
        # We can easily optimize query count in the resolve method
        return Ingredient.objects.select_related('category').all()
```



## Reference
* [graphene-django official tutorial](http://docs.graphene-python.org/projects/django/en/latest/tutorial-plain/)







