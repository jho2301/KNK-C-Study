# Low-Level Programming

이번 장에서는 비트조작에 대해서 설명한다.  
이런 프로그래밍 방법은 고성능, 그래픽, System제어 프로그램을 만들때 필요하다.  
컴파일러나 머신마다 다르게 구현되어있을 수 있기에, 최대한 한정된 모듈안에서만 사용할 것.

1. Bitwise Operator
2. Bit-Fields in Structure
3. Other Low-Level Techniques

## Bitwise Operator

    C는 int데이터를 bit레벨에서 다룰 수 있도록하는 6개의 비트연산자를 제공한다.

    - Shift 연산자 2개
    - Complement 연산자
    - and 연산자
    - exclusive or 연산자
    - inclusive or 연산자

### Bitwise Shift 연산자

|   Symbol   |        Meaning        |
| :--------: | :-------------------: |
|     <<     |      Left shift       |
|     >>     |      Right shift      |
| <<= or >>= | 연산과 할당을 한 번에 |

    Shift연산자는 int데이터의 비트열을 왼쪽, 오른쪽으로 변화시킬지 결정한다.
    이 연산자는 연산자우선순위가 산술연산자보다 순위가 낮다. 조심할 것!

    i << j : i의 비트열을 왼쪽으로 j만큼 이동시킨다.
             맨 오른쪽 자리에는 0이 자리한다.
    i >> j : i의 비트열을 오른쪽으로 j만큼 이동시킨다.
             i가 unsigned타입이거나 0을 포함한 양수라면 왼쪽에는 0이 자리한다.
             i가 음수라면, 결과는 머신이나 컴파일러의 규칙을 따른다.
             (이식성을 위해 양수의 데이터에만 shift연산자를 사용하자)

### Complement, _And_, Exclusive _Or_, Inclusive _Or_ 연산자

| Symbol |  Meaning   |
| :----: | :--------: |
|   ~    | complement |
|   &    |    and     |
|   ^    |    xor     |
|   \|   |     or     |

    ~연산자는 단항연산자. 피연산자에 1의 보수를 취해준다.
    and, or연산자는 비트열의 모든 자리에 대해 boolean 연산을 실시한다.
    우선순위 : ~, &, ^ is highest / | is lowest

## Bit-Fields in Structure

C는 Bit연산을 좀 더 쉽게 할 수 있게 대안을 제공한다.

```c
struct file_date {
    unsigned int day: 5;
    int month: 4, year: 7;

    // 1. 해당 변수에는 콜론 뒤의 숫자값만큼의 비트공간만 할당함.
    // 2. _Bool, int종류만 가능함.
    // 3. &로 주소값을 불러올 수 없다.
}
```

### How Bit-Fields Are Stored in structure

비트열을 다루는 규칙은 "storage units"에 따른다.

storage unit의 크기는 표준에 정의되어있지않다. 보통 8bit, 16bit, 32bit, 64bit다.

비트열을 storage unit에 저장하며, structure의 멤버들은 구분되는 공간없이 저장된다. 어떤 컴파일러는 멤버의 시작지점도 생략한다.

저장되는 순서 역시 머신, 컴파일러에 따라 상이함.

unnamed 변수를 0 bit라 선언하면 원래 저장되어야할 storage units는 생략하고 다음 새로운 storage units에 저장한다. n bit 만큼 선언하면 해당 n bit만큼 빈 공간이 된다.

## Other Low-Level Techniques
