### 16. Structures, Unions, and Enumerations

- structure(구조체)는 변수(멤버)의 집합이다. 

- union은 structure와 비슷하지만, 유니온의 멤버들은 같은 저장 공간을 공유한다. 즉, 한번에 하나의 멤버만 저장할 수 있다. 동시에 모든 멤버들을 저장할 수 없다.
- enumeration은 정수형 타입으로 값들은 프로그래머가 값들에 이름을 지정해준다.

#### 16.1 Structure Variables

- 데이터의 집합을 다루는 방법 중 배열을 사용하기 위해서는2가지 조건이 있다.
  1. 모든 데이터가 같은 타입
  2. 하나의 element에 접근하기 위해서는 위치를 알아야 한다.

- structure는 배열과 차이가 있다. 구조체의 변수(멤버)는 타입이 달라도 되고, 이름으로 접근하기 때문에 위치를 기억하지 않아도 된다.

- **structure 변수 선언**

  ```c
  struct { 
  	int number;
      char name[NAME_LEN+1];
      int on_hand;
  } part1, part2  ;
  ```

  part1, part2는 각각 struct 타입의 변수이다. 각각의 struct 변수는 3개의 멤버를 가지고 각각의 멤버는 이름을 가지고 있다. 

  만약 part1이 2000번 주소에 있다면 아래 처럼 순서대로 메모리에 저장된다.

  <img src="https://i.imgur.com/ZDxrpjx.png" alt="img" style="zoom: 67%;" />

- 선언시에 struct { ... } ...; 내부에서만 scope를 가지기 때문에, 한 struct에서 사용된 멤버 이름을 struct 외부에서 선언하여 사용 할 수 있다.

  ```c
  struct { 
  	int number;
      char name[NAME_LEN+1];
      int on_hand;
  } part1, part2  ;
  
  struct { 
      char name[NAME_LEN+1];
      int number;
  } emp1, emp2  ;
  
  int on_hand;
  ```

- **구조체 변수의 초기화**

  구조체 변수에 선언된 멤버 변수 순서대로 각각의 값을 {}안에 넣어 할당하면 선언과 동시에 초기화 할 수 있다.

  ```c
  struct { 
  	int number;
      char name[NAME_LEN+1];
      int on_hand;
  } part1 = {528, "Disk drive", 10}
    part2 = {914, "Printer cable", 5};
  ```

  배열과 마찬가지로, 모든 값을 초기화 하지 않을 수 있다. 그때 초기화되지 않은 멤버들에게는 0이 할당 된다.

  변수를 할당( `part1 = { i , "Disk drive", 10}`)하는 것은 원래 불가능했지만 c99부터는 가능

  **c99 **아래와 같이 멤버 이름을 이용하여 할당도 가능하다. (designated initalizer) 아래의 경우 할당하는 순서는 바뀌어도 상관 없다. 멤버의 이름은 **designator** 라고 불린다.

  ```c
  struct { 
  	int number;
      char name[NAME_LEN+1];
      int on_hand;
  } part1 = {.number = 528, .name = "Disk drive", .on_hand = 10};
  ```

- **구조체 연산**

  이름을 이용해 각 멤버에 접근 가능 (구조체 변수명.멤버 변수명)  예) `part.number`

  ```c
  printf("%d\n", part1.number);
  printf("%s\n", part1.name);
  printf("%d\n", part1.on_hand);
  
  part1.number++;
  part1.number = 258;
  
  scanf("%d", &part1.number);
  ```

  같은 타입의 구조체 끼리 할당 연산도 가능하다.

  ```c
  part2 = part1
  ```

  이렇게 하면, part1의 모든 멤버값이 part2의 멤버 같에 대입된다.

  이 특성은, 배열을 복사 할 때 편하게 사용할 수 있다.

  ```c
  struct { int a[ 10 ]; } a1, a2;
  ...
  a1 = a2;
  ```

  **할당연산을 제외하고 구조체 변수끼리 다른 연산은 불가능하다.**



#### 16.2 구조체 타입

- 동일한 구조의 스트럭쳐를 여러번 다른 곳에서 선언해야 할 경우 아래 코드처럼한다면, 코드가 복잡해지고 수정하기도 힘들다.  그리고 part1에 part2를 할당하는 일도 불가능하다. 또한, 구조체의 이름이 없기 때문에 함수를 호출 할 때 part1과 part2를 아규먼트로 사용할 수 없다.

  ```c
  struct { 
  	int number;
      char name[NAME_LEN+1];
      int on_hand;
  } part1;
  
  ...
  
  struct { 
  	int number;
      char name[NAME_LEN+1];
      int on_hand;
  } part;
  ```

  위와 같은 문제를 피하는 방법은 structure의 타입에 이름을 지정해 주는 것이다. structure 타입의 이름을 지정해주는 방법은 두가지가 있다. 한 가지 방법은 Structure Tag를 사용하는 것이고 다른 하나는 typedef를 이용하는 방법이다. 

- **Declaring a Structure Tag**

  ```c
  struct Part{
      nt number;
      char name[NAME_LEN+1];
      int on_hand;
  };
  ```

  위와 같이 structure의 타입에 part라는 이름을 붙일 수 있다. 마지막에 있는 ' ; ' 는 필수이다. 

  한번 tag를 만들면, tag이름을 이용하여 구조체 변수를 선언할 수 있다.

  ```c
  struct Part{
      nt number;
      char name[NAME_LEN+1];
      int on_hand;
  };
  ...
  struct Part part1, part2;
  Part part1, part2;  // WRONG  참고로 C++에서는 가능
  ```

  part 태그는  타입이 아니다. struct 없이는 아무의미가 없다. 

  structure 변수와 tag를 동시에 선언 할 수도 있다.

  ```c
  struct Part{
      nt number;
      char name[NAME_LEN+1];
      int on_hand;
  } part1, part2;
  ```

  

- **Defining a Structure Type**

  ```c
  typedef struct {
      nt number;
      char name[NAME_LEN+1];
      int on_hand;
  } Part;
  ...
  
  Part part1, part2;
  ```

  typedef는 struct Part를 사용할 수 없고, tag는 사용할 수 있다. 

  linked list를 사용 할 때에는 tag를 사용해야 한다.



- **Structures as Agruments and Retrun Values**

  함수 호출

  ```c
  void print_part (struct part p)
  {
       printf("part number: %d\n", p.number);
       printf("part name: %s\n", p.name);
       printf("Quantity on hand: %d\n", p.on_hand);
  }
  
  ....
  
  print_part(part1);
  ```

  리턴 값

  ```c
  struct part build_part (int numver, const char *name, int on_hand)
  {
      struct part p;
      strcpy(p.name, name);
      p.on_hand = on_hand;
      return p;
  }
  
  ...
      
  part1 = build_part(528, "Disk drive", 10);
  ```

  구조체를 리턴하거나 파라미터로 전달 할 때 모든 멤버를 copy하게 된다. 만약 구조체가 크다면, 복사를 하기 때문에 시간이 오래 걸릴 것이다. 

  모든 멤버의 값을 복사하는 것을 피하기 위해서 포인터를 활용 할 수 있다. (17.5에서 다룰 예정)

- FILE structure

  <stdio.h> 에 FILE 구조체가 포함되어 있고 FILE 구조체는 open file의 상태를 저장한다. 그렇기 때문에 FILE 구조체는 unique 해야한다. 그리고, open file에서 수행되는 모든 함수는 FILE구조체를 포인터 타입의 아규먼트가 사용되어 호출 된다.

- ```c
  void f(struct part part1)
  {
      struct part part2 = part1;
      ...
  }
  ```

  함수안에 위와 같이 초기화도 가능하다.

- C99 Compound Literals

  1. 함수르르 호출 할 때 아규먼트로 사용 가능

  ```c
  print_part((struct part) {528, "Disk drive", 10});
  
  //또는
  
  print_part((struct part) {.on_hand = 10,
                            .name = "Disk drive", 
                            .number = 528});
  ```
  
  2. 구조체를 초기화 할 때
  
  ```c
  part1 = (struct part) {528, "Disk drive", 10};
  ```
  
  위의 예제는 tag를 사용 했을 경우이다.
  
  만약, typedef를 사용했다면 struct를 사용하지 않고 (part)를 이용해서 사용할 수 있다.

  

#### 16.3 Nested Arrays and Structures

- 배열과 구조체는 서로 제한없이 결합하여 사용할 수 있다. 배열을 구조체의 멤버로 가지는 것과 구조체를 배열로 사용하는 것 모두 가능하다.

- **Nested Structure**

  ```c
  struct person_name {
      char first [First_NAME_LEN+1];
      char middle_initial;
      char last [LAST_NAME_LEN+1];
  };
  
  ...
  
  //구조체 내부에 구조체를 멤버로 가질 수 있다.
  struct student {
      struct person_name name;
      int id, age;
      char sex;
  } student1, student2;
  
  ...
  
  strcpy (student1.name.first, "Fred");
  
  //구조체를 내부 멤버로 가질 때 유용한 점은, 구조체를 한번에 처리 할 수 있다는 것이다. 예를 들면 함수를 호출 할 경우 아래와 같이 표현 가능하다.
  diplay_name(student1.name);
  
  //비슷하게 할당도 가능하다.
  struct person_name new_name;
  ...
  student1.name = new_name;
  ```

- **Arrays of Structures**

  ```c
  //구조체 배열 선언
  struct part inventory[100];
  ...
  //함수의 아규먼트로 사용
  print_part(inventory[i]);
  //멤버 연산
  inventory[i].number=883;
  ```

- **Initializing an Array of Structures**

  ```c
  struct dialing code{
      char *country;
      int code;
  };
  
  const struct dialing_code country_codes[]=
  { {"Korea", 82}, {"Germany", 49}, {"China", 55} };
  ```

  만약 const가 아닌 변수로 사용했다면, country가  포인터이기 때문에 문제가 된다.  내부 {} 는 생략해도 되지만, {}을 넣어주면 가독성이 좋다. 

-  **C99**

  ```c
  struct part inventory[100] = 
  {[0].number = 528, [0].on_hand = 10, [0].name[0]='\0'};
  ```

#### 16.4 Unions

- 구조체와 비슷하지만,  모든 멤버가 메모리에 동시에 저장될 수 없다. union의 메모리 크기는 멤버 중 가장 사이즈가 큰 멤버의 크기로 정해진다.

-  한 번에 하나의 멤버 값만 저장되기 때문에, 다른 멤버에 값을 할당하면 원래 있던 멤버의 값이 사라지고 그 위에 값을 덮어 씌운다.

- ```c
  union {
      int i ;
      double d;
  } u;
  ```

  구조체와 비슷하지만 데이터가 저장되는 형태에서 차이가 있다. 

  <img src="https://i.imgur.com/6few24H.png" alt="img" style="zoom: 80%;" />

  구조체는 12바이트, 유니온은 8바이트

  구조체는 각각의 멤버가 고유한 공간을 가지고, 유니온은 공유

  구조체는 각각의 멤버가 고유의 주소값을 가지고, 유니온은 주소가 같다

  ```c
  u.i=82;
  u.d = 74.8
  ```

  아래와 같이 초기화도 가능하다. 

  ```c
  //첫 번째 멤버인 i가 초기화 된다.
  union {
      int i;
      double d;
  } u = { 0 };
  
  // c99부터는 다른 멤버를 designated initializer를 사용하여 초기화 할 수 있다.
  union {
      int i;
      double d;
  } u = { .d =10.1 };
  ```

- **Using unions to save space**

  동시에 사용할 필요가 없는 변수들이 있을 경우

  ```c
  struct catalog_item {
      
      int stock_number;
      double price;
      int item_type;
      union {
          struct {
              char title [ TITLE_LEN + 1];
              char title [ TITLE_LEN + 1];
              int num_page;
          } book;
          struct {
              char design [DESIGN_LEN+1];
          } mug;
          struct {
              char design [DESIGN_LEN+1];
              int size;
              int colors;
          } shirts;
      } item;
  };
  
  struct catalog_item c;
      
  ...
  
  printf("%s", c.item.book.title);
  ```

   유니온 멤버에 값을 할 당 할 경우, 다른 멤버를 통해서 그 값에 접근 하는 것도 가능하다.

  ```c
  strcpy(c.item.mug.design, "Cats");
  
  printf("%s", c.item.shirts.design);  // 결과가 Cats이 나옴
  ```

  위 경우, mug멤버의 design에 cats을 할당했지만, shirts의 design과 메모리를 공유하고 같은 사이즈 이기 때문에 shirt.design을 출력하면 cats이 나온다.



- **Build Mixed Data Structures**

  ```c
  typedef union {
      int i;
      double d;
  } Number;
  
  Number number_arry[1000];
  
  number_arry[0].i=5;
  number_arry[1].i=8.395;
  ```

  위와 같이 각각 원소의 타입이 다른 배열로 활용 가능하다.

  아래와 같이 각각의 원소 타입에 따라 원소를 처리할 수 있다.

  ```c
  //불가능한 방법
  typedef union {
      int i;
      double d;
  } Number;
  
  void print_number (Number n){
      if (n contains an integer)  // 구분할 방법이 없다.
          printf("&d", n.i);
      else
          printf("%g", n.d);
  }
  
  // tag field 사용
  #define INT_KIND 0
  #define DOUBLE_KIND 1
  
  typedef struct {
      int kind;   /*tag field*/
      union {s
          int i;
          double d;
      } u;
  }Number;
  
  void print_number (Number n){
      if (n.kind == INT_KIND )
          printf("&d", n.u.i);
      else
          printf("%g", n.u.d);
  }
  ```

  

#### 16.5 Enumerations

- 의미 있는 작은 집합의 값들을 표현할 때 Enumeration이 유용하다.

  - ture / false
  - clubs / diamonds / hearts / spades

- 매크로를 이용할 수 있지만, 매크로는 같은 타입인지를 나타내지 않는다. 

- 또한, 매크로는 값으로 대치되기 때문에 디버깅시에  불편하다

- enum

  ```c
  enum {CLUBS, DIAMONDS, HEARTS, SPADES} s1,s2;
  ```

  각 멤버는 상수이고 s1,s2는 변수이다. enum은 함수내부에서 정의 되었으면, 그함수 내부에서만 사용 할 수 있다.

- enum tag

  ```c
  enum suit {CLUBS, DIAMONDS, HEARTS, SPADES};
  enum suit s1, s2;
  ```

- typedef

  ```c
  typedef enum {CLUBS, DIAMONDS, HEARTS, SPADES} Suit;
  Suit s1, s2;
  ```

- enum 멤버는 정수로 처리된다. 기본적으로, C컴파일러는 0부터 순차적으로 각 멤버에 값을 할당한다. (0, 1, 2, 3, ...)

  suit는 CLUBS == 0, DIAMONDS ==1, HEARTS ==2, SPADES ==3 으로 매칭된다.

- ```c
  enum suit {CLUBS=1, DIAMONDS=2, HEARTS=3, SPADES=4};
  enum dept {RESEARCH =20, PRODUCTION = 10, SALES =25};
  ```

  위와 같이 값을 직접 할당하는 것도 가능하다. 

- 두 개 이상의 멤버에 같은 값을 할당하는 것도 가능

- 값을 지정해 주지 않을 경우, 직전 멤버의 값보다 1큰 값이 할당 된다.

  ```C
  enum color {BLACK, GRAY = 7, RED, WHITE = 15};
  //각각 0, 7, 8, 15
  ```

- 활용

  ```c
  int i ;
  enum {CLUBS, DIAMONDS, HEARTS, SPADES} s;
  
  i = DIAMONDS;  //i는 1
  s = 0;    // s는 0(CLUBS)
  s++;      // s는 1(DIAMONDS)
  i = s+2;  // i는 3
  ```

  enumeration value를 쓰는 것이 편리하긴 하지만, 위험한 경우가 있다. 만약에 s=4; 라고 할 경우 s안에 4에 매칭되는 값이 없다.

- ```c
  typedef struct{
      enum {InT_KIN, DOUBLE_KIND} kind;
      union {
          int i;
          double d;
      } u;
  } Number ;
  ```

  매크로를 사용하지 않고, 타입이 다른 배열을 사용 하는 방법, 이 방법이 매크로보다 더 명확하게 타입을 보여준다.

#### Q&A

- sizeof 함수를 썼을 때 멤버의 전체 사이즈를 더한 것보다 큰 결과가 나오는 이유

  - ```c
    struct {
    	char a;
    	int b;
    }s;
    ```

    위 코드의 경우 s는 5바이트가 나와야 하지만, 몇몇 컴퓨터는 특정 숫자의 배수의 간격만큼 주소를 가진다. 이러한 요구를 만족시키기 위해 컴파일러는 구조체의 멤버들을 정렬하는데, 이때 두 멤버사이에 사용되지 않는 바이트가 존재 할 수 있다.

    만약, 데이터가 4의 배수로 시작해야 된다면, 위의 코드는 3개의 사용되지 않는 바이트를 가졌기 때문에 총 8바이트가 된다.

- 구조체를 다른 파일에서 공유하려면, 헤더파일에 구조체를 선언하고 다른파일에서 include하면 된다.
- 구조체는 ==로 왜 비교 못하는가
  - hole이 있기 때문에

- compound literal도 포인터를 가질 수 있나요?
  - 네 `print_part(&(struct part) {528, "Disk", 10});`

- enum 값들은 subscript로 사용될 수 있다. 
