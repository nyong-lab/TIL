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

## 문자열
### 문자열 출력
`System.out.println("안녕 : " + java);`
### 문자열 입력
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
### 두 문자열의 동일성 비교
str1.equals(str2) -> 두 문자열의 값이 동일하면 true, 두 값이 다르면 false  
str1.equalsIgnooreCas(str2) → 두 문자열의 대소문자를 무시하고 동일한지 판단, 출력값은 위와 같음.    
```java
if( name[j].compareTo( names[j+1] ) > 0 ) //첫번째 비교대상의 사전식 영어 순서가 더 높으면
if( name[j].compareTo( names[j+1] ) == 0 ) //두 문자열의 사전식 영어 크기가 동일하면( a == a)
if( name[j].compareTo( names[j+1] ) < 0 ) //두번째 대상의 사전식 영어 순서가 더 높으면
```
### text file 출력하기
```java
PrintWriter out = new PrintWriter( new FileWriter(fileName) ); 
```
### String Class 기본 메서드
* 문자열 동일성 검사
**boolean, equals(String)**  
```java
String str1 = "java";
String str2 = "Java";
boolean equals = str1.equals(str2); //대소문자 다르므로 두 문자열이 다르다 -> false
boolean equalsIgnoreCase = str1.equals(str2); //대소문자를 무시하므로 두 문자열은 같다 -> ture
```
* 문자열 사전식 순서
**int, compareTo(String)**
```java
String str1 = "absolute";
Stirng str2 = "base";
int result = str1.compareTo(str2); //str1의 사전식 크기가 더 크다(result = 양수)
```
* 문자열 길이
**int, length()**
```java
String str = "abcdef";
int length = str.length(); //문자열은 총 6개의 문자로 이루어져 있음 -> length = 6
```
* 특정 위치의 문자
**char, charAt(int)**
```java
String str = "ABCDEFG";
char ch = str.charAt(2); //(0번째부터 세므로)문자열의 두번째 자리에는 C가 있음.
```
* 지정한 문자의 위치 검색
**int, indexOf(char)**
```java
String str = "abcdef";
int index = str.indexOf("d"); //d는 str의 세번째 자리에 존재하므로 index = 3;
//만약 d가 없다면 음수 리턴(-1)
```
* 지정된 범위의 부분 문자열
**String, substring(int,int)**
```java
String str = "ABCDEF";
String substr = str.substring(0,2);
/*
"ABC"의 결과물이 나올 것 같지만, substring(i,j); 라고 되어있다고 치면
'i보다는 크거나 같고 j보다는 작다'의 뜻이기 때문에 결론적으로 "AB"가 저장된다.
```
















