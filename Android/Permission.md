# Dangerous Permission

매니페스트에 넣어준 권한은 앱을 설치할 때 사용자가 허용하면 한꺼번에 권한이 부여되는데, `마시멜로(API 23)` 부터는 중요한 권한들을 분류하여 설치 시점이 아니라 앱을 실행했을 때 사용자로부터 권한을 부여받도록 변경되었다. 

### 일반 권한과 위험 권한의 차이점

마시멜로 버전부터는 권한을 일반 권한과 위험 권한으로 나누었다. 앱을 설치하는 시점에 사용자에게 물어보는 기존방식은 사용자가 아무 생각없이 앱을 설치하는 경우가 많았고, 이에 따라 설치된 앱들이 단말의 주요 기능을 마음대로 사용했기 때문에 위험 권한으로 분류된 권한에 대해서는 앱을 설치할 때가 아니라 앱을 실행할 때 권한을 부여하도록 변경된 것이다. 만약 사용자가 권한을 부여하지 않으면 해당 기능은 동작하지 않는다.

위험 권한으로 분류된 주요 권한들을 보면 대부분 개인정보가 담겨있는 정보에 접근하거나 개인정보를 만들어낼 수 있는 단말의 주요 장치에 접근할 때 부여된다. 권한 그룹(Permission Group)은 동일한 기능을 접근하는데 몇 가지 세부 권한을 하나로 묶어주는 역할을 한다.

| 위험 권한 그룹의 분류(Permission Group) | 세부 권한(Permission)                                       |
| --------------------------------------- | ----------------------------------------------------------- |
| LOCATION(위치)                          | ACCESS_FINE_LOCATION/ACCESS_COARSE_LOCATION                 |
| CAMERA                                  | CAMERA                                                      |
| MICROPHONE                              | RECORD_AUDIO                                                |
| CONTACTA                                | READ_CONTACTS/WRITE_CONTACTS/GET_ACCOUNTS                   |
| PHONE                                   | READ_PHONE_STATE/CALL_PHONE/READ_CALL_LOG                   |
|                                         | WRITE_CALL_LOG/ADD_VOICEMAIL/USE_SIP/PROCESS_OUTGOING_CALLS |
| SMS                                     | SEND_SMS/RECEIVE_SMS/READ_SMS/RECEIVE_WAP_PUSH/RECEIVE_MMS  |
| CALENDER                                | READ_CALENDAR/WRITE_CALENDAR                                |
| SENSORS                                 | BODY_SENSORS                                                |
| STORAGE                                 | READ_EXTERNAL_STORAGE/WRITE_EXTERNAL_STORAGE                |

주요 위험 권한 외에 SD 카드에 접근할 때 사용하는 `READ_EXTERNAL_STORAGE` `WRITE_EXTERNAL_STORAGE` 도 위험 권한으로 분류되었다는 점에 주목할 것! 기존에 사용하는 많은 앱들이 SD 카드를 접근하고 있기 때문이다. 앞으로 SD카드에 접근할 일이 많을 것이므로 위험 권한을 부여하는 방법에 대해서 잘 알아두는게 좋다:wink:

## 위험 권한 부여하는 방법

### 기본적인 방법

```java
//AndroidManifest.xml 파일에 권한 추가
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="org.techtown.permission">

    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
      
//중략
```

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //위험 권한을 부여할 권한 지정하기
        String[] permissions = {
                Manifest.permission.READ_EXTERNAL_STORAGE,
                Manifest.permission.WRITE_EXTERNAL_STORAGE
        };

        //checkPermissions() 메서드 호출
        checkPermissions(permissions);
    }

    private void checkPermissions(String[] permissions) {
        ArrayList<String> targetList = new ArrayList<String>();

        //정해준 권한들에 대해서 그 권한이 부여되어 있는지 확인
        for(int i=0; i<permissions.length; i++){
            String curPermission = permissions[i];
            int permissionCheck = ContextCompat.checkSelfPermission(this,curPermission);

            if(permissionCheck == PackageManager.PERMISSION_GRANTED){
                Toast.makeText(this, curPermission + " 권한 있음." ,Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(this, curPermission + " 권한 없음.", Toast.LENGTH_SHORT).show();
                if(ActivityCompat.shouldShowRequestPermissionRationale(this,curPermission)){
                    Toast.makeText(this,curPermission + " 권한 설명 필요함.",Toast.LENGTH_SHORT).show();
                } else { //부여되지 않았다면 ArrayList 안에 넣었다가 부여되지 않은 권한들만 권한 요청
                    targetList.add(curPermission);
                }
            }
        }

        String[] targets = new String[targetList.size()];
        targetList.toArray(targets);

        ActivityCompat.requestPermissions(this,targets,101);
    }

    //요청 코드와 함께 사용자가 권한을 수락했는지 여부가 파라미터로 전달됨
    @Override
    public void onRequestPermissionsResult(int requestCode, String permissions[], int[] grantResults){
    //여러 권한을 한 번에 요청할 수도 있으니 grantResults 배열 변수 안에 수락 여부를 넣어 전달
    //앞에서 두 개의 권한을 요청했으므로 grantResults 배열의 길이는 2
        switch (requestCode){
            case 101 : {
                if(grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED){
                    Toast.makeText(this,"첫 번째 권한을 사용자가 승인함.", Toast.LENGTH_SHORT).show();
                } else {
                    Toast.makeText(this,"첫 번째 권한 거부됨.",Toast.LENGTH_SHORT).show();
                }

                return;
            }
        }
    }

}
```

### 외부 라이브러리를 이용하는 방법

```java
//build.gradle(:app)

allprojects {
    repositories {
        maven { url 'https://jitpack.io' }
    }
}

dependencies {
    //중략
    implementation 'com.github.pedroSG94:AutoPermissions:1.0.3'

}
```

```java
//AndroidManifest.xml 파일에 권한 추가
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="org.techtown.permission">

    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
      
//중략
```

```java
//MainActivity.java

public class MainActivity extends AppCompatActivity implements AutoPermissionsListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //위험 권한 자동부여 요청하기
        AutoPermissions.Companion.loadAllPermissions(this,101);
    }

    //권한 부여 요청 결과가 넘어오는 메서드
    @Override
    public void onRequestPermissionsResult(int requestCode, String permissions[], int[] grantResults){
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        //parsePermissions() 메서드 호출 -> 권한 부여 결과가 승인 또는 거부로 나뉘면서 onGranted() 또는 onDenied()메서드 호출됨
        AutoPermissions.Companion.parsePermissions(this, requestCode, permissions, this);
    }

    @Override
    public void onDenied(int requestCode, String[] permissions) {
        Toast.makeText(this, "permission denied : " + permissions.length, Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onGranted(int requestCode, String[] permissions) {
        Toast.makeText(this, "permission granted : " + permissions.length, Toast.LENGTH_SHORT).show();
    }

}
```

