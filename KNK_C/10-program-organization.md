# 10. Program Organization
프로그램이 한 개 이상의 function을  포함할 때, 발생하는 이슈에 대한 내용.
    
1. [local variable ](#local-variable)
2. [External(global) Variable ](#Externalglobal-Variable)
3. [blocks](#Blocks)
4. [scope](#Scope)
5. [Organizing a C Program](#Organizing-a-C-Program)
6. [QnA](#qna)

## Local variable
함수 안에 정의된 변수

```c
int example(int n) 
{
    int a = n; // a는 지역 변수   
    return a;
}
```
1. **Automatic Storage Duration**
   1. 자동적으로 변수의 저장공간이 할당되고, 해제됨
   
2. **Block scope**
   1. 변수가 참조될 수 있는 영역
   2. 블록 안에서만 참조할 수 있다.  
   
### Static Local Virable
static을 넣는다는 건, static storage duration(not automatic)임을 뜻함.  
프로그램 실행될 때 저장공간을 무조건 유지함.  
```c
void f(void) 
{
    static int i;
}
```

여전히 **block scope**를 가짐. (not visible from outside).  
함수가 한 번 또 호출됐을 때, **이전의 변수 값**을 가지고 있음.

### Parameter
매개변수는 지역변수와 같은 속성을 갖는다. 

1.  autobatic storage duration
2.  block scope  




## External(global) Variable
함수 바깥에 선언된 변수.  
함수는 external variable과 communicate할 수 있다.
>스택을 구현할 때, 전역 변수를 쓸 수 있다.
1. **static storage duration**
2. **File Scope**
   1. 파일 내부에서만 통용 됨  
     

### 전역변수의 장단점
함수 쓸 때 전역변수 말고 인자로 넘겨받는게 대개 더 낫다
#### 장점
1. 다양한 함수가 같은 변수를 사용하는게 간편함  
   
#### 단점
1.  프로그램 수정 시, 지속적으로 전역변수를 체크해야 함
2.  디버깅할 때 어려움
3.  프로그램을 재사용하기 어려움 (전역변수도 같이 옮겨줘야 함)




## Blocks
```c
int func(int a)
{
    int b = 0;
    if (a > b) 
    {
        int temp = a;
        a = b;
        b = temp;
    }
}
```
- C언어는 **compound Statements**를 허용한다.
- block안에 선언한 변수는 **Local variable** (automatic storage duration & block scope)
- block안에 따로 변수를 선언하면 
  - 가독성이 좋아진다. (부모 블록 변수와 자식 블록 변수가 나뉘어 선언됨)
  - 이름 충돌이 방지됨


## Scope
동일한 식별자라도 위치에 따라서 여러 개의 값일 수 있다.

```c
int i;

void f(int i) 
{
    i = 1;
}

void g(void)
{
    int i = 2;

    if (i > 0)
    {
        int i;
        i = 3;
    }

    i = 4;
}

void h(void)
{
    i = 5;
}
```


## Organizing a C Program
여러 파일을 통해 프로그램을 짤 수도 있다.  
아직까진 단일 파일에서 코딩하는 것만 배웠음.  

### 싱글 파일 프로그램 짜는 순서

```
1. #include 하기
2. #define 하기
3. 사용자 타입 정의
4. 전역 변수 선언
5. 사용할 함수 이름만 작성
6. 메인 함수 작성
7. 사용할 함수 body 작성
```


## QnA
1. 재귀함수에서 static 지역변수를 사용하면 어떻게 되나요?
   1. 재귀로 호출되는 모든 함수들은 같은 static 변수를 참조하게 된다.
2. 전역변수 i가 있는데, 바로 아래 선언된 함수에 i가 지역변수로 또 선언됐다. 괜찮나.
   1. 괜찮다. scope를 봐라