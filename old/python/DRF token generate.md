# DRF token generate


```python
from rest_framework.authtoken.models import Token
token, created = Token.objects.get_or_create(user_id=2)
```

1. `token.save()` 시 내부적으로 `key` 보유 여부를 확인.
2. `key`가 `None`인 경우 `generate_key()` method를 호출, `key` 생성 후 저장.