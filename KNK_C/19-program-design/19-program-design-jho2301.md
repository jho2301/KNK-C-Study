# Program Design

대형프로그램은 계속 수정될 것이기에, 가독성과 유지보수에 관한 계획이 필요하다.

1. modules
2. information hiding
3. abstract data types
4. a stack abstract data type
5. design issues for abstract data types

## modules

프로그램을 수많은 독립 module들이 유기적으로 구성된 것으로 보는게 유용하다.

### module이란

    - 프로그램의 일부로서 사용될 수 있는 기능들의 모음
    - 사용가능한 기능를 보여주는 interface를 갖고있다. (header file)
    - module의 실제 구현 코드는 module의 implementation 내부에 저장되어있다. (source file)
    - **c library는 module의 모음이다**

### 프로그램을 모듈화했을 때 장점

    1. 추상화 : 무슨 일을 하는지는 알지만 실제 구현내용은 몰라도 됨.
    2. 재사용성 : 여기저기다가 include해서 갖다 쓸 수 있음.
    3. 유지보수 : 프로그램이 모듈로 분산되어있기에 책임소재를 찾아 디버깅하기 용이함

### 좋은 모듈의 두 가지 조건

    - High Cohesion
      - module의 함수들은 관련된 것들이어야한다.
      - 함수들이 해결하려는 공통된 목표가 있다.
    - Low coupling
      - module들이 각각 독립적으로 동작해야지, 프로그램을 수정하고 재사용하기 쉬워진다.

### 모듈의 종류

    - data pool: 변수와 상수의 모음
    - library: 함수의 모음
    - abstract object: hidden 자료구조에서 작동하는 함수의 모음
    - abstract data type(ADT): 구현내용이 hidden인 type. ADT모듈내부의 함수를 실행할때 필요한 변수의 type

## information hiding

잘 만들어진 모듈은 사용자로부터 정보를 적절하게 숨긴다.  
변수나 함수에 static 키워드를 줘서 해당 파일 내부에서만 사용할 수 있도록 해줌. ( private의 역할 )

### 정보 은닉의 두 가지 장점

    1. 보안: 접근해봐야 문제만 일으키는 정보를 숨김
    2. 유연성: interface를 수정하지 않고 내부구현만 수정할 수도 있음

## abstract data types

abstract object는 인스턴스를 여러 개 만들 수 없다. -> 새로운 타입을 만들어줌.

### 캡슐화

    - C에서는 ADT의 내부 변수에 직접 접근할 수 있음. C의 한계
    - Incomplete Type으로 캡슐화를 일부 지원함.
      - ex) struct t;
    - Incomplete Type은 메모리의 크기를 모르기에 변수를 선언할 수 없다.
    - 하지만 pointer는 선언가능. 이를 함수에 인자로 넘겨주거나 할 수 있다.

## a stack abstract data type

종합 예제

## design issues for abstract data types

- naming convention: 여러 개의 ADT가 같은 함수명을 사용할 수 있다. -> 함수명의 접두어로 stack_create같이 ADT의 이름을 붙여주자.
- 에러 핸들링: stack으로 치면 pop이나 push전에 is_empty나 is_full을 먼저 실행해서 런타임에러를 막을 수 있음. 대안적인 방법으로는 pop이나 push가 return값으로 성공여부를 반환하는것(boolean타입으로) 마지막방법으로는 assert매크로를 이용해서 조건이 충족되지 않으면 프로그램을 종료시킴(if문 대신 사용가능)
- generic ADT: 변수 타입에 구애받지 않고 사용할 수 있는 방법. 포인터를 사용해라
  - 단, pointer로 표현될 수 없는 data(string이나 basic type이 아닌 data가 동적으로 할당되는 경우) 사용할 수가 없고,
  - 타입 에러 검사를 할 수 없어, 디버깅이 빡세진다.
