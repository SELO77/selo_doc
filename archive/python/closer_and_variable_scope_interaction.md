# Closer and Variable Scope Interaction
### Python has a dynamic type system
파이썬은 동적 타입 언어입니다. 동적 타입 언어의 특성상 동적할당이 가능하고, 그로인해 얻을 수 있는 몇 가지 특징들이 있습니다. 그 중에서 함수의 [일급객체](http://bestalign.github.io/2015/10/18/first-class-object/) 특징은 코드를 작성하는 사람에게 유연한 기능들을 제공합니다. 그 중관련된 기술인 Closer와 Python Variable Scope를 짧게 알아보려합니다.
_
사전에 일급객체의 개념에 대해 이해하는것이 Closer를 학습하는데 도움이 됩니다. 하지만 Closer를 살펴보고 일급객체를 학습한는 것도 나쁜 방법은 아니라고 생각되기때문에 가벼운 마음으로 읽어보시는 것도 도움이 되리라 생각됩니다. 제가 그랬거든요...

### Closer and Scope
`Closer` 란 자신이 정의된 스코프에 있는 변수를 참조하는 함수를 말합니다. `Scope`란 유효범위, 즉 그룹화된 참조영역입니다. 

