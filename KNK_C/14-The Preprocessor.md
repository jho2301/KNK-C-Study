### The Preprocessor(전처리기)
컴파일 하기 전에, #define이랑 #include 지시어들은 전처리기가 관리한다.

### 목차
  1. how the preprocessor works
  2. general rules of Directives
  3. macro definition
  4. conditional compilation
  5. 잘 안쓰는 지시어들 :#error, #line, #pragma


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
`#define 식별자 값`
- 값에는 식별자, 키워드, 숫자, 문자, 문자열, 연산자, 구두점이 올 수 있다.
- 코드 가독성에 좋다.
- 수정하기 쉽다. (전체 값이 아닌 매크로에 정의된 값 하나만 고치면 되므로)
- 오타를 잡아내기 쉽다. (숫자는 오타내도 알아보기 힘들다.)
- 
