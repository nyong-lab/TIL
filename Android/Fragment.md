# Fragment
하나의 화면을 여러부분으로 나누어 보여주거나 각각의 부분화면 단위로 바꿔서 보여주고 싶을 때 사용한다.  
프래그먼트를 사용하는 가장 큰 목적은, 분할된 화면들을 독립적으로 구성하고 그 상태를 관리하는데 있음!  
*(ex) 카카오톡의 탭 기능(친구목록/대화목록/설정 등)*  

프래그먼트가 화면 전체를 채우도록 할 수 있기 때문에, 사용자는 프래그먼트가 하나의 전체화면처럼 느끼게 된다.  
-> **액티비티를 전환하지 않아도 화면전환 효과를 낼 수 있음**  

## 프래그먼트를 화면에 추가하는 방법 
먼저 `XML 레이아웃에 뷰들을 배치` 하고, Fragment 클래스를 상속하여 `프래그먼트를 위한 자바 소스파일` 을 만든다.  

```java

//프래그먼트 클래스에 있는 주요 메서드

public final Activity getActivity() //이 프래그먼트를 포함하는 액티비티를 반환한다.
public final FragmentManager getFragmentManager()
//이 프래그먼트를 포함하는 액티비티에서 프래그먼트 객체들과 의사소통하는 프래그먼트 매니저를 반환한다.
public final Fragment getParentFragment()
//이 프래그먼트를 포함하는 부모가 프래그먼트일 경우 리턴한다. 액티비티이면 null을 반환한다.
public final int getId() //이 프래그먼트의 ID를 반환한다.

```

프래그먼트를 위한 클래스까지 만들었다면 `XML 레이아웃 파일의 내용을 소스 파일과 매칭`하는 과정이 필요한데,  
프래그먼트에는 setContentView() 메서드가 없다. -> LayoutInflater를 사용해 인플레이션 해야함.  
인플레이션 후 클래스에서 사용하도록 하는 코드는 onCreateView() 메서드 안에 들어가는데,  
인플레이션이 필요한 시점에 자동으로 호출되는 콜백 메서드이다.  
따라서 이 메서드 안에서 인플레이션을 위한 inflate()메서드를 호출하면 되고,  
인플레이션 과정이 끝나면 프래그먼트가 하나의 뷰처럼 동작할 수 있는 상태가 된다.  

새로 만든 프래그먼트를 메인 액티비티에 추가하는 방법은 `XML레이아웃에 <fragment> 태그를 사용해 추가` 하거나,  
`소스 코드에서 new 연산자로 객체를 만든 후 프래그먼트 매니저로 추가` 할 수 있다.  
코드에서 프래그먼트를 추가하고 싶다면 프래그먼트 관리를 담당하는 프래그먼트 매니저를 사용해야 한다.

```java

//프래그먼트 매니저 클래스에 있는 주요 메서드

public abstract FragmentTransaction beginTransaction() //프래그먼트를 변경하기 위한 트랙잭션을 시작한다.
public abstract Fragment findFragmentById() //ID를 이용해 프래그먼트 객체를 찾는다.
public abstract Fragment findFragmentByTag() //태그 정보를 사용해 프래그먼트 객체를 찾는다.
public abstract boolean executePendingTransactions()
//트랜잭션은 commit()메서드를 호출하면 실행되지만 비동기 방식으로 실행되므로 즉시 실행하고 싶다면 이 메서드를 추가로 호출해야 한다.

```

FragmentManager 객체는 프래그먼트를 액티비티에 추가, 교체 또는 삭제할 때 주로 사용할 수 있으며  
`getFragmentManager()` 메서드를 호출하면 참조할 수 있다.  
이전 버전의 단말에서도 동작할 수 있도록 appcompat 라이브러리의 기능을 사용하는  
`getSupportFragmentManager()` 메서드의 사용을 권장한다.  

프래그먼트의 대표적인 속성
특성|설명
---|---
뷰 특성|뷰그룹에 추가되거나 레이아웃의 일부가 될 수 있음(뷰에서 상속받은 것은 아님. 뷰를 담고 있는 일종의 틀)  
액티비티 특성|액티비티처럼 수명주기를 가지고 있음(컨택스트 객체는 아님, 라이프사이클은 액티비티에 종속)  

```java

//버튼을 클릭하면 바뀌는 프래그먼트(XML 생략)

public class MenuFragment extends Fragment {

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_menu, container, false);
        //onCreateView() 메서드의 파라미터로 LayoutInflater 객체가 전달되므로 이 객체의 inflate() 메서드를 바로 호출할 수 있음.
        //inflate() 메서드를 호출하면 인플레이션이 진행되고 그 결과로 ViewGroup 객체가 반환됨. return으로 반환!
    }
}

```

```java

public class MainFragment extends Fragment {

//메인 프래그먼트

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        ViewGroup rootView = (ViewGroup) inflater.inflate(R.layout.fragment_main, container, false);
        //최상위 레이아웃은 인플레이션을 통해 참조한 rootView 객체.
        //최상위 레이아웃은 메인 프래그먼트 안에 들어 있는 것이고, 메인 프래그먼트는 이 레이아웃을 화면에 보여주기 위한 틀임.

        Button button = rootView.findViewById(R.id.button);
        //rootView의 findViewById() 메서드를 이용해 레이아웃에 들어 있는 버튼 객체 찾아냄.
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                MainActivity activity = (MainActivity)getActivity();
                activity.onFragmentChanged( 0 );
                //MainActivity 객체를 참조한 후 onFragmentChanged() 메서드 호출
                //onFragmentChanged()는 메인 액티비티에 새로 추가할 메서드로, 프래그먼트 매니저를 이용해 프래그먼트를 전환하는 메서드임
                //이 프래그먼트에서 다른 프래그먼트를 직접 띄우는 것이 아니라 액티비티를 통해 띄워야 함!!
            }
        });

        return rootView;
    }
}

```

```java

//메인 xml

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/container">
    //레이아웃에 id속성 추가

    <fragment
        android:id="@+id/mainFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:name="org.techtown.fragment.MainFragment"/>
        //프래그먼트는 뷰와 다르게 뷰를 담고있는 공간만 확보함. 따라서 태그 이름으로 프래그먼트의 이름을 사용할 수 없으며,
        //name 속성에 새로 만든 MainFragment의 이름을 설정. 

</RelativeLayout>

```

```java

//메인 

public class MainActivity extends AppCompatActivity {
    MainFragment mainFragment;
    MenuFragment menuFragment;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mainFragment = (MainFragment)getSupportFragmentManager().findFragmentById(R.id.mainFragment);
        //메인 프래그먼트는 메인 XML에 추가되어 있으므로 ID를 사용하여 찾아야함
        //뷰가 아니므로 FragmentManager 객체의 findFragmentById()메서드를 사용해 찾아야 한다.
        menuFragment = new MenuFragment();
        //new 연산자를 사용해 새로운 객체로 만들어 변수에 할당
    }

    public void onFragmentChanged(int index){
        if(index == 0){
            getSupportFragmentManager().beginTransaction().replace(R.id.container,menuFragment).commit();
            //메인 액티비티에서 프래그먼트를 다루기 위해서는 먼저 getFragmentManager()메서드를 호출해 매니저 객체를 참조해야함!
            //프래그먼트 매니저 객체를 사용할 때는 트랜잭션이 사용됨
            //replace()메서드의 첫번째 파라미터는 프래그먼트를 담고 있는 레이아웃의 id가 되어야 함
        } else if(index == 1){
            getSupportFragmentManager().beginTransaction().replace(R.id.container,mainFragment).commit();
        }
    }

}

```

`public FragmentManager getFragmentManager()`  
프래그먼트 매니저는 프래그먼트를 다루는 작업을 해 주는 객체로 프래그먼트 추가, 삭제 또는 교체 등의 작업을 할 수 있게 한다.  
그런데 이런 작업들은 프래그먼트를 변경할 때 오류가 생기면 다시 원래 상태로 돌릴 수 있어야 하므로 트랜잭션 객체를 만들어 실행한다.  
트랜잭션 객체는 `beginTransaction()` 메서드를 호출하면 시작되고 `commit()` 메서드를 호출하면 실행된다.  


