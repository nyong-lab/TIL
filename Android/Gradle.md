# Gradle

안드로이드 앱을 실행하거나 앱 스토어에 올릴 때는 소스 파일이나 리소스 파일을 빌드하거나 배포하는 작업이 필요하다. 이 때 사용되는 것이 `Gradle` 이다. 다시 말해, 그래들은 안드로이드 스튜디오에서 사용하는 빌드 및 배포 도구인 것이다.

한 앱의 빌드 설정은 build.gradle 파일에 넣어 관리한다. 이 때 그래들 파일은 `프로젝트 수준` 과 `모듈 수준` 으로 나눠 관리하기 때문에 새로운 프로젝트를 만들면 두 개의 build.gradle 파일이 생긴다.

### build.gradle(project:~)

```java
buildscript {
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:4.0.0"
    }
}

allprojects {
    repositories {
        google()
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

이 파일은 프로젝트 안에 들어있는 모든 모듈에 적용되는 설정을 담고 있다. 이 파일을 수정하는 경우는 거의 없다. 가끔 외부 도구를 포함시키기 위해 buildscript의 dependencies 안에 classpath를 추가하는 정도의 수정만 있을 수 있다.

### build.gradle(Moudule:app)

```java
apply plugin: 'com.android.application'

android {
    compileSdkVersion 29 //빌드를 진행할 때 어떤 버전의 SDK를 사용할 것인지 지정. 보통 최신 버전의 SDK를 지정하게 된다
    buildToolsVersion "30.0.0"

    defaultConfig {
        applicationId "org.techtown.permission2" //이 앱의 id값. 전 세계에서 유일한 값으로 설정되어야 한다.
        minSdkVersion 16 //이 앱이 어떤 하위 버전까지 지원하도록 할 것인지 지정
        targetSdkVersion 29 //이 앱이 검증된 버전이 어떤 SDK 버전인지를 지정
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}

allprojects {
    repositories {
        maven { url 'https://jitpack.io' }
    }
}

dependencies { //외부 라이브러리 추가할 수 있음
  
  	//libs 폴더 안에 들어있는 jar파일을 읽어들이고 support패키지를 추가
    implementation fileTree(dir: "libs", include: ["*.jar"])
    implementation 'androidx.appcompat:appcompat:1.1.0'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'androidx.test.ext:junit:1.1.1'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'
		//직접 추가한 외부 라이브러리
    implementation 'com.github.pedroSG94:AutoPermissions:1.0.3'

}
```

이 파일은 각각의 모듈에 대한 설정을 담고 있다. 프로젝트가 만들어지면 app 모듈은 기본으로 만들어지는데, 이 파일이 app 모듈의 설정 정보를 담고있는 것이다. 만약 새로운 모듈을 추가한다면 그 모듈에 대한 build.gradle 파일도 새로 추가된다. 이 파일에는 빌드에 중요한 정보들이 들어 있다.