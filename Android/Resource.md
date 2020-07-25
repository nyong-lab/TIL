# Resource & Manifest

안드로이드 앱은 크게 `자바 코드` 와 `리소스` 로 구성된다. `자바 코드` 에서는 **앱의 흐름과 기능을 정의**하고, `리소스` 에서는 레이아웃이나 이미지처럼 **사용자에게 보여주기 위해 사용하는 파일이나 데이터를 관리한다.**

## Manifest

매니페스트가 리소스는 아니지만 **설치된 앱의 구성 요소**가 어떤 것인지, **어떤 권한이 부여**되었는지 시스템에 알려주기 때문에 매우 중요하다. 모든 안드로이드 앱은 **가장 상위 폴더에 매니페스트 파일이 있어야 하며**, 이 정보는 앱이 실행되기 전에 시스템이 알아야 할 내용들을 정의하고 있다.

### 매니페스트 파일에 들어갈 수 있는 태그 항목

|                        |                    |                      |
| ---------------------- | ------------------ | -------------------- |
| <action>               | <instrumentation>  | <provider>           |
| <activity>             | <intent-filter>    | <receiver>           |
| <activity-alias>       | <manifest>         | <service>            |
| <application>          | <meta-data>        | <uses-configuration> |
| <category>             | <permission>       | <uses-library>       |
| <data>                 | <permission-group> | <uses-permission>    |
| <grant-uri-permission> | <permission-tree>  | <uses-sdk>           |

`<activity>` `<service>` `<receiver>` `<provider>`와 같은 태그들은 앱 구성 요소를 등록하기 위해 사용된다.

### 매니페스트의 주요 역할

* 앱의 패키지 이름 지정
* 앱 구성 요소에 대한 정보 등록(액티비티, 서비스, 브로드캐스트 수신자, 내용 제공자)
* 각 구성 요소를 구현하는 클래스 이름 지정
* 앱이 가져야 하는 권한에 대한 정보 등록
* 다른 앱이 접근하기 위해 필요한 권한에 대한 정보 등록
* 앱 개발 과정에서 프로파일링을 위해 필요한 instrumentation 클래스 등록
* 앱에 필요한 안드로이드 API의 레벨 정보 등록
* 앱에서 사용하는 라이브러리 리스트

### 매니페스트 파일의 기본 구조

```
<manifest ...>
	<application ...>
		...
			<service android:name="org.techtown.service.MyService" ...>
				...
			</service>
			...
	</application>
</manifest>
```

매니페스트 파일에는 타이틀이나 아이콘과 같은 앱 자체의 정보를 속성으로 지정할 수 있으며, 이미지 리소스로 포함된 정보들은 "@drawable/..."과 같이 참조하여 지정할 수 있다. 이 때 애플리케이션을 의미하는 <application> 태그는 매니페스트 안에 반드시 하나만 있어야 하는데, 나머지 <application> 태그 안의 구성 요소들은 같은 태그가 여러 번 추가되어도 된다. 

메인 액티비티는 항상 다음과 같은 형태로 추가되어야 한다. 즉, 인텐트 필터에 들어가는 정보는 <action> 태그의 경우 MAIN이 되어야 하고 <category> 태그의 경우 LAUNCHER가 되어야 한다.

```
<activity android:name="org.techtown.hello.HelloActivity"
					android:label="@string/app_name">
	<intent-filter>
		<action android:name="android.intent.action.MAIN"/>
		<category android:name="android.intent.category.LAUNCHER"/>
	</intent-filter>
</activity>
```

## Resource

리소스를 자바 코드와 분리하는 이유는 이해하기 쉽고 유지관리가 용이하기 때문이다. 프로젝트를 처음 만든 후에는 /app/res 폴더 이외에 /app/assets 폴더를 따로 만들 수 있는데 두 가지 모두 리소스라고 할 수 있으며 대부분은 /app/res 폴더 밑에서 관리된다. **두 가지 데이터의 차이점**은 다음과 같다.

* Asset은 **동영상이나 웹페이지와 같이 용량이 큰 데이터**를 의미한다
* Resource는 빌드되어 설치 파일에 추가되지만 **Asset은 빌드되지 않는다**

리소스는 /app/res 폴더 밑에 있는 여러 가지 폴더에 나누어 저장되며 리소스 유형별로 서로 다른 폴더에 저장한다. 실제로 만들 수 있는 폴더는 훨씬 많으며, 필요한 경우 만들어 사용한다. 리소스가 갱신되면 그 때마다 리소스 정보가 R.java 파일에 자동으로 기록되며 그 정보는 리소스에 대한 내부적인 포인터 정보가 된다.

* /app/res/values 폴더에는 **문자열이나 기타 기본 데이터 타입에 해당하는 정보들이 저장**된다
* /app/res/drawable 폴더에는 이미지를 저장한다. **단말의 해상도에 따라 다른 이미지를 보여줄 수 있도록 각각의 폴더를 나누어 저장**할 수 있는데, 그러면 각 단말에서 해상도에 맞는 폴더를 찾아 그 안에 들어있는 이미지를 참조하게 된다. 이렇게 저장되어 있는 리소스 정보를 코드에서 사용할 때에는 Resources 객체를 참조하여 리소스를 읽어들여야 한다. `Context.getResources()` 메서드를 이용해 액티비티 안에서 언제든지 참조할 수 있으며, 이 객체에는 리소스의 유형에 따라 읽어 들일 수 있는 메서드가 정의되어 있어 필요에 따라 사용할 수 있다.

## Style & Theme

스타일과 테마는 **여러 가지 속성들을 한꺼번에 모아서 정의한 것**으로 가장 대표적인 예로 대화상자를 들 수 있다. 대화상자의 경우에는 액티비티와 달리 타이틀 부분이나 모서리 부분의 형태가 약간 다르게 보이는데 이런 속성들을 다이얼로그 테마로 정의하여 액티비티에 적용하면 대화상자 모양으로 보이게 된다. 안드로이드에서 자주 사용되는 스타일과 테마를 제공하긴 하지만 필요에 따라 직접 정의해 사용해야 한다. 만약 스타일을 직접 정의하여 사용하고 싶다면 /app/res/values/styles.xml 파일을 만들어야 한다.

```
//styles.xml 파일 안에 정의한 <style> 태그

<style name = "Alert" parent="android:Theme.Dialog">
	<item name = "android:windowBackground"> @drawable/alertBackground </item>
</style>
```



