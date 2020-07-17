# Navigation

몇개의 화면에서 공통으로 보여줄 수 있기 때문에 빠르게 메뉴 기능에 접근하고자 할 때 사용하고,  
로그인한 사용자의 프로필 정보나 설정 메뉴를 보여줄 때도 사용할 수 있다.  

바로가기 메뉴를 추가하는 가장 쉬운 방법은 안드로이드에서 첫 화면의 유형으로 제공하는 것을 사용하는 것인데,  
새로운 프로젝트를 만들때 `Navigation Drawer Activity` 를 선택하면 된다.  

```java
//AndroidManifest.xml

<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="org.techtown.drawer">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
              
            //기본 테마가 아니라 직접 만든 테마를 설정하기 위함  
            android:theme="@style/AppTheme.NoActionBar">
              
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

```java
//styles.xml

<resources>
    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

  	//상단의 액션바가 없는 스타일로 테마 설정됨
    <style name="AppTheme.NoActionBar">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
    </style>

    <style name="AppTheme.AppBarOverlay" parent="ThemeOverlay.AppCompat.Dark.ActionBar" />

    <style name="AppTheme.PopupOverlay" parent="ThemeOverlay.AppCompat.Light" />

</resources>
```

생성된 app_bar_main.xml과 content_main.xml 파일을 activity_main.xml에 통합하고 삭제

```java
//activity_main.xml

<androidx.drawerlayout.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:openDrawer="start">
    
    <androidx.coordinatorlayout.widget.CoordinatorLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent" >

    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/AppTheme.AppBarOverlay">
        
        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            app:popupTheme="@style/AppTheme.PopupOverlay" />

    </com.google.android.material.appbar.AppBarLayout>
    
    //메인화면의 역할을 할 레이아웃
    <FrameLayout 
        android:id="@+id/container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        //CoordinatiorLayout안에서 해당 레이아웃이 스크롤 등의 작업이 진행될 때 차지할 면적을 자동으로 계산
        app:layout_behavior="@string/appbar_scrolling_view_behavior">
        
    </FrameLayout>

    </androidx.coordinatorlayout.widget.CoordinatorLayout>
    
    <com.google.android.material.navigation.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true"
        //바로가기 메뉴의 상단에 표시되면서 사용자 프로필 등을 보여즐 수 있도록 함
        app:headerLayout="@layout/nav_header_main"
        //그 아래에 메뉴를 보여줌
        app:menu="@menu/activity_main_drawer" />
    
</androidx.drawerlayout.widget.DrawerLayout>
```

```java
//MainActivity.java

public class MainActivity extends AppCompatActivity implements NavigationView.OnNavigationItemSelectedListener, FragmentCallback {

    Fragment1 fragment1;
    Fragment2 fragment2;
    Fragment3 fragment3;

    DrawerLayout drawer;
    Toolbar toolbar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        drawer = findViewById(R.id.drawer_layout);
        ActionBarDrawerToggle toggle = new ActionBarDrawerToggle(this, drawer, toolbar, R.string.navigation_drawer_open, R.string.navigation_drawer_close);
        drawer.addDrawerListener(toggle);
        toggle.syncState();

        NavigationView navigationView = findViewById(R.id.nav_view);
        navigationView.setNavigationItemSelectedListener(this);

        fragment1 = new Fragment1();
        fragment2 = new Fragment2();
        fragment3 = new Fragment3();

        getSupportFragmentManager().beginTransaction().add(R.id.container, fragment1).commit();
    }
		
  	//system back키 눌렀을 때 호출되는 메서드
    @Override
    public void onBackPressed() {
        if(drawer.isDrawerOpen(GravityCompat.START)){ //바로가기 메뉴가 열려있으면
            drawer.closeDrawer(GravityCompat.START); //바로가기 메뉴를 닫는다
        } else {
            super.onBackPressed();
        }
    }
		
  	//바로가기 메뉴 안에서 메뉴를 누르면 호출
    @Override
    public boolean onNavigationItemSelected(@NonNull MenuItem item) {
        int id = item.getItemId();

        if(id == R.id.menu1){
            Toast.makeText(this,"첫 번째 메뉴 선택됨.",Toast.LENGTH_SHORT).show();
            onFragmentSelected(0,null);
        } else if(id == R.id.menu2){
            Toast.makeText(this,"두 번째 메뉴 선택됨.",Toast.LENGTH_SHORT).show();
            onFragmentSelected(1,null);
        } else if(id == R.id.menu3) {
            Toast.makeText(this, "세 번째 메뉴 선택됨.", Toast.LENGTH_SHORT).show();
            onFragmentSelected(2, null);
        }

        drawer.closeDrawer(GravityCompat.START);
        return true;
    }

    @Override
    public void onFragmentSelected(int position, Bundle bundle){
        Fragment curFragment = null;

        if(position == 0) {
            curFragment = fragment1;
            toolbar.setTitle("첫 번째 화면");
        } else if(position == 1){
            curFragment = fragment2;
            toolbar.setTitle("두 번째 화면");
        } else if(position == 2){
            curFragment = fragment3;
            toolbar.setTitle("세 번째 화면");
        }

        getSupportFragmentManager().beginTransaction().replace(R.id.container,curFragment).commit();
    }
}
```

```java
//FragmentCallback.java
//어떤 프래그먼트를 보여줄지 선택하는 메서드를 포함하고 있음

import android.os.Bundle;

public interface FragmentCallback {
    public void onFragmentSelected(int position, Bundle bundle);
}

```
바로가기 메뉴는 앱에서 자주 사용되므로 잘 알아두는 것이 좋다. 한번 연습해두면 코드를 그대로 복사해서 사용할 수 있으므로,  
어떻게 동작하는지 그 원리와 코드부분을 잘 이해하는 것이 중요하다!  
