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





