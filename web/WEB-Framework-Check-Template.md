Permalink: web-framework-check-template
Date: 2/12/2018
Tags: Web, Django

# WEB Framework Check Template

이 글에서 말하는 웹 프레임웍은 REST API 중심의 웹 프레임웍을 말한다. 해당 글의 작성 목적은 웹 프레임웍을 선택 혹은 구축하는데 있어 고려해볼사항들에 대해서 포괄적으로 언급한다. 파이썬 진형의 대표적인 웹 프레임웍 장고에서 많은 개념을 참고했다.

## 주요 고려 항목
* **Authentication**
	* OAuth2, JWT ...
* Explicit Project Structure
* Architecture
	* Design Pattern. MVC Pattern
	* 장고의 경우 View, Model, Serializer 와 같은 각 모듈의 명확한 역할 정의. 균일한 데이터 흐름을 갖게 하는게 매우 중요.
* **Middleware**
	* Throttling
	* 필요하다면 Device 분기
	* Logging
	* 현재 서버 환경별 Request, Response Handling
		* Beta 환경인 경우. 토큰발행등으로 Beta 서버 추가 인증.
* Dynamic and Elastic Settings Load. e.g) local, development, alpha, beta, production ...
* **DDD or Adaptable MSA**
	* 장고의 경우 앱을 Service 중심으로 분류. 서비스간 단일 인터페이스를 통해 의존관계 설립.
* REST API URL 별 일반화 가능여부.
	* 장고의 경우 `function view` => `class view`
		* `class view` 의 경우 API 별 permission, authentication, throttling 관리가 매우 편해짐.
* **Exception Handling Centralization**
* **Test Environment**
	* Test Framework
* **Cache Backend & Engine**
	* Redis, Memcached ...
* **Storage Engines**
	* MySQL, MSSQL, PostgreSQL ...
* **ORM Engine**
* API Versioning
	* /v1/
* API Documentation
	* Swagger
* Web 혹은 Admin 필요시 Template Engine
* Runtime Debugger
	* django-debbug-toolbar
* Serialization

## ON DEMAND
* Asynchronous task queue
	* Celery
	* Email, SMS 발송 같은 서비스
* Support DATA Team
	* DATA HOOK
* Log Centralization
