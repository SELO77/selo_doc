#Django-Signal

센더가 보내는 시그널을 처리하는 리시버. Hook의 확장개념.

### basic code

```python
from django.db.models.signals import pre_save
from django.db.models.signals import post_save
from django.db.models.signals import post_delete
from django.dispatch import receiver

@receiver(post_save, sender=User)
def receive_user_save(sender, instance, created, **kwargs)
	# receiver logic
```


### django has useful built-in signals. Especially, models signal!!
`django.db.models.signals.pre_save & django.db.models.signals.post_save`

Sent before or after a model’s save() method is called.


### receiver() code
```python
def receiver(signal, **kwargs):
    """
    A decorator for connecting receivers to signals. Used by passing in the
    signal (or list of signals) and keyword arguments to connect::

        @receiver(post_save, sender=MyModel)
        def signal_receiver(sender, **kwargs):
            ...

        @receiver([post_save, post_delete], sender=MyModel)
        def signals_receiver(sender, **kwargs):
            ...

    """
    def _decorator(func):
        if isinstance(signal, (list, tuple)):
            for s in signal:
                s.connect(func, **kwargs)
        else:
            signal.connect(func, **kwargs)
        return func
    return _decorator
```




[https://docs.djangoproject.com/en/1.10/topics/signals/](https://docs.djangoproject.com/en/1.10/topics/signals/)