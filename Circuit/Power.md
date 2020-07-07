# 전원(메인)회로

## 전압강하
전압을 강하해주는 부품에는 `SMPS`와 `레귤레이터`가 있다.  
SMPS : DC를 AC로 변환하여 전압을 강하하고 다시 DC로 변환해준다.  
레귤레이터 : 전압을 열로 방출해서 강하.  

## MCU, UART TO USB 
아두이노에는 UART를 USB로 넣어주는 부품이 내장되어 있지만,  
MCU를 따로 사용할 때는 코드를 USB로 업로드하기 위하여 UART TO USB 회로를 넣어주어야 한다.  

`CP2102` 크리스탈이 내장되어 있음.  
`CH340` 드라이브 설치가 필요하며, 크리스탈을 따로 달아주어야 한다.  
`FT232RL` 아두이노 정품에 사용되는 부품. 크리스탈이 내장되어 있다.  

![UART TO USB](https://github.com/nyong-lab/TIL/blob/master/Image/UART_TO_USB.jpg)  
우선 ISP로 부트로더 프로그램을 업로드 한 이후에 사용 가능.  

**AUTO RESET**  
원래 아두이노에 전원을 공급해주면 몇ms동안 프로그램 업로드 대기시간을 준 후 코드를 실행하게 되는데,  
코드를 실행한 후에는 코드를 업로드할 수 없게된다. 중간에 코드를 업로드 할 수 있도록 자동으로 리셋시켜주는 회로가 필요하다.  
DTR# 포트에 직렬로 100nf 캐패시터를 달아주고 리셋 포트 연결해주면 됨!  