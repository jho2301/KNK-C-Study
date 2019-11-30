# Function

1. 각각의 Function은 하나의 프로그램이다.

2. 프로그램을 작은 조각들로 나누고 다른 사람으로 하여금 이해하기 쉽게 수정하기 쉽게한다.
3. 함수는 중복을 피하게한다.
4. 함수는 재사용 가능하다.

함수는 세 부분으로 이루어져 있다.

1. body
2. argument
3. return

## 184p

함수의 시작부분은 자신의 리턴값의 타입이다. 예시에서는 double.

## 185p

함수는 꼭 리턴값이 있을 필요은 없다. return 문이 생략될 경우 void type이 리턴된다.

## 186p

함수 parameter는 아무것도 없을수 있다(즉 void일 수 있다.)

## 187p

1. 함수는 array를 리턴하지 않는다.
2. return-type이 void라면 어떤 값도 return 하지 않는다.

## 188p

if c88:

return type이 생략됬다면 return type은 int이다

if c99:

return type이 생략되는것을 허용하지 않는다. 다만 어떤 프로그래머들은 return type을 함수 위쪽에 적어놓는다. 이는 return type이 "unsigned long int" 처럼 길 경우에 유용하다.

## 189p

함수 호출 = 함수 이름 + arguments

printf 함수는 출력문의 길이를 return한다.

void casting????? casting 'void' is polite way of saying throwing away.

하지만 저자는 이것은 피곤한 일이기 때문에 책에서 생략 했다.

## 190p

is_prime 함수

## 191p

main 에서 선언한 n 은 is_prime의 nd에 영향을 끼치지 앟는다. (확인 요망)

함수는 꼭 위에서 선언 될 필요는 없다.

## 192p

한번더 보자. 테스트 필요!

이런 방식은 function prototype 이라고 부르며 prototype은 함수의 인자가 몇개인지, 인자가 각각 무슨 타입인지, 함수는 어떤 타입을 리턴하는지 알려준다.

함수의 인자명을 굳이 제공할 필요는 없다. 하지만 생략하지 않는 것을 추천한다.

## 193p

parameter vs argument

parameter은 함수 정의문에 있는것

argument는 함수에 전달되는 값

parameter의 값은 arguments의 복사본이기 때문에 함수안에서 얼마든지 수정 될 수 있다.

## 194p

Argument Conversions

만약 argument의 type과 parameter의 type이 다르다면..

if prototype function:

argument의 type을 parameter의 type으로 변환

ex) int argument -> double

else:

float -> double

char, short -> int (확인 요망)

## 195p

함수의 인자로 array가 들어갈수 있다.

만약 array 인자의 길이가 필요하다면 추가 인자를 입력해야한다. (12.3에 왜그런지 써있다. 확인 요망)

주의할 점!

함수는 array length를 제대로 입력했는지 체크할 방도가 없다. 이를 이용할 수도 있다! array의 길이가 100이고 함수에 40을 제공한다면 나머지 60을 무시할 수 있다. 반대는 불가능. index error

함수 안에서 array안의 값을 수정한다면 argument에도 영향을 끼친다. 이는 12.3에 설명되어 있다.

## 198p

#### variable-length array parameters

c99 부터 함수 parameter 의 array와 array length의 관계를 표현할 수 있다. 이떄 각 인자의 순서는 중요하다.

## 199p

array, array_length를 변수로 가지는 함수를 표현하는 다양한 방법.

asterisk??

## 200p

static keyword는 array의 길이의 최솟값을 보장한다. 이것은 프로그램 자체에 영향을 주진않지만 컴파일러의 속도 향상에 도움이된다. 메모리에서 해당요소를 미리 가져올수(prefetch)할 수 있기 때문에. 단 2차원 배열에서는 row에만 static keyword를 사용할 수 있다.

## 201p

Compound literal 한국어론 뭔지 모르겠다.

Compound literal 은 유용하다.

```c
sum_array((int []) {2 * i, i + j, j * k})
```

(const int[]) {5, 4} 는 array를 read_only로 만들수 있다. 수정불가

## 202p

return문에 3항 연산자도 사용 가능하다.

non-void function에서 아무것도 리턴하지 않을경우 compiler에 따라 에러를 발생 시킬수 있다. 주의하자.

## 203p

c99 부터 main함수의 return type을 생략하는것은 안된다. 원래는 됬었다.

main 함수의 void parameter도 생략할 수 있지만, 나중에 인자 두개넣는 경우도 있어서 생략하는것을 비추천한다.

main함수의 return 값을 status code로 활용하는 것은 좋은 연습이된다. 나중에 다른 프로그래머가 테스트 할 수도 있기 때문.

#### exit

exit(EXIT_SUCCESS) == exit(0) == return 0

exit(EXIT_FAILURE) == return 1

return 문은 main안에서 실행됬을 경우만 함수가 종료되지만, exit문은 다른함수 에서 호출되도 프로그램을 종료한다.

## 205p

#### 재귀문

재귀문은 termination condition(탈출 조건)에 신경 써야한다.

재귀문은 분할정복, 퀵소트 등에 쓰인다.

## QnA

함수안에서 함수를 선언할 수 있다. 다만 그 함수는 선언된 함수안에서만 사용될 수 있다.

저자는 함수를 다 빼놓는다고 한다.

void print_pun(void), print_count(int n ); ??

212 why can the first.. 중요
