### 17. Advanced Uses of Pointers

#### 17.1 Dynamic Storage Allocation

C의 자료구조는 사이즈가 보통 고정되어 있다. 예를 들면, 배열의 경우 한번 컴파일 되면 사이즈가 고정되어 있다. (C99부터는 VLA를 지원하지만, 이 것도 런타임 동안에 한번 길이가 정해지면 그 길이를 수정 할 수 없다.)

C는 프로그램이 실행되는 동안 저장공간을 새로 할당해주는 동적할당을 지원한다.

동적할당을 이용하면, 프로그램이 실행되는 동안 필요한 만큼 저장공간을 늘리거나 줄이는 것이 가능하다.

모든 자료형에 동적할당을 이용할 수 있지만, 스트링, 배열, 구조체에서 가장 많이 사용된다. 특히, 동적할당 구조체(structure)는 서로 함께 link를 할 수 있기 때문에, list, tree 등의 자료구조를 구현할 때 유용하다.

- **Memory Allocation Functions**

  동적할당 함수를 사용하기 위해 `<stdlib.h>` 라브러리를 include 해줘야 함

  **<동적할당 함수>**

  > - malloc : 메모리 블럭을 할당하지만, 초기화는 하지 않음
  > - calloc  : 메모리 블럭을 할당하고, 모든 값을 0으로 초기화 함
  > - realloc : 이미 할당되어 있는 메모리 블럭의 사이즈를 재설정 함
  >
  > 위의 세가지 동적할당 방법중에 malloc이 가장 속도가 빠르다.

  동적할당 함수가 호출 될 대, 함수는 어떤 타입의 데이터가 저장될지 알 수 없다. 그렇기 때문에, 일반적인 pointer를 리턴해줄 수 없다. 대신에, void * 타입의 값을 리턴해준다. void *는 generic 포인터로 단지 메모리의 주소만 전달해준다.

  

- **Null Pointers**

  메모리 할당 함수가 호출되었을 때, 메모리 블럭이 필요한만큼 충분히 크게 할당되지 않을 가능성이 있다. 만약 그렇다면, 함수는 null pointer를 리턴하게 된다. null pointer는 아무것도 가리키지 않는 pointer이고 다른포인터들과 구분되는 특별한 값을 가질 수 있다. 그렇기 때문에, 포인터 변수에 메모리 할당 함수의 리턴값을 저장한 후에, null pointer인지 테스트하는 것이 반드시 필요하다. 만약, null pointer가 포인터 변수에 저장되고, 포인터 변수를 이용해 메모리에 접근한다면 예상치 못한 결과를 일으키거나 충돌이 발생한다.

  > null pointer는 NULL이라는 이름의 매크로로 표현된다. 그렇기 때문에, malloc의 리턴값을 테스트 할 때 아래와 같이 할 수있다.
  >
  > ```c
  > p = malloc(10000);
  > if(p==NULL) {
  > 	/*할당에 실패 했을 경우 액션*/
  > }
  > ```
  >
  > 또는 , 아래와 같이 표현 가능하다.
  >
  > ```c
  > if((p=malloc(10000)) == NULL) {
  >  /*할당에 실패 했을 경우 액션*/
  > }
  > ```

  

  cf) **NULL 매크로**

  NULL매크로가 정의된 헤더등

  - `localel.h`, `stddef.h`, `stdio.h`, `stdlib.h`, `string.h`, `time.h`, `wchar.h`

  ```c
  if(p==NULL) 과 if(!p) 는 동일하게 동작
  if(p!=NuLL) 과 if(p != NULL) 도 동일하게 동작
  ```

#### 17.2 Dynamically allocated String

스트링은 문자의 배열로, 실제 얼마의 길이가 사용될지 미리 알기 어려운 경우가 있다.이때 동적할당을 이용하여 스트링의 길이를 프로그램이 실행중 일 때 결정할 수 있다.

- **Using malloc to Allocate Memory for a String**

  Prototype : 

  ```c
  void *malloc (size_t size);
  ```

  - 메모리 사이즈(바이트)를 파라미터로 가진다. 
  - size_t는 C라이브러리에 정의된 unsigned integer 타입이고, 엄청 큰 메모리 블럭을 할당하지 않는 한, 일반적인 정수라고 생각하면 된다.

  

  String 동적할당

  ```c
  char *p;
  p = malloc(n+1);
  ```

  - 길이가 n인 문자열의 스트링의 size = n + 1  // '\0'

  - p가 char* 타입이기 때문에, malloc의 void* 타입은 char*로 형변환 되어 할당 될 것이다. 하지만 조금더 명시적으로 해주기 위해 캐스팅을 사용 할 수 있다. `p = (char *) malloc (n+1)`
  - null 문자를 포함시키는 것을 까먹지마라.
  - 만약 p에 "abc"를 복사하면 아래와 같을 것이다.
  - `strcpy(p, "abc")`

  ![img](https://i.imgur.com/56U29mK.png)

- **Using Dynamic Storage Allocation in string functions**

  동적 할당은 함수내에서 새로 생긴 스트링의 포인터를 리턴해주는 것을 가능하게 한다. 

  ```c
  //두개의 스트링을 합쳐 새로운 스트링을 생성
  char * concat(const char *s1, const char *s2)
  {
      char *result;
      result = malloc(strlen(s1) + strlen(s2) + 1);
      if( result == NULL){
          print ("Error : malloc failed in concat\n")
              exit(EXIT_FAILURE);
      }
      strcpy(result, s1);
      strcat(result, s2);
      return result;
  }
  
  ...
      
  char *p;
  p = concat("abc", "def");
  
  ...
  ```

  동적할당은 조심히 다루어야 한다. 더이상 사용하지 않는 공간은 free를 해주어야 한다. 그렇지 않으면 나중에 메모리가 부족할 수 있다.s

- **Arrays of Dynamically Allocated Strings**

  ```c
    //remind2 (기존 2차원 배열을 이용했던 것을 1차원바열과 동적핟당을 이용)
  #include <stdio.h>
  #include <stdlib.h>  /*추가*/
  #include <string.h>
  
  #define MAX_REMIND 50 /* maximum number of reminders */
  #define MSG_LEN 60  /* max length of remnder message*/
  
  int read_line(char str[], int n);
  
  int main(void){
      char *reminders[MAX_REMIND];  /*char reminders[MAX_REMIND][MSG_LEN+1] 에서 바꿈*/
      char day_str[3], msg_str[MSG_LEN+1]; 
      int day, i, j, num_remind=0;
  
      for(;;){
          if(num_remind==MAX_REMIND){
              printf("-- No space left --\n");
              break;
          }
  
          printf("Enter day and reminder: ");
          scanf("%2d", &day);
          if(day==0)
              break;
          sprintf(day_str, "%2d", day);
          read_line(msg_str, MSG_LEN);
  
          for(i=0; i<num_remind; i++)
              if(strcmp(day_str, reminders[i])<0)
                  break;
          for(j=num_remind; j > i; j--)
              reminders[j] = reminders[j-1]; //strcpy(reminders[j], reminders[j-1]);
          
  		reminders[i] = malloc(2+strlen(msg_str)+1);
          if (reminders[i] == NULL) {
              printf("--No space left -- \n");
              break;
          }
  		strcpy(reminders[i], day_str);
          strcat(reminders[i], msg_str);
          
          num_remind++;
      }
  
      printf("\nDay Reminders\n");
      for(i=0; i<num_remind; i++){
          printf(" %s\n", reminders[i]);
      }
  
      return 0;
  }
  
  int read_line(char str[], int n){
      int ch, i=0;
  
      while((ch=getchar()) != '\n'){
          if(i<n){
              str[i++]=ch;
          }
      }
      str[i]='\0';
      return i;
  }
  ```

  

#### 17.3 Dynamically Allocated Arrays

- **Using malloc to allocate stortage for an array**

  string과 다른점은 메모리 공간을 얼마나 지정해줄지 정하기 어렵다는 점이다. 그렇기 때문에 **sizeof**함수를 사용하여 공간을 할당해주는 것이 좋다. 

  ```c
  int *a;
  a = malloc(n * sizeof(int))
      ..
  for (i = 0; o<n; i++){
      a[i] = 0;
  }
  ```



- **The calloc Function**

  calloc의 prototype :

  ```c
  void *calloc(size_t nmemb, size_t size);
  ```

  두 개의 파라미터를 가진다. size 바이트의 크기를 가지는 nmemb개가 저장될 수 있는 공간을 할당하고 모든 bit를 0으로 초기화한다.

  또한, void * 를 리턴하고, 만약 size만큼 공간 할당이 불가능하면 null pointer를 리턴한다. 

  ```c
  a = calloc(n,sizeof(int)); 
  //n개의 int형이 저장될 수 있는 공갈을 할당하고 0으로 초기화
  
  struct point (int x, y ; ) *p;
  p = calloc(1, sizeof(struct point));
  //p point 구조체 하나를 할당하고 0으로 모든값 초기화
  ```

  

- **The realloc Function**

  realloc의 prototype :

  ```c
  void *realloc(void *ptr, size_t size);
  ```

  한 번 메모리 공간을 할당 했지만, 너무 크거나 작게 할당한 경우 조절이 필요하다. 이때 realloc을 사용하여 공간의 크기를 조절할 수 있다.

  미리 공간이 할당된 공간을 가리키는 포인터와 새롭게 할당할 size를 파라미터로 가진다.

  realloc을 호출할 때 인자로 전달되는 포인터 아규먼트가 반드시 배열로 사용되는 메모리를 가리키는 포인터일 필요는 없지만, 제대로 사용하지 않으면 예상치 못한 결과를 가져올 수 있다.

  < realloc 사용시 주의 사항>

  - 메모리 블럭을 확장시킬 때 , 새로 추가된 부분의 바이트들을 초기화 하지 않는다.
  - 만약, 요구한 만큼 메모리 블럭을 확장 시킬수 없을 때, null pointer를 리턴하고, 기존 데이터는 남아 있다.
  - 만약, 첫 번째 아규먼트로 null pointer를 넘겨주고 호출하면 malloc처럼 실행된다.
  - 두 번째  아규먼트인 size를 0으로 전달하면, memory 할당을 해제(free)한다.

  만약, 공간을 확장하려 할 때, 확장될 예정인 공간에 이미 다른 공간이 할당되어 있다면, 다른공간에 새로 할당을 하고 이전 공간에 있던 데이터를 복사한다. 그렇기 때문에 주소가 바뀌는 상황을 대비하여 반드시 realloc을 사용 할 때는 리턴값을 받아 포인터 변수를 업데이트 해줘야 한다. 

#### 17.4 Deallocating Storage

동적 할당은 heap 영역에 저장된다. 동적할당 함수를 너무 많이 호출하는 거나 많은 양의 메모리를 할당해주는 것은 heap영역을 초과 하여 null pointer를 리턴값으로 가지게 된다.

\<memory leak>

아래의 경우, 처음에 p에 할당되었던 공간은 다시 사용 할 수 없게 되고, 이런 경우 p에 있었던 공간의 데이터는 garbage라고 불린다. 결국 메모리의 낭비로 이어지기 때문에 이 상황을 memory leak라고 한다. 몇 프로그램언어는 garbage collector가 있는데, 이는 자동으로 메모리를 재사용가능하게 해주는 것을 말한다. 하지만, c에는 garbage collector가 없다. 그렇기 때문에, free를 사용하여 할당된 영역을 해제해주는 작업이 필요하다.

```c
p = malloc(...);
q = malloc(...);
p=q;
```

![img](https://i.imgur.com/Ti6DwFJ.png)



- **The free Function**

  prototype : 

  ```c
  void free(void *ptr);
  ```

  할당되어 있는 메모리 블럭을 더 이상 사용하지 않을 때, 메모리를 해제해준다.

  ```c
  p = malloc(...);
  q = malloc(...);
  free(p);
  p=q;
  ```

   

- **The "Danling Pointer" Problem**

  `free(p)`를 호출해서 메모리 블럭을 release해줄 수 있지만, p에 저장된 주소 값은 변하지 않는다. 만약, p가 메모리 블럭의 포인터로 더이상 사용되는 것을 잊어버린다면, 문제가 발생할 수 있다.

  ```c
  char *p = malloc(4);
  ...
  free(p);
  ...
  strcpy(p, "abc"); /** WRONG **/
  ```

  p를 해제한 후 p가 가리키는 공간을 수정하면, 심각한 문제가 발생한다. 프로그램이 메모리를 더이상 컨트롤 할 수 없기 때문이다. 프로그램 충돌과 같은 재앙이 발생할 수 있다.

  댕글링 포인터는 찾기 힘들수 있다. 몇 개의 포인터가 같은 메모리 공간을 가리키고 있을 때, 같은 공간을 가리키고 있는 포인터가 모두 댕글링 포인터가 되기 때문이다.

#### 17.5 Linked Lists

동적할당은 list, tree, graph 등의 linked data structure를 구현하는데 유용하다. 

linked list는 구조체의 체인형태로 되어 있고, 하나의 구조체를 node라고 부르며, 각 node는 다음 node를 가리키는 포인터를 포함하고 있다. 마지막에 위치한 node의 포인터는 null 포인터이다.

linked list를 이용할 때 배열보다 좋은 점은 필요에 따라서 리스트 사이즈도 조절 가능한 형태로 이용이 가능하다는 것이다. 데이터를 삭제하거나 새로 삽입하는 것도 가능하다. 하지만, random access는 불가능하다는 단점이 있다. 배열에서는 어떤 값이라도 접근하는 속도는 같지만, linked list는 헤드(처음)부분부터 순차적으로 탐색하여 접근하기 때문에 헤드와 가까울 수록 빨리 접근이 가능하고 멀수록 오래걸린다.

![linked list에 대한 이미지 검색결과](https://www.educative.io/api/edpresso/shot/5077575695073280/image/5192456339456000)

- **Declaring a Node Type**

  ```c
  struct node{
  	int value;
      struct node *next;
  }
  ```

  typedef 가 아닌 tag를 사용해야 linked list를 구현 가능하다. tag를 사용하지 않으면 next 포인터를 선언할 방법이 없다.

  ```c
  struct node *first = NULL;
  ```

  first node에 NULL을 할당해, 초기의 비어있는 list를 생성. first 자체는 list에 포함되는 node라기 보다는, list의 첫 원소를 가리키는 포인터 역할을 하게된다.

- **Creating a Node**

  <순서>

  > 1. node가 저장될 공간을 할당을 함
  > 2. data를 node에 저장
  > 3. node를 list에 삽입

  node를 생성할 때, list에 삽입되기 전까지 node를 일시적으로 가리키는 포인터 변수가 필요하다. 

  ```c
  struct node *new_node;
  
  new_node = malloc(sizeof(struct node)); //node 구조체 만큼 사이즈를 설정
  ```

  공간을 할당했다면, 공간에 data를 저장해준다.

  ```c
  (*new_node).value = 10;
  ```

  괄호 필수(. 연산자가 * 연산자보다 우선순위를 가짐)

- **The -> Operator**

  right arrow selection :  *와 .  연산자 대신에 사용할 수 있다.

  ```c
  (*new_node).value = 10; 
  //아래와 같다.
  new_node -> value =10;
  ```

  주의) scanf 에서 -> 연산자를 써도 여전히 &가 필요함

  ```c
  scanf("%d", &new_node->value);
  ```

- **Inserting a Node at the Beginnig of a Linked List**

  linked list의 장점은 원하는 곳에 node를 삽입할 수 있다는 점이다. 먼저, 가장 쉬운 방법인 linked list의 시작점에 노드를 삽입하는 방법을 알아보겠다.

  new_node가 새로 삽입될 node를 가리키고, new_node의 멤버인 next 포인터에 first를 할당하고, first 포인터에는 new_node가 가리키는 주소가 들어가야 한다.

  ```c
  struct node{
  	int value;
      struct node *next;
  }
  ...
  struct node *first = NULL;
  struct node *new_node;
  
  new_node = malloc(sizeof(struct node)); 
  (*new_node).value = 10;
  //아래가 삽입하는 과정
  new_node->next = first;
  first = new_node;
  
  new_node = malloc(sizeof(struct node)); 
  (*new_node).value = 20;
  
  new_node->next = first;
  first = new_node;
  ```

  ![img](https://i.imgur.com/LNSfzn9.png)

  ![img](https://i.imgur.com/HdbnEQH.png)

  <삽입하는 함수>

  ```c
  struct node *add_to_list (struct node *list, int n)
  {
      struct node *new_node;
  
      new_node = malloc(sizeof(struct node)); 
      if(new_node == NULL)
      {
         //처리
      }
      (*new_node).value = n;
      new_node->next = list;
      return new_node;
  }
  
  first = add_to_list (first, 10);
  first = add_to_list (first, 20);
  ```

  \<여러개의 값을 입력받아 list로 생성해주는 함수>

  ```c
  struct node *read_numbers(void)
  {
      struct node *first = NULL;
      int n;
      
      printf("정수를 여러개 입력하세요. (끝내려면 0을 입력)");
      for(;;){
          scanf("%d", &n);
          if(n==0)
              retrun first;
          first = add_to_list(first, n);
      }
  }
  ```

- **Searching a Linked List**

  linked list의 처음부터 끝까지 순회하는 방법.

  ```c
  for(p=first; p! = NULL; p=p->next){
      ...
  }
  ```

  원하는 값을 찾아 주소값을 리턴하는 함수

  ```c
  struct node *search_list(struct node *list, int n)
  {
      struct node *p;
     
     	for(p=first; p! = NULL; p=p->next)
     		if(p->value == n)
        	  return p;
  	return NULL;
  }
  ```

   위의 함수의 list는 변경해도 괜찮기 때문에 아래와 같이 써도 된다.

  ```c
  struct node *search_list(struct node *list, int n)
  {  
     	for( ; list! = NULL && list->value != n; list = list->next)
     		;
  	return list;
  }
  ```

  for문대신 while문을 쓰면  아래와 같이 표현 가능하다.

  ```c
  struct node *search_list(struct node *list, int n)
  {  
     	whiler( list! = NULL && list->value != n)
          list = list->next;
  	return list;
  }
  ```

- **Deleting a  Node from a Linked List**

  > 1. 삭제할 Node로 이동
  > 2. 앞 노드를 뒷노드에 연결해줌 (해당노드를 무시하도록 만들어 줌)
  > 3. free함수를 호출해서 삭제할 node의 메모리를 해제함

  삭제할 노드를 찾는다고해도, 삭제할 노드의 앞 주소를 알기 위해서는 trailing pointer를 사용해야한다. 아래 코드에서는 cur은 삭제할 node를 찾는 용도로, prev를 trailing pointer로 사용한다.

  ```c
  for( cur = list, prev = NULL;
     	 cur != NULL && cur->value !=n;
       prev = cur, cur =cur->next) // 왼쪽부터 실행
   ;
  ```

  삭제하는 함수는 아래와 같이 표현 가능하다.

  ```c
  struct node *delete_from_list(struct node *list, int n)
  {
      struct node *cur, *prev;
      
      for( cur = list, prev = NULL;
     		 cur != NULL && cur->value !=n;
      	 prev = cur, cur =cur->next)
     	 ;
      if( cur ==NULL )   //not found
          retrun list;
      if(prev == NULL)  //the first node
          list = list->next;
      else
          prev->next = cur->next; //some other node
      free(cur);
      return list;
  }
  ```

- **Ordered Lists**

  데이터의 크기에 따라 정렬되어 있는 list를 ordered list라 한다. orderd list에 데이터를 삽입하는 것은 어렵지만, orderd list에서 어떤 데이터를 찾는 것은 빨라진다. 



#### 17.6 Pointers to Pointers

pointers to pointers개념은 linked list에서도 자주 사용된다. list에 새로운 node를 삽입하는 함수를 아래와 같은 함수로 새롭게 표현할 수 있다.

```c
//리턴 값을 가지는 함수

struct node *add_to_list (struct node *list, int n)
{
    struct node *new_node;

    new_node = malloc(sizeof(struct node)); 
    if(new_node == NULL)
    {
       //처리
    }
    new_node->value = n;
    new_node->next = list;
    return new_node;
}

first = add_to_list (first, 10); //first에 리턴값을 할당해야 함
first = add_to_list (first, 20);

// 리턴값이 없는 방법.
void (struct node **list, int n) // 더블(이중) 포인터 사용
{
    struct node *new_node;

    new_node = malloc(sizeof(struct node)); 
    if(new_node == NULL)
    {
       //처리
    }
    new_node->value = n;
    new_node->next = *list;   //list
    *list = new_node;
}

add_to_list (&first, 10); // 값을 first에 할당하지 않고, first의 주소를 넘김
add_to_list (&first, 20);
```

위 코드에서 list를 더블 포인터로 선언한 이유는 다음과 같다.

만약, struct node *list 로 선언되 되었고 함수의 호출도 add_to_list(first, 10);과 같이 되었다고 가정하면, list = new_node; 형식으로 list에 새로 삽입된 node의 주소가 할당 될 것이다. 하지만,  list와 first가 같은 node를 가리키고 있는 것은 맞지만 두개가 완전히 동일한 것은 아니다.(호출 시 주소값만 복사된 것) 해결방법으로, 더블 포인터를 이용해서 **list가 first의 주소를 받게 되면 \*\*list는 first를 가리키게 되고, *list는 first를 역참조한 것이기 때문에 *list에 new_node를 할당하면 first 자체의 값이 바뀌게 되는 것이다.

#### 17.7 Pointers to Functions

함수도 메모리에 위치하기 때문에 모든 함수는 주소를 가지고 있다.

- **Function Pointers as Arguments**

  f라는 함수를 아규먼트로 intergrate라는 함수로 전달하기 위해서는 f라는 함수를 포인터로 전달해야한다. 

  프로토타입 : 

  ```c
  double integrate(double (*f)(double), double a, double b);
  //f가 가리키는 함수는 파라미터와 리턴값이 모두 double 타입인 함수이다.
  ```

  위 코드에서, (*f)의 괄호는 f 자체가 함수 포인터라는 것을 명시해준다. (리턴값이 포인터인 함수가 아니라는 것을 의미)

  함수 포인터가 아닌 그냥 함수를 넘기는 것도 가능하다. 컴파일러 관점에서는 위와 아래의 코드가 같은 의미를 가진다.

  ```c
  double integrate(double f(double), double a, double b);
  ```

  integrate 함수를 호출할 때, 함수의 이름을 아규먼트로 준다

  ```c
  result = integrate(sin, 0.0, PI/2);
  ```

  함수를 아규먼트로 사용할 때, 괄호를 사용하지 않는다.(sin() : X) C컴파일러는 함수 호출에 대한 코드를 생성하는 것 대신, 함수 포인터를 제공한다. 위 코드는 sin함수를 호출하는 것이 아니라, sin함수의 pointer를 integrate함수로 전달해준다. (함수의 이름(sin)만 쓰이면 그 자체가 포인터다.)

  integerate 함수 내부에서 함수 포인터를 사용하는 방법은 아래와 같다.

  ```c
  y = (*f)(x);
  ```

  *f는 f가 가리키는 함수를 의미하고, x는 그 함수의 아규먼트이다. 즉, integrate(sin, 0.0, PI/2); 함수가 호출되었다면, *f는 sin 함수를 호출 할 것이다. 또한, C는 (\*f)대신 f(x)로 호출하는 것도 허용한다. 하지만 f가 포인터라는 것을 명시하기 위해 (\*f)라고 쓰는 것이 더 좋은 방법이다.

- **Other uses of Function Pointer**

  함수 포인터를 변수나 배열 또는 구조체, 유니온에 담을 수 있다. 또한, 함수 포인터를 리턴하는 함수를 선언할 수도있다.

  <함수 포인터를 변수에 할당하는 법>

  ```c
  // 함수 포인터를 담을 수 있는 변수
  void (*pf)(int);   // pf포인터는 int를 파라미터로 가지고 void 리턴타입 인 모든 함수를 가리킬 수 있다.
  
  pf = f; //f의 프로토 타입이 void f(int x); 라고 하면 pf에 f의 주소를 할당가능하다. &를 사용하지 않는 것을 주의( &f : X )
  
  (*pf)(i); //또는
  pf(i); //와 같이 함수를 호출 할 수 있다.
  ```

  <함수 포인터를 배열에 할당하는 법>

  ```c
  void (*file_cmd[])(void) = {new_cmd,
                             open_cmd,
                             close_cmd,
                             close_all_cmd,
                             save_cmd,
                             scve_as_cmd,
                             save_all_cmd,
                             print_cmd,
                             exit_cmd
                             };
  //9개의 함수를 가리키는 포인터 배열을 선언, 모든 함수는 파라미터와 리턴값을 가지지 않는 함수이다.
  //만약 n이 0~8 값을 가진다면, 아래와 같이 원하는 함수를 호출 할 수 있다.
  (*file_cmd[n])(); //또는
  file_cmd[n]();
  ```

  switch 구문을 이용해서 위와 비슷한 기능을 구현할 수 있지만, 함수 포인터 배열을 이용하는 것은 프로그램이 실행되는 동안 바뀔 수 있는 유연성을 가지고 있다.

#### 17.8 Restricted Pointer (C99)

참고) https://dojang.io/mod/page/view.php?id=760

C99부터 restrict 키워드를 제공한다. 

```c
int * restrict p;
```

restrict를 이용해 선언된 포인터를 restrict pointer 라고 부른다.  p가 가리키는 대상의 값이 변경된다면, 해당 대상에 p를 이용해서 접근하는 방법 외에는 접근이 불가능해진다. 아래의 경우는 동적할당을 예로들었지만, 특정한 변수의 주소를 가리키는 경우도 마찬가지이다.

```c
int * restrict p;
int * restrict q;

p = malloc(sizeof(int));
q = p;
*q = 0;   // 비정의 행동을 하게됨
```

p는 restrict 포인터 이기 때문에, p가 가리키는 대상을 q로 접근할 수 없기 때문에 비정의 행동이 일어나게 된다. 

cf) 대상에 접근하는 방법이 하나보다 많은 경우를 aliasing 이라고 한다. 위의 경우 같은 대상을 p와 q가 가리키고 있기 떄문에 *p와 *q는 aliases 이다.

restrict 포인터는 컴파일러에게 최적화(다른 공간을 가르키고 있다고 프로그래머가 직접 컴파일러에게 알려주는 것과 마찬가지기 때문에 컴파일러가 해당 공간을 재확인하는 작업을 를 하라고 알려주는 키워드입니다(메모리가 다른 공간을 가리킨다고 보장한다거나 메모리 공간을 검사하는 용도가 아닙니다). 만약 같은 메모리 공간을 가리키는 포인터에 restrict를 붙여서 컴파일하게 되면 최적화 때문에 잘못된 결과가 나올 수 있으니 주의해야 합니다. 따라서 포인터가 가리키는 메모리 공간을 프로그래머가 직접 확인한 뒤 다른 공간을 가리킬 때만 restrict를 사용해야 합니다.

또한, restrict 포인터가 extern 없이 지역변수로 선언된다면, restricct 포인터가 선언된 블럭 이외에서 실행되지 않는다. 함수의 파라미터로 사용되는 경우에는 해당 함수 내부에서만 사용이 가능하다. 만약, 파일 스코프를 가지도록 선언이 되었다면 프로그램이 끝날때 까지 적용된다.

\<memcpy vs memmove>

```c
void *memcpy(void * restrict s1, const void * restrict s2, size_t n);
```

memcpy는 바이트들을 복사하는 거고, strcpy는 문자열을 복사하는 것.

s2를 s1에  n바이트 만큼 복사.

s1과 s2가 restrict 포인터라는 것은 서로 다른 대상을 가르키고 있는 것을 의미.(하지만, 보증해주는 것은 아님, 프로그래머가 주의해서 다른 포인터를 주어야함)

```c
void *memmove(void *  s1, const void *  s2, size_t n);
```

memmove는 momcpy와 동일한 기능을 수행한다. 

restrict를 사용하지 않아, s1과 s2가 같아도 문제가 없다.

예를 들면, 한칸 씩 배열을 쉬프트 시키는 것도 가능하다.

```c
memmove(&a[0], &a[i], 99*sizeof(int));
```

만약, 정교한 튜닝을 하는 경우가 아니라면 보통 restrict는 사용하지 않지만, 많은 C라이브러리에 사용되기 때문에 알고 있는 것이 좋다.



#### 17.9 Flexible Array Members (C99)

가끔, 구조체 안에 사이즈가 정해지지 않은 배열을 정의 해야할 경우가 있다. 예를 들면,  보통 스트링은 null문자와 함께 사용되지만, null문자를 제외하고 사용하는게 더 유용한 경우가 있다. 예를 들면, 구조체를 활용하여, 실제 문자의 길이와 null문자를 포함하지 않는 char 배열을 선언하는 방식이 있다.

```c
struct vstring {
	int len;
	char chars[N];
};
```

위의 경우 N은 최대 매크로로 지정해준 최대사이즈 이다. 고정된 값의 배열을 쓰는 것은 메모리의 낭비일 뿐만 아니라, 최대값이 정해져 있다는 단점이 있다.

전통적으로 프로그래머는 이 문제를 chars의 길이를 1로 지정하하고 동적할당을 하는 방식으로 해결 했다. 

```c
struct vstring {
	int len;
	char chars[1]; //dummy value
};
...
struct vstring *str = malloc(sizeof(struct vstring ) + n - 1 ); //-1 : dummy value의 크기가 1byte기 때문에 그만큼 빼준다.
str -> len = n;
```

선언된 구조체 보다 n -1 만큼 더 메모리 공간을 할당 해서 사용하는 일종의 트릭이다. chars 배열을 이용하여 메모리에 저장이 가능하다. 이 방법을 "struct hack" 이라고 부른다.	

struct hack은 문자배열의 길이에 제한이 없고, 다양하게 쓰이는 장점으로, 시간이 흐름에 따라, 많은 컴파일러들이 이것을 공식적으로 지원하게 되었고, 심지어 길이가 없는 배열도 선언을 가능하게 했다. zero-length array는 C89는 지원이 되지않지만 C99부터는 지원이 된다. 

또한, C99부터는 flexible array member를 지원하여 struct hack과 같은 기능을 지원한다. 배열의 길이를 생략 가능하게 하는 방법이다.

```c
struct vstring {
	int len;
	char chars[]; //dummy value가 없다.
};
...
struct vstring *str = malloc(sizeof(struct vstring ) + n ); // dummy 값이 없기 때문에 -1을 해주지 않아도 된다.
str -> len = n;
```

<주의점!!>

Flexible Array Member는 항상 구조체의 마지막 멤버로 와야한다. 

또한, Flexible Array Member이외에 적어도 하나의 변수가 구조체 내에 존재해야 한다. 구조체를 복사할 때, Flexible Array Member만 있다면, 실체가 없기 때문에 복사 되지 않는다.

Flexible Array Members를 포함하는 구조체는 incomplete type이다.  incomplete type은 얼마나 메모리가 필요한지 결정할 수 없는 부분이다.

#### Q&A

- NULL 매크로는 무엇을 의미하는가?

  > NULL은 사실 0이다. 포인터에 0이 할당되면, C컴파일러는 null pointer로 취급한다. 
  >
  > 그냥 poiter 인것을 명확하게 해주기 위해 0대신 NULL을 사용하는 것이다.

- 헤더파일에 #define NULL (void *) 0 로 정의가 되어있는데, 0을 void * 로 타입캐스팅해주는 이유는 ?

  >  C컴파일러가 null pointer의 잘못된 사용을 감지할 수 있게 해준다. 
  >
  >  만약 정수형 타입의 i가 있을 때, i = NULL; 을 써주면 컴파일러가 정수형 타입에 void * 타입을 할당한다고 경고를 한다.
  >
  >  두 번째 이점은, Variable-Length Argument Lists를 가지는 함수에 NULL 아규먼트로 호출할 때, NULL이 0으로 정의 되어있으면, 컴파일러가 정수 0값으로 잘못 전달한다.  (일반적인 함수에서는 묵시적 형변환이 일어나지만, Variable-Length Argument Lists함수에서는 컴파일러가 알 수 없기 때문에 의도대로 동작하지 않음)

- NULL을 null문자 대신에 쓸 수 있나?

  > 아니오. NULL은 null pointer의 매크로이다. 서로 다르다.

