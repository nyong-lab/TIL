# Tab

하나의 화면에 너무 많은 구성 요소를 넣으면 성능이나 사용성(Usability)면에서 좋지 않다.  
하나의 화면을 나타내는 액티비티를 최대한 많이 분리시켜서 하나의 화면에 보이는 뷰의 개수를 줄여주는 것이 좋다.  
때로는, 하나의 화면에 여러 가지 구성 요소를 넣어두고 필요할 때 전환하여 보여주는게 좋을 때도 있는데 대표적인 것이 서브화면이다.  

몇 개의 버튼을 두고 그중 하나의 버튼을 눌러 서브화면을 전환하는 방식처럼 하나의 뷰에서  
여러 개의 정보를 볼 때 일반적으로 사용하는 뷰로 탭(Tab)을 들 수 있다.  
탭을 누를 때마다 내용이 보이는 화면 영역이 전환되어 나타나는데, `상단 탭` 과 `하단 탭` 으로 구분할 수 있다.  
`상단 탭` 은 액션바에 탭 기능을 넣어 보여주는 방법으로 제공되며, `하단 탭` 은 별도의 위젯으로 제공된다.  

## 상단 탭 만들기

```java
//acticity_main.xml
//메인 레이아웃을 잡아주기 위함

<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    //액션바 영역을 포함한 전체 화면의 위치를 잡아주는 역할
    <androidx.coordinatorlayout.widget.CoordinatorLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintTop_toTopOf="parent">

        //액션바를 가리킴. 이 안에는 Toolbar가 들어갈 수 있으며, 탭을 사용하는 경우 탭의 버튼이 들어갈 수 있는 TabLayout을 추가할 수 있다
        <com.google.android.material.appbar.AppBarLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">

            <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@color/colorPrimaryDark"
            android:elevation="1dp"
            android:theme="@style/ThemeOverlay.AppCompat.Dark">
								
              	//Toolbar안에 텍스트뷰를 넣어 제목을 표시 할 수 있게함
                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="타이틀"
                    android:textAppearance="@style/Base.TextAppearance.Widget.AppCompat.Toolbar.Title" />
                      
            </androidx.appcompat.widget.Toolbar>

            <com.google.android.material.tabs.TabLayout
                android:id="@+id/tabs"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:background="@android:color/background_light"
                android:elevation="1dp" //z축 그림자. background에 색이 있어야 적용됨
                app:tabGravity="fill" //탭의 너비를 동일하게 정렬
                app:tabMode="fixed" //모든 탭을 화면에 표시되도록 설정
                  
                app:tabSelectedTextColor="@color/colorAccent"
                app:tabTextColor="@color/colorPrimary" />
        </com.google.android.material.appbar.AppBarLayout>

        //화면의 내용을 구성할 수 있음. 이 안에 프래그먼트 들어가도록 할 예정
        <FrameLayout
            android:id="@+id/container"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior">

        </FrameLayout>

    </androidx.coordinatorlayout.widget.CoordinatorLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

```java
//Fragment1~3.java
//fragment1~3.xml파일을 각각 인플레이션 해줌

public class Fragment1 extends Fragment {

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment1, container, false);
    }
}
```

```java
//MainActivity.java

public class MainActivity extends AppCompatActivity {
    Toolbar toolbar;

    Fragment1 fragment1;
    Fragment2 fragment2;
    Fragment3 fragment3;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
     		//XML 레이아웃에서 정의한 Toolbar객체는 코드에서 setSupportActionBar()메서드를 사용해 액션바로 설정해야함
        //액티비티에 디폴트로 만들어진 액션바가 없을 경우에만 동작
        //처음에 액션바가 들어있는 테마를 지정함. 따라서 /app/res/values 폴더 안에 있는
        //styles.xml 파일을 열고 noActionBar 스타일로 변경해주자!

        ActionBar actionBar = getSupportActionBar();
        actionBar.setDisplayShowTitleEnabled(false);

        fragment1 = new Fragment1();
        fragment2 = new Fragment2();
        fragment3 = new Fragment3();

        getSupportFragmentManager().beginTransaction().replace(R.id.container, fragment1).commit();

        TabLayout tabs = findViewById(R.id.tabs);
        tabs.addTab(tabs.newTab().setText("통화기록"));
        tabs.addTab(tabs.newTab().setText("스팸기록"));
        tabs.addTab(tabs.newTab().setText("연락처"));

        tabs.addOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
            @Override
            public void onTabSelected(TabLayout.Tab tab) {
                int position = tab.getPosition();
                Log.d("MainActivity", "선택된 탭 : " + position);
                Fragment selected = null;
                if(position == 0){
                    selected = fragment1;
                } else if(position == 1){
                    selected = fragment2;
                } else if (position == 2){
                    selected = fragment3;
                }

                getSupportFragmentManager().beginTransaction().replace(R.id.container, selected).commit();
            }

            @Override
            public void onTabUnselected(TabLayout.Tab tab) {

            }

            @Override
            public void onTabReselected(TabLayout.Tab tab) {

            }
        });
    }
}
```

탭에 글자만 표시했지만, 필요에 따라 아이콘이 보이도록 하거나 아이콘과 글자가 함께 보이도록 만들 수도 있다.  

## 하단 탭 만들기

하단 탭은 BottomNavigationView 위젯으로 만들 수 있다.  
이 위젯도 머티리얼 라이브러리를 사용하기 때문에 외부 라이브러리를 추가하는 과정을 진행해야한다.  
하단의 각 탭에는 이미지나 글자가 들어갈 수 있는데, 이 버튼들은 메뉴 XML파일로 만들게 된다.

```java
//menu_bottom.xml

//@android:drawable은 기본API에 포함된 이미지를 참조할 수 있도록 함
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/tab1"
        app:showAsAction="ifRoom"
        android:enabled="true"
        android:icon="@android:drawable/ic_dialog_email"
        android:title="이메일"/>

    <item
        android:id="@+id/tab2"
        app:showAsAction="ifRoom"
        android:enabled="true"
        android:icon="@android:drawable/ic_dialog_info"
        android:title="정보"/>

    <item
        android:id="@+id/tab3"
        app:showAsAction="ifRoom"
        android:enabled="true"
        android:icon="@android:drawable/ic_dialog_map"
        android:title="위치"/>

</menu>
```

```java
//activity_main.xml

<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <FrameLayout
        android:id="@+id/container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"/>

    //하단 탭 보여주는 위젯, 화면의 하단에 배치
    <com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/bottom_navigation"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:itemBackground="@color/colorPrimary" //탭의 배경색
        app:itemIconTint="@drawable/item_color" //아이콘 색상
        app:itemTextColor="@drawable/item_color" //텍스트 색상
        app:menu="@menu/menu_bottom"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

```java
//item_color.xml

<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_checked="true" android:color="#ffffff" />
    <item android:color="#A1A1A1"  />
</selector>
```

```java
//MainActivity.java

public class MainActivity extends AppCompatActivity {

    Fragment1 fragment1;
    Fragment2 fragment2;
    Fragment3 fragment3;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        fragment1 = new Fragment1();
        fragment2 = new Fragment2();
        fragment3 = new Fragment3();

        getSupportFragmentManager().beginTransaction().replace(R.id.container,fragment1).commit();

        BottomNavigationView bottomNavigation = findViewById(R.id.bottom_navigation);
        bottomNavigation.setOnNavigationItemSelectedListener(new BottomNavigationView.OnNavigationItemSelectedListener() {
            @Override
            public boolean onNavigationItemSelected(@NonNull MenuItem item) {
                switch(item.getItemId()){
                    case R.id.tab1 :
                        Toast.makeText(getApplicationContext(),"첫 번째 탭 선택됨",Toast.LENGTH_SHORT).show();
                        getSupportFragmentManager().beginTransaction().replace(R.id.container,fragment1).commit();

                        return true;

                    case R.id.tab2 :
                        Toast.makeText(getApplicationContext(),"두 번째 탭 선택됨",Toast.LENGTH_SHORT).show();
                        getSupportFragmentManager().beginTransaction().replace(R.id.container,fragment2).commit();

                        return true;

                    case R.id.tab3 :
                        Toast.makeText(getApplicationContext(),"세 번째 탭 선택됨",Toast.LENGTH_SHORT).show();
                        getSupportFragmentManager().beginTransaction().replace(R.id.container,fragment3).commit();

                        return true;
                }

                return false;
            }
        });
    }
}
```





