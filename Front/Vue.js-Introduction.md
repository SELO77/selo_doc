# Vue.js 소개 및 개념정리

다음 글은 전문 프론트 개발자가 아닌 야매 프론트 지식으로 쓰여젔으므로 참고하는데 주의가 필요합니다. [NHN 자바스크립트개발가이드-vue.js](https://github.com/nhnent/fe.javascript/wiki/October-10---October-14,-2016)를 바탕으로 쓰여젔습니다.

## 소개
Vue.js는 View에 최적화 된 프레임워크이다. MVVM(Model-View-ViewModel) 디자인 패턴을 갖음. Components를 사용하여 재사용이 가능한 UI들을 묶고 재사용 가능하게 함. 템플릿 개발 권장.

## 아키텍처 (MVVM)

### ViewModel(VM on MVVM) 핵심
데이터와 액션을 관리합니다. 기본적으로 ViewModel은 Vue initiate 하면서 관련 Options의 상태를 셋팅할 수 있습니다. 셋팅 가능한 상태를 카테고리별로 분류해보겠습니다.

* **Dom:** `el`, `template`, `render` ...
* **Data:** `data`, `props`, `propsData`, `computed`, `methods`, `watch` ...
* **Lifecycle Hooks:** `beforeCreated`, `created`, `beforeMount`, `mounted`, `updated`, `deactivated`, `destroyed` ...
* **Assets:** `components`, `filters`, `directives`
* **Misc:** `parent`, `mixins`, `name`, `extends`

ViewModel은 Dom과 Data를 dataBinding 하고, Model의 변화를 감지합니다. 또한 생명주기 훅을 통해 Dom과 Data의 변화에 유연하게 대처할 수 있습니다.

### ViewModel Lifecycle
아래 다이어그램은 Vuejs의 ViewModel의 숲을 이해하기 위한 핵심입니다. 리엑트에 익숙하신 분이라면 리엑트 컴포넌트의 라이프 사이클을 같이 생각해보시면 좋을것 같네요. 

아래 다이어그램을 전체적으로 이해한 후 각 요소들의 세부사항을 찾아보시는 방식으로 진행하시면 될 것같습니다.

<img src="https://cloud.githubusercontent.com/assets/18183560/19237330/4b57e578-8f37-11e6-9e92-770ead269b23.png">

저와 같은 경우는 Dom이 생성될때 Data binding해주고, data 변경시 update를 해주는 작업을 많이 합니다. 

1. `beforeCreate` 훅에 initial data를 받아오는 로직을 짜는게 좋을 것 같습니다. 
2. Vuejs에서는 react의 State가 변하면 해당 Component가 rerender 되는 거와 같이 해당 ViewModel의 Data가 변하면 Binding된 DOM이 rerender 됩니다.

### Template
VueJS의 Template은 HTML 기반의 템플릿 사용을 권장한다. ViewModel의 템플릿을 실제 Dom과 매핑하고, 템플릿에 Data를 바인딩합니다.

Vuejs는 DOM 조작을 위해 다양한 Directives(지시자)를 제공합니다. 지시자를 가진 엘리먼트에 특정 액션이 발생했을 때, 지시자에 할당된 자바스크립트 구문이 실행되고  상황에 따라 데이터와 템플릿 상태가 변경됩니다. 이러한 지시자를 통해 reactive 한 구현이 가능해 집니다.

### Data flow
ViewModel Instance는 `watcher` 객체를 포함하고, data의 변경을 감시하고, 변경이 일어나면 자시자에게 이를 알리고 해당 지시자를 가진 DOM이 rerender 될 수 있도록 도와줍니다.


<img src="https://cloud.githubusercontent.com/assets/4476469/19268051/16ec32a6-8fed-11e6-9dcb-ab30244f6df5.png">


## 재사용 가능한 View. [Component](https://vuejs.org/v2/guide/components.html)!!

필자가 생각하는 요즘 Front Framework의 핵심은 databinding, reactive, component라 생각한다. 앞 두개의 주제 databinding과 reactive는 개발자가 작성해야 할 코드를 혁신적으로 줄여주고, component는 재사용 가능한 view를 만듬으로서 생산성을 극대화 시켜준다. 물론, 이러한 개념들을 이해하고 사용하기 위한 학습비용이 들어가긴하지만 말이다. 어쨌든 필자 새로는 위 세가지.  귀찮은 작업들을 해결하기 위해 Frontframework을 학습하고 사용한다.






## References
[NHN 자바스크립트개발가이드-vue.js](https://github.com/nhnent/fe.javascript/wiki/October-10---October-14,-2016)