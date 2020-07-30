# 화면 전환하기

## 레이아웃 인플레이션

안드로이드 앱은, 화면 배치를 알려주는 XML 레이아웃 파일과  
화면의 기능을 담당하는 소스코드 파일로 분리되어 있다.  

```java
public class Main Activity extends AppCompatActivity {

  @Override
  protect void onCreate(Bundle saveInstanceState){
    super.onCreate(saveInstanceState);
    setContentView(R.layout.activitiy_main);
    //setContentView 메서드가 XML레이아웃 파일을 연결한다고 추측할 수 있다.
    //R.layout.레이아웃 파일명(확장자X)
    }
    
}
```

앱이 실행될 때 XML 레이아웃의 내용이 메모리에 객체화 되고, 객체화된 XML 레이아웃을 소스파일에서 사용한다.  
XML레이아웃의 내용이 메모리에 객체화되는 과정을 `인플레이션(Inflation)` 이라고 한다. 앱이 실행되는 시점에 메모리에 객체화 된다. 

`setContentView()` 메서드는 `1` 화면에 나타낼 뷰를 지정하거나 `2` 레이아웃 내용을 메모리에 객체화하는 두가지 역할을 수행하는데,  
액티비티의 화면 `전체` 를 설정하는 역할만 수행하지 부분 레이아웃을 메모리에 객체화할 수 없다. -> 인플레이터 사용해야 함.  
이를 위해 안드로이드는 `LayoutInflater` 라는 클래스를 제공하는데, 시스템 서비스(단말 시작되면서 항상 실행되는 서비스)로 제공하는 클래스이므로  
`getSystemService()` 메서드를 이용해 `LayoutInflater` 객체를 참조한 후 사용해야 한다.  

```java
public class Main Activity extends AppCompatActivity {
  ConstraintLayout container;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_menu);

        //메인 XML파일에서 부분 레이아웃이 추가될 레이아웃 이름 container로 지정
        container = findViewById(R.id.container);
        //버튼을 클릭했을 때 호출되는 onClick()메서드 안에서 container변수 참조할 수 있음.

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener(){
            @Override
            public void onClick(View view){
            
                LayoutInflater inflater = (LayoutInflater)
                                    getSystemService(Context.LAYOUT_INFLATER_SERVICE);
                //LayoutInflater 객체 참조
                inflater.inflate(R.layout.sub1, container, true);
                //container을 id로 갖는 레이아웃 객체에 sub1.xml 파일의 레이아웃을 설정
                //이 과정을 통해 부분 레이아웃에 정의된 뷰들이 메모리에 로딩되며 객체화 과정을 거치게 됨
                CheckBox checkBox = container.findViewById(R.id.checkBox);
                checkBox.setText("로딩되었어요.");
                //부분 레이아웃은 container 객체에 설정되었으므로 container 객체의 findViewById() 메서드를 사용해야 함.
            }
        });
    }
    
}
```

XML레이아웃으로 화면 형태(부분 레이아웃)을 만드는 것이 일반적이지만, 직접 new Button(this)과 같은 코드를  
부모 컨테이너에 추가할 수도 있다. 하지만 소스코드의 양이 많아지고 복잡해지므로 XML레이아웃을 사용하는 것이 좋다.  

`View inflate(int resource, ViewGroup root)`  
inflate()메서드는 첫 번째 파라미터로 XML레이아웃 리소스를, 두번째 파라미터로 부모 컨테이너를 지정한다.  
`static LayoutInflater LayoutInflater.from(Context context)`  
getSystemService()메서드를 호출하는 방법을 사용하거나, LayoutInflater클래스의 from()메서드를 호출하여 참조할 수 있다.  

레이아웃 객체화 과정은 앱이 실행될 때(런타임) 레이아웃 XML파일에 정의된 내용들이 메모리에 객체화되기 때문에 매우 중요하며,  
이 과정을 `인플레이션` 이라고 한다. 명확하게 이해해야함!  

## 여러 화면 만들고 화면 간 전환하기

대부분의 앱은 여러 화면으로 구성되어 있고, 화면을 전환하며 실행된다.  
화면은 액티비티로 구현되므로, 액티비티를 전환하는 것과 같다.  

### 안드로이드 앱의 기본 구성 요소
`액티비티(Activity)` `서비스(Service)` `브로드캐스트 수신자(BroadcastReceiver)` `내용 제공자(Content Provider)`  
앱을 만들어 단말에 설치했을 때 안드로이드 시스템이 이 요소에 대한 정보를 요구한다.  
`Android Manifest.xml` 파일이 이 정보들을 담고있으며, 외에 다양한 정보가 들어간다.  

`<activity>` 태그는 우리가 만든 액티비티에 대한 정보를 포함하고 있으며, 새로운 액티비티를 만들어 앱에 추가한다면  
이 매니페스트 파일에 새 액티비티 정보를 추가해야 한다. 그래야 시스템이 새 액티비티에 대한 정보 알 수 있음!  

### 액티비티

`startActivity()` 메서드를 사용하면 소스 코드에서 액티비티를 띄울 수 있다.  

실제로 앱을 만들다보면 단순히 액티비티를 띄워주는 것이 아니라 어떤 액티비티를 띄운 것인지,  
띄웠던 액티비티로부터 다시 원래의 액티비티로 돌아오면서 응답을 받아 처리하는 코드가 필요해진다.  
-> `startActivityForResult()` 메서드

```java
//메인 액티비티 위에 열리는 액티비티
public class MenuActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_menu);

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener(){
            public void onClick(View v){
                Intent intent = new Intent(); //Intent 클래스를 사용해 객체를 만듬
                intent.putExtra("name", "mike"); //name의 값을 부가 데이터로 넣음
                setResult(RESULT_OK,intent); //응답 보내기
                finish(); //현재 액티비티 없애기
            }
        });
    }
}
```
`setResult()` 메서드를 호출할 때 인텐트 객체가 파라미터로 전달되며,  
새로 띄운 액티비티에서 이전 액티비티로 인텐트를 전달하고 싶을 때 사용하는 메서드이다.  

`setResult(응답 코드, 인텐트)`  
응답 코드와 인텐트는 새 액티비티를 띄운 원래 액티비티에 전달됨.  

```java
//메인 액티비티
public class MainActivity extends AppCompatActivity {
    public static final int REQUEST_CODE_MENU = 101;
    //새 액티비티 띄울 때 보낼 요청 코드, 마음대로 지정해도 되지만 액티비티가 여러개라면 중복되지 않게 설정!
    //새 액티비티로부터 응답을 받을 때 다시 전달받을 값. -> 어떤 액티비티로부터 온 응답인지 구분 가능해진다.

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        //새로 띄운 액티비티로부터 받은 응답을 처리하는 메서드
        //requestCode는 액티비티를 띄울 때 전달했던 코드와 같음. 어떤 액티비티로부터 응답을 받은 것인지 구분 가능.
        //resultCode는 새 액티비티로부터 전달된 응답 코드임. 새 액티비티에서 처리한 결과가 정상인지 아닌지 구분하는데 사용!
        //intent는 새 액티비티로부터 전달 받은 인텐트. 이 인텐트 안에 새 액티비티의 데이터를 전달할 수 있다. -> putExtra()메서드 이용
        super.onActivityResult(requestCode, resultCode, data);  
        
        if(requestCode == REQUEST_CODE_MENU){
            Toast.makeText(getApplicationContext(),"onActivityResult 메서드 호출됨. 요청 코드 : " + requestCode +
                    ", 결과 코드 : " + resultCode, Toast.LENGTH_LONG).show();

            if(resultCode == RESULT_OK){
                String name = data.getStringExtra("name");
                Toast.makeText(getApplicationContext(),"응답으로 전달된 Name : " + name, Toast.LENGTH_LONG).show();
            }
        }
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener(){
            @Override
            public void onClick(View view){
                Intent intent = new Intent(getApplicationContext(), MenuActivity.class);
                //인텐트 객체는 액티비티를 띄울 목적으로 사용되며, 액티비티간 데이터 전달하는 데에도 사용.
                startActivityForResult(intent,REQUEST_CODE_MENU);
                //새 액티비티 띄우고, 새 액티비티의 응답을 받을 수 있음
            }
        });
    }
}
```

### 화면 전환의 과정
1. 새로운 액티비티 만들기(XML레이아웃 파일 하나, 자바 소스파일 하나)  
2. 새로운 액티비티의 XML레이아웃 정의하기  
3. 메인 액티비티에서 새로운 액티비티 띄우기 `startActivityForResult()`  
4. 새로운 액티비티에서 응답 보내기 `setResult()`  
5. 응답 처리하기 `onActivityResult()`  

## 인텐트

android.content 패키지 안에 정의되어 있다. 앱 구성요소 간에 작업수행을 위한 정보를 전달하는 역할을 한다.  
액티비티와 액티비티 간 사이의 소통, 액티비티와 서비스 간 사이의 소통 등을 돕는 역할을 한다고 이해하면 됨!  

* 다른 앱 구성요소에 인텐트를 전달할 수 있는 대표적인 메서드  

`startActivity() 또는 startActivityForResult()` : 액티비티를 화면에 띄울 때  
`startService() 또는 binService()` : 서비스를 시작할 때  
`broadcastIntent()` : 인텐트 객체를 브로드캐스팅 방식으로 전송할 때  

**이 메서드들을 호출할 때 인텐트가 파라미터로 전달되며,  
이렇게 전달된 파라미터는 앱 구성 요소인 액티비티, 서비스, 브로드캐스트 수신자로 전달될 수 있다.**  

### 인텐트의 기본 구성 요소

액션(Action), 데이터(Data)  
: 액션은 수행할 기능, 데이터는 액션이 수행될 대상의 데이터를 의미한다.  

* 액션과 데이터를 사용하는 대표적인 예 

속성|설명
---|---
ACTION_DIAL tel:01077881234 | 주어진 전화번호를 이용해 전화걸기 화면을 보여줌  
ACTION_VIEW tel:01088773628 | 위와 기능 같음. URI값의 유형에 따라 VIEW 액션이 다른 기능을 수행함  
ACTION_EDIT content://contacts/people/2 | 전화번호부 데이터베이스에 있는 정보 중에서 ID값이 2인 정보를 편집하기 위한 화면을 보여줌  
ACTION_VIEW content://contacts/people | 전화번호부 데이터베이스의 내용을 보여줌  

만약 `http://` 처럼 특정 포맷을 사용하면, 그 포맷은 등록된 MIME 타입으로 구분한다.  
MIME 타입은 일반적으로 웹 서버에서 사용하는 MIME 타입과 같다.  
(http://로 시작하는 문자열은 웹페이지 주소를 나타내는 URL이라고 인식하는 것과 같은)  
결국, 인텐트 전달 메커니즘도 이렇게 MIME 타입을 구분한 후 설치된 앱들 중에 적절한 것을 찾아 액티비티를 띄워주는 것임.  

* 명시적 인텐트(Explicit Intent)  
인텐트에 클래스 객체나 컴포넌트 이름을 지정하여 호출할 대상을 확실히 알 수 있는 경우  
```java
Intent intent = new Intent(this, NewActivity.class);
startActivity(intent);
```

* 암시적 인텐트(Implicit Intent)  
액션과 데이터를 지정하긴 했지만 호출할 대상이 달라질 수 있는 경우. 주로 앱과 앱간의 소통에 사용한다.  
MIME 타입에 따라 시스템에서 적절한 다른 앱의 액티비티를 찾은 후 띄우는 방식을 사용한다.  
```java
Intent sendIntent = new Intent();
sendIntent.setAction(Intent.ACTION_CALL);
startActivity(sendIntent);
```

**암시적 인텐트는 액션과 데이터로 구성되지만 그 외에도 여러가지 속성을 가지고 있다.**  

`1` 범주 : 액션이 실행되는데 필요한 추가적인 정보 제공  
`2` 타입 : 인텐트에 들어가는 데이터의 MIME 타입을 명시적으로 지정  
`3` 컴포넌트 : 인텐트에 사용될 컴포넌트 클래스 이름을 명시적으로 지정한다.  
-> 새로운 액티비티를 정의하고 그 액티비티의 클래스 객체를 인텐트에 전달하여 실행하는 방법도 컴포넌트를 지정하는 방식과 같음!  
`4` 부가 데이터 : 인텐트는 추가적인 정보를 넣을 수 있도록 번들 객체를 담고있다.  
이 객체를 통해 인텐트 안에 더 많은 정보를 넣어 다른 앱 구성요소에 전달할 수 있다.  

```java
public class MainActivity extends AppCompatActivity {
    EditText editText;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        editText = findViewById(R.id.editText);

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String data = editText.getText().toString();

                Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(data));
                startActivity(intent);
            }
        });

        Button button2 = findViewById(R.id.button2);
        button2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent();
                ComponentName name = new ComponentName("org.techtown.samplecallintent",
                        "org.techtown.samplecallintent.MenuActivity");
                //컴포넌트 이름을 지정할 수 있는 객체 생성
                //첫번째 파라미터 - 패키지 이름, 두번째 파라미터 - 클래스 이름
                intent.setComponent(name);
                //인텐트 객체에 컴포넌트 지정
                startActivityForResult(intent,101);
                //액티비티 띄우기
            }
        });

    }
}
```

## 플래그와 부가 데이터

### 플래그

액티비티는 액티비티 매니저라는 객체에 의해 '액티비티 스택'이라는 것으로 관리된다. 그리고 이 스택은 액티비티를 차곡차곡 쌓아두었다가  
가장 상위에 있는 액티비티가 없어지면 이전의 액티비티가 다시 화면에 보이게 한다. 이렇게 일반적인 스택 구조를 이용해 액티비티가 관리되기는 하지만,  
동일한 액티비티를 여러 번 실행한다면 동일한 액티비티가 스택에 여러번 들어가게 되고, 동시에 데이터를 여러 번 접근하거나  
리소스를 여러번 사용하는 문제가 발생할 수 있다. 이러한 문제들을 해결할 수 있도록 도와주는 것이 `플래그(FLAG)` 이다.  

`FLAG_ACTIVITIY_SINGLE_TOP`  
액티비티를 생성할 때 이미 생성된 액티비티가 있으면 그 액티비티를 그대로 사용하라는 플래그.  

Q. 화면에 보이는 액티비티가 새로 만들어지지 않고 기존에 있는 것이 보인다면, 시스템에서 전달하는 인텐트 객체는 어떻게 전달받는가?  
액티비티가 이미 메모리에 객체로 생성되어 있으면, 액티비티를 다시 띄우더라도 `onCreate()` 메서드가 호출되지 않는다.  
따라서 `onNewIntent()` 메서드를 재정의하여 사용해야 한다. `onNewIntent()` 메서드는 액티비티가 이미 객체로 만들어져 있을 때  
시스템으로부터 자동으로 호출되며, 파라미터로 인텐트 객체를 전달 받을 수 있다.  

`FLAG_ACTIVITY_NO_HISTORY`  
처음 이후에 실행된 액티비티는 액티비티 스택에 추가되지 않게하는 플래그.  
즉, 플래그가 설정되지 않은 경우에는 BACK키를 누르면 이전의 액티비티가 보이지만,  
이 플래그를 사용하면 항상 맨 처음에 실행되었던 액티비티가 바로 보이게 된다.  

`FLAG_ACTIVITY_CLEAR_TOP`  
이 액티비티 위에 있는 다른 액티비티를 모두 종료한다.  
홈 화면과 같ㅇ이 다른 액티비티보다 항상 우선하는 액티비티를 만들 때 유용하게 쓸 수 있음.  

### 부가 데이터

한 액티비티에서 다른 액티비티를 띄울 때 데이터를 전달해야 하는 경우도 있다.  
가장 간단한 방법은 별도의 클래스를 만든 다음 그 안에 클래스 변수를 만들어 두 개의 화면에서 모두 그 변수를 참조하게 하면 되지만,  
안드로이드는 **다른 앱에서 내가 만든 화면을 띄울 수도 있기 때문에 변수를 공유하는 방식이 불가능할 수도 있다**  
따라서, 액티비티를 띄울 때 전달되는 인텐트 안에 부가 데이터를 넣어 전달하는 방법을 권장한다.  

인텐트 안에는 번들객체가 들어있는데, `putExtra()` 메서드와 `get~Extra()` 메서드로 데이터를 넣거나 빼낼 수 있다.  
번들 객체 안에 넣은 데이터를 부가데이터라고 하며, 시스템에서는 건드리지 않고 다른 앱 구성요소로 전달된다.  

```java
Intent putExtra(String name, String Value)
Intent putExtra(String name, int Value)
Intent putExtra(String name, boolean Value)

String getStringExtra(String name)
int getIntExtra(String name, int defaultValue)
boolean getBooleanExtra(String name, boolean defaultValue)
```

get~ 형태를 가진 메서드는 데이터 값이 없으면 디폴트로 설정한 defaultValue값이 반환되며,  
객체 자료형의 경우 바이트 배열로 변환하거나, Serializable 인터페이스를 구현하는 객체를 만들어 직렬화한다음 전달해야한다.  
-> `Serializable` 보다 `Parcelable` 인터페이스를 권장함. 비슷하나 크기가 더 작고 빠르다.  

`Parcelable` 인터페이스 필수 구현 메소드  
`public abstract int describeContents()`, `public abstract void wirteToParcel(Parcel dest, int flags)`  

> 사실 이 Parcelable 인터페이스 부분이 잘 이해가 안되어서 아직 공부가 더 필요할 것 같다.  
> 잘 정리해놓은 것 같은 블로그가 있어서 기록용으로 올려둔다. [:airplane:](https://arsviator.blogspot.com/2010/10/parcelable%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%9C-%EC%98%A4%EB%B8%8C%EC%A0%9D%ED%8A%B8-%EC%A0%84%EB%8B%AC-object.html)

## 태스크 관리  

앱은 하나의 프로세스 위에서 동작한다.  
* 프로세스가 하나 실행되고, 그 위에 VM(Vertual Machine, 가상머신)이 만들어지며 그 위에서 앱이 실행된다.  

시스템에서 인텐트를 보내는 방법으로 내가 만든 앱에서 전화앱을 띄울 수 있는데, 이렇게 하면 전화 앱이 별도의 프로세스로 동작하게 된다.  
그런데 전화 앱에서 Back키를 누르면 다시 내가 만든 앱 화면으로 돌아올 수 있어야 하는데  
**프로세스는 독립적인 상자와 같아서 프로세스간의 정보 공유가 어렵다.** 그래서 `태스크(Task)` 라는게 있는 것!    
태스크는 앱이 어떻게 동작할 지 결정할 때 사용되며, 어떤 화면들이 같이 동작해야 하는지 흐름을 관리할 수 있다.

### 프로세스와 태스크

하나의 프로세스에서 다른 프로세스의 화면을 띄우려면 시스템의 도움이 필요하다.  
시스템에서 이런 액티비티의 각종 정보를 저장해두기 위해 태스크라는 것을 만들게 된다.  
시스템이 알아서 태스크를 관리하긴 하지만, 직접 제어해야하는 경우도 있다.  

```java
//manifest
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="org.techtown.sampletask">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity"
                  android:launchMode="singleTop">
                  //태스크의 가장 위쪽에 있는 액티비티는 더이상 새로 만들지 않게 됨
                  //MainActivity쪽으로 전달되는 인텐트는 onNewIntent()메서드로 전달받아야 한다
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

## 액티비티의 수명주기와 SharedPreferences

### 액티비티의 수명주기

액티비티는 처음 실행될 때 메모리에 만들어지는 과정부터 시작해서 실행과 중지,  
그리고 메모리에서 해제되는 여러 과정의 상태 정보를 갖고 있으며 이런 상태 정보는 시스템이 관리하면서 각각의 상태에 해당하는 메서드를 자동으로 호출한다.  

**<액티비티의 대표적인 상태 정보>**  
상태|설명
---|---
실행(Running)|화면상에 액티비티가 보이면서 실행되어 있는 상태. 액티비티 스택의 최상위에 있으며 포커스를 가지고 있음
일시정지(Paused)|사용자에게 보이지만 다른 액티비티가 위에 있어 포커스를 받지 못하는 상태(대화상자가 위에 있어 일부가 가려진 상태)
중지(Stopped)|다른 액티비티에 의해 완전히 가려져 보이지 않는 상태  

이렇게 액티비티의 상태 정보가 변화하는 것을 `액티비티의 수명주기(Life Cycle)` 이라고 하며, 각각에 해당하는 메서드가 자동으로 호출된다.  

상태 메서드|설명
--------|---
onCreate()|액티비티가 처음에 만들어졌을 때 호출됨  
||화면에 보이는 뷰들의 일반적인 상태를 설정하는 부분  
||이전 상태가 저장되어 있는 경우에는 번들 객체를 참조하여 이전 상태 복원 가능  
||이 메서드 다음에는 항상 onStart() 메서드가 호출됨  
onStart()|액티비티가 화면에 보이기 바로 전에 호출됨  
||액티비티가 화면 상에 보이면 이 메서드 다음에 onResume() 메서드가 호출됨  
||액티비티가 화면에서 가려지게 되면 이 메서드 다음에 onStop() 메서드가 호출됨  
onResume()|액티비티가 사용자와 상호작용하기 바로 전에 호출됨  
onRestart()|액티비티가 중지된 이후에 호출되는 메서드로 다시 시작되기 바로 전에 호출됨  
||이 메서드 다음에는 항상 onStart() 메서드가 호출됨  
onPause()|또 다른 액티비티를 시작하려고 할 때 호출됨  
||저장되지 않은 데이터를 저장소에 저장하거나 애니메이션 중인 작업을 중지하는 등의 기능을 수행하는 메서드  
||이 메서드가 리턴하기 전에는 다음 액티비티가 시작될 수 없으므로 이 작업은 매우 빨리 수행된 후 리턴되어야 함  
||액티비티가 이 상태에 들어가면 시스템은 액티비티를 강제 종료할 수 있음   
onStop()|액티비티가 사용자에게 더 이상 보이지 않을 때 호출됨  
||액티비티가 소멸되거나 또 다른 액티비티가 화면을 가릴 때 호출됨  
||액티비티가 이 상태에 들어가면 시스템은 액티비티를 강제 종료할 수 있음  
onDestroy()|액티비티가 소멸되어 없어지기 전에 호출됨  
||이 메서드는 액티비티가 받는 마지막 호출이 됨  
||액티비티가 앱에 의해 종료되거나(finsh()메서드 호출) 시스템이 강제로 종료시키는 경우에 호출될 수 있음  
||위의 두 가지 경우를 구분할 때 isFinishing()메서드를 이용함  
||액티비티가 이 상태에 들어가면 시스템은 액티비티를 강제 종료할 수 있음  

### SharedPreferences

앱 안에서 간단한 데이터를 저장하거나 복원할 때 `SharedPreferences` 를 사용할 수 있다.  
앱 내부에 파일을 하나 만드는데 이 파일 안에서 데이터를 저장하거나 읽어올 수 있게 한다.  
실제로 파일을 만들 필요 없이 SharedPreferences의 저장, 복원 메서드를 호출하면 된다!  

```java
public class MainActivity extends AppCompatActivity {
    public static final int REQUEST_CODE_MENU = 101;
    EditText NameInput;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        println("onCreate Called");

        NameInput = findViewById(R.id.nameInput);
        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(getApplicationContext(), MenuActivity.class);
                startActivityForResult(intent,REQUEST_CODE_MENU);
            }
        });
    }

    @Override
    protected void onStart() {
        super.onStart();
       println("onStart Called");
    }

    @Override
    protected void onStop() {
        super.onStop();
        println("onStop Called");
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        println("onDestroy Called");
    }

    @Override
    protected void onPause() {
        super.onPause();
        saveState();
        println("onPause Called, save State");
    }

    @Override
    protected void onResume() {
        super.onResume();
        restoreState();
        println("onResume Called, reload State");
    }

    public void println(String data){
        Toast.makeText(this,data,Toast.LENGTH_LONG).show();
        Log.d("LifeCheck",data);
    }

    public void restoreState(){//설정 정보에 저장된 데이터를 가져와 토스트 메세지로 보여줌
        SharedPreferences pref = getSharedPreferences("pref", Activity.MODE_PRIVATE);
        if( (pref != null) && (pref.contains("name")) ){
            String name = pref.getString("name", "");
            NameInput.setText(name);
        }
    }

    protected void saveState(){ //현재 입력상자에 입력된 데이터 저장
        //데이터를 저장할 때는 SharedPreferences를 사용하며 pref 문자열을 저장소의 이름으로 사용함
        SharedPreferences pref = getSharedPreferences("pref", Activity.MODE_PRIVATE);
        //SharedPreferences 객체를 사용하려면 getSharedPreferenced() 메서드로 참조
        SharedPreferences.Editor editor = pref.edit();
        //데이터를 저장할 수 있도록 edit()메서드를 제공. edit() 메서드를 호출한 후,
        editor.putString( "name", NameInput.getText().toString() );
        //put~()메서드로 저장하려는 데이터 설정할 수 있음
        editor.commit();
        //설정한 후 commit() 메서드 호출해야 실제로 저장됨
    }

    protected void clearState(){
        SharedPreferences pref = getSharedPreferences("pref",Activity.MODE_PRIVATE);
        SharedPreferences.Editor editor = pref.edit();
        editor.clear();
        editor.commit();
    }
}
```



