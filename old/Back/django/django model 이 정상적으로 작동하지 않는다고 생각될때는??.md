# django model 이 정상적으로 작동하지 않는다고 생각될때는??

```python
blog = Blog.objects.filter(id=1)
print str(blog.query)
```

별거없다. 위에처럼 해당 QuerySet의 query를 직접 눈으로 확인하고, 내가 의도하는대로 쿼리가 작성됬는지 확인하면 된다. 보통 쿼리를 확인하게 되면 내가 의도한 바와 django model의 raw query가 다른형식으로 작성된 경우가 있다. 

결론, 장고 모델의 이해부족으로 발생할 수 있는 이러한 문제를 눈으로 확인하고, 해결한다.

