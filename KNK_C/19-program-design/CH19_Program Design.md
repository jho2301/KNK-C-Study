# CH19_Program Design

Created: Dec 18, 2019 10:56 AM

- 19.1 - how to view a C program as a collection of modules  that provide services to each other
- 19.2 - information hiding
- 19.3 - abstract data types
- 19.4 - how abstract data types are defined and implemented
- 19.5 - limitations for defining abstract data types

# 19.1 Modules

module이란?

*모듈*이란, *서비스(함수)*의 모음을 의미한다. 서비스는 프로그램의 여러 파트(*클라이언트*)들이 접근할 수도 있다. 각각의 모듈은 실행 가능한 서비스가 적혀있는 *인터페이스*가 있고, 이에 관련된 상세한 정보와 코드는 모듈의 *implementation(구현)*에 저장되어있다. 

짧게 말해서, 모듈이란 어떤 기능을 처리하는 코드가 모여있는 덩어리이다. 

[[ 용어 ] 모듈(Module)이란](https://redssin.tistory.com/entry/%EC%9A%A9%EC%96%B4-%EB%AA%A8%EB%93%88Module%EC%9D%B4%EB%9E%80)

- 서비스 - 함수
- 구현(implementation) - 헤더 파일 (stack.c)
- 클라이언트 - 소스 파일 (cal.c)

C 라이브러리는 모듈의 집합이라 볼 수 있다. 라이브러리 내부에 있는 헤더는 라이브러리 내용을 모듈로 연결해주는 인터페이스인 것이다. 예를 들어서 보면 이해가 더 잘 가는데, `<stdio.h>` 는 입출력 관련된 함수들이 담긴 모듈로 연결시켜주는 인터페이스인 셈.

모듈의 형태로 관리하는 것에는 크게 3가지 이점이 있다고 한다.

- **Abstraction** - 추상화. header 파일에 포함된 함수 등을 사용할 때, 그 원리를 굳이 다 적거나 이해하지 않아도 충분히 활용할 수 있다.
- **Reusability** - 재사용에 용이하게 작성하여 사용할 수 있다.
- **Maintainabilty** - 오류가 나거나 수정이 필요할 때, 코드 전체가 아닌 부분부분을 손보면 얼마든지 개선할 수 있기 때문에 유지보수가 편리해진다. 세 가지 중 이 부분이 가장 중요한 이점이라고 한다.

## Cohesion and Coupling

좋은 모듈은 아래 두 가지 조건을 포함한다.

- **High cohesion** - 하나의 모듈 안의 내용들은 모두 관계있는 것들로 묶어주면 좋다.
- **Low coupling** - 모듈끼리는 서로 최대한 독립적이어야 한다.

## Types of Modules

모듈은 아래와 같은 카테고리로 나누어 묶으면 좋다.

- **data pool** - 변수와 상수의 모음. 관련된 상수를 여럿 모아 header파일로 적용하면 좋다고 설명하고 있다. C 라이브러리 중에서 `<float.h>` 와 `<limits.h>`가 이에 해당한다.
- **library** - 관련된 함수의 모음. `<string.h>`는 문자열과 관련된 함수의 모음이다.
- **abstract object** - 숨겨진 데이터 구조 안에서 작동하는 함수의 모음. (여기서 object란 작업을 한 데이터를 모아둔 번들을 의미한다? a collection of data bundled with operations on the data) 앞서 나온 stack module이 여기에 해당된다고 한다.
- **abstract data type(ADT)** - 19.3 참고

# 19.2 Information Hiding

잘 설계된 모듈은 정보은닉을 하기도 한다. 의도적으로 특정 정보를 숨기는 것은 다음 두 가지 이점이 있다. 

- Security - 구조를 모르는 이상 건드려서 수정을 할 수 없기 때문에 형태를 유지하는데 있어 안전하다고 볼 수 있다.
- Flexibility -  잘 설계되었다면 수정이 용이하다. 모듈을 다시 구현할 필요는 있겠지만 (데이터 타입이 바뀌거나 하면), 모듈의 인터페이스를 수정할 필요는 없을 것이라고 설명하고 있다. (위에 추가하는 h나 c들)

정보은닉의 예시로는 `static` 변수가 있다고 한다. 

## A Stack Module

Stack 모듈의 경우, 외부에서 접근하지 못하게 하기 위해서 변수와 함수에 static을 붙여 은닉한 케이스이다. 

가독성을 위해 매크로에서 public 과 private이라는 용어로 변경해서 사용하는 방법도 있다.

    #define PUBLIC //빈칸
    #define PRIVATE static

# 19.3 Abstract Data Types

이런 추상 오브젝트의 경우 일회성인 점을 보완하기 위해, 새로운 type으로 만들어줄 수 있다. 

위에서 본 Stack을 새로운 데이터 타입으로 생성한다면 우리는 두 개의 stack을 같은 프로그램에서 동시에 사용할 수 있게 된다.

    Stack s1, s2;
    
    make_empty(&s1);
    make_empty(&s2);
    push(&s1, 1);
    push(&s2, 2);
    if(!is_empty(&s1))
    	printf("%d\n", pop(&s1));

클라이언트의 입장에서 s1, s2 변수가 스트럭쳐인지, 포인터인지는 중요하지 않다. 그저 해당 변수에 붙은 기능을 잘 사용할 수만 있으면 된다. 이런 걸 두고 우리는 추상화라고 한다.

Stack 타입을 사용하려면 우리는 헤더 파일을 생성해야 한다. Stack.h의 내용 중 Stack 타입의 스트럭쳐 선언은 아래와 같다.

    #define STACK_SIZE 100
    
    typedef struct {
    	int contents[STACK_SIZE];
    	int top;
    } Stack;
    
    void make_empty(Stack *s);
    bool is_empty(const Stack *s);
    bool is_full(const Stack *s);
    void push(Stack *s, int i)
    int pop(Stack *s);

## Encapsulation

C는 캡슐화 기능이 떨어지는 언어 중 하나이다. 앞서 본 Stack 또한 abstract data type이 아닌 이유는 클라이언트의 입장에서 언제든 구조를 확인하고 접근할 수 있기 때문이다.

## Incomplete Types

 C에서 캡슐화를 할 수 있는 유일한 방법은 incomplete type을 통해서이다. 

    struct t;

incomplete type은 위와 같이 선언할 수 있다. 스트럭쳐인 것은 알 수 있으나 정확한 구조는 알 수 없어 사이즈를 판단할 수 없는 선언문이라 할 수 있다. 

이런 타입으로는 새로운 변수를 선언할 수는 없으나 포인터로 해당 타입을 가리킬 수는 있다.

    typedef struct t *T; 
    
    // 이해한 것이 맞다면 T i; 라고 선언하여 T타입의 i 변수를 선언하고 이 i 변수는
    // t라는 이름의 incomplete type의 스트럭쳐를 가리키는 포인터 변수인 것이다.

위와 같이 작성하면 t라는 이름의 스트럭쳐를 가리키는 T타입의 포인터가 된다. 다만 이런 경우 `->` 는 사용할 수 없다. (구조를 알 수 없기 때문에)

# 19.4 A Stack Abstract Data Type

추상 데이터 타입의 활용 예를 보기 위해 앞서 계속 언급된 stack 모듈을 활용해보자. 

## Defining the Interface for the Stack ADT

추상 데이터 타입인 stack은 우선 `stack-ADT.h` 파일에 선언해보자. `Stack`데이터 타입은 `stack_type` 스트럭쳐를 가리키는 포인터로 선언하고, 해당 스트럭쳐는 incomplete type으로 설정한다. 이 스트럭쳐의 구성은 stack을 구현하는 파일에서 완성시킬 예정이다. 

**stackADT.h (version 1)**

    #ifndef STACKADT_H
    #define STACKADT_H
    
    #include <stdbool.h>
    
    typedef struct stack_type *Stack;
    
    Stack create(void);
    void destroy(Stack s);
    void make_empty(Stack s);
    bool is_empty(Stack s);
    bool is_full(STack s);
    void push(Stack s, int i);
    int pop(Stack s);
    
    #endif

`stackADT.h`를 가져오면 클라이언트는 `Stack`타입의 변수를 선언할 수 있고 밑의 함수도 불러올 수 있지만 `stack_type`의 스트럭쳐의 멤버에는 접근할 수 없다. 이 스트럭쳐는 다른 파일에서 선언될 예정이기 때문이다.

앞선 예시들에 비해 추가된 함수는 `create`와`destroy`인데 `create`는 `Stack`변수를 하나 만들어내어 빈stack을 위한 공간을 마련하게 되는 것이고, `destroy`는 해당 공간을 없애는 함수이다.

위의 헤더파일의 내용을 활용하여 클라이언트에서 작업을 한다면 아래처럼 활용할 수 있다. 

    int main(void)
    {
    	Stack s1, s2; 
    	int n;
    
    	s1=create();
    	s2=create();
    
    	push(s1, 1);
    	push(s1, 2);
    
    	destroy(s2);
    	...
    	return 0;
    }

## Implementing the Stack ADT Using a Fixed-Length Array 스택 추상 데이터 타입을 고정 길이 배열로 구현하기

앞서 헤더파일에 prototype을 선언했으니 가장 간단한 구현 방법은 `stackADT.c`파일을 만들고 그 안에 상세 내용을 적어주는 것이다. 

    #include <stdio.h>
    #include <stdlib.h>
    #include "stackADT.h"
    
    #define STACK_SIZE 100
    
    struct stack_type {
    	int contents[STACK_SIZE];
    	int top;
    }
    
    Stack create(void)
    {
    	Stack s=malloc(sizeof(struct stack_type));
    	if(s==NULL)
    		terminate("Error in create");
    	s->top=0; //s가 가리키는 스트럭쳐의 멤버 중 top의 값을 0으로 하라
    	return s;
    }
    
    ...

## Changing the Item Type in the Stack ADT

stack은 사실상 int 외에도 다른 타입의 데이터를 담을 수 있다. 이런 점을 반영하려면 담기는 데이터 타입이 언제든 쉽게 변화될 수 있도록 `Item`이라는 타입을 `typedef`를 통해 새로 만들어내면 이후에는 `int` 부분만 바꾸어도 쉽게 다른 데이터 타입을 가지도록 수정할 수 있다. 위에서 작성한 헤더파일을 이런 점을 반영하여 수정해보자.

    #ifndef STACKADT_H
    #define STACKADT_H
    
    #include <stdbool.h>
    
    typedef int Item;
    
    typedef struct stack_type *Stack;
    
    Stack create(void);
    void destroy(Stack s);
    void make_empty(Stack s);
    bool is_empty(Stack s);
    bool is_full(STack s);
    void push(Stack s, Item i);
    Item pop(Stack s);
    
    #endif

이런 변화에 맞춰 `stackADT.c`파일도 변경이 필요하다.

    ...
    struct stack_type {
    	Item contents[STACK_SIZE];
    	int top;
    }
    ...

## Implementing the Stack ADT Using a Dynamic Array

위의 예시의 스트럭쳐는 길이가 고정되어있다는 문제가 있다. 이를 해결하는 방법은 두 가지가 있는데, 하나는 스택을 linked-list로 구현하는 것이고, 다른 하나는 고정 길이 대신 배열을 가리키는 포인터를 멤버로 선언하는 것이다.

먼저, 포인터를 활용하는 방법을 살펴보자.

    struct stack_type {
    	Item *contents;
    	int top;
    	int size; // 최대 길이. stack이 is_full인지 확인하기 위한 멤버이기도 함.
    }

위에 새로이 추가된 size 멤버를 활용하여 create 함수도 변화를 준다면 아래와 같다.

    Stack create(int size); // prototype
    
    Stack create(int size)
    {
    	Stack s=malloc(sizeof(struct stack_type));
    	if(s==NULL)
    		terminate("Error in create");
    	s->contents=malloc(size * sizeof(Item));
    	if(s->contents==NULL) {
    		free(s);
    		terminate("Error in create");
    	}
    	s->top=0;
    	s->size=size;
    	return s;
    }

이제 `create`함수는 malloc을 두 번 사용하게 된다. 한번은 stack_type스트럭쳐의 공간을 확보하기 위해, 다른 한번은 stack_type스트럭쳐가 지니고 있는 배열이 담길 공간을 확보하기 위해서이다. 이와 같은 이유로, 만약 Stack변수를 destroy함수로 없애려 한다면 해당 함수도 아래와 같이 수정되어야한다.

    void destroy(Stack s)
    {
    	free(s->contents); // s가 가리키는 스트럭쳐가 보유한 배열의 공간 파기
    	free(s); // s가 가리키는 스트럭쳐가 있는 공간 파기
    }

## Implementing the Stack ADT Using a Linked List

배열을 포인터로 바꿔서 적어주더라도 여전히 길이가 한번 정해지면 고정된다는 문제가 남아있다. 이를 해결하기 위해선 node의 연결로 구성된 linked list를 활용할 수 있는데, 이럴 경우 node라는 이름의 스트럭쳐를 새로이 선언해주어야 한다.

    // node는 배열 한칸한칸이라고 생각하면 된다.
    struct node {
    	Item data;
    	struct node *next;
    };
    
    struct stack_type {
    	struct node *top; // 가장 위에 있는 데이터, 즉 노드 스트럭쳐를 가리킨다.
    };

`Stack`이 `node`를 바로 가리키면 편할 것 같지만, 이미 작성된 코드를 유지보수하기 위해선 `Stack`이 `stack_type`스트럭쳐의 포인터인 것이 훨씬 편리하다. 그래서 굳이 `stack_type`스트럭쳐를 유지하고 있는 것. 이후에 만약 저 linked-list의 길이를 나타내는 멤버를 추가하고 싶다면, `stack_type`스트럭쳐의 선언에다가 추가해주면 간편하다.

그렇다면 함수들 또한 함께 바뀌게 되는데, 대표적으로 `pop`함수(가장 위의 데이터를 뽑아주고 해당 데이터를 stack에서 없앤다)가 어떻게 변화했는지 살펴보자.

    Item pop(Stack s)
    {
    	struct node *old_top; // 임시 node 포인터변수. 가장 위에 걸 여기에 담았다 없앨 예정
    	Item i;
    
    	if(is_empty(s))
    		terminate("Error in pop");
    
    	old_top=s->top; // old_top은 가장 위의 노드를 가리키는 포인터가 된다.
    	i = old_top->data; // i는 가장 위의 노드의 data 멤버의 값이 된다.
    	s->top=old_top->next; // s가 가리키는 stack_type의 top포인터변수는 가장 위에 것 다음 노드를 가리키게 된다.
    	free(old_top); // 가장 위의 노드를 가리키는 포인터를 담아둔 old_top을 없앤다.
    	return i; 
    }

# 19.5 Design Issues for Abstract Data Types

C에선 어찌됐든 추상 데이터 타입을 효율적으로 쓰기엔 여러모로 문제가 많다고 한다. 문제들과 나름대로의 해결방법은 아래와 같다.

### Naming Conventions 작명 문제

위의 stack의 경우도 대부분 간결한 함수명을 가지고 있다 (아무래도 만들고 없애고 하는 과정이 데이터 타입을 선언할 땐 반드시 필요하기 때문에). 그 탓에 만약 이런 추상 데이터 타입이 여러개라면, 함수명이 겹칠 가능성이 높다. `create_stack`같은 형태로 수정해줄 수는 있으나 불편한 부분.

### Error Handling

위의 stack예시처럼, error를 방지하기 위해 손을 봐야하는 부분이 많다. 사용자 입장에서도 에러를 쉽게 확인하기 위해 `pop`같은 함수가 `Item`만이 아니라 성공여부를 `bool`로 return하게 하는 등의 여러 방편을 적용해볼 수 있다. C의 라이브러리에서 제공하는 매크로 중 `assert`는 특정 조건이 만족하지 않으면 프로그램을 바로 종료시킬 수도 있어 if문을 대체할 수도 있다고 한다. 

### Generic ADTs

위에서 Item을 define하여 추상 데이터 타입인 stack_type이 가질 수 있는 멤버의 타입을 정해준 바가 있다. 다만, 이런 코드의 치명적인 단점은 모든 stack이 동일한 데이터 타입만을 데이터로 가질 수 있다는 점이다. 만약 다른 데이터 타입을 가지는 stack을 만들고 싶다면 헤더파일과 c파일 모두 복사해서 Item부분만 변경하고 클라이언트에 추가해줘야하는 문제가 있다. 

이런 단점을 해결할 수 있는 방법으로 제시된 것은 void 포인터이다. 작성한다면 대략 이런 식이다.

    void push(Stack s, void *p);
    void *pop(Stack s); // 맨 위의 데이터를 가리키는 포인터가 반환된다.

그러나 void 포인터도 만능은 아니다. 두 가지 단점이 있다. 

- 포인터로 표현될 수 없는 기본 데이터 타입은 사용할 수 없다.
- 넘겨준 포인터의 데이터 타입을 비교하여 에러를 체크하는 방법이 불가능하다.

**void pointer관련**

void pointer는 역참조가 불가능하다. 아래는 코드 예시이다.

    int main() 
    { 
        int a = 10; 
        void *ptr = &a; 
        printf("%d", *ptr); // void pointer 역참조
        return 0; 
    }

    // 결과창
    Compiler Error: 'void*' is not a pointer-to-object type

이를 해결하고 싶다면 캐스팅을 해주는 방법이 있다.

    int main() 
    { 
        int a[2] = {1, 2}; 
        void *ptr = &a; 
        ptr = ptr + sizeof(int); 
        printf("%d", *(int *)ptr); // ptr을 int형 포인터 변수로 캐스팅
        return 0; 
    }

위의 stack예시에서는 이런 캐스팅이 불가하기 때문에 기본 데이터 타입은 사용할 수 없다고 하는 것. (아마도...)

### ADT in Newer Languages

C를 계승한 객체지향 언어인 c++이나 JAVA의 경우 위의 문제를 해결하기 위해 class와 같은 개념을 제공한다. 함수(메소드)명도 다른 클래스라면 동일한 이름으로 선언될 수 있기 때문에 중복 문제에서도 자유롭다. JAVA였다면, `Stack`은 `class Stack`으로 선언되었을 것이다. 

# Q&A

incomplete type의 다른 예시는 무엇이 있나요?

길이가 선언되지 않은 배열, 멤버를 정의하지 않은 union, void 등도 모두 incomplete type의 예시이다(얼마의 공간을 차지할지 알 수 없기 때문에)

incomplete type의 사용에 관한 제약은 어떤 것이 있나요?

- 스트럭쳐나 유니온의 멤버로 사용될 수 없다.
- 배열의 요소로 사용될 수 없다.
- 함수의 인자로 사용될 수 없다 (선언에서는 괜찮다고 한다)