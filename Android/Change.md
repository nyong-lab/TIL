# 화면 전환하기
> :blue_book:  
> Do it Android 2020  

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

레이아웃 객체화 과정은 앱이 실행될 때(런타임) 레이아웃 XML파일에 정의된 내용들이 메모리에 객체화되기 때문에 매우 중요하며,  
이 과정을 `인플레이션` 이라고 한다. 명확하게 이해해야함!  
