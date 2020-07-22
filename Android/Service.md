# Service

카카오톡은 앱이 실행되어있지 않거나 화면이 사용자에게 보이지 않는 상태에서도 다른 사람이 보낸 메세지를 받을 수 있다. 화면 없이 백그라운드에서 실행되는 서비스가 있기 때문이다. 서비스란, 백그라운드에서 실행되는 앱의 구성 요소를 말한다. 앱의 구성 요소이므로 시스템에서 관리하며, 액티비티를 매니페스트 파일에 등록했던 것처럼 새로 만든 서비스도 매니페스트 파일에 꼭 등록해야 한다.

### 서비스의 실행 원리와 역할

서비스를 실행하려면 메인 액티비티에서 `startService()` 메서드를 호출하면 된다. 서비스의 주요 역할 중 하나는 단말이 항상 실행되어 있는 상태로 다른 단말과 데이터를 주고받거나 필요한 기능을 백그라운드에서 실행하는 것이다. 그래서 서비스는 실행된 상태를 계속 유지하기 위해 서비스가 비정상적으로 종료되더라도 시스템이 자동으로 재실행한다.

서비스를 시작시키기 위해 `startService()` 메서드를 호출할 때는 인텐트 객체를 파라미터로 전달한다. 이 인텐트 객체는 어떤 서비스를 실행할 것인지에 대한 정보를 담고있으며, 시스템은 서비스를 시작시킨 후 인텐트 객체를 서비스에 전달한다. 그런데 서비스가 실행 중이면 실행 이후에 `startService()` 메서드를 여러 번 호출해도 서비스는 이미 메모리에 만들어진 상태로 유지된다. 따라서 `startService()` 메서드는 서비스를 시작하는 목적 이외에 인텐트를 전달하는 목적으로도 자주 사용된다.

예를 들어, 액티비티에서 서비스로 데이터를 전달하려면 인텐트 객체를 만들고 부가 데이터를 넣은 후 `startService()` 메서드를 호출하면서 전달하면 된다. 그런데 앞에서 가정하고 있는 상태는 서비스가 `startService()` 메서드에 의하여 메모리에 만들어져 있는 상태이다. 이런 경우에는 시스템이 `onCreate()` 메서드가 아니라 `onStartCommand()` 메서드를 실행한다. `onStartCommand()` 메서드는 서비스로 전달된 인텐트 객체를 처리하는 메서드이다.

```java
//서비스 만들면 매니페스트 파일에 service 태그 추가됨.

<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="org.techtown.service">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <service
            android:name=".MyService"
            android:enabled="true"
            android:exported="true"></service>

        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

액티비티 -> 서비스로 데이터를 전달할 때는 `startService()` 메서드를 사용하며, 인텐트 안에 부가 데이터를 추가하여 전달하면 된다. 반대로 서비스 -> 액티비티로 데이터를 전달할 때는 서비스에서 `startActivity()` 메서드를 사용한다.

```java
//MainActivity.java
//activity_main.xml은 생략(editText와 button)

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
                String name = editText.getText().toString();

                Intent intent = new Intent(getApplicationContext(), MyService.class);
              	//서비스 쪽으로 전달한 인텐트 객체의 데이터가 어떤 목적으로 사용되는지를 구별하기 위함
                intent.putExtra("command", "show");
              	//입력상자에서 가져온 문자열을 전달하기 위함
                intent.putExtra("name",name);
              	//인텐트 객체는 MyService.class의 onStartCommand()메서드로 전달됨
                startService(intent);
            }
        });
				
      	//액티비티가 새로 만들어질 때 전달된 인텐트 처리
        Intent passedIntent = getIntent();
        processIntent(passedIntent);
    }

  	//액티비티가 이미 만들어져 있을 때 전달된 인텐트 처리
    @Override
    protected void onNewIntent(Intent intent) {
        processIntent(intent);
        super.onNewIntent(intent);
    }

    private void processIntent(Intent intent) {
        if(intent != null){
            String command = intent.getStringExtra("command");
            String name = intent.getStringExtra("name");

            Toast.makeText(this, "command : " + command + ", name : " + name, Toast.LENGTH_SHORT).show();
        }
    }
}
```

```java
//MyService.java

public class MyService extends Service {
    public static final String TAG = "MyService";

    public MyService() {
    }

    @Override
    public void onCreate() {
        super.onCreate();
        Log.d(TAG, "onCreate() 호출됨");
    }

  	/* 인텐트 객체를 전달받는 메서드
  	 * 인텐트 객체가 null이면 Servic.START_STICKY 반환한다
  	 * 이 값을 반환하면 서비스가 비정상 종료되었다는 의미이므로 시스템이 자동으로 재시작함
  	 */
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Log.d(TAG,"onStartCommand() 호출됨");
				
      	//인텐트 객체가 null이 아니면 호출
        if(intent == null){
            return Service.START_STICKY;
        } else { 
            processCommand(intent);
        }
      
        return super.onStartCommand(intent, flags, startId);
    }

    private void processCommand(Intent intent){
      	//인텐트에서 부가데이터 가져오기
        String command = intent.getStringExtra("command");
        String name = intent.getStringExtra("name");

        Log.d(TAG,"command : " + command + ", name : " + name);

        for(int i=0; i<5; i++){
            try{
                Thread.sleep(1000);
            } catch (Exception e){
            }
            Log.d(TAG,"Waiting " + i + " seconds.");
        }
      
      	//서비스에서 액티비티로 데이터 전달하고 싶으면, 서비스에서 startActivity() 메서드 사용.
        Intent showIntent = new Intent(getApplicationContext(), MainActivity.class);
      
      	/* 서비스에서 startActivity()를 호출하면 새로운 태스크를 생성하도록 FLAG_ACTIVITY_NEW_TASK 플래그를 인텐트에 추가
      	 * 서비스는 화면이 없기 때문에 화면이 없는 서비스에서 화면이 있는 액티비티를 띄우려면 새로운 태스크를 만들어야하기 때문
      	 * MainActivity객체가 이미 메모리에 만들어져 있을 때 재사용하도록 FLAG_ACTIVITY_SINGLE_TOP 과 CLEAR_TOP 플래그도 추가
      	 */
        showIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK|Intent.FLAG_ACTIVITY_SINGLE_TOP|Intent.FLAG_ACTIVITY_CLEAR_TOP);
        showIntent.putExtra("command", "show");
        showIntent.putExtra("name", name + " from service.");
        startActivity(showIntent);
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
    }

  	//서비스가 서버 역할을 하면서 액티비티와 연결될 수 있도록 만드는 것을 '바인딩(Binding)'이라고 한다
  	//이를 위해 메서드를 재정의 해준다. 하지만 여기서는 바인딩 사용X, 정의만 해주기
    @Override
    public IBinder onBind(Intent intent) {
        throw new UnsupportedOperationException("Not yet implemented");
    }
}
```

요즘 앱들은 서비스를 자주 사용한다. 따라서 액티비티에서 서비스로 데이터를 전달하거나, 서비스에서 액티비티로 데이터를 전달하는 방법을 잘 알아두어야 한다. 실행된 서비스를 종료하고 싶다면 `stopService()` 메서드를 호출하면 된다. Service 외에 IntentService라는 클래스가 있는데, 이 클래스는 위의 서비스와 달리 필요한 함수가 수행되고 나면 종료된다. 인텐트 서비스에는 `onHandleIntent` 라는 이름의 메서드가 있으며 이 함수는 `onStartCommand()` 메서드로 전달된 인텐트를 받으면서 실행되고 함수의 실행이 끝나면 서비스는 자동 종료된다.