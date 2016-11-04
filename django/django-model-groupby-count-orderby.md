# Django model GROUB BY and COUNT and ORDER BY


### Raw query
```sql
SELECT user, COUNT(*) AS COUNT FROM `shoppinglist`
	GROUP BY user
	ORDER BY COUNT DESC
	LIMIT 25
``` 


### Django model queryset
```python
from django.db.models import Count

q = ShoppingList.objects.values('user').annotate(num_users=Count('user')).order_by('-num_users')[:25]

# check raw query
print str(q.query)
```