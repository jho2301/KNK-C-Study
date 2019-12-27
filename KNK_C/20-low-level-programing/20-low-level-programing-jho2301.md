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
    우선순위 ~, &, ^ is highest / | is lowest

## Bit-Fields in Structure

## Other Low-Level Techniques
