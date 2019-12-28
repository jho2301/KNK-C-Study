## 20.1 BItwise Operators

C는 6개의 비트연산자(bitwise operators)를 제공한다.

#### Bitwise Shift Operators

| Symbol | Meaning     |
| ------ | ----------- |
| <<     | left shift  |
| >>     | right shift |

```C
unsigned short i, j;

i = 13;			/* i is now 13 (binary 0000000000001101) */
j = i << 2;		/* i is now 52 (binary 0000000000110100) */
j = i >> 2;		/* i is now  3 (binary 0000000000000011) */
```

i << j는 i 내의 비트들이 왼쪽으로 j만큼 이동한 값

i >> j는 i 내의 비트들이 오른쪽으로 j만큼 이동한 값

> 이식성(portalbility) 때문에 shift 연산은 unsigned number에만 사용하는 것이 좋다.

변수의 값을 수정하고자 할 때는 <<= 또는 >>= 를 사용한다.

```C
i = 13;		/* i is now 13 (binary 0000000000001101) */
j <<= 2;	/* i is now 52 (binary 0000000000110100) */
j >>= 2;	/* i is now 13 (binary 0000000000000011) */ 
```

#### Bitwise Complement, And, Exclusive Or, and Inclusive Or

| Symbol | Meaning                    |
| ------ | -------------------------- |
| ~      | bitwise complement (NOT)   |
| &      | bitwise and (AND)          |
| ^      | bitwise exclusive or (XOR) |
| \|     | bitwise inclusive or (OR)  |

```C
unsigned short i, j, k;

i = 21;		/* i is now    21 (binary 0000000000010101) */
j = 56;		/* j is now    56 (binary 0000000000111000) */
k = ~i;		/* k is now 65514 (binary 1111111111101010) */
k = i & j;	/* k is now    16 (binary 0000000000010000) */
k = i ^ j;	/* k is now    45 (binary 0000000000101101) */
k = i | j;	/* k is now    61 (binary 0000000000111101) */
```

&=, ^=, |= 대입연산자 가능, 당연히 ~=는 안됨

연산자 우선순위: ~, &, ^, | 

i ^ j & ~k 는 i ^ (j & (~k)) 와 같다.

```C
// & ^ | 의 연산 우선순위는 관계연산자, 비교연산자보다 낮다.
unsigned short i, j, k;

i = 21;      /* i is now    21 (binary 0000000000010101) */
j = 56;      /* j is now    56 (binary 0000000000111000) */
k = i & j;   /* k is now    16 (binary 0000000000010000) */

if (k == 16) {
    printf("hi\n"); // print
}

if (i & j == 16) {
    printf("hello\n"); // not print
} else {
    printf("world\n"); // print
}
```

#### Using the Bitwise Operators to Access Bits

```C
// Setting a bit: suppose that we want to set bit 4 of i
i = 0x0000;		/* i is now 0000000000000000 */
i |= 0x0010;	/* i is now 0000000000010000 */
// 만약 j에 이미 bit 위치 정보가 있다면 -> j가 4라면
i |= 1 << j
    
// Clearing a bit: To clear bit 4 of i
i = 0x00ff;			/* i is now 0000000011111111 */
i &= ~0x0010;		/* i is now 0000000011101111 */
i &= ~(1 << j);		/* clears bit j */

// Testing a bit
if (i & 0x0010)   /* tests bit 4 */
if (i & 1 << j)   /* tests bit j */
```

#### Using the Bitwise Operators to Access Bit-Fields

Bit fields: several Consecutive bits ??

```C
// Modifying a bit field
i = i & ~0x0070 | 0x0050;		/* stores 101 in bits 4-6 */
// 만약 j가 4-6번 비트에 담겨야할 값을 가지고 있다면..
i = (i & ~0x0070) | (j << 4);
i = i & ~0x0070 | j << 4;


// Retrieving a bit field
j = i & 0x0007;		/* retrieves bits 0-2 */
// 만약 얻고 싶은 bit field(4-6)가 오른쪽 끝에 자리잡고 있지 않다면 맨 오른쪽으로 옮겨주고 값을 얻는다
j = (i >> 4) & 0x0007;         /* retrieves bits 4-6 */

```



## 20.2 Bit-Fields in Structures

앞서 bit fields를 다루는 기술은 꽤 헷갈린다. 이에 대한 대안으로 구조체에서 bit fields를 다룰 수 있다.

```C
struct file_date {
    unsigned int day: 5;
    unsigned int month: 4;
    unsigned int year: 7;
};
// same
struct file_date {
    unsigned int day: 5, month: 4, year: 7;
};
```

각 멤버 뒤 숫자는 bit의 길이를 나타낸다.

bit fields의 type은 int, unsigned int, signed int 이어야한다. 

다만 int를 쓰는것은 애매하다. ?? some compilar treat the field's high-order bit as a sign bit, but others don't

portability를 위해 모든 bit fields를  unsigned int 나 signed int로 선언하는 것이 좋다.

```C
struct file_date fd;

fd.day = 28;
fd.month = 12;
fd.year = 8;		/* represents 1988 */
```

bitwise operator를 사용하여 bit field를 다루는 것이 아주 조금 빠르긴하지만 읽기 쉬운 프로그램을 작성하기 위해서 구조체를 사용하는 것이 좋다.

#### How Bit-Fields Are Stored

Bit fields를 다루는 룰은 storage unit 에 따라 달라진다. storage unit은 일반적으로 8, 16 32 bit이다

아까 file date 예시는 16bit였다. (만약 8bit도 가능하다 month field를 두개의 storage unit에 분리해 놓는다면)

bit field가 할당되는 순서도 컴파일러에 따라 다르다 (왼쪽 -> 오른쪽,  오른쪽 -> 왼쪽)

이름 없는 bit field도 선언 가능

이름 없는 bit field의 길이를 0으로 설정하여 storage unit의 시작지점을  특정할 수 있다.

```C
struct file_time {
    unsigned int : 5;          /* not used */
    unsigned int minutes: 6;
    unsigned int hours: 5;
};
struct s {
    unsigned int a: 4;
    unsigned int : 0;     /* 0-length bit-field */
    unsigned int b: 8;
};
```

#### 20.3 Other Low-Level Techniques

#### Defining Machine-Dependent Types

#### Using Unions to provide Multiple Views of Data

```C
union int_date {
    unsigned short i;
    struct file_date fd;
};

void print_date(unsigned short n) 
{
    union int_date u;
    u.i = n;
    printf("%d/%d/%d\n", u.fd.month, u.fd.day, u.fd.year + 1980);
}
```

Using unions to allow multiple views of data is especially useful when working with **register** 
