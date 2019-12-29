### 22. Input/Output

표준입출력, 파일입출력, stream. FILE TYPE, Output redirection, file과 binary file, open/close 함수, formatted input/output, unfformated io, random access operation on file, sprint,sscan 함수

#### 22.1 Streams

스트림은 입력의 source 또는 출력의 목적지를 의미한다. 작은 프로그램들은 대부분 하나의 입력 스트림과 출력 스트림으로 구성되지만, 큰 프로그램의 경우 여러 스트림이 복합적으로 사용된다. 

- File Pointers

  - FILE *
    - FILE 타입은 stdio.h 에 선언되어 있다.
    - C에서 스트림에 접근하는 것은  file pointer를 통해서 하게 된다.
    - `FILE *fp1, *fp2;`

- Standard Streams and Redirection

  - stdio.h  헤더는 3가지 standard stream을 제공하고 프로그래머는 바로 사용할 수 있다. 
    - stdin : standard input (키보드)
    - stdout : standard output (모니터)
    - stdout : standard error (모니터)
  - 일반적으로, tandard 스트림은 키보드로 입력받고 모니터로 출력하지만, redirction을 통해 변경이 가능하다.
  - input redirection
    - < 문자 사용
    - demo <in.dat    // demo프로그램의 표준 입력을 키보드 대신 in.dat으로 하겠다는 의미

  - output redirection
    - \> 문자 사용
    - demo >out.dat  // demo프로그램의 표준 출력을 모니터 대신 out.dat 으로 하겠다는 의미
    - 파일로 표준 출력을 내보낼 경우 에러 발생을 파악하기 힘든 경우가 있다. 그럴 때는 에러 메세지를 stderr를 이용하여 출력하면 에러가 발생할 경우 바로 모니터에서 확인이 가능하다.
  - 동시 사용 가능
    - demo < in.dat > out.dat  // < 또는 > 문자와 파일 사이에 공백이 있어도 무관
    - demo > out.dat <in.dat // 순서 무관

- Text Files versus Binary FIles

  - stdio.h 헤더는 text와 binary 두가지의 파일을 지원한다. text file은 문자들로 표현된다. binary 파일은 문자로 표현될 필요가 없는 자료들을 저장하는데 사용된다. 
  - Text File
    - 라인으로 구분된다.
      - https://m.blog.naver.com/PostView.nhn?blogId=nkkh159&logNo=220837224425&proxyReferer=https%3A%2F%2Fwww.google.com%2F
      - 라인 피드 (LF : Line Feed, '0x0a', '\n') : 캐럿을 다음 줄(현재 위치에서 바로 아래)로 이동 시킨다.
      - 캐리지 리턴 (CR : Carriage Return, ' 0x0d', '\r' ) : 캐럿을 줄의 맨 앞으로 이동 시킨다.
      - 운영체제 마다 줄 바꿈 정의 가 다름
        - 유닉스/리눅스/맥 : LF만으로 줄 바꿈을 정의
        - 윈도우 : CR LF로 줄바꿈 정의
        - 맥의 초기 버전(9 버전 이하) : CR로 줄바꿈을 정의
    - EOF(end-of-file)를 가지고 있을 수 있다. 
      - 윈도우 : ctrl-Z / 'x1a'
      - 유닉스를 포함한 대부분의 OS에는 EOF 캐릭터가 없다.
  - Binary File
    - 라인으로 구분되지 않음. end-of-line나 end-of-file 마커가 없고, 모든 바이트가 동등하게 취급된다.

  - 데이터를 파일\로 저장할 때 텍스트 파일과 바이너리 파일 중 어떤 형태로 저장할지 고려해야 한다.  예를 들어 32767을 저장할 때 아스키코드로 저장할지 2진 정수형태로 저장할 지 결정 할 수 있는데, 텍스트 파일로 저장한다면 5바이트, 정수로 저장한다면 2바이트가 사용되기 때문에 어떤것이 효율적인지 고려해야한다.

  - 다루어야할 파일이 텍스트파일인지 바이너리 파일인지 모를 때에는 바이너리 파일로 취급하는 것이 더 안전하다.

#### 22.2 File Operations

- redirection은 file을 open,close를 하지 않아도 되기때문에 간편하지만, 많은 제약이 따른다. 여러개의 파일을 읽고 쓰는 경우는 특히 redirection 만으로는 불가능하다.

- redirection만으 충분하지 않는 경우라면, stdio.h가 제공하는 파일 연산자 대신 다른 연산자를 써야한다.

- opening a File, closing a file, 파일의 버퍼 방식 변경, 파일의 삭제, 파일의 이름변경 등

- Opening a File

  파일을 열기 위해서는 fopen 함수를 호출해야 한다.

  프로토 타입 : 

  ```c
  FILE *fopen(const char * restirct filename, const char * restrict mode)
  ```

  첫 번째 인자 : 파일이름 ( 파일의 위치까지 포함 )

  두 번째  인자 : 파일권한 (r : read)

  ```c
  fopen("c:\project\test1.dat", "r"); // 윈도우에서 \로 경로를 표시하는데, \t는 escapse sequcnce 이기 때문에 오류 발생
  fopen("c:\\project\\test1.dat", "r");  //윈도우
  fopen("c:/project/test1.dat", "r"); //UNIX (윈도우도 이렇게 사용 가능하다)
  ```

  fopen 함수는 file 포인터를 리턴한다.

  ```c
  fp = fopen("in.dat", "r");
  ```

  만약, 파일이름이 잘못되거나 경로가 잘못되어 파일을 열 수 없을 경우에는, null poinetr를 리턴한다.

- Modes

  > "r" : 읽기
  >
  > "w" : 쓰기, 덮어쓰기(파일이 존재할 필요 없음)
  >
  > "a" : 추가(덧붙이기-appeding) (파일이 존재할 필요 없음)
  >
  > "r+" : 읽고 쓰기, 처음부터 시작
  >
  > "w+" : 읽고 쓰기, 파일이 존재하면 잘라냄
  >
  > "a+" : 읽고 쓰기, 파일이 존재하면 덧붙임(append)

  binary 파일일 경우 b를 써줌

  > "rb" : 읽기
  >
  > "wb" : 쓰기, 덮어쓰기(파일이 존재할 필요 없음)
  >
  > "ab" : 추가(덧붙이기-appeding) (파일이 존재할 필요 없음)
  >
  > "r+b 또는 rb+" : 읽고 쓰기, 처음부터 시작
  >
  > "w+b 또는 wb+" : 읽고 쓰기, 파일이 존재하면 잘라냄
  >
  > "a+b 또는 ab+" : 읽고 쓰기, 파일이 존재하면 덧붙임(append)

  +를 사용해서 읽기,쓰기를 모두 사용하는 경우 읽기 작업과 쓰기작업을 교대로 사용할 수 없다. 읽기를 시작했다면 EOF까지 읽기 작업만 할 수 있다. 또한, 쓰기 작업을 하고 있다면 fflush를 호출하거나 file-positioning 함수를 호출하기 전까지는 읽기 작업을 할 수 없다.

- Closing a File

  프로토타입 : 

  ```C
  int fclose(FILE *stream);
  ```

  fclose 함수는 프로그램에서 더이상 사용하지 않는 파일을 끝낼 때 사용한다. fclose함수의 argument는 fopen이나 freopen함수에 호출되었던 file 포인터를 가질 수 있다. fclose는 파일이 성공적으로닫혀면 0을 리턴하고, 그렇지 않으면 error code인 EOF(매크로로 stdio.h에 정의됨)를 리턴한다.

  ```c
  #include <stdio.h>
  #include <stdlib.h>
  
  #define FILE_NAME "example.dat"
  
  int main(void)
  {
      FILE *fp;
      fp = fopen(FILE_NAME, "r");
      if(fp=NULL){
          printf("Cant't open %s\n", FILE_NAME);
          exit(EXIT_FAILURE);
      }
      ...
      fclose(fp);
      return 0;
  }
  ```

  fopen함수를 쓸 때 `FILE *fp= fopen(FILE_NAME, "r");`으로 선언과 초기화를 동시에 할 수도 있고, `if((fp=fopen(FILE_NAME,"r")) == NULL);`로 코드를 줄일 수 있다. 

- Attaching a File to an Open Stream

  ```c
  FILE *freopen(const char * restrict filename,
                const char * restrict mode,
                FILE * restrict stream);
  ```

  freopen은 다른 파일을 이미 열려있는 파일 스트림에 덧붙이는 함수이다.

  일반적인 freopen의 사용법은 하나의 파일을 standard stream(stdin, stdout, stderr) 중 하나와 결합하는 것이다.

  만약, 프로그램이 foo라는 파일에 쓰기를 시작하게 하려면 아래와 같이 함수를 호출하면 된다.

  ```c
  freopen("foo", "w", stdout);
  ```

   redirection이나 freopen으로 미리 stdout과 결합되어 있는 파일이 닫히고 난 뒤에, freopen 함수가 foo를 열고 stdout과 결합시킨다.

  freopen함수는 보통 세번째 인자를 리턴한다. 만약, 파일을 열 수 없는 경우 null pointer를 리턴한다. (이미 있던 file이 닫혀지지 않더라면, freopen 함수는 에러를 무시한다.)

  C99 부터 filename이 null pointer면, freopen은 stream 모드를 바꾼다. 

- Obtaining File Names from the Command Line

  demo라는 프로그램을 names.dat와 dates.dat 파일함께 실행하는 법

  > demo names.dat dates.dat

  argc : 커맨드라인 아규먼트의 수

  argv : 스트링 아규먼트의 배열 포인터

  ```c
  int main(int argc, char *argv[])
  {
  	...
  }
  ```

  위의 예에서 argv[0]은 프로그램 이름인 demo이고, argv[1]은 names.dat 이고 argv[2]는 dates.dat이다. 

- Temporary Files

  프로그램이 실행되는 동안에만 생성되었다가 프로그램이 종료되면 사라지는 파일

  tmpfile (wb+ 모드) : 파일을 close하거나 프로그램이 끝날때까지 존재

  ```c
  //프로토타입
  FILE *tmpfile(void);
  
  //사용법
  FILE *tempptr;
  ...
  tempptr = tmpfile();
  ```

  tmpfile 함수는 사용하기 간편하지만, 두 가지 단점이 존재한다.

  1. 생성된 파일의 이름을 모른다는 점
  2. 임시 생성된 파일을 영구적으로 만들 수 없다는 점

  그렇기 때문에 fopen으로 파일을 여는 것이 나을 수도 있다. 하지만 fopen으로 임시파일을 생성할 때 이미 존재하고 있는 파일의 이름은 사용하면 안되기 때문에 중복되지 않는 새로운 파일명을 생성해주어야 한다. tempnam은 임시파일의 유일한 이름을 생성해준다.

  tmpnam :  중복되지 않는 임시파일의 이름 생성

  ```c
  //프로토타입
  char *tmpnam(char *s);
  
  //NULL포인터를 아규먼트로 가질 때 파일이름을 static 변수로 저장하고, 포인터를 리턴
  char *filename;
  ...
  filename = tepnam(NULL);
  
  //사용자가 지정한 배열에 파일 중복되지 않는 파일이름을 저장함
  char filename[L_tmpnam];
  ...
  tmpnam(filename);
  ```

  주의) 

  배열을 사용할 때에는 L_tmpnam보다 배열의 길이가 길어야 함

  너무 자주 tmpnam을 호출하면 안됨(TMP_MAX를 초과하면 안됨)

  만약 파일이름 생성에 실패하면, null pointer 반환

- File Buffering

  ```c
  //버퍼 관련 함수의 프로토 타입
  int fflush(FILE *stream);
  void setbuf(FILE * restrict stream,
  			char * restrict buf);
  int setvbuf(FILE * restrict stream,
             	char * restric buf,
              int mode, size_t size);
  ```

  데이터를 디스크에 저장하거나 디스크에서 가져오는 것은 느리다. 성능을 향상시키기 위해 버퍼링을 사용한다. 

  출력 스트림에 쓰여진 데이터는 실제로 메모리의 버퍼영역에 저장되고, 버퍼가 가득차거나 스트림이 닫히면, 퍼버는 flush된다.

  입력 스트림도 같은 방식으로 버퍼링된다. 버퍼가 입력된 데이터를 저장하고 버퍼로 부터 입력을 받는다.

  버퍼에서 바이트단위의 데이터를 읽거나 저장하는 것은 거의 시간이 들지 않고 disk에서 읽으면 오래걸린다. 그렇기 때문에 한번에 큰 블럭을 버퍼처리하여 효율적으로 사용이 가능하다. 

  `fflush`

  프로그래머가 파일로의 출력을 쓰면, 데이터는 버퍼에 먼저 저장 된다. 버퍼는 가득 차거나 파일이 닫히면 flush된다. 하지만, fflush를 사용해서 사용자가 원하는 시점에 flush를 할 수 있다.

  에러 없이 실행되면 0을 리턴하고, 에러가 발생하면 EOF를 리턴한다.

  ```c
  //fp 스트림을 flush함
  fflush(fp);
  //NULL을 넣으면 모든 출력 스트림을 flush함
  fflush(NULL);
  ```

  `setvbuf`

  스트림이 버퍼링되는 방식을 변경하고, 버퍼의 사이즈와 위치를 변경할 수 있다. 세 번째 아규먼트인 mode에는 아래의 매크로를 사용할 수 있다.

  > _IOFBF : full buffering (버퍼가 비었을 때 데이터를 읽거나, 버퍼가 가득 찼을 때 데이터를 쓴다.)
  >
  > _IOLBF : line buffering (한 번에 한 줄씩 데이터를 스트림으로 부터 읽거나 쓴다.)
  >
  > _IONBF : no buffering (버퍼 없이, 데이터를 직접 스트림으로 부터 읽거나 쓴다.)

  (full buffering이 기본 값)

  만약 두 번째 인자가 null pointer가 아니라면 원하는 버퍼의 주소값을 주어야 한다. 버퍼는 static storage duration, automatic strage duration을 가질 수 있고  심지어 동적할당이 가능하다. ~~버퍼를 automatic으로 만드는 것은 버퍼의 공간이 자동으로 블록 출구로 재선언 되도록 허락하는 것이다.~~ 동적할당으로 버퍼를 설정한다면 버퍼가 더이상 필요하지 않을 경우 버퍼를 해제하는 것도 가능하다. setvbuf의 마지막 아규먼트는 버퍼의 사이즈를 결정하고, 클수록 퍼포먼스가 좋고, 작을수록 메모리를 절약할 수 있다.

  ```c
  char buffer[N];
  
  ...
  
  setvbuf(stream, buffer, _IOGBG, N);
  ```

  setvbuf는 stream 이 open되고 호출되어야 한다.

  null pointer를 두 번째 인자로 호출하는 방법이 있는데, 특정 사이즈의 버퍼를 생성할 때 쓰인다. 에러가 발생하지 않으면 0을 발생하면 0이 아닌 값을 리턴한다. 

  `setbuf`

  setbuf는 조금 오래된 함수이다. 만약 setbuf(stream, buf)로 호출되었다면, (void) setvbuf(stream, NULL, _IONBF, 0); 또는 (void) setvbuf(stream, buf, _IOFBF, BUFSIZ); 와 동일하다.

  요즘에는 선호되지 않는 방식이다.

- Miscellaneous file Operations

  ```c
  int remove (const char * filename);
  int rename(const char * old, const char *new);
  ```

  두 함수 모두 에러가 없으면 0을, 에러가 발생하면 0이 아닌 숫자를 리턴

  remove : 파일을 삭제함

  `remove("foo");`

  만약, fopen을 사용해서 임시파일을 생성 했다면 remove로 파일을 프로그램이 끝나기 전에 호출하여 임시파일을 삭제할 수 있다. 파일을 삭제하기 전에 파일은 clsoe된 상태여야 한다. 

  rename : 파일 이름은 변경함

  `rename("foo","bar");`

  임시로 만들었던 파일을 영구저장하기 위해 fopen으로 만들었던 파일의 이름을 변경할 때 유용하다. 이미 존재하고 있는 파일의 이름은 사용할 수 없다. 또한, 파일을 close한 상태에서 이름 변경이 가능하다.

#### 22.3 Formatted I/O

- printf 함수

  ```c
  int fprintf(FILE * restrict stream,
             	const char * restrict format, ... );
  int printf (const char * restric format, ...);
  ```

  가변 아규먼트를 가진다.

  리턴값은 문자의 갯수, 에러 발생시 음수 값 리턴

  ```c
  printf("Total : %d\n", total); // stdout
  fprintf(fp, "Total : %d\n", total); // fp
  fprintf(stderr, "error"); // stderr : stdout을 redirection 했어도 화면에 출력되기 때문에 에러는 stderr을 통해 출력하는 것이 좋다.
  ```

- printf Conversion Specifications

  아규먼트를 문자로 변환하여 출력하는 법

  %뒤에 5가지 distinct item 이있고, 순서가 맞아야 한다.

  ![image-20191228185530475](C:\Users\JUNHYEONG\AppData\Roaming\Typora\typora-user-images\image-20191228185530475.png)

  - Flags (optional) : 숫자가 표현되는 방식에 영향을 준다.

    - \- : 좌측 정렬 ( 기본값은 우측 정렬)
    - \+ : 부호 표시 ( + or -)  (기본값은음수일 때만 부호 표시됨)
    - 공백 : 양수일 때는 부호를 출력하지 않고 공백공간으로 표시, 음수 일 때는 - 부호 출력
    - \# : 진법에 맞게 숫자 앞에 0, 0x, 0X를 붙임
    - 0 : 출력하는 폭의 남는 공간에 0으로 채움

  - Minimum field width (optional) : 표시되는 공간의 최소길이를 지정한다. 만약 출력이 표시되는 공간보다 좁으면 왼쪽 부분은 공백으로 채워진다. 

  - Precision (optional) : conversion에 따라 의미가 다르다. .뒤에 숫자가 온다. 

    - d, i, o, u, x, X : minimum number of digits (길이가 부족하면 앞이 0으로 채워짐)
    - a, A, E, f, F : decimal point 이후 자릿 수
    - g, G : 의미있는 digits의 수
    - s : maximum number of bytes

  - Length modifier(optional) : 자료형의 길이를 조절하는데 사용되고,  conversion specifier와 같이 조합되어 사용 된다.

    ![img](https://i.imgur.com/8heFUt3.png)

  - Conversion specifier (필수) : 자료형 타입마다 정해져 있지만, 자동으로 형변환 되는 기능으로 편리하게 사용가능하다. double 타입의 값은 f,F,e,E,g,G,a,A와 사용되지만, float 함수를 쓰더라도 자동으로 double로 형변환되어 함수로 전달된다.

    ![image-20191228212402000](C:\Users\JUNHYEONG\AppData\Roaming\Typora\typora-user-images\image-20191228212402000.png)

- 예시

  

  ![img](https://i.imgur.com/dvMlyPc.png)

  ![img](https://i.imgur.com/VxGibDf.png)

  

  ![img](https://i.imgur.com/AbmwhHK.png)

  ![img](https://i.imgur.com/eFSv3RO.png)

  별을 사용해서 별에 인자값을 넣어 대신할 수 있다. 아래는 모두 같은 출력을 보인다.

  ![img](https://i.imgur.com/7DURgza.png)

  

- sacnf 함수

```c
int fscanf(FILE * restrict stream, const char *restrict format, ...);
int scanf(const char* restrict format,...);
//
scanf("%d%d", &i, &j);
fscanf(fp,"%d%d", &i, &j);
```

에러 발생시 EOF 리턴, 에러 발생안하면 데이터 아이템 수를 리턴

- scanf format strings
  - Conversion Specifications
  - White-space Characters : 공백문자를 매칭
  - Non-white-space characters : 같은 문자를 매칭

- 예시

  ![img](https://i.imgur.com/TLdYa61.png)

  ![img](https://i.imgur.com/ZqA7Bz0.png)

  ![img](https://i.imgur.com/ywN1CPT.png)

- Detectin End-of-File and Error Conditions

  ```c
  void clearerr(FILE *stream);
  int feof(FILE *stream);
  int ferror(FILE *stream);
  ```

  scanf에서 n개의 값을 읽을 때, 리턴값이 n보다 작은 경우는 3가지의 경우가 있다.

  - End-of-File : 파일의 끝을 만난 경우
  - Read error : 스트림으로 부터 문자를 읽을 수 없을 경우
  - Matching failure : 데이터 포맷이 맞지 않는 경우 

  EOF 와 Read error는 indicator를 변화시키기 때문에, 초기화를 해주어야 한다. (matching failure는 indicator를 바꾸지 않음)

  초기화 할때 clearerr 사용

  ferror은 read error를 감지

  feof는 eof를 감지

#### 22.4 Character I/O

하나의 문자를 입출력하는 함수, 문자를 char형이 아니라 int형으로 취급한다. 그 이유는 입력함수가end of file 또는 에러 상태를 EOF를 리턴해서 표현하는데 EOF는 음의 정수 상수이다.

- Output Functions

  ```c
  //함수 프로토타입
  int fputc(int c, FILE *stream);
  int putc(int c, FILE *stream);
  int putchar(int c);
  //함수 활용
  putchar(ch);   //stdout에 ch 문자를 쓴다.
  fputc(ch, fp);  //fp 파일에 ch 문자를 쓴다.
  putc(ch, fp);  //fp 파일에 ch 문자를 쓴다.
  ```

  putc 함수를 매크로로 정의할 수 있으므로 인수를 여러 번 평가할 수 있습니다.

  putchar는 매크로 사용 불가

- Input Functins

  ```c
  //함수 프로토타입
  int fgetc(FILE *stream);
  int getc(FILE *stream);
  int getchar(void);
  int ungetc(int c, FILE *stream);
  //함수 활용
  ch = getchar();  //stdin 문자 읽기
  ch = fgetc(fp);  //fp파일로 부터 문자 읽기
  ch = getc(fp);   //fp파일로 부터 문자 읽기
  ```

  

#### 22.5 Line I/O

라인 단위로 입출력

- Output Functions

  ```c
  int fputs (const char* restrict s, FILE * restrict stream);
  int puts (const char *s);
  //활용
  puts("Hi, there!"); //stdout
  fputs("Hi, there!", fp);  //fp
  ```

- Input Functions

  ```c
  char *fgets(char * restrict s, int n, FILE * restrict stream);
  char *gets(char *s);
  //활용
  gets(str);  //stdin
  fgets(str, sizeof(str), fp);   //fp, 개행 문자 또는 str사이즈만큼 읽음
  ```



#### 22.6 Block I/O

블럭 단위로 입출력

```c
size_t fread(void * restrict ptr, size_t size, size_t nmemb);
size_t fwrite(const void * restrict prt, size_t size, size_t nmemb, FILE *restrict stream);

//활용
fread(a, sizeof(a[0]), sizeof(a) / sizeof(a[0]), fp);
n=fread(a, sizeof(a[0]), sizeof(a)/sizeof(a[0]), fp);
```



#### 22.7 File Positioning

파일이 열렸을 때, 파일의 위치는 모드에 따라 다르긴 하지만 보통 파일의 시작부분에 있다가 읽고 쓰고하면서 점차 위치가 변한다.

- fseek

```c
int fseek(FILE *stream, long int offset, int whence)
```

파일의 위치를 변경한다.

첫번쨰 인자는 파일 포인터, 두번째인자는 오프셋, 세번째 인자는 기준 위치점이다.

일반적으로 0을 리턴하고, 에러발생 시 0이 아닌값 리턴

매크로

> SEEK_SET : 파일의 시작점
>
> SEEK_CUR : 현재 위치
>
> SKKE_END : 파일의 끝

```c
fseek(fp, 0L, SEEK_SET);  //시작점으로 이동
fseek(fp, 0L, SEEK_END);  //끝점으로 이동
fseek(fp, -10L, SEEK_CUR);  //현재 위치에서 뒤로 10바이트 만큼 이동
```

- ftell

```c
long int ftell (FILE *stream);
```

현재 파일의 포지션 위치를 반환

에러 발생시 -1리턴

```c
long file_pos;
...
file_pos = ftell(fp);  // 현재 위치를 저장
...
fssek(fp, file_pos, SEEK_SET);  // 저장했던 위치로 다시 돌어가기
```

- rewind

```c
void rewind(FILE *stream);
```

파일의 위치를 시작점으로 돌리는 것

- fgetpos & fsetpos

```c
int fgetpos(FILE *restrict stream, fpos_t *restrict pos);
int fsetpos(FILE * stream, const fpos_t *pos);
```

엄청 큰 파일을 다룰 때 fseek이나 ftell의 범위(long int)를 넘어서는 경우가 있다, fpos_t라는 타입을 이용하여 위치를 저장 할 수 있다.  두 함수 모두 성공시 0을 실패시 0이아닌값을 리턴한다.

```c
fpos_t = file_pos;
..
fgetpos(fp, &file_pos); // 현재 위치를 저장
...
fsetpos(fp, &file_pos); // 저장했던 위치로 다시 돌아가기
```



#### 22.8 String I/O

- 출력 함수

```c
int sprintf(char * restict s, const char *restrict format, ...);
//
sprintf(date, "%d/%d/%d",9,20,2010);
```

문자열에 저장시키고 마지막에 null문자를 추가함

문자열의 길이(null문자 제외)를 리턴함

```c
int snprintf(char * restict s, size_t n, const char *restrict format, ...);
//
snprintf(name, 13, "&s, %s", "Einstein", "Albert");
//결과 13길이를 넘어가면 저장하지 않음. Einsteing, Al 까지만 저장
```

문자열에 저장시키고 마지막에 null문자를 추가함

문자열의 길이(null문자 제외)를 리턴함

인코딩 에러가 있으면 음수 리턴

- 입력 함수

```c
int sscanf(const char * restrict s. const char * restrict format,...);
//
fgets(str, sizeof(Str), stdin);
sscanf(str, "%d%d", &i, &j); //str에서 두개의 정수를 추출함
```

파일의 끝까지 가면 EOF를 리턴, 보통은 성공적으로 읽고 저장한 데이터 아이템 수를 리턴함 
