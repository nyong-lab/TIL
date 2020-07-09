# Data Structure
> 인프런 강의  
> JAVA로 배우는 자료구조

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


