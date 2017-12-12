# django-rest-framework token 발행

### 배경지식

`SessionAuthentication`을 메인 인증방식으로 사용하고 있습니다. 하지만 특수한 목적으로 해당 API만 다른 인증방식이 필요하였고, DRF에서 제공하는 Token 인증을 이용하여 간단히 문제를 해결하였습니다.

### Implement

```python
from rest_framework.authtoken.models import Token
token, created = Token.objects.get_or_create(user_id=2)
```

1. `token.save()` 시 내부적으로 `key` 보유 여부를 확인.
2. `key`가 `None`인 경우 `generate_key()` method를 호출, `key` 생성 후 저장.
