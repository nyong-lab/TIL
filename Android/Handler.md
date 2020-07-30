# Handler
> [개념 이해를 도와준 블로그(코틀린)](https://jungwoon.github.io/android/2019/09/25/Handler-Looper/)

`스레드(Thread)` 는 **동시 수행이 가능한 작업 단위**이며, 현재 수행 중인 작업 이외의 기능을 **동시에 처리할 때** 새로운 스레드를 만들어 처리한다. 멀티 스레드 방식은 같은 프로세스 안에 들어 있으면서 메모리 리소스를 공유하므로 효율적인 처리가 가능하다. 하지만 동시에 리소스에 접근할 때 `데드락(DeadLock)` 이 발생하여 시스템이 비정상적으로 동작할 수도 있다. 여러개의 스레드가 **동시에 공통 메모리 리소스에 접근**할 때 데드락이 발생한다. 데드락이란 **동시에 두 곳 이상에서 요청이 생겼을 때 어떤 것을 먼저 처리할지 판단할 수 없어 발생**하는 시스템 상의 문제이다. 

안드로이드에서 UI를 처리할 때 사용되는 기본 스레드를 **메인 스레드**라고 부른다. 메인 스레드에서 이미 UI에 접근하고 있으므로 새로 생성한 다른 스레드에서는 `핸들러(Handler)` 객체를 사용해서 메시지를 전달함으로써 메인 스레드에서 처리하도록 만들 수 있다.

> 동시 접근으로 발생하는 데드락 문제를 해결하는 가장 간단한 방법은 작업을 **순서대로** 처리하는 것이다. 이 역할은 각 스레드 안에서 동작하는 **핸들러**가 담당한다.

### 스레드와 핸들러 사용하기

스레드는 new 연산자로 객체를 생성한 후 `start()` 메서드를 호출하면 시작할 수 있다. Thread 클래스에 정의된 생성자는 크게 파라미터가 없는 경우와 Runnable 객체를 파라미터로 갖는 두 가지로 구분할 수 있다. 일반적으로 Thread 클래스를 상속한 새로운 클래스를 정의한 후 객체를 만들어 시작하는 방법을 사용한다.

```java
public class MainActivity extends AppCompatActivity {
    TextView textView;
    int value = 0;

    ValueHandler handler = new ValueHandler();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = (TextView) findViewById(R.id.textView);

        Button button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                BackgroundThread thread = new BackgroundThread();
                thread.start();
            }
        });

        Button button2 = (Button) findViewById(R.id.button2);
        button2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //textView.setText("현재 값 : " + value);
            }
        });
    }

    class BackgroundThread extends Thread {

        int value = 0;
        boolean running = false;

        public void run(){
            running = true;

            while(running){
               try {
                    Thread.sleep(1000);
                } catch (Exception e){}
                value += 1;
								
              	//새로 만든 스레드가 수행하려는 정보를 메인 스레드로 전달하기 위해서는
              	//먼저 핸들러가 관리하는 메시지 큐에서 처리할 수 있는 메시지 객체 하나를 참조해야함
              	//obtainMessage() 메서드 호출의 결과로 메시지 객체를 반환받음
                Message message = handler.obtainMessage();
                Bundle bundle = new Bundle();
                bundle.putInt("value", value);
              	//필요한 정보를 메시지 객체에 넣음
                message.setData(bundle);
              	//핸들러로 메시지 객체 보내기
                handler.sendMessage(message);
            }
        }
    }

    class ValueHandler extends Handler {
				
      	//핸들러 안에서 전달받은 메시지 객체 처리하기
      	//handleMessage()메서드를 다시 정의하면 메시지가 메인 스레드에서 수행될 때 필요한 기능을 넣어둘 수 있음
        @Override
        public void handleMessage(@NonNull Message msg) {
            super.handleMessage(msg);
						
            Bundle bundle = msg.getData();
            int value = bundle.getInt("value");
            textView.setText("현재 값 : " + value);
        }
    }

}
```

### Runnable 객체 실행하기

위 방법은 일반적이지만 코드가 복잡해 보이는 단점이 있다. 핸들러 클래스는 메시지 전송 방법 이외에 Runnable 객체를 실행시킬 수 있는 방법을 제공한다. 즉, 새로 만든 Runnable 객체를 핸들러의 `post()` 메서드로 전달해주면 이 객체에 정의된 `run()` 메서드 안의 코드들은 메인 스레드에서 실행된다.

```java
public class MainActivity extends AppCompatActivity {
    TextView textView;
  	//API의 기본 핸들러 객체 생성
    Handler handler2 = new Handler();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = (TextView) findViewById(R.id.textView);

        Button button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
              
                new Thread(new Runnable() {
                    int value = 0;
                    boolean running = false;

                    @Override
                    public void run() {
                        running = true;
                        while (running) {
                            value += 1;
														
                          	//핸들러의 post() 메서드 호출
                            handler2.post(new Runnable() {
                                @Override
                                public void run() {
                                    textView.setText("현재 값 : " + value);
                                }
                            });

                            try {
                                Thread.sleep(1000);
                            } catch (Exception e){}
                        }
                    }

                }).start();
            }
        });

    }
```

## 일정 시간 후에 실행하기

웹서버와 같은 원격 서버에 접속한 후 웹페이지를 요청할 때 응답이 늦어지거나 응답이 없으면 앱이 대기하고 있는 상황이 지속되는 문제가 생김. 이런 경우에 기본적으로 별도의 스레드를 만들어 처리하게 된다. 하지만 버튼을 클릭해서 간단하게 접속 처리하는 경우에는 메인 스레드 내에서 지연 시간을 주는 것만으로도 UI의 멈춤 현상을 방지할 수 있고, 단순히 Thread.sleep() 메서드를 사용해서 잠깐 대기 상태로 있다가 다시 실행할 수도 있다. 하지만 핸들러로 실행되는 코드는 메시지 큐를 통과하면서 순차적으로 실행되기 때문에 UI 객체들에 영향을 주지 않으면서 지연 시간을 두고 실행된다.

```java
public class MainActivity extends AppCompatActivity {
    TextView textView;

    Handler handler = new Handler();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = findViewById(R.id.textView);

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                request();
            }
        });
    }

    private void request() {
        String title = "원격 요청";
        String message = "데이터를 요청하시겠습니까?";
        String titleButtonYes = "예";
        String titleButtonNo = "아니오";
        AlertDialog dialog = makeRequestDialog(title,message,titleButtonYes,titleButtonNo);
        dialog.show();

        textView.setText("대화상자 표시중...");
    }

    private AlertDialog makeRequestDialog(String title, String message, String titleButtonYes, String titleButtonNo) {
        AlertDialog.Builder requestDialog = new AlertDialog.Builder(this);
        requestDialog.setTitle(title);
        requestDialog.setMessage(message);
        requestDialog.setPositiveButton(titleButtonYes, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                textView.setText("5초 후에 결과 표시됨.");

                //일정 시간이 지난 후에 특정 코드를 실행시킬 때 사용하는 핸들러의 postDelayed() 메서드
                handler.postDelayed(new Runnable() {
                    @Override
                    public void run() {
                        textView.setText("요청 완료됨");
                    }
                }, 5000);
            }
        });

        requestDialog.setNegativeButton(titleButtonNo, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {}
        });

        return requestDialog.create();
    }
}
```

핸들러는 메시지 큐를 사용하므로 메시지들을 순서대로 처리하지만 메시지를 넣을 때 시간을 지정하면 원하는 시간에 메시지를 처리하게 만들 수 있다. 따라서 일정 시간 후에 실행시킬 때 유용하게 사용된다. 시간을 지정할 때는 핸들러의 `sendMessage()` 메서드와 유사한 이름을 가진 다음과 같은 두 가지 메서드를 사용할 수 있다.

```java
//메시지를 보낼 때 시간을 지정할 수 있음
public boolean sendMessageAtTime(Message msg, long uptimeMillis)
//메시지가 일정 시간이 지난 후 실행되도록 설정할 수 있음
public boolean sendMessageDelayed(Message msg, long delayMillis)
  
* Runnable 객체를 실행하는 post() 메서드도 postAtTime()과 postDelayed() 메서드가 있어 같은 기능 수행
```

### 스레드로 메시지 전송하기

메인 스레드에서 별도의 스레드로 메시지를 전달하는 방법이 필요할 때가 있다. 하지만 별도의 스레드가 관리하는 동일한 객체를 여러 스레드가 접근할 때는 별도의 스레드 안에 들어있는 메시지 큐를 이용해 순서대로 접근하도록 만들어야 한다. 핸들러가 처리하는 메시지 큐는 `루퍼(Looper)` 로 처리되는데, 그 과정은 일반적인 이벤트 처리 과정과 유사하다. 루퍼는 메시지 큐에 들어오는 메시지를 지속적으로 보면서 하나씩 처리하게 된다. 메인 스레드는 UI 객체들을 처리하기 위해 메시지 큐와 루퍼를 사용한다. 그러나 **별도의 스레드를 새로 만들었을 때는 루퍼가 없다**. 따라서 메인 스레드나 다른 스레드에서 메시지 전송 방식으로 스레드에 데이터를 전달한 후 **순차적으로 작업을 수행하고 싶다면 루퍼를 만든 후 실행해야 한다**.

```java
public class MainActivity extends AppCompatActivity {
    EditText editText;
    TextView textView;

    Handler handler = new Handler();
    ProcessThread thread;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        editText = findViewById(R.id.editText);
        textView = findViewById(R.id.textView);

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String input = editText.getText().toString();
                //Message 객체는 Message.obtain() 메서드 이용해 참조 가능
                Message message = Message.obtain();
                message.obj = input;

                //새로 만든 별도의 스레드로 Message 객체 전송
                thread.processHandler.sendMessage(message);
            }
        });

        //전달받은 문자열에 다른 문자열을 덧붙여 메인 스레드 쪽으로 다시 전송
        thread = new ProcessThread();
    }

    class ProcessThread extends Thread {
        ProcessHandler processHandler = new ProcessHandler();

        public void run() {
            //Looper.prepare(), Looper.loop() 메서드 호출하면 Message 객체 전달받을 수 있음
            Looper.prepare();
            Looper.loop();
        }

        class ProcessHandler extends Handler {
            public void handleMessage(Message msg) {
                final String output = msg.obj + "from thread";
                handler.post(new Runnable() {
                    @Override
                    public void run() {
                        textView.setText(output);
                    }
                });
            }
        }
    }
}
```



