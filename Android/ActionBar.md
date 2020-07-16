# ActionBar

## Option Menu, Context Menu

안드로이드의 화면 아래쪽에는 `홈` `메뉴` `Back` 버튼 세개가 있다.  
`메뉴` 버튼을 눌렀을 때 숨어있던 메뉴가 보이도록 할 수도 있고, 앱의 상단 타이틀부분에 메뉴버튼을 배치하고  
그것을 눌렀을 때 메뉴가 보이도록 할 수 있다. 이것을 **옵션 메뉴**라고 한다.  
입력 상자를 길게 눌렀을 때 나타나는 `복사하기` `붙여넣기` 등의 팝업 형태의 메뉴는 **컨텍스트 메뉴**라고 한다.  

## Option Menu

옵션 메뉴는 액션바에 포함되어 보이도록 설정되어 있는데, 액션바는 *앱의 제목이 보이는 위쪽부분*을 말한다.  

```java

//밑의 두 메서드를 재정의하면 쉽게 메뉴를 추가할 수 있다
public boolean onCreateOptionsMenu(Menu menu)
public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo menuInfo)

//두 메서드에서 Menu나 ContextMenu객체가 전달되는 것을 알 수 있는데, 이 객체의 add() 메서드를 사용해 메뉴 아이템을 추가하게 된다.
//메뉴 아이템을 추가할 수 있는 대표적인 메서드들은 다음과 같다
MenuItem add(int groupId, int itemId, int order, CharSequence title)
MenuItem add(int groupId, int itemId, int order, int titleRes)
SubMenu addSubMenu(int titleRes)
//groupId : 아이템을 하나의 그룹으로 묶을 때 사용
//itemId : 아이템이 갖는 고유 ID 값으로, 아이템이 선택되었을 때 각각의 아이템을 구분할 때 사용할 수 있음
//아이템이 많아서 서브메뉴로 추가하고 싶을 때는 addSubMenu()메서드 사용

//코드에서 메뉴를 추가하는 것보다는 XML에서 메뉴의 속성을 정의한 후 객체로 로딩하여 참조하는 것이 더 간단함

```

```java

//안드로이드 스튜디오는 app/res/menu폴더 안에 메뉴를 위한 XML파일이 만들어진다는 것을 미리 알고 있음.
//메뉴를 위한 XML파일은 반드시 menu폴더 안에 들어있어야 함.

<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    
    //<item> 태그는 하나의 메뉴에 대한 정보를 담고 있음

    <item android:id="@+id/menu_refresh" //각각의 메뉴를 구분하기 위해 사용됨
        android:title="새로고침" //메뉴에 표시되는 글자
        android:icon="@drawable/menu_refresh"
        app:showAsAction="always"/> //이 메뉴를 항상 보이게 할 것인지 아니면 숨겨둘 것인지 지정

    <item android:id="@+id/menu_search"
        android:title="검색"
        android:icon="@drawable/menu_search"
        app:showAsAction="always"/>

    <item android:id="@+id/menu_settings"
        android:title="설정"
        android:icon="@drawable/menu_settings"
        app:showAsAction="always"/>

</menu>

```

showAsAction 속성에 설정할 수 있는 값

showAsAction 속성 값|설명
------------------|---
always|항상 액션바에 아이템을 추가하여 표시한다  
never|액션바에 아이템을 추가하여 표시하지 않는다(디폴트 값)  
ifRoom|액션바에 여유 공간이 있을 때만 아이템 표시  
withText|title 속성으로 설정된 제목을 같이 표시  
collapseActionView|아이템에 설정한 뷰(actionViewLayout)의 아이콘만 표시  

```java 

//MainActivity.java에 재정의된 onCreateOptionsMenu() 메서드는 액티비티가 만들어질 때 자동으로 호출되어 화면에 메뉴기능을 추가할 수 있도록 한다.
//XML파일처럼 소스코드에서 인플레이션한 후 메뉴에 설정할 수 있는데, 메뉴 인플레이터 객체를 사용한다.

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(@NonNull MenuItem item) { //사용자가 메뉴 아이템을 선택했을 때 자동 호출됨
        int curId = item.getItemId();
        switch(curId){
            case R.id.menu_refresh :
                Toast.makeText(this,"새로고침 메뉴가 선택되었습니다.",Toast.LENGTH_SHORT).show();
                break;
            case R.id.menu_search :
                Toast.makeText(this,"검색 메뉴가 선택되었습니다",Toast.LENGTH_SHORT).show();
                break;
            case R.id.menu_settings :
                Toast.makeText(this,"설정 메뉴가 선택되었습니다.",Toast.LENGTH_SHORT).show();
                break;
        }

        return super.onOptionsItemSelected(item);
    }

}

//화면이 처음 만들어질 때 메뉴를 정해놓은 것이 아니라 화면이 띄워진 후에 메뉴를 바꾸고 싶다면 onPrepareOptionsMenu() 메서드를 재정의하여 사용하면 됨
//이 메서드는 메뉴가 새로 보일 때마다 호출되므로 메뉴 항목을 추가하거나 뺄 수 있어 메뉴 아이템을 변경할 수 있음.

```

## Context Menu

어떤 뷰에 필요한 기능만을 모아 정의해 놓은 것으로 특정 뷰를 손가락으로 길게 눌렀을 때 보이게 된다.  
> [공부할 때 참고한 블로그](https://lktprogrammer.tistory.com/162)  

```java

/*컨텍스트 메뉴도 두 개의 메서드를 다시 정의하면 사용할 수 있다.  
 *컨텍스트 메뉴를 특정 뷰에 등록하고 싶을 때 사용하는 메서드.
 *이 메서드로 컨텍스트 메뉴를 등록하면 각각의 메뉴 아이템을 선택했을 때 onContextItemSelected() 메서드가 호출되므로
 *이 메서드의 파라미터로 전달되는 MenuItem 객체를 사용해 선택된 메뉴 아이템의 정보를 확인한 후 처리할 수 있다.
 */

void Activity.registerForContextMenu(View view)

```

## ActionBar

액션바는 앱의 상단에 타이틀 뿐만 아니라 버튼이나 입력상자 등을 배치할 수 있는 공간이므로, 앱을 만들 때 유용하게 사용할 수 있다.  
액션바를 화면에 보이게 만들고 싶으면 show() 메서드를 호출하고, 감추고 싶다면 hide()를 호출하면 된다.  

```java

ActionBar abar = getActionBar();
abar.show();
abar.hide();

//setSubtitle() 메서드를 사용하면 타이틀의 부제목을 달아줄 수도 있음!  

```

액션바 만들어보기    

```java

//MainActivity

import androidx.appcompat.app.ActionBar;

public class MainActivity extends AppCompatActivity {
    ActionBar abar; //ActionBar 자료형의 변수 선언

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        abar = getSupportActionBar(); //XML레이아웃에 들어있는 ActionBar객체 참조

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                abar.setLogo(R.drawable.home);
                abar.setDisplayOptions(ActionBar.DISPLAY_SHOW_HOME|ActionBar.DISPLAY_USE_LOGO);
                /*버튼 클릭했을 때 액션바가 보이는 모양을 바꾸도록 setDisplayOptions()메서드 사용
                
                 *액션바의 디스플레이 옵션으로 설정할 수 있는 상수들 
                 *DISPLAY_USE_LOGO : 홈 아이콘 부분에 로고 아이콘을 사용
                 *DISPLAY_SHOW_HOME : 홈 아이콘을 표시하도록 함
                 *DISPLAY_HOME_AS_UP : 홈 아이콘에 뒤로가기 모양의 < 아이콘을 같이 표시
                 *DISPLAY_SHOW_TITLE : 타이틀을 표시
                 */
            }
        });
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(@NonNull MenuItem item) {
    //중략
    }
    
}
    
```
```java

//메뉴XML

<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item android:id="@+id/menu_refresh"
        android:title="새로고침"
        android:orderInCategory="101" //메뉴가 보이는 순서를 결정, 작은 숫자부터 순서대로 지정
        android:icon="@drawable/menu_refresh"
        app:showAsAction="always"/>

    <item android:id="@+id/menu_search"
        android:title="검색"
        android:orderInCategory="102"
        app:showAsAction="always|withText"
        app:actionLayout="@layout/search_layout"/>

    <item android:id="@+id/menu_settings"
        android:title="설정"
        android:orderInCategory="103"
        android:icon="@drawable/menu_settings"
        app:showAsAction="never"/> //액션바에 메뉴가 보이지 않게 됨

</menu>

```





