# Javascript-replace-with-regex


```javascript
var d = new Date().toISOString().slice(0, 19).replace(/[A-Z]/gi, " ");
console.log(d); //"2016-10-24 11:03:32"
```


/패턴/정규식옵션
 
패턴 : 대체할 문자를 입력
정규식옵션 : g, i, m 중에 필요한 것 입력
 
g (global) : 첫번째 문자만이 아닌 패턴에 해당하는 모든 문자들을 검색하여 대체한다.
i (ignoreCase) : 대소문자 구분하지 않음.
m (multillineM) : 여러 줄 검색



## Bonus

모든 공백 체크 정규식
var regExp = /\s/g;
 
숫자만 체크 정규식
var regExp = /^[0-9]+$/;

이메일 체크 정규식
var regExp = /^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/i;

핸드폰번호 정규식
var regExp = /^\d{3}-\d{3,4}-\d{4}$/;

일반 전화번호 정규식
var regExp = /^\d{2,3}-\d{3,4}-\d{4}$/;

아이디나 비밀번호 정규식 
var regExp = /^[a-z0-9_]{4,20}$/;

휴대폰번호 체크 정규식 
var regExp = /^01([0|1|6|7|8|9]?)-?([0-9]{3,4})-?([0-9]{4})$/;


## Reference

[http://letusgo.tistory.com/entry/Javascript-%EC%A0%95%EA%B7%9C%EC%8B%9D-%EC%B2%B4%ED%81%AC-%EC%98%88%EC%A0%9C](http://letusgo.tistory.com/entry/Javascript-%EC%A0%95%EA%B7%9C%EC%8B%9D-%EC%B2%B4%ED%81%AC-%EC%98%88%EC%A0%9C)