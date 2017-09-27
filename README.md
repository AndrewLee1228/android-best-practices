# android-best-practices

4명의 안드로이드 개발자가 코딩컨벤션을 맞추어 나가는 중이다.

## Check List
* [Typedef](#typedef)
* [Api 응답 Entity 또는 ViewModel에서 문자와 상수의 열거형 데이터가 존재한다면 TypeDef를 사용하여 다루자.](#entity-viewmodel-typedef)
* [api 응답 노드의 key , value 값의 형태와 이름이 적절한지 확인](#api-응답-노드의-key-,-value-값의-형태와-이름이-적절한지-확인)
* [api 응답 공통 모델 사용](#api-응답-공통-모델-사용)
* [api는 응답에서 SerializedName 는 필요할 경우에만 사용.](#api는-응답에서-serializedname-는-필요할-경우에만-사용.)
* [api 응답 Entity의 모델은 getter만 가진다 Retrofit이 값을 자동으로 바인딩 해주니 setter는 필요없다.](#api-응답-entity의-모델은-getter만-가진다-retrofit이-값을-자동으로-바인딩-해주니-setter는-필요없다.)
* [getString() 사용주의! String id를 사용할 수 있다면 그대로 써라!](#getstring-사용주의!-string-id를-사용할-수-있다면-그대로-써라!)
* [Resource id for the format string](#resource-id-for-the-format-string)
* [별도로 value 에 대한 처리가 없다면 바로 사용하자!](#별도로-value-에-대한-처리가-없다면-바로-사용하자!)
* [Null이거나 기본값이 필요할 경우 기존 사용법을 찾아보거나 기본값이 무엇이어야 하는지 의문을 가져야한다.](#null이거나-기본값이-필요할-경우-기존-사용법을-찾아보거나-기본값이-무엇이어야-하는지-의문을-가져야한다.)
* [VMBuilder 에서 View에 사용될 값을 만들지 말자](#vmbuilder-에서-view에-사용될-값을-만들지-말자)
* [추가된 (또는 리펙토링된) 클래스나 메서드의 범위를 체크하고 접근제한자를 사용한다.](#추가된-또는-리펙토링된-클래스나-메서드의-범위를-체크하고-접근제한자를-사용한다.)
* [나 또는 다른 사람의 추가 작업이 남았다면 TODO를 활용하자](#나-또는-다른-사람의-추가-작업이-남았다면-todo를-활용하자)
* [메시지는 친절하게 (주석,커밋,PR 메시지 등…)](#메시지는-친절하게-주석,커밋,pr-메시지-등...)
* [api 응답 Entity 주석](#api-응답-entity-주석)
* [MVP,MVPI 구조](#mvp,mvpi-구조)
* [주석과 Android Anotation](#주석과-android-anotation)
* [Android Lint](#androidlint)

### PR 탭의 Filter 기능을 이용해서 본인의 PR만 Search 할 수 있다.

![](https://i.imgur.com/RlqU2G4.png)

PR 탭의 Filter 기능을 이용해서 본인의 PR만 Search 할 수 있다.

### Typedef
* 열거형을 사용하는 가장 큰 이유는 숫자 상수를 이용하는 것보다 열거형 상수를 사용하는 것이 훨씬 직관적이기 때문이다.
* 게다가 열거형에서는 상수들을 묶어서 관리 할 수 있다는 장점도 있다.안드로이드에서는 열거형 타입 Enum을 권장하지 않는다
* 이유는 다음과 같다
* 안드로이드에서 ENUM을 과도하게 사용하면 DEX 크기가 증가하고 런타임 메모리 할당 크기가 늘어난다.
* 그래서 ENUM 대신 정수 또는 문자열 상수를 사용하는 것이 좋습니다.
* https://www.youtube.com/watch?v=Hzs6OBcvNQE
* https://android.jlelse.eu/android-performance-avoid-using-enum-on-android-326be0794dc3
* 그렇지만 android annotation에서 더 괜찮은 것을 제공하므로 이것을 사용하기로 한다.
* 해결책!
* Android는 TypeDef 주석 이있는 주석 라이브러리를 제공합니다 .
* @IntDef(발음:인트데프 어노테이션) 및 @StringDef 주석을 사용하면 정수 및 문자열 집합으로 구성된 열거형 주석을 생성하여 다른 유형의 코드 참조에 대한 유효성을 검사할 수 있습니다.
* 이러한 주석은 특정 매개 변수, 반환 값 또는 필드가 특정 상수 집합을 참조하는지 확인합니다. 또한 코드 완성을 통해 허용 된 상수를 자동으로 제공 할 수 있습니다.
* int의 사이즈와 성능의 이점을 그대로 쓸 수 있게 됩니다.
* 팁!
* 그전에는 꼭 필요할 경우에만 Enum을 사용했는데 그래도 Enum이 남아 있어서 코드에 대한 최적화가 고민이 된다면?
* 코드가 잘 짜여졌다면 프로가드에서 열거형 타입을 int형에 최적화 해줍니다. (공짜로 최적화 해줌!)

### Entity viewmodel Typedef
우리는 Enum을 쓰면 안되므로 TypeDef를 사용하여 상수 들을 표현한다.
정수 또는 문자열 상수를 TypeDef로 정의하기 전에 중복되는 정의가 다른 곳에 선언 되어있는지 확인하고
있다면 그것을 사용한다.
1. Api 응답 Entity 에 상수 또는 문자열의 열거형 데이터가 존재 한다면
   api 패키지 아래에 JoyInterfaceDefinition 등의 interface를 만들고 TypeDef를 정의한다.
2. ViewModel에서 TypeDef의 정의는 같은 패키지 레벨에 "PlaceItemType"와 같은 interface를 만들면 된다.

### api 응답 노드의 key , value 값의 형태와 이름이 적절한지 확인
이전에 사용되는 비슷한 값이 있다면 비교해보고 의미를 파악하자 그래도 모르겠다면 서버에게 물어보기!
![](https://i.imgur.com/Eaw2rxH.png)

### api 응답 공통 모델 사용
code, message 만 있는 경우는 com.yanolja.api.response.Result 와 같은 모델을 공통으로 사용하고 있다.

### api는 응답에서 SerializedName 는 필요할 경우에만 사용.

### api 응답 Entity의 모델은 getter만 가진다 Retrofit이 값을 자동으로 바인딩 해주니 setter는 필요없다.

### getString 사용주의! String id를 사용할 수 있다면 그대로 써라!

### Resource id for the format string
getString 내부에 String.format이 지원된다.
```java
message = getContext().getString(R.string.share_place_coupon_description, placeDetail.getMaxBenefit());
```

### 별도로 value 에 대한 처리가 없다면 바로 사용하자!
![](https://i.imgur.com/89W3GQw.png)

### Null이거나 기본값이 필요할 경우 기존 사용법을 찾아보거나 기본값이 무엇이어야 하는지 의문을 가져야한다.
![](https://i.imgur.com/8xZXlK0.png)

### VMBuilder 에서 View에 사용될 값을 만들지 말자
Entity의 내용을 대부분 ViewModel로 그대로 바인딩하고 (열거형 타입이 필요하다면 convert 할 필요는 있음)
View에 노출될 값을 조합해야 한다면 View에서 ViewModel의 값을 get하여 조합하자.

### 추가된 또는 리펙토링된 클래스나 메서드의 범위를 체크하고 접근제한자를 사용한다.

### 나 또는 다른 사람의 추가 작업이 남았다면 TODO를 활용하자

### 메시지는 친절하게 주석,커밋,PR 메시지 등...
![](https://i.imgur.com/p8IKEAL.png)

### api 응답 Entity 주석
숙소이용내역 모델의 주석을 참고할 것
응답 json과 변수주석을 달아준다.

### MVP,MVPI 구조
추가되거나 리펙토링이 필요한 코드에서 구조를 MVP,MVPI 변경하자!

### 주석과 Android Anotation
추가된 클래스,메서드,변수의 주석을 추가한다.
변경된(리펙토링) 클래스,메서드,변수의 주석을 수정한다.
메서드 파라미터와 반환 값에  NonNull,Nullable 의 Android annotation 를 추가한다.

1. Commit한 내역에 Diff를 해서 변경 내역을 보면서 주석 여부를 체크!
2. 메서드 파라미터,반환 값에  NonNull,Nullable annotation 있는지 확인
   변경 코드에서 infer Nullity를 실행하여 Null 주석을 자동으로 추가 그리고 변경 점을 한번 체크!

### androidlint


- 사용하지 않는 리소스 제거 (리펙토링 중간에 사용하지 않는 String resource,layout xml 등이 생길 수 있다.)
- 작업한 목록에서 Lint 한번씩 돌리면서 작업 진행하기!




