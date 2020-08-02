# OUTPUT

## PWM

PWM(Pulse Width Modulation, 펄스 폭 변조) 효율적인 스위칭 제어의 기본이 되는, 디지털 출력으로 아날로그 회로를 제어하는 기술. HIGH, LOW를 계속 반복하는데, HIGH 시간을 길게 하느냐, LOW 시간을 길게하느냐에 따라 모터의 속도나 LED의 밝기의 변화를 확인 할 수 있다.

DC모터의 경우는 전압 변화에 기계적 반응이 느리기 때문에, LED의 경우는 인간의 눈으로 인지할 수 있는 속도보다 빠르게 On/Off 되기 때문에 속도/밝기가 조절됨!

### TR

대표적으로 많이 쓰는 부품은 2N222. 700mA정도까지 수용할 수 있다.

![TR](https://github.com/nyong-lab/TIL/blob/master/Image/TR.jpg) 

### FET

약 20A까지 수용할 수 있다.

![FET](https://github.com/nyong-lab/TIL/blob/master/Image/FET.jpg) 

**보통 TR, FET로 대부분의 출력을 제어하지만 특수한 경우 릴레이를 사용한다**.

### 릴레이

전자석 원리. 물리적인 버튼으로 제어하며 AC와 DC 모두 제어 가능하다. 220V 20A도 수용 가능하다. 아두이노는 AC와 전자석에 영향을 많이 받기 때문에, 릴레이를 사용하려면 보호회로 추가가 필요하다.

![RELAY](https://github.com/nyong-lab/TIL/blob/master/Image/RELAY.jpg) 

