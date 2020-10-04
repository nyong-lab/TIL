# Data Structure

## Eclipse, JAVA 개발환경

자바에서는 **클래스**가 프로그램을 구성하는 기본 단위이며, 메서드와 변수 모두 클래스 외부에 선언할 수 없다.  
변수를 클래스 내 메서드 외부에 생성한다면 **전역변수**와 같은 역할을 하고, 메서드 안에 선언한 변수는 **지역변수**와 같은 역할을 한다.  
메서드 외부에 변수/배열 선언할 때는, static을 붙혀주어야 사용할 수 있다. (이유는 다음 강의에서 다룰 예정)  

클래스 명을 지을 때 첫글자는 **대문자**로 시작하도록 하고, **클래스 이름**과 **클래스 파일명**(xxx.java)이 같아야 한다.  
만약 클래스의 이름을 수정하고 싶으면, *Refactor - rename* 사용해서 바꿔주기!  
변수명 변경할 때 Refactor 사용해주면 코드상에 존재하는 같은 변수명이 한번에 같이 바뀐다 :thumbsup:  

Q. import 에러가 났는데 무엇을 import 해야할 지 모르겠다면?  
A. *Source - Organize Imports* 해주면 자동으로 불러와진다!  

## 배열

```java
int [] numbers; //numbers라는 배열을 정수형으로 선언
numbers = new int [10]; //정수가 들어갈 수 있는 열개의 방을 만들어 준다
int [] numbers = new int [10]; //한줄로 합쳐 표현
```

# 문자열

## 문자열 출력
`System.out.println("안녕 : " + java);`

## 문자열 입력
```java
//키보드로 값 입력
//정수값을 받아오고 싶다면 sc.nextInt 사용, 문자열을 읽어오고 싶다면 sc.next 사용
Scanner sc = new Scanner(System.in);

//text파일로 값 입력
Scanner sc = new Scanner( new File("input.txt") );

//text파일의 끝을 읽는 메서드
while( sc.hasNext() ){

}

//try - catch문
try{
//실행문
}
catch(FileNotFoundException e){
//파일을 찾을 수 없을 때 어떻게 할 것인지
System.exit(1); //프로그램 종료
}
```

## 두 문자열의 동일성 비교
str1.equals(str2) -> 두 문자열의 값이 동일하면 true, 두 값이 다르면 false  
str1.equalsIgnooreCas(str2) → 두 문자열의 대소문자를 무시하고 동일한지 판단, 출력값은 위와 같음.    
```java
if( name[j].compareTo( names[j+1] ) > 0 ) //첫번째 비교대상의 사전식 영어 순서가 더 높으면
if( name[j].compareTo( names[j+1] ) == 0 ) //두 문자열의 사전식 영어 크기가 동일하면( a == a)
if( name[j].compareTo( names[j+1] ) < 0 ) //두번째 대상의 사전식 영어 순서가 더 높으면
```

## text file 출력하기
```java
PrintWriter out = new PrintWriter( new FileWriter(fileName) ); 
```

## String Class 기본 메서드
* 문자열 동일성 검사 `boolean, equals(String)`
```java
String str1 = "java";
String str2 = "Java";
boolean equals = str1.equals(str2); //대소문자 다르므로 두 문자열이 다르다 -> false
boolean equalsIgnoreCase = str1.equals(str2); //대소문자를 무시하므로 두 문자열은 같다 -> ture
```

* 문자열 사전식 순서 `int, compareTo(String)`
```java
String str1 = "absolute";
Stirng str2 = "base";
int result = str1.compareTo(str2); //str1의 사전식 크기가 더 크다(result = 양수)
```

* 문자열 길이 `int, length()`
```java
String str = "abcdef";
int length = str.length(); //문자열은 총 6개의 문자로 이루어져 있음 -> length = 6
```

* 특정 위치의 문자 `char, charAt(int)`
```java
String str = "ABCDEFG";
char ch = str.charAt(2); //(0번째부터 세므로)문자열의 두번째 자리에는 C가 있음.
```

* 지정한 문자의 위치 검색 `int, indexOf(char)` 
```java
String str = "abcdef";
int index = str.indexOf("d"); //d는 str의 세번째 자리에 존재하므로 index = 3;
//만약 d가 없다면 음수 리턴(-1)
```

* 지정된 범위의 부분 문자열 `String, substring(int,int)`
```java
String str = "ABCDEF";
String substr = str.substring(0,2);
/*
"ABC"의 결과물이 나올 것 같지만, substring(i,j); 라고 되어있다고 치면
'i보다는 크거나 같고 j보다는 작다'의 뜻이기 때문에 결론적으로 "AB"가 저장된다.
```

# 클래스, 객체, 참조변수

## 클래스
* 서로 관련있는 데이터들을 하나의 단위로 묶어둔 것  
* int, double과 같이 하나의 데이터 `타입`이다.  
> 다만 int,double처럼 Java가 미리 정해놓은 타입이 아니라, 사용자가 정의한 새로운 타입이라는 의미에서  
> `사용자 정의 타입` 이라고 부르기도 한다.  
> int 혹은 double형 변수를 선언하고 사용하는 것처럼 Person1형 변수를 선언하고 사용
```java
int count = 0;
Person1 first = new Person1();
Person1 [] members = new Person1[100];
```
* 프리미티브 타입과 클래스의 차이점  

**Primitive Type**  
```java
int count = 0;
//count라는 이름의 정수형 변수가 만들어지고, 그 "안"에 정수값 0이 저장된다.
```

**Class**
```java
Person1 first = new Person1();
//first라는 이름의 변수가 만들어지지만 그 "안"에 사람 이름과 전화번호가 저장되지는 않는다.
//이름과 전화번호를 저장할 "Person1 객체(Object)"는 new명령으로 따로 만들어야하고,
//변수 first에는 그 객체의 주소(참조)를 저장할 수 있다.
```
> 모든 프리미티브 타입의 변수는 `보통 변수`이다. 즉, 변수 자체에 값이 저장된다.  
> 프리미티브 타입이 아닌 모든 변수는 `참조 변수`이다.  
> 즉, 실제 데이터가 저장될 `객체`는 `new 명령`으로 따로 만들어야 하고, 참조변수에는 그 `객체의 주소`를 저장한다!  

## 배열과 참조변수
### int [] numbers = new int[8];  
프리미티브 타입의 배열이라고 하더라도 배열의 각 원소가 프리미티브 타입인 것이지 배열 자체가 프리미티브 타입인 것은 아니다.  
따라서 배열의 이름 numbers는 참조 변수이다. 8칸을 가진 배열이 실제로 만들어지고, 그 주소가 참조변수 numbers에 저장된다.  

### Person1 [] members = new Person1[8];  
members는 배열의 이름이므로 당연히 참조 변수이다. 배열의 각 칸은 Person1 타입이다.  
그런데 Person1은 프리미티브 타입이 아니다. 따라서 배열의 각 칸도 참조 변수이다.  
즉, 이 상태에서 배열의 각 칸에 다음과 같이 바로 이름과 번호를 저장할 수는 없다.  

```java
//저장 안됨
members[2].name = "John";
members[2].number = "01084394827";

//이렇게 해야함!
members[2] = new Person1();
members[2].name = "John";
members[2].number = "01038372937";
```

### 값에 의한 호출 : 배열  

```java
//호출문
bubbleSort(data,count);

//호출된 메서드
public static void bubbleSort(int[] data2, int n)
} 
```

`1` 호출하는 순간 변수 data의 값이 호출된 메서드의 매개변수 data2로 `복사`된다.  
`2` 그런데 복사된 값은 배열의 주소이다. 그러므로 data와 data2는 실제로 동일한 배열을 가리키게 된다.  
`3` 따라서 배열 data2의 내용을 변경하면 배열 data의 내용도 변경된다.  

> 즉, 배열을 매개변수로 넘겨주고 호출된 메서드에서 배열의 값을 수정하면 원본 배열의 값도 수정되는 것은  
> "값에 의한 호출"의 예외가 아니라 배열의 이름이 참조변수이기 때문에 벌어진 일이다.  

# 메서드와 생성자

클래스는 서로 관련있는 데이터들을 하나의 단위로 묶어두기 위한 것인데, 데이터뿐 아니라 그 데이터와 관련이 깊은 `메서드` 도 함께 묶어둘 수 있다.  
이렇게 함으로써 코드의 `응집도(Cohesion)` 를 높이고, `결합도(Coupling)` 를 낮출 수 있다.  

## 객체

객체지향 프로그래밍에서 `객체` 란 `"데이터" + "메서드"` 이다.  
데이터는 객체의 `정적 속성(ex - 계수와 차수)` 을 표현하며,  
메서드는 객체의 기능 `동적 속성(ex - x의 값을 주면 자신의 값을 계산해준다)` 을 표현한다.  

## 생성자

```java
class Term3{
  int coef;
  int expo;
  
  public Term3(int c,int e){
    coef = c;
    expo = e;
  }
}
```

클래스와 동일한 이름을 가지며, return 타입이 없는 메서드를 `생성자(Constructor)` 라고 한다.  
> new 명령으로 객체가 생성될 때 자동으로 실행된다(호출이 필요하지 않음)  
> 주 목적은 객체의 데이터 필드값을 초기화하기 위함이다.  

매개변수의 타입과 갯수만 구분할 수 있으면 여러개의 생성자를 가질 수 있다.  

```java
public Polynomial3(){
  nTerms = 0;
  terms = new Term3[100];
}

public Polynomial3(char name){
  this.name = name;
  //받는 매개변수의 이름과 객체에 선언된 데이터(?)의 이름이 같을 경우,
  //this를 사용해서 이 객체의 데이터라는 것을 명시해줄 수 있다.
  nTerms = 0;
  terms = new Term3[100];
}
```

생성자가 없을경우 객체를 생성한 후 따로 값을 초기화 해주어야 하지만,  
생성자를 만들 경우 생성과 초기화가 한번에 가능하다.  

# Static 그리고 Public

## Static

[Static에 대해 잘 정리해둔 블로그](https://mangkyu.tistory.com/47)  

클래스는 실체가 아니므로 클래스의 데이터 필드에 데이터를 저장할 수 없고, 클래스의 멤버 메서드를 실행할 수 없다.  
new명령으로 해당 클래스 타입의 객체를 만든 후, 그 객체의 데이터를 저장하고, 그 객체의 멤버 메서드를 실행하는 것이다.  

여기에 한가지 `예외` 가 있는데, 그것이 바로 `static` 이다. `static` 멤버는 클래스 안에 실제로 존재하며, 객체에는 존재하지 않는다.  

### Static VS Non-static 

static 멤버는 class 멤버이고, non-static 멤버는 object 멤버이다.  

```java

public class Test {
  public static int s = 0;
  public int t = 0;

  public static void print1(){
    System.out.println("s = " + s);
  }

  public void print2(){
    System.out.println("t = " + t);
  }
}

public class TestTest {
  Test test1 = new Test();
	
  test1.t = 10;
  test1.s = 100; //Test.s = 100; 이라고 쓰는것이 더 자연스러움. 이해를 돕기위해 이렇게 쓴 것!
  test1.print2();

  Test test2 = new Test();
	
  test2.print1(); //마찬가지로 Test.print1(); 더 자연스러움!
  test2.print2();
}

/* 결과는 t = 10, s = 100, t = 0 이 나온다.
 * s는 class멤버이므로, A객체에서 값을 바꿔버려도 B객체에서도 바뀐 값이 똑같이 나오는 것!
 
```

### Static 메서드/필드의 용도

`1` `main 메서드` 는 반드시 `static` 이어야 함.  
`2` 상수 혹은 클래스당 하나만 유지하고 있으면 되는 값(또는 객체) *(ex) Math.PI, System.out*    
`3` 순수하게 기능만으로 정의되는 메서드. 대표적인 예로는 수학 함수들  

## 접근제어자

`public` 클래스 외부에서 접근이 가능하다  
`private` 클래스 내부에서만 접근이 가능하다  
`default` 동일 패키지에 있는 다른 클래스에서 접근 가능하다  
`protected` 동일 패키지의 다른 클래스와 다른 패키지의 하위클래스에서도 접근 가능하다  

### 데이터 캡슐화(data encapsulation/information hiding)

중요한 데이터를 보존하고 보호하기 위함. 모든 데이터 멤버를 private으로 만들고 필요한 경우에 public한 get/set 메서드를 제공한다.  
이렇게 하면 객체가 제공해주는 메서드를 통하지 않고서는 객체 내부의 데이터에 접근할 수가 없다.  

```java

public class abc {
  public int a;
  public int b;
  
  public getB(){ //외부에서 데이터를 읽을 수만 있고 쓸 수 없음(getter method)
    return b;
  }
  
  public setB(int b){ //외부에서 데이터를 써줌(setter method)
    this.b = b;
  }
}

```

# 상속

기존에 어떠한 클래스를 가지고있는데, 이 클래스와 `IS-A 관계` 에 있고 자신만의 `새로운 속성` 을 가진 클래스를 만들 필요가 있을 때 사용한다. 

> **[IS-A 관계]**
> 사람은 이름과 나이, 주소를 가지고 있다. 학생은 사람이다. 따라서 이름과 나이, 주소를 가지고 있으며 학번도 가지고 있다.  
> 하지만 모든 사람이 학생은 아니고, 학번을 가지고 있는 것은 아니다.  

```java
public class Computer {
	
	public String manufacturer;
	public String processor;
	public int ramSize;
	public int diskSize;
	public double processorSpeed;
	
	public Computer(String man, String proc, int ram, int disk, double procSpeed) {
		manufacturer = man;
		processor = proc;
		ramSize = ram;
		diskSize = disk;
		processorSpeed = procSpeed;
	}
	
	public double computePower() {
		return ramSize * processorSpeed;
	}
	
	public double getRamSize() {
		return ramSize;
	}
	
	public double getProcessorSpeed() {
		return processorSpeed;
	}
	
	public int getDiskSize() {
		return diskSize;
	}
	
	public String toString() {
		String result = "Manufacturer: " + manufacturer +
						"\\nCPU: " + processor +
						"\\nRAM: " + ramSize + " megabytes" +
						"\\nDisk: " + diskSize + " gigabytes" +
						"\\nProcessor speed: " + processorSpeed + " gigahertz";
		
		return result;
	}

}

public class Notebook extends Computer {

	/* 컴퓨터 클래스의 데이터, 메서드를 모두 가지고 있게 된다. 눈에 보이지 않을 뿐!
	 * 컴퓨터는 super class(=base class, parent class)
	 * 노트북은 sub class(=extended class, child class)
	 */

}
```

### 상속과 생성자

자바에서 모든 클래스는 `반드시 생성자` 를 가진다.  
생성자가 없을 경우 눈에 보이지는 않지만 자동으로 `no-parameter 생성자` 가 만들어짐!  
만약 생성자가 하나라도 있을 경우 자동으로 만들어지지 않는다.  

모든 서브 클래스의 생성자는 먼저 수퍼클래스의 생성자를 호출한다.  
`1` super(...)을 통해 명시적으로 호출해 주거나  
`2` 그렇지 않을 경우에는 자동으로 no-parameter 생성자가 호출된다.  

*흔한 오류 : 수퍼클래스에 no-parameter 생성자가 없는데, 서브클래스의 생성자에서 super(...) 호출을 안해주는 경우*  

```java
public class Notebook extends Computer {
	
	public double screenSize;
	public double weight;
	
	public Notebook(String man, String proc, int ram, int disk, double procSpeed, double screen, double weight) {
		
		super(man, proc, ram, disk, procSpeed);
		/* super(arguments) 
		 * 수퍼클래스의 생성자 중에서 매개변수 리스트가 일치하는 생성자를 호출한다.
		 * super()를 호출할 경우 반드시 생성자 내에서 첫 문장이어야 한다.
		 */

		screenSize = screen;
		this.weight = weight;
		
	}
	
	public static void main(String [] args) {
		
		Notebook test = new Notebook( "Dell", "i5", 4, 1000, 3.2, 15.6, 1.2 );
		System.out.println(test.computePower());
	
	}
	

}
```

## Overriding

서브클래스가 수퍼클래스에서 선언된 것과 같은 메서드를 가질때 메서드 오버라이딩이라고 한다.  수퍼클래스에서 사용했었던 메서드에 특정 구현을 부여하는데 사용된다.

```java
public String toString() {

		//수퍼클래스의 모든 내용을 변경하고 싶다면 처음부터 다시 써주면 된다.
		String result = "Manufacturer: " + manufacturer +
										"\\nCPU: " + processor +
										"\\nRAM: " + ramSize + " megabytes" +
										"\\nDisk: " + diskSize + " gigabytes" +
										"\\nProcessor speed: " + processorSpeed + " gigahertz" +
										"\\nScreen Size: " + screenSize + " inches" +
										"\\nWeight: " + weight + " kg";
	
		//하지만 수퍼클래스의 내용의 뒷부분에 덧붙히기만 한다면 이런식으로 코드를 작성하는게 깔끔하다.
		String result = super.toString() +
							"\\nScreen Size: " + screenSize + " inches" +
							"\\nWeight: " + weight + " kg";
		
		return result;
	}
```

# 다형성(Polymorphism)

수퍼클래스 타입의 참조변수가 서브클래스 타입의 객체를 참조할 수 있다.

```java
Computer theComputer = new Notebook("Bravo", "Intel", 4, 240, 2/4, 15.07.5);
```

theComputer는 Computer 타입의 변수이면서 실제로는 Notebook 개체를 참조하고 있다. 그리고 두 클래스는 각자의 toString() 메서드를 가지고 있다. 그렇다면 여기서 둘 중 어떤 toString() 메서드가 실행될까? Notebook 클래스의 toString() 메서드가 실행된다. 즉, `동적 바인딩(Dynamic binding)` 이 일어난다.

# class Object, Wrapper class

## class Object

클래스 Object는 Java에서 **모든 클래스의 superclass**이다. Java의 모든 클래스는 만들어주지 않아도 이미 equals와 toString 메서드를 가지고 있다. 단, 내 의도대로 작동하지는 않을 것이다(= 오버라이드 해야함)

<class Object의 멤버 메서드>

1. boolean equals(Object obj)
2. int hashCode()
3. String toString()
4. Class<?> getClass()

### toString()

만약 `toString()` 메서드를 따로 만들어주지 않은 클래스의 객체에 대해서 `toString()` 메서드를 호출하면 다음과 같은 String이 반환된다.

- Section3.Test@d716361(클래스 이름@객체의 hash code)

### equals(Object)

Object 클래스의 equals 메서드의 매개변수는 Object 타입이다

```java
public boolean equals (Object other) {

}
```

매개변수로 제공된 객체와 자기 자신의 동일성을 검사하며, 의도대로 사용하려면 오버라이드 해야한다.

```java
//class Person에서 equals 메서드를 override한 예

@Override
public boolean equals(Object obj){
	if(obj == this) return true;
	if(obj == null) return false;
	if(this.getClass() == obj.getClass()){
		Person other = (Person) obj;
		//두 Person 객체가 name이 동일하면 동일하다고 할 것인지, number까지 동일해야 동일하다고 할지는 내가 결정할 문제!
		return name.equals(other.name) && number.equals(other.number);
	} else {
		return false;
	}
}
```

### Class Class

모든 클래스는 하나의 Class 객체를 가진다. 이 객체는 각각의 클래스에 대해서 유일하다. 메서드 `getClass()` 는 Object 클래스가 제공하는 메서드이며, 이 유일한 Class 객체를 반환한다. 만약 앞의 예제에서 `this.getClass() == obj.getClass()` 가 true라면 우리는 비교 대상인 두 객체(this와 obj)가 동일한 클래스의 객체임을 알 수 있다.

# 추상클래스와 인터페이스

## Wrapper class

Java에서 primitive type 데이터와 non-primitive type 데이터, 즉 객체는 근본적으로 다르게 처리된다. 가령 Object 타입의 배열에는 다형성의 원리에 의해서 모든 종류의 객체를 저장할 수 있다. 하지만, int, double, char 등의 primitive type 데이터는 객체가 아니므로 저장할 수 없다.

때때로 primitive type 데이터를 객체로 만들어야 할 경우가 있다. 이럴 때 `Integer, Double, Character` 등의 `wrapper class` 를 이용한다.

기본 타입의 데이터를 하나의 객체로 포장해주는 클래스 : Integer, Double, Boolean 등

```java
Object [] array = new Object[100];
int a = 20;
Integer age = new Integer(a); //wrapping
array[0] = age;
int b = age.intValue(); //unwrapping
System.out.println(b);

* Autoboxing과 Unboxing
Object [] theArray = new Object[100];
//JAVA 컴파일러가 자동으로 Integer 객체로 변환해준다
theArray[0] = 10; 
//theArray[0]에 저장된 것은 Integer 객체이지만 JAVA 컴파일러가 자동으로 정수로 변환해준다.
int a = (Integer)theArray[0]; 
```

데이터 타입간의 변환 기능을 제공

```java
String str = "1234";
int d = Integer.parseInt(str);
```

## abstract class

- 추상(abstract) 메서드는 선언만 있고 구현이 없는 메서드
- 추상 메서드를 포함한 클래스는 추상 클래스
- 추상 메서드와 추상 클래스는 키워드 abstract로 표시
- 추상 클래스는 객체를 만들 수 없음. 따라서 서브 클래스를 만드는 용도로만 사용됨

```java
public abstract class Event {
	
	public String title;
	public Event(String title) {
		this.title = title;
	}

	public abstract boolean isRelevant(MyDate date);

}
```

Event를 상속한 oneday, duration, deadline 클래스의 객체는 Event 배열에 들어갈 수 있다. 근데 각각의 객체 안에 들어있는 메서드를 사용하려고 하면 컴파일 오류가 나게 된다. Event 클래스에는 그 메서드가 없기 때문! 해결하기 위해서는 Event 안에 그 메서드를 입력해주면 되지만,  동적 바인딩으로 인해 사실상 그 메서드에 들어가 있는 코드는 사용되지 않는다. 따라서 추상 메서드를 사용한다. 추상 메서드를 사용하면 그 클래스 역시 추상 클래스가 되며, 객체를 생성할 수 없게 된다.

## Interface

- 추상 메서드만을 가진 순수한 추상 클래스
- static final 데이터 멤버(상수)를 가질 수 있음

```java
public interface Payable{
	public double calcSalary();
	public boolean salaried();
	public static final double DEDUCTIONS = 25.5;
}

public class Professor implements Payable{
	public boolean calcSalary(){...}
	public boolean salaried(){...}
}

* Payable 인터페이스를 구현(implements)하는 클래스는 메서드 calcSalary와 salaried를 실제로 구현해야 한다.
```

# Generic Programming

제너릭 프로그래밍은 **데이터 형식에 의존하지 않고**, 하나의 값이 여러 다른 데이터 타입들을 가질 수 있는 기술에 중점을 두어 재사용성을 높일 수 있는 프로그래밍 방식

### Generic한 변수/자료구조

```
Event ev;
Event[] events = new Event[capacity];
Object obj;
```

### Generic한 알고리즘(method)

```
Arrays.sort(shapes,0,n);
```

### Generic 클래스

```
Generics
```

## Generics

```java
//T라는 가상의 타입에 의해서 parameterized된 클래스
public class Box<T>{ 
	private T t;
	public void set(T t){ this.t = t; }
	public T get(){return t;}
}

//객체를 생성하는 시점에 가상의 타입 T를 실제하는 타입으로 지정해준다
Box<Integer> integerBox = new Box<Integer>();
integerBox.set(new Integer(10));

Box<Event> eventBox = new Box<Event>();
eventBox.set(new OneDayEvent("dinner", new MyDate(2017,2,10)));

//2개 이상의 type parameter를 가질 수도 있다.
public class Pair<K,V>{
	private K key;
	private V value;
	public void set(K key, V value){this.key = key; this.value = value}
	public K getKey(){return key;}
}

//객체를 생성하는 시점에 가상의 타입 K와 V를 실제하는 타입으로 지정해준다.
Pair<String, Integer> p1 = new Pair<String, Integer>();
p1.set("Even", 9);
Pair<String, String> p2 = new Pair<String, String>();
```

### class Object vs Generics

```java
//class Object
//get()의 return type은 Object이므로 반드시 type casting을 해주어야 한다
public class Box{
	private Object t;
	public void set(Object t){this.t = t;}
	public Object get(){return t;}
}

Box box = new Box();
box.set(new Integer(10));
Integer a = (Integer)box.get();

//Generics
public class Box<T>{ 
	private T t;
	public void set(T t){ this.t = t; }
	public T get(){return t;}
}

Box<Integer> box = new Box<Integer>();
box.set(new Integer(10));
Integer a = box.get();
```

# 연결리스트

## 리스트(list)

데이터간의 순서관계가 있음. 기본적인 연산에는 **삽입(Insert), 삭제(remove), 검색(search)** 등이 있다. 리스트를 구현하는 대표적인 두 가지 방법에는, 배열과 연결리스트가 있다.

### 배열 vs 연결리스트

`배열`은 **크기가 고정**되어 **reallocation**이 필요하며, 리스트의 중간에 원소를 삽입하거나 삭제할 경우 **다수의 데이터를 옮겨야한다**. 그에비해 `연결리스트`는 **다른 데이터의 이동없이** 중간에 삽입하거나 삭제할 수 있으며, 길이의 제한이 없다. 하지만, 랜덤 액세스(배열의 최대 장점)가 불가능하다. 각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식의 자료구조이며, 데이터를 담고 있는 노드들이 연결되어 있고, 노드의 포인터가 이전, 다음 노드와의 연결을 담당한다. **즉, 탐색 또는 정렬을 자주하는 경우에는 배열을 사용하고, 데이터의 추가/삭제가 많은 경우 연결 리스트를 사용하는 것을 권장한다.**

### 노드

각각의 노드는 **데이터 필드**와 하나 혹은 그 이상의 **링크 필드**로 구성된다. 링크 필드는 **다음 노드**를 참조하며, **첫 번째 노드의 주소는 따로 저장**해야 한다.

```java
public class Node<T> {
	public T data;
	public Node<T> next;
	
	public Node(T item) {
		data = item;
		next = null;
	}
}
```

```java
public class MySingleLinkedList<T> {
	
	public Node<T> head; //첫번째 노드의 주소
	public int size; //보유하고 있는 노드의 갯수
	
	public MySingleLinkedList() {
		head = null;
		size = 0;
	}
	
	public void addFirst(T item) {
		
		Node<T> newNode = new Node<T>(item); // T : type parameter
		newNode.next = head;
		head = newNode;
		size++;
		
	}
	
	public void addAfter( Node<T> before, T item) {
		Node<T> temp = new Node<T>(item);
		temp.next = before.next;
		before.next = temp;
		size++;
	}
	
	public void add(int index, T item) { // insert
		
		if(index < 0 || index > size)
			return;
		if(index == 0)
			addFirst(item);
		else {
			Node<T> q = getNode(index-1);
			addAfter(q,item);
		}
	
	}
	
	public T removeFirst() {
		
		if(head == null)
			return null;
		
		T temp = head.data;
		head = head.next;
		size--;
		return temp;
		
	}
	
	public T removeAfter( Node<T> before) {
		
		if(before.next == null)
			return null;
		
		T temp = before.next.data;
		before.next = before.next.next;
		size--;
		return temp;
		
	}
	
	public T remove(int index) { // delete

		if( index < 0 || index >= size )
			return null;
		if(index == 0)
			return removeFirst();
		Node<T> prev = getNode(index-1);
		return removeAfter(prev);	
		
	}
	
	public T remove(T item) {
		
		Node<T> p = head, q = null;
		while(p!=null && !p.data.equals(item)) {
			q = p;
			p = p.next;
		}
		
		if(p==null)
			return null;
		if(q==null)
			return removeFirst();
		else 
			return removeAfter(q);
		
	}
	
	public Node<T> getNode(int index) {
		
		if(index < 0 || index >= size)
			return null;
		Node<T> p = head;
		for(int i=0; i<index; i++)
			p = p.next;
		return p;
		
	}
	
	public T get(int index) {
		
		if(index < 0 || index >= size)
			return null;
		
		return getNode(index).data;
		
	}
	
	public int indexOf(T item) { // search
	
	Node<T> p = head;
	int index = 0;
	
	while( p != null ) {
		if(p.data.equals(item))
			return index;
		p = p.next;
		index++;
	}
		return -1;
		
	}
	
	public static void main(String[] args) {
		
		MySingleLinkedList<String> list = new MySingleLinkedList<String>();
		
		list.addFirst("Saturday");
		list.addFirst("Friday");
		list.add(2, "Monday");
		list.add(2, "Tuesday");
		list.remove("Friday");
		int index = list.indexOf("Saturday");
		
	}
}
```

- 연결 리스트를 다루는 프로그램에서 가장 주의할 점은 내가 작성한 코드가 일반적인 경우만이 아니라 특수한 혹은 극단적인 경우에도 문제 없이 작동하는지 철저히 확인하는 것이다. 이 경우에는 기존의 연결 리스트의 크기가 0인경우, 즉 head가 null인 경우에도 문제가 없는지 확인해야 한다.
- 단순연결리스트에 새로운 노드를 `삽입`할 때 삽입할 위치의 바로 앞 노드의 주소가 필요하다. 즉, **어떤 노드의 뒤에 삽입하는 것은 간단하지만**, 반대로 **어떤 노드의 앞에 삽입하는 것은 간단하지 않다**. 어떤 노드를 `삭제`할 때는, 삭제할 노드의 바로 앞 노드의 주소가 필요하다. 삭제할 노드의 주소만으로는 삭제하기 힘들다.
- 연결리스트의 노드들을 처음부터 순서대로 방문하는 것을 `순회(traverse)` 라고 한다. indexof 메서드는 입력된 데이터 item과 동일한 데이터를 저장한 노드를 찾아서 그 노드번호(index)를 반환한다. 그것을 위해서 연결리스트를 순회한다.

