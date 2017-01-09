# drf-throttling
throttling give request count limit on specific condition.

request header의 `X-Forwarded-For` and `Remote-Addr` 로 identify. django default cache에 throttle 정보가 저장됨. 차후에 같은 identifier 진입시 저장된 캐쉬 조회, throttled 여부 판단. 물론 조회 후 `num_requests += 1` 등 정보 갱신. `u'throttle_%(scope)s_%(ident)s'` cache format 은 다음과 같음.


UnitTest 작성시 주의점: overide_settings decorator 씹힘. django.conf.settings는 바뀌나 각 throttle class는 이미 초기 값과 함께 셋팅됨. debbuging시 각 throttleClass 의`self.THROTTLE_RATES` 을 살펴보면 확인할 수 있음.

overide_settings의 실행시점을 이해하지 못한 나의 불찰...

해결방법은 아래 코드 확인.

```python
    def _switch_throttle_rate(self, view_cls, throttle_cls, *args):
        throttle_tuple = view_cls.throttle_classes
        try:
            index = throttle_tuple.index(throttle_cls)
        except ValueError as e:
            raise e
        throttle_tuple[index].THROTTLE_RATES = settings.REST_FRAMEWORK['DEFAULT_THROTTLE_RATES']
        
    @override_settings(REST_FRAMEWORK={변경할 settings}
    def test_some_function(self):
    	self. _switch_throttle_rate(view_cls, throttle_cls)
```

먼저 throttle rate 변경을 원하는 테스트에 `override_settings`를 통해 기본 `REST_FRAMEWORK` 셋팅을 변경하고, target throttle_class의 `THROTTLE_RATES`를 바꿔치기 한다. 이러한 해결방법의 장점은 다른 테스트에 영향을 미치지 않는다는 점이다.


[http://www.django-rest-framework.org/api-guide/throttling/#userratethrottle](http://www.django-rest-framework.org/api-guide/throttling/#userratethrottle)