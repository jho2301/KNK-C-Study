# The Preprocessor(전처리기)
컴파일 하기 전에, #define이랑 #include 지시어들은 전처리기가 관리한다.

### 목차
  1. how the preprocessor works
  2. general rules of Directives
  3. macro definition
  4. conditional compilation
  5. 잘 안쓰는 지시어들 :#error, #line, #pragma
  6. QnA


## how the preprocessor works
 #표시가 붙은 것들을 처리하는게 전처리기가 하는 일
 - #define: 매크로, 어떤 값을 대신하는 이름을 정의함. 사전에 정의된 값을 매크로와 바꿔줌
 - #include: 특정한 파일을 열어서, 해당 내용을 불러오는 파일의 일부로 포함한다.  
  ![](https://regularcodes.com/images/pick/c-preprocessor.png)
   - 전처리기는 지시어가 포함된 C프로그램을 입력받아, 지시어를 처리한 c프로그램을 출력한다.
   - 출력된 c프로그램은 에러 검사 후 object코드로 바꾸기 위해 컴파일러로 보낸다.  
  
- 전처리기는 **주석**도 공백으로 치환함.
- 요즘 전처리기는 컴파일러의 일부로 포함되어 있다.
- 전처리기는 에러검사를 하지 않기에, 에러가 발생할 여지가 있다. 그리고 이 경우 디버깅이 어려울 수 있음.

## general rules of Directives
### 지시어는 주로 세 가지로 분류됨.
1. Macro definition
   - #define(매크로 정의), #undef(매크로 해제)
2. File inclusion
   - #include
3. Conditional compilation
   - #if, #ifdef, #ifndef, #elif, #else, #endif : 조건에 따라 코드 블록을 포함함.
4. 나머지는 #error, #line, #pragma

### general rule
- always begin with the '#'
- 내용 구분에 사용되는 공백은 여러 개 써도 상관없음  
  `#         define           N         100 `
- 라인피드로 구분됨 (여러 줄을 작성하고 싶다면 '\'를 사용할 것) 
```C
# define SEVERAL_LINE (SIDES * \
                        TRACKS * \
                        SECTORS * )
```
- 어느 위치에서든 선언 가능

## Macro Definitions
### Simple macro (non-parameter)
`#define 식별자 정의`
- 값에는 식별자, 키워드, 숫자, 문자, 문자열, 연산자, 구두점이 올 수 있다.
- 코드 가독성에 좋다.
- 수정하기 쉽다. (전체 값이 아닌 매크로에 정의된 값 하나만 고치면 되므로)
- 오타를 잡아내기 쉽다. (숫자는 오타내도 알아보기 힘들다.)
- C의 문법을 커스텀할 수 있다. 
  ```c
  #define BEGIN {
  #define END }
  ```
- 타입의 이름을 바꿀 수 있다.
  ```c
  #define BOOL int
  ```
- 조건 컴파일을 제어한다.
  ```c
  #define DEBUG //디버깅 모드
  ```
- 상수로 사용할 땐 대문자로 쓴다. (다른 용도로 사용할 때에 대해선 합의가 없음.)
  
### Parameterized Macro
`#define 식별자 (x, y, z, ... , k) 정의`
  
```c
#define MAX(x,y) ((x) > (y) ? (x) : (y))
#define IS_EVEN(n) ((n) % 2 == 0)
```
- 간단한 함수로 사용된다.
- 인자가 없을 수도 있다.
- 퍼포먼스가 향상 됨. (런타임 오버헤드:함수는 인자를 카피해야함. inline function은 매크로 사용없이 오버헤드를 제거함. )
- '제네릭'하다. => 인자 타입을 체크하지 않음
- 컴파일 된 코드 사이즈가 크다.
- 매크로를 가리키는 포인터 사용 불가
- 매크로는 인자를 한 번 이상 평가할 수 있음. (함수는 한 번인데)
    ```c
    //전처리 전 코드
    n = MAX(i++, j)
    //전처리 후 코드
    n = ((i++)>(j)? (i++) : (j)) //i가 참일 시에, 두 번 평가됨
    ```
-중복되는 코드를 축약해 사용할 수 있다.(snippet)
```c
#define PRINT_INT(n) print("%\n", n)
```

### # Operator
> 매크로의 값 부분에는 #과 ##연산자를 사용할 수 있다.
- #연산자는 macro의 인자를 string으로 변환시킨다.(stringization)
  ```c
  #define PRINT_INT(n) printf(#n " = %d\n", n)

### ## Operator
- ##연산자는 두 개의 토큰을 하나의 토큰으로 만들 수 있다. (잘 사용되진 않음)
  ```c
  #define MK_ID(n) i##n

  //전처리 전 코드
  int MK_ID(1), MK_ID(2)
  //전처리 후 코드
  int i1, i2
  ```

### General Properties of Macros
- 매크로의 값은 다른 매크로를 불러올 수도 있다. 
```c
  #define PI 3.14159
  #define PIPI (2*PI) //rescan 대상
```
- 매크로를 부르려면 토큰 전부가 매크로의 식별자여야 한다.
- 매크로는 파일 끝까지 효과가 지속된다.
- 중복되게 매크로를 정의하지 말아라.
- #undef를 사용하면 매크로를 해제할 수 있다.

### 매크로의 정의 속 괄호
- 매크로가 잘못된 결과를 출력할 수 있기에, 적절하게 괄호를 넣어주어라. (컴파일러가 연산자 우선순위를 헷갈릴 수 있음)
- 괄호를 넣는 규칙 2가지
  - 매크로 정의에 연산자가 들어있을 경우
  - 매크로가 인자를 가지고 있을 경우

### creating Longer Macros
- 콤마(,) 연산자를 사용한다.
  ```c
  #define ECHO(s) (gets(s), puts(s))
  ```

### predefined Macros
- 먼저 정의된 매크로는 문자열이나 정수를 나타냄  
- `__DATE__`, `__TIME__`:버전 관리할 때 용이함.
- `__LINE__`, `__FILE__`:디버깅할 때 용이함.
    


| name       | Description           |
| ---------- | --------------------- |
| `__LINE__` | 컴파일 된 파일의 라인 넘버       |
| `__FILE__` | 컴파일 된 파일의 이름          |
| `__DATE__` | 컴파일 날짜                |
| `__TIME__` | 컴파일 시간                |
| `__STDC__` | 컴파일러가 C스탠다드를 준수하면 1출력 |


### Empty Macro Arguments
- c99는 인자를 생략해도 된다.
```c
#define ADD(x,y) (x+y)
i = ADD(,k)
```
  
### Macros with a Varable Number of arguments
- ...연산자를 사용해서 인자를 여러개 받을 수 있다. 
- ...연산자로 받은 인자를 나타낼 때는 `__VA_ARGS__` 매크로를 활용함

### The __func__ Identifier
- 전처리기에서는 아무 것도 안함.
- 디버깅에 유용하다.
- 모든 함수는 `__func__`식별자에 접근할 수 있다.
- 이 식별자는 실행 중인 함수의 이름을 string으로 반환한다.


## Conditional Compilation
- 프로그램 코드를 포함할 것인가 제외하고 컴파일할 것인가. 
- 전처리기가 테스트를 수행한 결과에 따라 결정

### #if & #endif
`#if 상수표현식` 상수표현식이 0이면 false, 그렇지 않으면 true
`#endif`
```c
//디버깅 상황
#define DEBUG 1

#if DEBUG //디버깅 중이라면
printf("value of i: %d", i)
#endif
```

### defined Operator
- 매크로 식별자 앞에 `defined`를 붙여주면 정의된 매크로이다 => 1, 정의되지 않은 매크로식별자이다 => 0을 반환
- 보통 #if와 같이 쓰임
  ```c
  #if defined(DEBUG) //괄호는 써도되고 안써도됨
  ```

### #ifdef & #ifndef
- #ifdef = #if + defined 
- #ifndef = #if + !defined

### #elif & #else
- elif : else if
  ```c
  #if expression1
  ~~~~~
  #elif expression2 //expression1이 0인데, expression2는 0이 아니라면
  ~~~~~
  #else //위에 expresion들이 모두 0이라면 
  ~~~~~
  #endif
  ```

### uses of conditional compilation
- 여러 머신에서 써야할 때, 기계마다 코드를 다르게 가져감
- 여러 컴파일러를 사용할 때, 컴파일러마다 코드를 다르게 가져감
- 매크로에 디폴트값을 줌(매크로가 선언이 안되어있다면, 조건부 컴파일로 매크로에 디폴트값 할당)



## Miscellaneous Directives
#error: 예외처리시 사용함, 전처리기가 #error를 만나면 #error의 값을 출력하고 종료
#line: `__line__`과 같이 사용, 전처리기가 #line을 만나면 그 다음 줄 번호는 #line의 값으로 초기화 됨
#pragma: 컴파일러가 특정한 동작을 하도록 요청함.

## QnA
---
q: 한 행에 #만 쓰여 있는 것도 가능?  
a: 가능, null directive임. 지시어 영역을 한눈에 알아보기 편하게 해줌.
---
q: 어떤 상수를 매크로로 사용 할지 모르겠다. 가이드라인 있음?
a: 0, 1 빼고 모든 숫자 상수를 매크로로 써라  
   문자, 문자열은 문제의 여지가 있는데, 이것들은 매크로로 쓴다고 항상 가독성이 올라가지가 않음.
   내가 제안하는건 
   1. 상수가 한 번 이상 사용되면
   2. 상수가 수정될 수도 있을거 같으면 정도다. 
---
q: #연산자로 stringize한다고 했는데, 인자가 `"` 나 `\` 이면 어떻게 되냐
a: `"`는 `\"`로 `\`는 `\\`로 바뀐다.
---
