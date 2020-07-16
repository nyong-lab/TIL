# ViewPager

손가락으로 좌우 스크롤하여 넘겨볼 수 있는 기능. 화면 일부분만 차지하고 있어도 그 부분에서만 좌우 스크롤이 동작한다.  
뷰페이저는 그 안에 프래그먼트를 넣을 수 있고, 좌우 스크롤로 프래그먼트를 전환하게 된다.  
뷰페이저는 내부에서 어댑터라는 것과 상호작용하게 되어 있는데, 이것은 뷰페이저가 여러 개의 아이템 중에 하나를 보여주는 방식으로 동작하기 때문이다.

```java
//activity_main.xml
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/button"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <androidx.viewpager.widget.ViewPager
        android:id="@+id/pager"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/button" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

```java
//MainActivity.java

public class MainActivity extends AppCompatActivity {
 	  ViewPager pager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        //XML레이아웃에 들어있는 ViewPager객체를 findViewById()메서드로 찾아 참조한 후 클래스 안에 선언한 변수에 할당
        pager = findViewById(R.id.pager);
        //미리 로딩해 놓을 아이템의 개수를 세개로 설정(뷰페이저는 어댑터가 가지고 있는 아이템 중에서 몇개를 미리 로딩해둘 수 있음)
        pager.setOffscreenPageLimit(3);
        MyPagerAdapter adapter = new MyPagerAdapter(getSupportFragmentManager());
      
				//addItem()메서드를 호출하면서 프래그먼트 객체를 파라미터로 넘겨주면 어댑터 안에 들어있는 ArrayList에 추가된다
        Fragment1 fragment1 = new Fragment1();
        adapter.addItem(fragment1);

        Fragment2 fragment2 = new Fragment2();
        adapter.addItem(fragment2);

        Fragment3 fragment3 = new Fragment3();
        adapter.addItem(fragment3);
      
  			//뷰페이저 객체에 어댑터 객체 설정
      	//이때부터 뷰페이저는 어댑터에 있는 프래그먼트들을 화면에 보여줄 수 있게 됨
        pager.setAdapter(adapter);
    }

    class MyPagerAdapter extends FragmentStatePagerAdapter {
     	  //어댑터는 뷰페이저에 보여줄 각 프래그먼트를 관리하는 역할을 하며, 뷰페이저에 설정하면 서로 상호작용하며 화면에 표시해주게 된다.
        ArrayList<Fragment> items = new ArrayList<Fragment>();
      	//프래그먼트를 담아둘 ArrayList객체 생성
        public MyPagerAdapter(FragmentManager fm){
            super(fm);
        }
				//ArrayList객체 안에 프래그먼트를 추가할 수 있는 메서드
        public void addItem(Fragment item){
            items.add(item);
        }
				
      	//프래그먼트를 가져갈 수 있는 메서드
        @NonNull
        @Override
        public Fragment getItem(int position) {
            return items.get(position);
        }
				
      	//프래그먼트의 개수를 확인할 수 있는 메서드
        @Override
        public int getCount() {
            return items.size();
        }
    }
}
```

사용자가 손가락으로 화면을 전환하지 않고 코드에서 전환시키고 싶다면 뷰페이저 객체의 `setCurrentItem()` 메서드를 사용하면 된다.

```java
        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                pager.setCurrentItem(1); //버튼을 클릭하면 두 번째 프래그먼트 화면이 보이게 됨
            }
        });
```

전체 아이템의 개수와 현재 보고 있는 아이템이 어떤 것인지 보여주는 역할을 하는 `타이틀스트립(TitleStrip)` 

```java
//activity_main.xml

<androidx.viewpager.widget.ViewPager
        android:id="@+id/pager"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/button">

        <androidx.viewpager.widget.PagerTitleStrip
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_gravity="top"
            android:background="#A4ABC5"
            android:paddingTop="5dp"
            android:paddingBottom="5dp"
            android:textColor="#FFFFFF">

        </androidx.viewpager.widget.PagerTitleStrip>

    </androidx.viewpager.widget.ViewPager>
```

```java
//MainActivity.java의 MyPagerAdapter Class마지막부분에 메서드 추가
				@Nullable
        @Override
        public CharSequence getPageTitle(int position) {
            return "~"+position+"~";
        }
```

