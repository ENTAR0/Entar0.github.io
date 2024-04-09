---
title: "아두이노 및 센서 핀아웃 공부"
date: 2024-03-28 02:23:00 +0900
categories: [HardWare]
tags: [TeamProject, Arduino, Sensor, GYRO]
---
# 1. 소개글
2024년 맞이해서 창업동아리 활동을 시작하게 되었습니다. 첫번째 프로젝트로 애완견을 위한 스마트 커넥티드 웨어러블을 제작하기로 하여서 아두이노로 기기를 제작해보기로 하였습니다. 

```
사용 센서 및 MCU
1. Arduino pro mini
2. mpu6050
3. ESP8266-6
4. 3.3V <-> 5V 변압기(logic level converter)
5. 100m 구부림 휨 압력센서 500G
6. Pulse Sensor Heart Rate Sensor
7. NEO-6M GY-GPS6MV2
```

<center>
<img src="https://github.com/cisco/openh264/assets/56510688/8b399d39-8b84-4697-9a11-57ea6be4abc9" width="" height=""/>
<p><b>[그림1]. 사용 센서(mm)</b></p>
</center>


# 2. HardWare Specification
## 2.1. Arduino pro mini
작은 케이스안에 모든 부품을 조립해야하기 때문에 범용성 높은 Arduino pro mini를 선택했습니다. 
보통 아두이노 초기화 시 Flash에서 데이터를 읽어 SRAM에 저장하여 코드 실행 시 빠르게 처리 할 수 있도록 작동됩니다.

| 아두이노 보드종류 |   미니   |
|:----------------:|:------:|
| MCU              | ATmega328 |
| 동작 주파수(HZ)  | 8 or 16 |
| 크기(mm)         | 17.8x33 |
| 동작 전압(V)     | 3.3 or 5 |
| 추천 입력 전압(V)     | 3.35 ~ 12(3.3V model) or 5 ~ 12(5V model) |
| 플래쉬(KByte)    | 32      |
| EEPROM(KByte)    | 2       |
| SRAM(KByte)      | 1       |
| 디지털(핀)       | 14      |
| PWM(핀)          | 6       |
| 아날로그(핀)     | 6       |
| 전류(mA)             | 40      |
| 케이블           | x       |

- 플래쉬 : 비휘발성 저장장치
- EEPROM : 보통 부트로더 저장장치
- SRAM : 동적 메모리 부족할 때 땡겨 쓸 수 있음.
- PWM : Pulse Width Modulation의 약자로 아날로그와 유사한 제어가 가능한 디지털 신호를 입출력 할 수 있다.

<center>
<img src="https://github.com/cisco/openh264/assets/56510688/29a63d1e-dc60-42f1-8989-cc6802f16e3b" width="720" height=""/>
<p><b>[그림2]. ARDUINO PROMINI</b></p>
</center>

## 2.2. mpu6050
강아지의 앞발 움직임을 통해 현재 무슨 동작을 취하고 있는지 인식하기 위해서 앞발의 3차원 움직임을 추적할 자이로센서를 선택했습니다.


<center>
<img src="https://github.com/cisco/openh264/assets/56510688/1dd549e8-e017-4fc3-9f92-462a81c5faae" width="" height=""/>
<p><b>[그림2]. MPU6050</b></p>
</center>

<center>
<img src="https://github.com/cisco/openh264/assets/56510688/62a33cdb-067e-4bed-9069-9a02c532b10f" width="" height=""/>
<p><b>[그림2]. MPU6050</b></p>
</center>
