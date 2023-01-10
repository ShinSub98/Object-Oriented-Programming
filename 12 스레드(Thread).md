## 프로세스(Process)
==**프로세스(Process)**==: **실행 중**인 하나의 애플리케이션으로, 필요한 메모리를 OS에게 할당받아 코드를 실행한다. 동시에 여러 프로세스를 수행하는 <u>멀티태스킹</u>도 가능하다.

==**멀티 프로세스**==: 운영체제에서 할당받은 메모리에서 실행되기 때문에 <u>독립적</u>이며, 한 프로세스에서 오류가 생겨도 다른 프로세스에서 문제가 발생하지 않음.



## 스레드(Thread)
==**스레드(Thread)**==: OS는 멀티태스킹을 위해 프로세스마다 자원을 할당하여 병렬적으로 실행한다. 이 때 각 작업을 실행하기 위해 순차적으로 코드를 이어놓은 것을 <u>스레드(Thread)</u>라고 한다.

==**멀티 스레드**==: 하나의 프로세스로 복수의 작업(스레드)를 처리하는 것
- 데이터를 분할해 병렬로 처리하거나 여러 클라이언트의 요청을 처리하는 서버를 개발할 때 사용
- 한 스레드에서 오류가 발생하면 전체 프로세스가 종료될 위험이 있음

> [!note] 
> 각 프로세스는 독립적인 메모리 영역을 사용하기 때문에 각자의 예외에 대해 독립적이지만, 같은 프로세스 안의 스레드들은 같은 메모리 영역에서 실행되기 때문에 서로 영향을 받는다. 


### 메인 스레드(Main Thread)
==**메인 스레드(Main Thread)**==: main() 메소드가 실행되는 스레드로, 모든 자바 애플리케이션이 시작되는 영역.
- main() 메소드를 위에서 아래로 순차적으로 실행
- main() 메소드의 마지막 코드를 실행하거나 return문을 만나면 종료
- 메인 스레드는 필요에 따라 다른 스레드를 생성해 <u>멀티 스레드</u> 구현
	- 멀티 스레드를 사용하는 어플리케이션은 **스레드가 하나라도 남아있으면** 프로세스가 종료되지 않는다.
![](https://i.imgur.com/q6heUW2.png)


### 작업 스레드
==**작업 스레드**==
- 멀티 스레드 애플리케이션을 개발하기 위해선 먼저 몇 개의 작업을 병렬로 실행할지 결정
- 작업 스레드는 객체로 생성되기 때문에 이를 생성하기 위한 **클래스가 필요**
	- `java.lang.Thread` 클래스를 직접 객체화하여 생성
	- Thread 클래스를 상속해 자식 클래스를 생성 가능


#### 작업 스레드 생성
==**java.lang.Thread 클래스로부터 직접 생성**==
**Runnable**: 작업 스레드가 실행할 수 있는 코드를 가지고 있는 객체. <u>인터페이스 타입</u>이기 때문에 구현 객체를 만들어서 연결해야 한다.
(Runnable은 작업 내용을 가지고 있는 객체일 뿐으로, **실제 스레드는 아니다**.)

`start()` 메소드로 호출된 작업 스레드는 매개값으로 받은 Runnable의 `run()` 메소드를 실행하여 자기 작업 처리
![](https://i.imgur.com/WA3uOHU.png)

```java
//작업 스레드가 될 클래스 작성
public class WT implements Runnalbe{
	public void run(){
		// ~ 작업 스레드 코드 ~
	}
}


//메인 클래스 작성
public class Main {
	public static void main(String[] args) {
		Runnable runnable = new WT(); //Runnable 타입으로 객체 생성
		Thread thread = new Thread(runnable); //Thread 타입으로 스레드 객체 생성
		y.start(); //WT의 run() 메소드 실행
	}
}
```


==**스레드 하위 클래스로부터 생성**==
1. **Runnable에 대한 익명 구현 객체 생성**
```java
//
pulbic class WT{
	public static void main(String[] args){
		Thread thread = new Thread(new Runnalbe(){
			@Override
			public void run(){
				 // ~ 작업 스레드 코드 ~
			}
		)};
		thread.start(); //작업 스레드 실행
	}
}
```


2. **Thread 클래스를 상속하여 `run()` 메소드를 재정의**
```java
//Thread의 자식 클래스 작성
public class WT extends Thread{ //Thread 클래스를 상속
	@Override
	public void run(){
		// ~ 스레드 실행 코드 ~
	}
}

//메인 메소드
Thread thread = new WT();
thread.start();
```

> [!tip] 
> 예문들을 보니 보통 Thread 클래스를 상속하여 자식 객체를 만드는 방법을 많이 쓰는 듯. 


### 스레드의 이름
- 각 스레드에는 이름이 있으며, 디버깅 작업 시 스레드의 작업을 확인하기 위해 사용
- 메인 스레드의 이름은 'main'이며, <u>개발자가 생성한 스레드는 자동적으로 'Thread-n'으로 설정</u>
	- 다른 이름으로 설정하기 위해선 `setName()` 메소드 사용
```java
//스레드 이름 변경
스레드객체명.setName("스레드 이름");

//스레드 이름 불러오기
스레드객체명.getName();

//이 문장을 실행한 스레드에 대한 정보 획득
Thread 변수명 = Thread.currentThread();
```


예)
```java
//Thread 클래스를 상속받은 클래스 작성
public class WT extends Thread{
	// ~ 스레드 코드
}


//메인 메소드
Thread thread = new WT(); //Thread 타입의 객체 생성

System.out.println(thread.getName());
>>> Thread-0

thread.setName("WT"); //WT로 스레드 이름을 수정
System.out.println(thread.getName());
>>> WT


Thread mainThread = Thread.currentThread(); //이 문장을 실행한 메인 메소드의 스레드 객체 획득

System.out.println(mainThread.getName());
>>> main

mainThread.setNmae("메인"); //메인 메소드의 스레드 이름 수정
System.out.println(mainThread.getName());
>>> 메인
```


### 동기화 메소드
멀티 스레드에서 각 스레드는 **같은 메모리 영역을 공유**한다.
따라서 서로 다른 스레드가 하나의 객체를 공유하는 상황도 발생하는데
이 경우 각각의 스레드에서 실행된 작업이 서로에게 영향을 줄 수 있다.

예시)
```java
public class Main{
	public static void main(String[] args){
		Calculator calc = new Calculator(); //객체 참조변수 calc 선언
		
		T1 t1 = new T1(); //t1 스레드 생성
		t1.setCalculator(calc); //calc을 매개값으로 입력
		t1.start(); //객체 calc을 사용한 작업 실행
		
		T2 t2 = new T2(); //t2 스레드 생성
		t2.setCalculator(calc); //calc을 매개값으로 입력
		
		t2.start(); //t1이 작업한 내용이 반영된 calc을 사용하여 작업하게 됨
	}
}
```

==**동기화 메소드**==: 어떤 스레드가 사용하고 있는 객체를 다른 스레드가 수정할 수 없게 잠가두는 메소드
- **임계 영역**(Critical section): 한 번에 하나의 스레드만 실행할 수 있는 코드 영역
- **동기화**(synchronized) 메소드: 스레드 객체 내부에서 실시하면 즉시 객체에 잠금을 건다.

동기화 메소드를 만들기 위해서는 메소드를 선언할 때 `synchronized`를 작성한다.
```java
public synchronized void method(){
	//~ 단 하나의 스레드만 실행 가능한 "임계 영역" ~
}
```

> [!note] 
>  동기화 메소드는 **실행되는 순간 해당 메소드 전체에 잠금**이 걸리며, **메소드가 종료되면 잠금이 해제**된다.
>
>또한 하나의 객체에 여러 동기화 메소드가 있을 경우, **하나의 동기화 메소드만 실행되어도 다른 스레드에서는 해당 객체의 모든 동기화 메소드를 호출할 수 없다.**
>하지만 해당 객체의 일반 메소드는 다른 스레드도 호출할 수 있다.
>
>즉, <u>A객체의 동기화 메소드가 하나라도 실행되면 A객체의 모든 동기화 메소드에 잠금</u>이 걸리지만, <u>일반 메소드는 통상처럼 사용 가능</u>하다.

> [!summary] 
> **원래 스레드는 하나의 객체를 공유해도 <u>병렬</u>로 동시에 작업**하지만, **동기화 메소드로 선언된 경우 이를 순서에 따라 <u>직렬</u>로 처리**한다.


### 스레드 상태
스레드 객체의 `.start()` 메소드를 호출해도 바로 실행되는 것이 아니라 ==**실행 대기 상태**==가 된다.

1. ==**실행 대기 상태**==: 스레드 객체가 실행되기 전 차례를 기다리고 있는 상태. 
	- 실행 대기 상태에 있는 스레드는 차례가 되면 실행 상태로 전환되며, `run()` 메소드를 실행한다.
2. ==**실행(running) 상태**==: OS가 CPU에서 실행하도록 선택한 스레드 객체의 상태.
	- `run()` 메소드가 종료되기 전에 다시 실행 대기 상태로 돌아가 **반복**할 수 있다.
	- 경우에 따라 실행 대기 상태가 아니라 "**일시 정지 상태**"가 되기도 한다.
		- ==**일시 정지 상태**==: 스레드를 실행할 수 없는 상태(예외)로, 이후에는 실행 대기 상태가 된다.
3. ==**종료(terminated) 상태**==: `run()` 메소드가 종료된 스레드 객체의 상태.
	- 실행 상태에서 `run()` 메소드를 모두 실행하면 스레드가 종료된다.

> [!summary] 
> **스레드 객체 생성** → **실행 대기** → **실행**과 **실행 대기**를 반복 or **일시 정지 상태** 후에 **실행 대기 상태** → **`run()` 메소드 모두 실행 시 종료**


#### 스레드 상태 제어
<u>실행 중인 스레드의 상태를 제어</u>

|        메소드        | 설명                                                                                                                                                                   |
|:--------------------:| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|    `interrupt()`     | 예외가 발생하여 일시 정지 상태가 된 스레드를 InterruptedException 예외처리를 발생시켜 **예외 처리 코드**로 이동하여 실행 대기 상태로 이동하거나 바로 종료할 수도 있다. |
| `sleep(long 밀리초)` | 매개값 만큼 스레드를 일시 정지 상태로 전환하고, 시간이 끝나면 실행 대기 상태로 전환                                                                                    |
|       `stop()`       | 스레드를 즉시 종료. **불완전한 종료이므로 가급적 사용 자제**                                                                                                                                                                       |

==**스레드를 임의 시간 만큼 일시 정지**==: `sleep(long 밀리초)`
일시 정지 상태에서도 InterruptedException 예외가 발생할 수 있으므로 **이를 대비한 예외 처리 필요**
```java
try{ //예외가 발생할 수 있으므로 try 블록 안에 작성
	Thread.sleep(1000); //1000밀리초 만큼 일시 정지
} catch(InterruptedException e) {
	//예외 처리 코드
}
```

예) 3초 주기로 문자열을 출력
```java
for (int i=0; i<10; i++) {
	System.out.println("문자열"); //문자열을 출력한 뒤
	try{
		Thread.sleep(3000); //3초 기다리고 다시 반복
	} catch(InterruptedException e) {}
}
```


==**실행 중인 스레드를 갑작스럽게 종료해야 하는 경우**==
스레드는 가급적 `run()` 메소드를 모두 끝내 <u>자동 종료시키는 것을 목표</u>로 해야 한다.
이를 위해 **stop 플래그**를 이용할 수 있다.
```java
private boolean stop; //stop 플래그 필드

public void run(){
	while(!stop){ //stop == false인 동안 반복
		// 실행 코드
	}
	//스레드가 사용한 자원을 정리
}
```

예)
```java
//스레드
public class WT extedns Thread{
	private boolean stop;
	
	public void setStop(boolean stop){
		this.stop = stop;
	}
	
	public void run(){
		while(!stop){ }
	}
}


//실행 클래스
public class Main{
	public static void main(Stirng[] args){
		WT wt = new WT();
		wt.start(); //wt의 run() 메소드 시작
		
		try{ //run()메소드 시작 후 다음 코드 시작까지 1초 대기
			Tread.sleep(1000);
		} catch(InterruptedException e) {}
		
		wt.setStop(true); //1초 대기가 끝나면 stop=true로 만들어 run() 종료
	}
}
```


==**스레드에 예외가 발생해 일시정지가 되었을 때**==
`interrupt()` 메소드를 사용해 InterruptedException을 발생시켜서 **`run()`를 정상 종료시킴**
```java
public class ThreadB extends Thread{
	public void run(){
		try{
			while(true){
				//반복할 내용
				
				Thread.sleep(1); //예외 발생 시 작동
			}
		} catch(InterruptedException e){ //예외 처리 구문
		}
	}
}

public class ThreadA extends Thread{
	ThreadB threadB = new ThreadB();
	threadB.start();
	
	threadB.interrupt(); //threadB에 예외를 발생시켜 일시 정지 상태로 전환
}
```


### 데몬 스레드(Daemon Thread)
==**데몬 스레드(Deamon Thread)**==: 주 스레드의 작업을 돕는 보조적인 역할을 수행하는 스레드
- **주 스레드가 종료되면 함께 강제 자동 종료**
- 예시) 워드의 자동 저장 / 미디어 플레이어의 재생 기능 등


==**데몬 스레드 생성**==
`setDaemon(true)`
```java
public static void main(String[] args){
	WT wt = new WT();
	wt.setDaemon(true); //데몬 스레드로 생성
	wt.start();
}
```


예시)
```java
//1초 주기로 save() 메소드를 호출하는 데몬 스레드
public class AutoSaveThread extends Thread{
	public void save(){
		System.out.println("작업 내용을 저장");
	}
	
	@Override
	public void run(){
		while(true){
			try{
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				break;
			}
			save();
		}
	}
}


//실행 클래스
public class Main{
	public static void main(String[] args) {
		AutoSaveThread autoSaveThread = new AutoSaveThread();
		autoSaveThread.setDaemon(true);
		autoSaveThread.start();
		
		try{
			Thread.sleep(3000);
		} catch (InterruptedException e) {
		}
		System.out.println("메인 스레드 종료");
	}
}
```