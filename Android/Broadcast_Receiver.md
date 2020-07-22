# Broadcast Receiver

안드로이드에서 여러 앱 구성 요소에 메시지를 전달할 때 브로드캐스팅을 사용함! 예를 들어, 다른 사람으로부터 문자를 받았을 때 이 문자를 SMS수신 앱에 알려줘야 한다면 브로드캐스팅을 사용하면 된다. 이런 메시지 전달 방식을 글로벌 이벤트라고 부른다. 글로벌 이벤트의 대표적인 예로는 '전화가 왔습니다'와 같은 사용자 알림을 들 수 있다.

제작한 앱에서 브로드캐스팅 메시지를 받고 싶다면 `Broadcast Receiver` 를 만들어 앱에 등록하면 된다. 서비스와 마찬가지로 브로드캐스트 수신자도 앱 구성 요소이며, 따라서 새로운 브로드캐스트 수신자를 만들면 매니페스트 파일에 등록해야 시스템이 알 수 있고 화면도 없게 된다. 단, 브로드캐스트 수신자는 매니페스트 등록 방식이 아닌 소스 코드에서 `registerReceiver()` 메서드를 사용해 시스템에 등록할 수 있다. 소스 코드를 이용해 브로드캐스트 수신자를 등록하면 액티비티 안에서 브로드캐스트 메시지를 전달받아 바로 다른 작업을 수행하도록 만들 수 있는 장점이 있다.

브로드캐스트 수신자에는 `onRecieve()` 메서드를 정의해야 한다. 이 메서드는 원하는 브로드캐스트 메시지가 도착하면 자동으로 호출된다. 모든 메시지는 인텐트 안에 넣어 전달되는데, 시스템의 모든 메시지를 받을 수는 없으므로 원하는 메시지를 받기 위해서 인텐트 필터를 사용해 시스템에 등록하면 된다. 

### 브로드캐스트 수신자 사용, SMS 내용 액티비티에 나타내기

```java
//브로드캐스트 수신자 만들어지면 매니페스트 파일 안에 <receiver>태그 추가됨

<application
        //중략
        <receiver
          	//name 속성에 수신자 이름 정하기
            android:name=".SmsReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
              	//액션 정보를 넣어 어떤 인텐트를 받을 것인지 지정
              	//SMS메시지가 들어간 인텐트를 구분하기 위한 액션 정보
                <action android:name="android.provider.Telephony.SMS_RECEIVED" />
            </intent-filter>
        </receiver>
        //중략
</manifest>
```

```java
//SmsReceiver.java

public class SmsReceiver extends BroadcastReceiver {
    private static final String TAG = "SmsReceiver";
    public SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

  	//SMS받으면 자동으로 호출되는 메서드. 파라미터로 전달되는 Intent객체 안에 SMS데이터가 들어있다.
    @Override
    public void onReceive(Context context, Intent intent) {
        Log.d(TAG,"onReceive 호출됨");
      
      	//인텐트에서 Bundle객체 가져오기
        Bundle bundle = intent.getExtras();
      	//SMS메시지 객체를 만드는 메서드 호출
        SmsMessage[] messages = parseSmsMessage(bundle);

        if(messages != null && messages.length > 0){
          	//발신자 번호 확인하려면 getOriginatingAddres() 메서드 호출
            String sender = messages[0].getOriginatingAddress();
          	//문자 내용 확인하려면 getMessageBody() 메서드 호출
            String contents = messages[0].getMessageBody();
        		//문자 메시지 받은 시각
            Date receivedDate = new Date(messages[0].getTimestampMillis());

            Log.d( TAG, "sender : " + sender + ", contents : " + contents + ", date : " + receivedDate.toString() );
          	//새로운 화면을 띄우기 위한 메서드 호출
            sendToActivity(context, sender, contents, receivedDate);
        }
    }
  
		//다른 앱을 만들 때도 재사용 가능
  	//SMS데이터를 확인할 수 있도록 안드로이드 API에 정해둔 코드를 사용하므로 수정될 일이 거의 없기때문
    private SmsMessage[] parseSmsMessage(Bundle bundle) {
      	//Bundle객체에 들어있는 부가데이터 중에서 pdus 가져오기
        Object[] objs = (Object[]) bundle.get("pdus");
        SmsMessage[] messages = new SmsMessage[objs.length];

      	//단말 OS 버전에 따라 다른 방식으로 메서드 호출하기
        int smsCount = objs.length;
        for(int i=0; i < smsCount; i++){
          	//OS가 마시멜로 버전과 같거나 그 이후 버전일 때 중괄호 안의 코드를 실행
            if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.M){
                String format = bundle.getString("format");
              	//createFromPdu()메서드 사용해 SmsMessage객체로 변환하면 SMS데이터 확인 가능
                messages[i] = SmsMessage.createFromPdu((byte[])objs[i],format);
            } else {
                messages[i] = SmsMessage.createFromPdu((byte[]) objs[i]);
            }
        }
        return messages;
    }
		//SmsActivity로 인텐트를 보내기 위해 만든 것
    private void sendToActivity(Context context, String sender, String contents, Date receivedDate) {
     
        Intent myIntent = new Intent(context, SmsActivity.class);
      	//브로드캐스트 수신자는 화면이 없으므로 FLAG_ACTIVITY_NEW_TASK 꼭 추가해주기
        myIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK|Intent.FLAG_ACTIVITY_SINGLE_TOP|Intent.FLAG_ACTIVITY_CLEAR_TOP);

        myIntent.putExtra("sender", sender);
        myIntent.putExtra("contents", contents);
        myIntent.putExtra("receivedDate",format.format(receivedDate) );
      
				//인텐트를 SmsActivity쪽으로 전달
        context.startActivity(myIntent);
    }

}

```

앱에서 SMS를 수신하려면 RECEIVE_SMS라는 권한이 있어야 한다.

```java
//AndroidManifest.xml

<manifest xmlns:android="http://schemas.android.com/apk/res/android"
package="org.techtown.receiver">

	//<application>태그 위쪽에 <uses-permission>태그 추가
  //위험 권한. 소스 파일에서 앱 실행 후에 사용자가 권한을 부여할 수 있도록 별도의 코드가 추가되어야 함
	<uses-permission android:name="android.permission.RECEIVE_SMS" />
  
//중략
</manifest>
```

외부 라이브러리를 추가하여 간단하게 위험 권한을 추가하는 코드 넣어주기

```java
//build.gradle(Module:app)

//중략
//buildTypse 밑에 적어주기
allprojects{
    repositories{
        maven{url 'https://jitpack.io'}
    }
}

dependencies {
    //중략
    implementation 'com.github.pedroSG94:AutoPermissions:1.0.3'
}
```

```java
//MainActivity.java

//MainActivity가 인터페이스 구현하도록 하기
public class MainActivity extends AppCompatActivity implements AutoPermissionsListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

      	//모든 위험 권한을 자동 부여하도록 하는 메서드 호출
        AutoPermissions.Companion.loadAllPermissions(this,101);
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);

        AutoPermissions.Companion.parsePermissions(this, requestCode, permissions,this);
    }

    @Override
    public void onDenied(int requestCode, String[] permissions) {
        Toast.makeText(this,"permission denied : " + permissions.length, Toast.LENGTH_SHORT).show();

    }

    @Override
    public void onGranted(int requestCode, String[] permissions) {
        Toast.makeText(this, "permission granted : " + permissions.length, Toast.LENGTH_LONG);
    }
}
```

```java
//SmsActivity.java
//activity_sms.xml 생략

public class SmsActivity extends AppCompatActivity {

    TextView smsNumber;
    TextView smsContents;
    TextView smsDate;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sms);

        smsNumber = findViewById(R.id.SmsNumber);
        smsContents = findViewById(R.id.SmsContents);
        smsDate = findViewById(R.id.SmsDate);

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                finish();
            }
        });

        Intent passedIntent = getIntent();
        processIntent(passedIntent);
    }

    @Override
    protected void onNewIntent(Intent intent) {
        processIntent(intent);
        super.onNewIntent(intent);
    }
		
  	//인텐트가 null이 아니면 그 안에 들어있는 부가 데이터를 화면에 보여주기
    private void processIntent(Intent intent) {
        if(intent != null){
            String sender = intent.getStringExtra("sender");
            String contents = intent.getStringExtra("contents");
            String receivedDate = intent.getStringExtra("receivedDate");

            smsNumber.setText(sender);
            smsContents.setText(contents);
            smsDate.setText(receivedDate);
        }

    }
}
```

브로드캐스트 수신자는 매니페스트 파일 안에 `<receiver>` 태그로 추가되어 있지만 매니페스트에 등록하지 않고 소스 파일에서 `registerReceiver()` 메서드를 사용해 등록할 수도 있다. 소스 파일에서 등록하면 화면이 사용자에게 보일 때만 브로드캐스트 수신자에게 메시지를 받도록 만들 수 있음. 필요에 따라 매니페스트 파일이나 소스 파일에 등록하여 사용!

### 브로드캐스트 수신자 동작 방식 정리

단말에서는 SMS문자를 받았을 때 텔레포니(TelePhony) 모듈이 처리하도록 한다. 처리된 정보는 인텐트에 담겨 브로드캐스팅 방식으로 다른 앱에 전달된다. 지금 제작해본 앱도 그중의 하나가 되어 인텐트를 전달받으며, 인텐트를 전달 받았을 때 `onReceive()` 메서드가 자동으로 호출된다.

