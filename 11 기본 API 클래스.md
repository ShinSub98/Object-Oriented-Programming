java.lang 패키지 내 ==**주요 클래스**== 및 용도

|                    클래스                    | 용도                                                                                                          |
|:--------------------------------------------:|:------------------------------------------------------------------------------------------------------------- |
|                    Object                    | 자바 클래스의 최상위 클래스                                                                                   |
|                    System                    | 표준 입력장치(키보드)로부터 데이터를 입력<br>표준 출력 장치(모니터)로 데이터 출력<br>JVM 종료<br>GC 실행 요청 |
|                    Class                     | 클래스를 메모리로 불러올 때 사용                                                                              |
|                    String                    | 문자열을 저장하고 여러 가지 정보를 불러올 때 사용                                                             |
| Wrapper(Int, Double, Boolean 등의 기본 타입) | 기본 타입의 데이터 객체 생성<br>문자열을 기본 타입으로 변환<br>입력값 검사에 사용                             |
|                     Math                     | 수학 함수를 이용할 때 사용                                                                                    |


==**자바 API 문서 모음**==
https://docs.oracle.com/en/java/javae/index.html
-  주소 타고 접속
	1. \[All Modules\] 목록에서 java.base를 찾아 접속
	2. \[Packages] 목록에서 java.lang 접속
	3. \[Class Summary] 목록에서 필요한 클래스 찾아오기
- 검색
	1. 우측 상단에서 필요한 API 검색

![](https://grm-project-template-bucket.s3.ap-northeast-2.amazonaws.com/lesson/les_HaEGk_1668415907671/81106c09c874b0c31a715bc3474ee99fe250c54df391018a8305b7c23616d4c2.png)![](https://grm-project-template-bucket.s3.ap-northeast-2.amazonaws.com/lesson/les_HaEGk_1668415907671/4dab3cd2b6e1dc30c63362cb9cac52072480d25018cc914f5ab53db7905b2a4c.png)
\[All Methods] : 전체 메소드 목록
\[Static Methods]: 정적 메소드 목록
\[instance Methods] : 인스턴스 메소드 목록
Modifier and Type : static 또는 protected 여부와 리턴 타입 표시
Method와 Description의 메소드 이름 클릭하면 상세 설명 페이지로 이동

## Object 클래스
==**모든 클래스는 Object 클래스의 자식이거나 자손 클래스**==이다.

### 객체 비교(equals())
`equals()` 메소드는 **객체를 매개값**으로 받으며, 이 때 타입은 ==Object==가 된다.
- 모든 객체는 Object 타입으로 자동 변환될 수 있다.
```java
public boolean equals(Object 매개변수명) {
	// ~
	return
}

>>> true //동일 객체
>>> false //다른 객체
```

`equals()`는 객체를 논리적으로 동등한지 확인한다.
이는 주소값이 아니라 ==**객체의 데이터를 기준으로 판별**==함을 의미한다.

`equals()` 메소드를 재정의할 때는 매개값인 **비교 객체의 타입이 비교할 객체와 같은지 확인**하기 위해 `instanceof` 연산자를 통해 확인해야 한다.
비교 객체가 동일한 타입이라면, 기준 객체 타입으로 강제 타입 변환한 후 ==필드값이 동일한지 검사==한다.

```java
@Overriding //자바 기본 API에 존재하는 메소드이기 때문에 재정의 형식 선언
public boolean equals(Object obj){ //Object 타입으로 매개값 입력
	if (obj instanceof 비교클래스){ //obj와 비교클래스가 같은 클래스 출신이라면
		비교클래스 참조변수 = (비교클래스) obj; //obj를 비교클래스와 같은 타입으로 강제 변환
		if(필드값.equals(참조변수.필드값)){ //각 개체의 데이터 중 비교하고 싶은 값
			return true; //맞으면 true 리턴
		}
	}
	return false;
}
```

예)
```java
//비교 클래스
public class Exam1 {  
    int x;  
	  
    public boolean equals(Object obj){  
        if (obj instanceof Exam1) {  
            Exam1 exam1 = (Exam1) obj;  
            if (x == exam1.x) {  
                return true;  
            }  
        }  
        return false;  
    }  
}

//실행 클래스
public class Main {  
    public static void main(String[] args) {  
        Exam1 ex1 = new Exam1();  
        Exam1 ex2 = new Exam1();  
		  
        ex1.x = 5;  
        ex2.x = 5;  
		  
        System.out.println(ex1.equals(ex2));  
    }  
}

>>> true //x에 같은 값을 주었을 경우
>>> false //x에 다른 값을 주었을 경우
```


### 객체 해시코드: hashCode()
==**객체 해시코드(hashCode())**==
- 객체를 식별하는 하나의 정수값 (= 주소값)
- Object 클래스의 객체 해시코드 메소드를 사용하면 객체 주소값을 해시코드로 리턴
	- 각 개체는 서로 다른 해시코드를 가지고 있다.
	- 각 해시코드의 논리적 동등성을 비교하기 위해서는 `hashCode()`를 재정의하여 사용해야 한다.
```java
@Override
public int hashCode(){
	return x; //해시값
}
```

==**논리적으로 동등한 객체**==는 **내용이 같아** `equals()`가 true인 객체이며
==**물리적으로 동등한 객체**==는 **메모리 주소값이 같아** `hashCode()`의 리턴값이 같은 객체이다.


### 객체 문자 정보: toString()
`toString()`: 객체의 ==**문자 정보**==를 리턴하는 메소드
- **문자 정보**: 객체를 문자열로 표현한 값. '클래스이름@(16진수해시코드)'로 표현
```java
Object obj = new Object();
System.out.print(obj.toString());
```

`toString()` 메소드 그대로의 리턴값은 큰 의미가 없다.
하지만 Object 클래스의 하위 클래스들은 이를 **재정의**하여 간결하고 유의미한 정보 생성한다.
- Ex1) Date 클래스의 날짜 및 시간 정보
- Ex2) **저장하고 있는 문자열을 리턴**

==**현재 날짜 및 시간**==
```java
import java.util.Date; //Date 모듈 import


Date x = new Date();
System.out.println(x.toString());
```


==**설정한 문자열 출력**==
```java
public class A{
	int x;
	int y;
	
	public Stirng toString(){
		int sum = x+y;
		return x + "+" + y + " = " + sum;
	}
}

public class Main{
	public static void main(String[] args) {  
	    A a = new A();
		
		a.x = 5;
		a.y = 3;
		
		System.out.println(a.toString());
	}
}
>>> "5 + 3 = 8"
```



## System 클래스
==**System 클래스**==
- System 클래스의 모든 멤버는 **정적 멤버**(Static)이다.
- 프로그램 종료, 데이터 입출력, 현재 시간 읽기 등


### 프로그램 종료: exit()
`exit()`: JVM을 강제로 종료하는 메소드로, `exit()`이 지정한 int 매개값을 **종료 상태값**이라고 한다.
```java
for(int i=0; i<10; i++){
	if(i==5){
		System.exit(0); //i==5가 되면 exit()의 매개값이 0이 되면서 프로그램이 종료됨
	}
}
System.out.println("종료 텍스트"); //출력되지 않음
```


### 현재 시각: currentTimeMillis(), nanoTime()
`currentTimeMillis()`: 현재 시각을 밀리초 단위(1/1000초)로 long값 리턴
`nanoTime()`: 현재 시각을 나노초 단위(1/100000000초) 단위로 long값 리턴
```java
long time1 = System.currentTimeMillis();
long time2 = System.nanoTime();
```



## Class 클래스
자바는 클래스 및 인터페이스의 ==메타 데이터==를 Class 클래스로 관리한다.
- 메타 데이터 : 클래스의 이름, 생성자, 필드, 메소드에 대한 정보

==**Class 객체 얻기**==: `getClass()`, `forName()`
```java
// 패키지 P에 클래스 C가 있을 때... 

//클래스로부터 직접 얻는 방법
//.1
Class 참조변수명 = C.class;

//2. (이 때는 메소드 뒤에 throws ClassNotFoundException를 붙여줘야 한다.)
Class 참조변수명 = Class.forName("P.C")


//객체로부터 얻는 방법
C c = new C();
Class 참조변수명 = c.getClass();



참조변수명.getName()
>>> P.C

참조변수명.getSimpleName();
>>>C

참조변수명.getPackage().getName();
>>>P
```

==**리소스의 절대 경로 출력**==
C.java 파일에 M.jpg 파일이 있을 때
```java
C c = C.class;

String 참조변수명 = c.getResource("M.jpg").getPath();
>>> M.jpg의 경로
```



## String 클래스
==**String 생성자**==: 직접 String 객체 생성

==**byte 배열을 문자열로 반환**==
배열 내용 그대로 문자열이 되는 것이 아니라, Char 타입처럼 값에 해당하는 문자열로 바뀐다.
```java
//byte x[] 배열이 있을 때

//1. 배열 전체를 String 객체로 변환
String 참조변수명 = new String(x);

//2. 배열의 일부를 String 객체로 변환
String 참조변수명 = new String(x, 시작idx, 변환할 요소 개수)
```

==**키보드로 입력받은 바이트 배열을 문자열로 변환**==
`System.in.read()`를 사용하여 입력받은 내용을 매개값으로 하여 바이트 배열에 저장
※ 입력 시 누른 엔터에 대한 값 두 개도 함께 저장된다.
```java
public static void main(String[] args) throws ClassNotFoundException, IOException {
	byte x[] = new byte[100]; //배열 생성
	
	System.out.prin("입력");
	int n = System.in.read(x); //변수 n에 입력받은 문자 수를 저장하고, 배열 x에는 입력값을 저장
	
	String str = new String(x, 0, n-2); //x배열에서 입력받은 값 만큼만 문자열로 변환
	// -2는 엔터 입력으로 생긴 값을 뺀 것
}
```

| 리턴타입 |            문자열객체.메소드 이름(매개변수)             | 설명                                                            |
|:--------:|:-------------------------------------------------------:| --------------------------------------------------------------- |
|   char   |                    `charAt(int idx)`                    | 특정 위치의 문자를 리턴                                         |
| boolean  |                `equals(Object anObject)`                | 두 문자열을 비교                                                |
| byte\[]  |                      `getBytes[]`                       | byte의 배열로 리턴                                             |
| byte\[]  |               `getBytes(Charset charset)`               | 주어진 문자셋으로 인코딩한 byte\[]로 리턴                       |
|   int    |                  `indexOf(String str)`                  | 문자열 내에서 주어진 문자열의 위치를 리턴                       |
|   int    |                       `length()`                        | 총 문자의 수를 리턴                                             |
|  String  | `replace(CharSequence target, CharSequence replacement)` | target 부분을 replacement로 바꾼 새로운 문자열을 리턴         |
|  String  |               `substring(int beginIndex)`               | beginIndex 위치에서 끝까지 잘라낸 새로운 문자열을 리턴          |
|  String  |        `substring(int beginIndex, int endIndex)`        | beginIndex 위치에서 endIndex 전까지 잘라낸 새로운 문자열을 리턴 |
|  String  |                     `toLowerCase()`                     | 소문자로 변환하여 리턴                                          |
|  String  |                     `toUpperCase()`                     | 대문자로 변환하여 리턴                                          |
|  String  |                        `trim()`                         | 앞뒤 공백을 제거한 새로운 문자열을 리턴                         |
|  String  |                    `valueOf(int i)`                     | 기본 타입 값을 문자열로 리턴                                    |

==**문자 추출**==: `charAt()`
```java
String x = "abc";
char y = x.charAt(0);

System.out.print(y);
>>>"a"
```



## Wrapper 클래스
==**Wrapper(포장) 객체**==
- 기본 타입의 값을 내부에 포장
- 포장하고 있는 기본 타입의 값은 외부에서 변경 불가

| 기본 타입 | 포장 클래스 |
|:---------:|:-----------:|
|   byte    |    Byte     |
|   char    |  Character  |
|   short   |    Short    |
|    int    |  Interger   |
|   long    |    Long     |
|   float   |    Float    |
|  double   |   Double    |
|  boolean  |   Boolean   |

** 변수에 기본 타입의 값을 저장할 경우**
```java
Byte x = new Byte(10);
```

**변수에 문자열을 참조할 경우**
※ Char은 문자로 표시되므로 적용 불가
```java
Byte x = new Byte("10");
```


### Boxing / Unboxing
==**Boxing**==: 기본 타입의 값을 포장 객체로 만드는 과정
==**Unboxing**==: 포장 객체에서 기본 타입의 값을 얻어내는 과정
```java
//Boxing
Integer x = Integer.valueOf(1000);
Long y = Long.valueOf(10000);
Integer z = Integer.valueof("123");

//Unboxing
int valueX = x.intValue();
long valueY = y.longValue();
int valueZ = z.intValue();
```


==**자동 박싱 / 자동 언박싱**==
포장 클래스 타입 변수에 기본 값을 저장하면 **자동 박싱**
```java
Integer x = 100; //자동 박싱
```

기본 타입에 포장 객체를 대입하거나 연산한 경우 **자동 언박싱**
```java
Integer x = 100; //자동 박싱
int y = x; //자동 언박싱
int z = 100 + x; //x를 자동 언박싱
```


==**문자열을 기본 타입으로 변환**==
'**parse+기본 타입**'의 정적 메소드

| 기본 타입 |           메소드            |
|:---------:|:---------------------------:|
|   byte    |   `Byte.parseByte("10");`   |
|   short   |  `Short.parseShort("10");`  |
|    int    | `Integer.parseInt("100");`  |
|   long    |   `Long.parseLong("10");`   |
|   float   | `Float.parseFloat("10F");`  |
|  double   | `Double.parseDouble("10");` |
|  boolean  | `Boolean.parseBoolean("true")`|

```java
int x = Integer.parseInt("10"); //x에 문자열 "10"을 10으로 변환하여 저장
```


==**포장 값 비교**==
== 및 != 연산자는 참조값을 비교하는 연산자이므로 객체 비교에는 부적합하다.
따라서 **먼저 포장 객체를 언박싱 한 후에 == 및 != 연산자를 사용**한다.
(-128 ~ 127 사이의 값은 바로 비교할 수 있지만 어차피 범위가 너무 작아 의미가 없음)



## Math 클래스
`java.lang.Math` 클래스는 수학적 계산에 사용하며
정적 메소드이므로 Math 클래스를 통해 바로 호출 및 사용 가능

|                         메소드                          |         설명          |             예문              |  리턴값  |
|:-------------------------------------------------------:|:---------------------:|:-----------------------------:|:--------:|
|         int abs(int a)<br>double abs(double a)          |        절대값         |    `int x = Math.abs(-5);`    |  x = 5   |
|                  double ceil(doubla a)                  |        올림값         | `double x = Math.ceil(5.3);`  | x = 6.0  |
|                 double floor(double a)                  |        버림값         | `double x = Math.floor(5.3);` |  x=5.0   |
| int max(int a, int b)<br>double max(double a, double b) |        최대값         |  `int x = Math.max(10, 11);`  |  x = 11  |
| int min(int a, int b)<br>double min(double a, double b) |        최소값         |  `int x = Math.min(10, 11);`  |  x = 10  |
|                     double random()                     |        랜덤값         |  `double x = Math.random();`  | 0.0 <= x |
|                  double rint(double a)                  | 가까운 정수의<br>실수 값 | `double x = Math.rint(5.3);`  | x = 5.0  |
|                  long round(double a)                   |       반올림값        |  `long x = Math.round(5.3);`  |  x = 5   |



## java.util 패키지
==**java.util 패키지**==: `Import` 문을 사용해 불러와야 하는 클래스

|  클래스  | 용도                               |
|:--------:| ---------------------------------- |
|   Date   | 객체에 현재 연/월/일/시간의 정보를 저장            |
| Calendar | 해당 운영체제의 Calendar 객체를 통해<br>연/월/일/요일/오전후/시간 등의 정보 획득 |

### Date 클래스
**날짜를 표현하는 클래스**
객체 간 날짜 정보를 주고받거나 매개 변수 및 리턴 타입으로 주로 사용
```java
//객체 생성
Date x = new Date(); //현재 시간 객체 생성
String str = x.toString(); //현재 시각을 문자열로 변환하여 참조

System.out.println(x);
>>> Mon Jan 08 20:40:00 KST 2023
```

==**SimpleDateFormat**==
날짜 형식의 문자열을 얻기 위해 사용
- SimpleDateFormat 생성자의 매개값은 형식 문자열
- yyyy(년도), MM(월), dd(일)
```java
Date x = new Date();
String str = x.toString();

//출력 형식 정의
SimpleDateFormat y = new SimpleDateFormat("yyyy년 MM월 dd일 hh시 mm분 ss초");

String str = y.format(x);

System.out.print(str);
>>> 2023년 01월 08일 8시 40분 00초
```


### Calendar 클래스
달력을 표현한 **추상 클래스** (new 연산자를 사용하여 인스턴스를 생성할 수 없다.)

`getInstance()` 메소드를 사용해 현재 OS 상 시간대 기준으로 하위 객체를 생성할 수 있다.
```java
Calendar x = Calendar.getInstance(); //객체 생성
```


**Calendar 클래스로 생성한 객체 x에 대해 ==int타입==으로 값을 리턴받는 방법**

|             시간              |             메소드              |
|:-----------------------------:|:-------------------------------:|
|             연도              |     `x.get(Calendar.YEAR);`     |
|              월               |     `x.get(Calendar.MONTH)`     |
|              일               | `x.get(Calendar.DAY_OF_MONTH);` |
|  요일<br>(일요일=0부터 시작)  | `x.get(Calendar.DAY_OF_WEEK);`  |
| 오전/오후<br>(오전:0, 오후:1) |    `x.get(Calendar.AM_PM);`     |
|              시               |     `x.get(Calendar.HOUR);`     |
|              분               |    `x.get(Calendar.MINUTE);`    |
|              초               |    `x.get(Calendar.SECOND);`    |

```java
Calendar x = new Calendar();

int y = x.get(Calendar.HOUR);
System.out.print(y);
>>> 3 //12시간 형식으로 표시
```