https://www.notion.so/sohyun/CH13_Strings-75b5f361d8444f2aa203901e60f07e08?v=03750e073b154bb084350420d821e6b8

# CH13_Strings

Created: Dec 02, 2019 9:37 AM

### Table of Contents

- 13.1 - rules that govern string literals
- 13.2 - declaring single variables
- 13.3 - ways to read and write strings
- 13.4 - how to write functions that process strings
- 13.5 - string-handling functions in the C library
- 13.6 - idioms for working with strings
- 13.7 - setting up arrays whose elements are pointers to strings of different lengths

# 13.1 String Literals 문자 리터럴

*string literals* (문자 리터럴)이란 "큰 따옴표 안에 들어가있는 문자들"을 의미한다.

    "이것은 문자 리터럴입니다."
    "This is what string literals look like."

### Escape Sequences in String Literals 스트링 리터럴 속 이스케이프 시퀀스

Escape sequence는 `\`(역슬래시)뒤에 캐릭터를 붙여서 개행이나 여백 등 특정한 형태의 모양을 출력해준다. `\` 뒤에 8진수, 16진수의 코드가 와도 무방하나 잘 사용하지는 않는다.

**주의점**

`\` 뒤에 8진수가 오는 경우 최대 세자리까지만 입력할 수 있다. 

16진수 코드는 16진수 문자가 아닌 것을 만나면 코드가 끝인 것을 인식한다.

    "\1234" // \123에 문자 4가 붙은 형태로 이해하면 된다.
    "\xfcber" // \xfc에서 보통은 끊어주겠지만 c나 b도 16진수라 컴파일러 따라 다르게 나올 수 있는 점 주의
    // "\xfc""ber"라고 표기하면 된다고 한다.

### Counting a String Literal 스트링 리터럴 카운팅

`\` 를 이용해서 코드 작성 단계에서 줄바꿈을 표시해줄 수 있다 (실제 출력창 개행이 아님). 단 역슬래시 뒤에 다른 문자가 오면 안 되고 해당 줄의 마지막 문자여야 한다. 

    printf("Hello world. \
    This is a new line but not printed as a new line.");

역슬래시를 활용한 코드 작성 단계의 개행은 꼭 String에만 한정된 것은 아니다. 다만 String의 경우 편의를 위해 두 개의 string literal이 나란히 적혀있다면, 중간에 공백은 무시하고 그 둘을 하나로 이어준다. 그렇기에 아래와 같이 작성해도 무방하다.

    printf("Hello world."
    			 "This is a new line but not printed as a new line.");

### How String Literals Are Stored 스트링 리터럴이 저장되는 방식

`String`은 단순히 요약하자면 `character`의 배열이다. 

C 컴파일러가 `n`의 길이를 갖는 스트링 리터럴을 만나면 메모리에 `n+1` 바이트의 공간을 확보한다. +1의 공간은 `null character`가 담기는 공간으로 스트링 리터럴의 끝을 의미한다. 

`null character`는 비트가 모두 0인 바이트로 `\0`으로 표현되기도 한다. 

**주의점**

`null character` `\0` 와 `0`은 다른 것을 기억하자. null character의 코드는 0이지만 '0'은 아스키 기준 48번이다. 

**예시**

    "abc"

![CH13_Strings/Untitled.png](CH13_Strings/Untitled.png)

    ""

![CH13_Strings/Untitled%201.png](CH13_Strings/Untitled%201.png)

**결국 스트링 리터럴은 `char` type의 배열에 들어가있기 때문에 컴파일러는 스트링 리터럴을 `char`타입의 포인터로 취급한다.** 

    printf("abc");

→ printf 함수가 호출되면 "abc"의 주소를 넘겨주는 것이다 (문자 'a'가 담겨있는 공간을 가리키는 포인터)

### Operations on String Literals

    char *p;
    
    p = "abc";
    // p에 문자 a, b, c가 복사되어 들어가는 것이 아니라 a, b, c가 담긴 배열을 가리키는 포인터
    
    char ch;
    ch = "abc"[1]; // ch는 'b'라는 char타입의 값이 담긴다.

**주의점**

스트링 리터럴은 포인터지만 간접 참조로 값을 바꾸려고 하면 오류가 발생한다.

    char *p = "abc";
    
    *p = 'd';  // 오류

### String Literals versus Character Constants 문자 리터럴 vs 문자형 상수

"a"라는 한 개짜리 문자가 담겨있는 문자 리터럴과 'a'라는 character가 담긴 문자형 상수는 엄연히 다른 개념이다. 

전자는 character 'a'가 담겨있는 메모리 위치를 가리키는 포인터의 개념이고,

후자는 'a'에 해당하는 정수값이 담긴 변수의 개념이다

**주의점**

스트링과 문자를 혼용해서 쓰지 않도록 하자. 

    printf("\n"); // printf는 포인터를 인자로 받기 때문에 올바른 문법
    printf('\n'); // 잘못된 사용법. 문자형은 포인터가 아니기 때문.

# 13.2 String Variables 스트링 변수

스트링은 null character를 마지막 인자로 가지는 character의 배열이다. 만약 char의 배열을 만들어놓고 여기에 string을 담고 싶다면  null character를 빼먹지 않도록 조심해야 한다. 

    #define  STR_LEN 80
    ...
    char str[STR_LEN+1]; 

→ 길이 80의 char 배열을 생성하는 코드. 단, +1은 마지막 null character위해 작성하는 것으로 스트링을 담을 char의 배열은 항상 길이보다 +1로 생성해주는 것이 좋다.

→ 길이가 81이라고 해서 무조건 해당 배열이 81의 길이를 갖는 것은 아니다. 스트링의 길이는 어디까지나 null character를 기준으로 판단되기 때문이다.

### **Initializing a String Variable 스트링 변수 초기화하기**

    char date1[8] = "June 14";

![CH13_Strings/Untitled%202.png](CH13_Strings/Untitled%202.png)

→ "June 14" 자체가 문자 리터럴이라기보다, C의 입장에선 배열 선언을 위한 축약된 문구로 인식된다고 이해하면 편하다. 위의 코드는 다음과도 같기 때문이다.

    char date1[8] = {'J', 'u', 'n', 'e', ' ', '1', '4' , '\0'};

만약 배열의 길이가 문자 리터럴보다 길다면 남는 공간은 `\0` 으로 채워준다. 

**주의점** 배열의 길이가 짧은 경우, 채울 수 있는 데까지만 채우고 뒤에 와야할 `\0`을 누락해버리니 주의!

문자 리터럴은 길이를 늘 파악하기가 힘들기 때문에 편의를 위해 배열의 길이를 비워두고 선언할 수도 있다. 

    char date4[] = "June 14";
    // 컴파일러가 알아서 8칸을 생성하고 값을 넣어준다.
    // 단, 길이가 가변적인 것이 아니라 생성 시에 알아서 길이를 고정시키는 것 뿐.

### Character Arrays vs Character Pointers

문자 리터럴을 `char`배열을 선언해서 넣어주는 것과, 포인터 변수를 선언하여 대입해주는 것은 차이가 있다.

    char date[] = "June 14"; // date는 배열
    // vs
    char *date = "June 14"; // date는 포인터

단순히 `char`배열의 포인터를 넘겨주는 상황이라면 두 형식 모두 `date`를 넘겨주면 된다. 

그러나 두 선언이 같은 의미인 것은 아니다.

**`date[]` 선언**

- date 배열 안에 담긴 값들이 수정될 수 있다.
- date는 배열의 이름이다.

**`*date` 선언**

- 13.1에서 본 것처럼 포인터 변수로 선언된 문자 리터럴은 수정할 수 없다. 아래는 위에서 보았던 예시이다.

        char *p = "abc";
        *p = 'd'; // 오류

- 포인터의 경우 나중에 다른 strings를 가리키게 할 수도 있다.

    char *p; // 이렇게만 하면 string을 가리키는 포인터로 사용 불가
    
    p[0] = 'a'; // 오류. p가 어딜 가리키고 있는지 알 수가 없기 때문.
    
    char str[STR_LEN+1], *p; // STR_LEN+1길이의 문자배열 및 포인터 변수 p 선언
    p=str; // 포인터 p는 str배열의 첫번째를 가리키게 된다.

→ 이렇게 선언한다면 p를 스트링처럼 사용할 수 있게 된다.

# 13.3 Reading and Writing Strings 스트링 읽기와 입력

### Writing Strings Using `printf`and `puts`

    char str[] = "Are we having fun yet?";
    printf("%s\n", str);
    
    // Are we having fun yet? 이 출력된다.

→ `printf`의 원리는 `string`에 담긴 문자를 하나하나 출력하다가 null character를 만나면 멈추는 것이다. (만약 null character가 없다면 메모리를 계속 뒤져가며 null character가 언젠가 나오면 멈춘다고 한다)

부분만 출력하고 싶다면 아래처럼 쓸 수 있다.

    pritnf("%.6s\n", str);
    
    // Are we 라고 출력된다. %.ps에서 p가 출력할 문자의 개수를 의미한다. 

    char str[] = "AAAAAA";
    printf("%10.3s\n", str);
    
    //"       AAA" 형태로 터미널에 출력된다.

`printf` 말고도 문자열을 출력할 수 있는 함수로는 `puts`가 있다. 

    char str[] = "AAAAAA";
    puts(str);
    puts(str);
    
    // AAAAAA
    	 AAAAAA
    	 형태로 출력된다.

### Reading Strings Using `scanf` and `gets`

    char str[10];
    scanf("%s", str);
    // str은 이미 배열이기에 저 자체로 포인터라 &가 필요하지 않다.
    // 저 한 줄만 실행하면 입력할 때마다 str[0]의 값으로 넣게 된다.

`string`을 읽어오는 경우(문자를 읽어오는 것과는 다르다. 문자는 `%c`로 읽어오고 스트링은 `%s`로 읽어오며 함수 내에서 취급 방법이 다르다)

**공통점**

`scanf`와 `gets`함수 모두 마지막에 null character를 저장한다. 

**차이점**

**`scanf`**

- white space를 읽지 않는다. 즉, 개행이나 여백을 만나면 그 뒤는 읽어오지 않고 멈춘다.

`gets`

- white space를 포함해서 읽는다 (스킵하지 않는다).
- 개행 기호를 만나면 멈춘다. (개행 기호 자리에 null character를 대체시킨다)

    char i;
    scanf("%c", &i);
    // 문자를 입력받는 경우 개행기호를 입력 받는다.
    // i에는 개행기호를 의미하는 문자가 들어가고, 실제 출력하면 개행이 된다.

    // "Hello My name is Grace"를 입력하면
    
    char str[10];
    scanf("%s", str);
    printf("%s", str);
    // "Hello"만 출력한다. 첫 공백 뒤가 담기지 않은 것.
    
    gets(str);
    printf("%s", str);
    // "Hello My name is Grace"를 출력한다.

***cf) white space란?***

컴퓨터에서 콘솔이나 프린터로 찍었을 때 공백을 표현하는 문자들을 의미한다. (Alternatively referred to as spacing or white space, white space is any section of a document that is unused or space around an object.)

    ' '      space 
    '\t'     horizontal tab 
    '\n'     newline
    '\v'     vertical tab 
    '\f'     feed 
    '\r'     carriage return

특정 기호가 화이트 스페이스인지 아닌지 확인하고자 한다면 `ctype.h` 라이브러리의 `isspace` 함수를 활용하는 방법이 있다. 

**주의점**

`scanf`도 `gets`도 입력받은 string이 배열의 길이를 초과하는지 자동으로 파악할 수는 없다. 

`scanf`의 경우는 입력받을 때 길이를 정해두기 위해 `%ns`라고 적어서 n개의 문자만을 저장하게 강제할 수 있다. 

gets는 이런 안정 장치가 없기 때문에 `fgets`라는 것을 사용하는 걸 추천하고 있다. 

### Reading Strings Character by Character

`scanf`와 `gets`는 위험 요소가 있기 때문에 개발자들은 종종 input을 위한 함수를 직접 만들기도 한다. 이런 함수를 만들 때 고려해봄직한 부분들은 아래와 같다. 

- 화이트스페이스를 스킵할 것인가?
- 읽어오는 동작이 어떤 문자를 만나면 정지할 것인가? 또 이 문자를 스트링에 저장할 것인가 말 것인가?
- 입력받은 내용이 너무 길면 어떻게 처리할 것인가? 남은 문자는 버릴 것인가 아니면 다음에 입력받도록 남겨둘 것인가?

책에서는 아래의 예시를 코드로 나타내는 법을 설명해준다:

화이트스페이스를 무시하지 않고, 개행 기호를 만나면 입력받는 것을 정지하며, 읽지 않은 나머지 문자들은 버리는 함수.

    // prototype 선언
    int read_line(char str[], int n);
    
    // 메인 함수
    int main(void){
      char a[10];
    
      read_line(a, 10); // abcdefghijklmnop 를 입력하면
      printf("%s", a); // abcdefghij 까지만 출력된다.
    }
    
    // 본 함수
    int read_line(char str[], int n) {
    	int ch, i = 0;
    
    	while ((ch = getchar()) != '\n') {
    		if (i < n) {
    			str[i++] = ch;
    		}
    	}
    
    	str[i] = '\0';  /* 스트링 종료 */
    	return i;  /* 담긴 문자의 개수 */
    }

***cf) `getchar` 함수***

- 단 하나의 문자를 입력받는 함수
- 함수를 사용할 때 인자를 따로 넘겨주지 않아도 된다.
- 입력받은 문자를 int로 반환해준다.

    char a;
    a=getchar(); //asdfj라고 입력해도 a만 넣어진다.

![CH13_Strings/Untitled%203.png](CH13_Strings/Untitled%203.png)

# 13.4 Accessing the Characters in a String 스트링 안의 문자 접근하기

문자열 또한 배열의 형태를 띄고 있기에 subscription으로 접근이 가능하다.

문자열 안에 공백의 개수를 알고 싶다면?

    int count_spaces(const char s[]) {
    // const를 표기하여 배열이 변경되지 않는 걸 명시
    // s는 문자열이라 컴파일러가 길이를 알기에 길이를 인자로 받을 필요가 없다.
    	int count = 0, i;
    
    	for (i = 0; s[i] != '\0'; i++) {
    		if (s[i] == ' ') {
    			count++;
    		}
    	}
    
    	return count;
    }

위의 코드는 포인터 연산을 활용하여 변수 `i`를 제거하고 다시 작성할 수 있다.

    int count_spaces(const char *s) {
    	int count = 0;
    
    	for (; s != '\0'; s++) {
    		if (*s == ' ') {
    			count++;
    		}
    	}
    
    	return count;
    }

→ 여기서 `const`가 붙어있는 이유는 함수가 포인터`s`가 담는 주소를 변경시키지 못하게 하기 위함이 아니라, 포인터`s`가 가리키는 값을 변경하지 못하도록 적어준 부분이다. (아래와 동일. 참고) 함수 안의 `s`는 그저 인자로 받은 포인터가 복사된 것에 불과하기 때문에 얼마든지 변경 가능.

    int f(const int *p){
    	*p = 0;  // 불가
    }

위의 두 가지 예시 중 어느 것이 더 효율적이거나 올바른 건 아니기에 둘 다 사용해도 무방하다. 단 전통적인 C 프로그래밍에선 배열은 포인터로 접근하는 편이라고 설명하고 있다. 

파라미터에 `s[]`, `*s` 둘 중 어떤 형태이든 호출 시 배열 이름, 포인터, 문자 리터럴 모두 인자로 적을 수 있다. 모두 일종의 포인터 개념이라 차이가 없기 때문.

# 13.5 Using the C String Library

문자열은 연산자를 활용해 비교가 불가하다. 배열의 이름은 lvalue가 될 수 없기 때문. 

    char str1[10], str2[10];
    
    str1 = "abc"; // 불가
    str2 = str1; // 불가

단, 선언 `=`를 사용할 수 있다. 이런 경우, `=`가 대입연산자는 아니라고 한다. 

    char str1[10] = "abc";

관계 연산자로 문자열을 비교할 수는 있으나, 두 배열의 내용을 비교하는 것이 아니라 포인터로서의 비교만 실행된다.

    if (str1 == str2) ... // 결과는 0이다. 둘은 다른 포인터이다.

문자열은 여러가지로 까다롭기 때문에 C에선 `string.h` 라이브러리를 제공해주고 있다. 

### `<string.h>` 라이브러리

    #inclue <string.h>

string 라이브러리 안의 대부분의 함수는 보통 인자로 하나 이상의 문자열을 받도록 되어있다. 문자열은 보통 `char *` 형태로 선언되기 때문에 문자배열이든, `char *` 타입의 변수든 문자 리터럴이든 관계는 없다. 

**✔`strcpy` 함수**

문자열s2를 s1에 복사하는 함수. (정확히는 s2가 가리키는 배열을 s1이 가리키는 배열에 복사하는 것) s2의 첫번째 null character가 있는 지점까지 복사하게 된다 (null character도 복사된다)

    char *strcpy(char *s1, const char *s2);

→ s1을 return한다. (s1이 가리키는 내용)

→ s2가 가리키는 배열은 변형되지 않기 때문에 const를 붙여준다.

배열에는 대입 연산자가 불가하기 때문에 이미 선언된 배열, 혹은 문자열의 내용을 변경하고 싶을 때 사용하면 좋은 함수

    char str1[10];
    char str2[5]="Hello";
    
    strcpy(str1, str2);
    strcpy(str1, "abcd");

리턴값이 s1인 걸 활용해서 중첩해서도 활용이 가능하다.

    strcpy(str1, strcpy(str2, "abcd"));
    // str1, str2 모두 "abcd"로 변경된다. 
    // strcpy(str2, "abcd")의 리턴값이 "abcd"이기 때문.

**주의점**

`strcpy`함수는 두 배열의 길이를 자동으로 파악하지 못한다. 복사되는 값이 배열의 길이를 초과하면 알 수 없는 오류가 일어날 수 있다. 

**✔`strncpy`함수**

`strcpy`와 비슷하나 복사할 문자의 개수를 한정하는 인자가 추가된다.

    strncpy(str1, str2, sizeof(str1));
    // sizeof를 이용해서 str1의 길이가 허용하는 만큼만 복사.
    // char 하나의 크기는 sizeof함수를 쓰면 1이다. 1byte이기 때문

길이를 지정해주었다고 해서 마지막 문자로 null character를 복사해주는 것은 아니기 때문에 조금 더 안전하게 복사하자면 아래와 같이 쓸 수 있다.

    strncpy(str1, str2, sizeof(str1));
    str1[sizeof(str1)-1] = '\0';

**✔`strlen`(String Length) 함수**

문자열 `s`의 길이를 반환한다. null character는 길이에 포함하지 않는다. (`sizeof`로 접근하면 null character를 포함한다)

    size_t strlen(const char *s);
    // size_t는 unsigned integer를 나타내는 typedef이다.
    // 리턴되는 값은 결국 정수라고 생각하면 된다.
    
    int len;
    len = strlen("abc");  // len 은 3이다.
    len = strlen("");  // len은 0이다.

**✔`strcat`(String Concatenation) 함수**

`s2` 문자열을 `s1` 문자열 뒤에 이어준다. 호출하면 이어붙인 s1이 return된다.

    char *strcat(char *s1, const char *s2);

    strcpy(str1, "abc");
    strcat(str1, "def");  // str1은 "abcdef"

그러나 `strcat`함수 또한 s1의 길이를 초과하여 이어붙이게 되면 오류가 된다.

**✔`strncat` 함수**

strncpy와 마찬가지로 길이와 관계된 인자가 추가된 함수. 복사할 문자의 개수를 지정해준다.

    strncat(str1, str2, sizeof(str1)-strlen(str1)-1);
    // str1의 길이에서 str1이 담고있는 문자열의 길이를 빼고
    // null character공간을 위해 1을 빼주면 남은 공간이 나온다.
    // 해당 공간의 크기를 세번재로 인자로 넘겨주면 넘치지 않게 이어붙여준다.

**✔`strcmp` (String Comparison) 함수**

문자열간의 비교를 가능하게 해주는 함수. 관계연산자(`<, ≤, <, ≤`)와 동등연산자(`==, !=`)를 활용하여 비교를 할 수 있도록 해준다. 

    // prototype 선언
    int strcmp(const char *s1, const char *s2);
    
    // str1 < str2?
    if (strcmp(str1, str2) < 0)
    
    // str1 <= str2?
    if (strcmp(str1, str2) <= 0)

**문자열을 비교하는 기준이란?**

두 문자열을 앞에서부터 비교해나가면서 같은 위치에 동일한 값이 아닌 경우, 아스키코드상 작은 값이 작다고 판단된다. 내용이 겹치고 길이만 다르다면 긴 것이 더 크다.

    "abd" < "abe"
    "abc" < "abcd"

**ASCII 관련**

- 순서가 있는 문자들은 모두 순차적인 코드를 갖는다.  (A~Z, a~z 등등)
- 대문자는 소문자보다 작다 (코드에서 앞에 있기 때문)
- 숫자는 문자들보다 앞이다.
- space는 모든 문자들보다 앞선다 (아스키코드 32번)

### 예제 - remind.c 만들기

- 주석 포함된 코드

        #include <stdio.h>
        #include <string.h>
        
        #define MAX_REMIND 50 /* maximum number of reminders */
        #define MSG_LEN 60  /* max length of remnder message*/
        
        int read_line(char str[], int n);
        
        int main(void){
            char reminders[MAX_REMIND][MSG_LEN+1];
            char day_str[3], msg_str[MSG_LEN+1]; //day_str은 날짜가 담긴 문자열
            int day, i, j, num_remind=0;
        
            for(;;){
                if(num_remind==MAX_REMIND){
                    printf("-- No space left --\n");
                    break;
                }
        
                printf("Enter day and reminder: ");
                scanf("%2d", &day);
                // 이러고나면 앞에 숫자 2개는 read_line에서 접근하지 못하게 사라짐
                // 2d 니까 공간 두 개 확보라 숫자가 하나라도 두 칸 형태로 들어가는 모양
                if(day==0){
                    break;
                }
        
                sprintf(day_str, "%2d", day);
                read_line(msg_str, MSG_LEN);
        
                for(i=0; i<num_remind; i++){
                    // day_str의 내용이 reminders의 i행의 내용보다 작다면?
                    // 스트링의 길이가 길다고 무조건 큰 게 아니라 앞의 두 숫자끼리만 비교하는 셈
                    if(strcmp(day_str, reminders[i])<0){
                        break;
                    }
                }
                for(j=num_remind; j > i; j--){
                    strcpy(reminders[j], reminders[j-1]);
                }
        
                strcpy(reminders[i], day_str);
                strcat(reminders[i], msg_str);
                // 숫자가 사라진 상태라 뒤에 일정은 여백 한칸으로 시작한다.
        
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

# 13.6 String Idioms

`strlen`과 `strcat` 재구현해보자. 단, str 관련 함수들은 이름이 이미 정해진 게 많으니 `my_strlen` 아니면 아예 다른 이름으로 지정하는 게 좋음.

### Searching for the End of a String

`strlen` 함수를 풀어 써보면 아래와 같다.

    size_t strlen(const char *s){
       size_t n;
    
       for (n=0; *s != '0'; s++){
          n++;
       }
    
       return n;
    }

조금 더 속도를 위해 함수를 변경해본다면 아래와 같다.

    size_t strlen(const char *s){
       size_t n=0;
    
       while(*s++){
          n++;
       }
    
       return 0;
    }

포인터-포인터=길이 개념을 이용해서 아래와 같은 식으로 속도를 올리는 코드를 작성해볼 수도 있다.

    size_t strlen(const char *s){
       const char *p = s;
    
       while (*s){
          s++;
       }
     
       return s-p; // 위치를 이동한 만큼을 길이로 뱉어준다.
    }

→ 해당 형태가 속도가 빨라질 수 있는 건 `while`문 안에서 `n`을 증가시켜주지 않아도 되기 때문이다. 

    while(*s){
      s++;
    }
    // s가 null character가 있는 위치를 가리키게 된다. 
    // 아래처럼도 쓸 수 있다.
    // 단, 아래 형식은 null character 다음 공간을 가리키게 된다.
    
    while(*s++){
      ;
    }

위의 표현은 "문자열 끝의 null character를 찾기" 의 idiom이다. 

### Copying a String

S1에 s2를 이어주는 함수 `strcat`은 아래와 같이 풀어 쓸 수 있다. 

    char *strcat(char *s1, const char *s2){
      char *p = s1;
    
      while(*p != '\0'){
        p++;
      }
      while(*s2 != '\0'){
        *p = *s2;
        p++;
        s2++;
      }
     
      *p = '\0';
      return s1;
    }

위의 예시처럼 해당 코드도 포인터 개념과 while문의 특성을 활용하여 조금 더 단축시킬 수 있다. 

    char *strcat(char *s1, const char *s2){
      char *p = s1;
      
      while(*p){
        p++;
      }
      while(*p++ = *s2++){ // while문의 조건문에 대입이 발생한다면 대입되는 값을 넘겨준다고 보면 된다.
      // *s2가 0이라도 대입은 먼저 발생하고 그 뒤에 while문에 0을 대입하기 때문에 null character도 자동으로 복사가 된다.
      // 컴파일러따라 오류가 날 수도 있는데 그 때는 while((*p++ = *s2++) != 0) 혹은 while((*p++ = *s2++)) 로 적어줄 수 있다고 한다. 
        ;
      }
    
      return s1;
    }

# 13.7 Array of Strings

    char planets[][8] = {"Mercury", "Venus",
                         "Mars", "Jupiter"};
    
    // 문자열의 배열은 배열을 배열 안에 넣어두는 형태이기 때문에 2차원 배열로 정의한다.
    // 행의 개수는 자동으로 들어간 문자열의 개수가 되기 때문에 굳이 명시하지 않아도 무방하다.
    // 단 최대 길이 때문에 열의 개수를 명시해야 한다.

위의 형태로 문자열의 배열을 만든다면 직관이지만 모든 문자열의 길이가 8로 고정된다는 단점이 있다. 이렇게 되면 "Mars" 같이 짧은 문자열은 여백이 많이 남아 메모리를 비효율적으로 사용하게 된다. 이 때 적용할 수 있는 것이 포인터 배열이다.

    char *planets[] = {"Mercury", "Venus",
                         "Mars", "Jupiter"};
    
    // 배열의 칸마다 각각의 문자열이 들어있는 배열을 가리키는 포인터가 담기게 된다.
    
    planets[0][0]=='M' // true
    planets[1][2]=='n' // true
    
    // M으로 시작하는 문자열만을 출력하려면?
    for(i=0; i<4; i++){
      if(planets[i][0]=='M'){ // "M" 이 아니라 'M'
        printf("%s begins with M\n", planets[i]);
      }
    }

### Command-Line Arguments

C의 main 함수에 파라미터를 표기하여 실제 컴파일 단계에서 커맨드 창을 통해 값을 받아와 활용할 수 있다.

    int main(int argc, char *argv[])
    {
      ...
    }
    
    // argc는 커맨드 창에 입력할 문자열 개수 (프로그램명 포함), 즉 argv 배열의 길이-1
    // argv[]는 입력한 문자열들을 가리키는 포인터 배열
    // argv[0]은 프로그램명을 가리키는 포인터, argv[1] ~ argv[argc-1]는 나머지 입력값, argv[argc]는 null pointer
    
    // argc, argv라는 작명은 convention(관례)일뿐, 원한다면 다른 걸로 적어도 작동한다. 

입력받은 내용들을 출력하고 싶다면 아래처럼 접근할 수 있다.

    char **p;
    
    for(p=&argv[1]; *p != NULL; p++){
      printf("%s\n", *p);
    }
    
    // argv[1]은 풀어보면 argv배열의 2번째 인자인 문자열의 첫번째 인자, 즉 첫번째 문자를 가리킨다. 입력창에 프로그램명 뒤 Jupiter라고 입력했다면 'J'를 가리킨다.
    // &argv[1]는 위에서 설명한 포인터를 가리키는 포인터. 즉 p는 이중포인터이다.
    // *p도 결국 포인터이고 NULL이라는 것도 포인터기 때문에 비교가 가능한 것이다. *p가 빈 것을 가리키면 for 문은 종료된다.

# Q&A

`Printf`의 인자가 문자 포인터라면 해당 자리에 문자 리터럴 대신에 문자열이 담긴 변수를 적어도 되나요?

    char fmt[] = "%d\n";
    int i;
    ...
    printf(fmt, i);
    
    // printf("%d\n", i); 와 똑같은 형태이나 왼편에 적어준 문자 리터럴을 배열에 담아두고 배열 이름을 대신 적은 것.

`Printf`로 `str`문자열을 출력하고 싶을 때 그냥 `printf(str);`이라고 해도 되나요?

    가능하지만 만약 문자열이 '%'를 포함하고 있다면 원치 않는 형태로 작동할 수 있기 때문에 위험이 있다.

`**argv` 와 `*argv[]`는 같나요?

    파라미터를 선언할 때는 아래 두 개는 같은 의미이다.
    *a == a[] // 둘 다 결국 포인터를 받겠다는 소리. 배열의 이름이 그 자체로 포인터인 점을 참고하자.
    
    // 다만 이는 어디까지나 a가 배열의 이름일 떄만 컴파일러가 알아서 해독해준다. 
    // *a[]는 포인터들을 인자로 갖는 배열, 즉 포인터 배열 선언인 걸 잊지말자.

[C difference between *[] and **](https://stackoverflow.com/questions/4208444/c-difference-between-and)
