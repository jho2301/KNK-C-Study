## 15장

#### include directive

1. #include <filename>
   주로 c 표준 라이브러리를 가져올 때 사용
   시스템 헤더 폴더 (UNIX의 경우 /usr/include/)에서 가져옴

2. #include "filename"
   현재 폴더에서 가져옴, 현재 폴더에서 못찾을 경우 시스템 헤더 폴더를 탐색

   ```c
   # include "\cprogs\include\utils.h" /* 비추천 */
   # include "..\include\utils.h" / 추천
   ```

   절대경로 보단 상대 경로를 추천. preprocessor가 오류가 날수 있기 때문.

3. #include tokens

   ```c
   #if defined(IA32)
   	#define CPU_FILE "ia32.h"
   #elif defined(IA64)
   	#define CPU_FILE "ia64.h"
   #elif defined(AMD64)
   	#define CPU_FILE "amd64.h"
   #endif
   
   #include CPU_FILE
   ```

   ia, amd 아키텍쳐?

#### sharing macro definitions and type definitions

```c
#define BOOL int
#define TRUE 1
#define FALSE 0
```

macro와 type을 정의한 헤더파일을 만드는 것은 편하다

* 소스파일마다 type과 macro를 정의하는 시간을 아낄 수 있다.
* type과 macro를 수정하기에 유용하다.  헤더파일 하나만 바꾸면 되기 때문
* 어떤 macro나 type에 대해 서로 다른 소스파일에서 다르게 정의하는 것을 막을 수 있다.

#### sharing  variable Declarations

``` c
extern int i;
```

extern은 컴파일러가 

#### protecting header files

헤더파일이 중복으로 컴파일 되는것을 막기위해 #ifndef 를 사용한다.

```c
#ifndef BOOLEAN_H
#define BOoLEAN_H

#define TRUE 1
#define FALSE 0
typedef int Bool;

#endif
```

이렇게하면 중복으로 매크로가 정의되는 것을 막을 수 있다.

#### #error directives

```c
#ifndef __STDC__
#error THIS header require standart c compiler
#endif
```

헤더파일이 비표준 컴파일러와 함께 사용되지 않도록 #ifndef directive와 함께 __STDC__ 정의 여부를 따짐

#### defining macros outside a program

gcc 는 -D 옵션으로 macro를 정의할수 있다.

```shell
gcc -DDEBUG=1 foo.c
```

#### QNA

1. 왜 소스파일마다 헤더파일을 만들어야하나? 그냥 큰 하나의 헤더파일 만들면 안되나?
   헤더파일을 하나만 만들면 편집하기 쉽다는 장점이 있지만, 다른 프로그래머가 읽을 때 불편하다.