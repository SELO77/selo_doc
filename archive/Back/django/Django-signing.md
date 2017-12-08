# Django-signing

### justification

* Signing is a widely used web application security technique. Django uses it in a few places already (the form wizard and sessions contrib apps), and it is useful any time an application might want to pass data through an untrusted channel and ensure it hasn't been tampered on the other side. The Web is generally an untrusted channel.
* Signed cookies can replace sessions in many use cases, with the significant bonus that unlike sessions they do not require a round-trip to a persistent store.
* Signing is hard to do right, and most hand-rolled implementations make similar mistakes. Signing is best handled with hmac and sha1, but Django implementations currently tend to use the weaker MD5 without hmac. A signing implementation in Django core could be audited by expert cryptographers, providing a secure base on which other Django applications could build.


* signing 서명
* 서명된 쿠키는 세션을 대체할 수 있다.
* 서명 혹은 값이 대체되었다면, `BadSignature` exception will be raised.


### You may also find signing useful for the following:
* Generating “recover my account” URLs for sending to users who have lost their password.
* Ensuring data stored in hidden form fields has not been tampered with.
* Generating one-time secret URLs for allowing temporary access to a protected resource, for example a downloadable file that a user has paid for.



* [https://docs.djangoproject.com/en/1.7/topics/signing/](https://docs.djangoproject.com/en/1.7/topics/signing/)

* [https://code.djangoproject.com/wiki/Signing](https://code.djangoproject.com/wiki/Signing)