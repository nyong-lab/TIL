# Audio

## 앰프 등급
:speaker: 소리는 AC이며, 소리의 크기는 전압에 비례한다.   

### A Class AMP

![A-amp](https://github.com/nyong-lab/TIL/blob/master/Image/A_AMP.jpg)  
전압을 증폭시켜서 소리를 키워줌. 메인 전압을 넘겨 증폭할 수 없으며 열이 많이 발생하고 높은 전압이 필요하다.  
노이즈 없이 사운드가 깨끗하며, 원 신호의 손상 없이 소리를 증폭할 수 있다.  

### B Class AMP

![B-amp](https://github.com/nyong-lab/TIL/blob/master/Image/B_AMP.jpg)  
(-)전압을 만들어 소리의 폭을 두배로 증폭해준다. 크기가 작고, 적은 전압으로도 소리를 크게 키울 수 있지만  
강제로 소리의 크기를 늘려주기 때문에 노이즈가 생긴다.  

## DF PLAYER MINI

![DF_PLAYER](https://github.com/nyong-lab/TIL/blob/master/Image/DF_PLAYER.jpg)  
`RX,TX` 아두이노와 소통하는 부분  
`DAC_R, DAC_L` A CLASS AMP로 연결되어 증폭 후 스테레오로 출력  
`SPK_1, SPK_2` 오디오 신호선으로, 두선 다 스피커 하나에 연결해야함. C CLASS AMP로 연결되어 증폭 후 모노로 출력되며 최대 출력은 3W.  
`BUSY` 재생중일땐 HIGH, 끝나면 LOW. 소리가 재생되고 있는지 아닌지를 판단해주는 포트이다.  
* 아두이노는 통신레벨 5.0V, DF PLAYER는 3.3V인데 BUSY포트를 사용하면 아두이노가 간혹 DF PLAYER로  
역신호를 보내는 경우가 있어서 DF PLAYER가 죽는 경우가 생긴다.  
