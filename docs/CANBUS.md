# CANBUS (Controller Area Network)

이 문서는 Klipper의 CAN bus 지원에 대해 설명합니다.

## Device Hardware

Klipper는 현재 stm32 칩에서만 CAN을 지원합니다. 또한, 마이크로 컨트롤러 칩은 CAN을 지원해야 하고 CAN 트랜시버가 보드에 있어야 합니다.
CAN용으로 컴파일하려면 "make menuconfig"를 실행하고 통신 인터페이스에서 "CAN bus"를 선택합니다. 마지막으로 마이크로 컨트롤러 코드를 컴파일하고 해당보드에 flash 합니다.


## Host Hardware

CAN bus를 사용하려면 호스트 어댑터가 필요합니다.
현재 두 가지 일반적인 옵션이 있습니다.

1. [Waveshare Raspberry Pi CAN hat](https://www.waveshare.com/rs485-can-hat.htm)을 사용하거나 다른 클론제품을 사용합니다.
   

2. USB CAN 어댑터를 사용하거나, (예: [https://hacker-gadgets.com/product/cantact-usb-can-adapter/](https://hacker-gadgets.com/product/cantact-usb-can-adapter/)). 시중에 사용가능한 다양한 USB to CAN 어댑터가 있습니다. - 고를때 [candlelight firmware](https://github.com/candle-usb/candleLight_fw)를 실행할 수 있는지 확인하는 것이 좋습니다.
(불행히도 일부 USB 어댑터에 결함이 있고 펌웨어 잠금이 되어 있으므로 구매 전 확인하시기 바랍니다.)

또한 어댑터를 사용하도록 호스트 운영 체제를 구성해야 합니다. 이것은 일반적으로 다음의 내용이 포함된 `/etc/network/interfaces.d/can0` 라는 이름의 새 파일을 만들어 수행됩니다.

```
auto can0
iface can0 can static
    bitrate 500000
    up ifconfig $IFACE txqueuelen 128
```

Note : "Raspberry Pi CAN hat"도 필요합니다.
[config.txt에서 변경](https://www.waveshare.com/wiki/RS485_CAN_HAT).


## 종단저항

CAN 버스에는 CANH와 CANL 와이어 사이에 2개의 120옴 저항이 있어야 합니다. 이상적으로는 버스의 각 끝에 하나의 저항이 있어야합니다.

일부 장치에는 120옴 저항이 내장되어 있습니다. (예: "Waveshare Raspberry Pi CAN 모자"에는 쉽게 제거할 수 없는 저항이 납땜되어 있음).
일부 장치에는 저항이 전혀 포함되어 있지 않습니다. 다른 장치에는 저항을 선택하는 메커니즘이 있습니다(일반적으로 "핀 점퍼" 연결).
CAN 버스에 있는 모든 장치의 회로도를 확인하여 버스에 2개의 120 Ohm 저항이 있는지 확인하십시오.

저항이 올바른지 테스트하기 위해 프린터의 전원을 끄고 멀티미터를 사용하여 CANH와 CANL 와이어 사이의 저항을 확인할 수 있습니다.
제대로 배선된 CAN bus에서 ~60옴이 나타나야 합니다.

## 새로운 micro-controllers의 canbus_uuid 찾기

Each micro-controller on the CAN bus is assigned a unique id based on
the factory chip identifier encoded into each micro-controller. To
find each micro-controller device id, make sure the hardware is
powered and wired correctly, and then run:

CAN 버스의 각 마이크로 컨트롤러에는 각 마이크로 컨트롤러에 인코딩된 공장 칩 식별자를 기반으로 고유한 ID가 할당됩니다.
각 마이크로 컨트롤러 장치 ID를 찾으려면 하드웨어에 전원이 공급 및 배선이 올바르게 연결되었는지 확인한 후 다음을 실행합니다.

```
~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0
```

만약 초기화되지 않은 CAN 장치가 감지되면 위의 명령은 다음과 같은 행을 보고합니다.

```
Found canbus_uuid=11aa22bb33cc
```

각 장치에는 고유한 식별자가 있습니다. 위의 예에서 `11aa22bb33cc`는 마이크로 컨트롤러의 "canbus_uuid"입니다.

Note : `canbus_query.py` 도구는 초기화되지 않은 장치만 보고합니다. 만약 Klipper(또는 유사한 도구)가 장치를 구성하면 더 이상 목록에 나타나지 않습니다.


## Klipper 설정하기

장치와 통신하기 위해 CAN bus를 사용하도록 Klipper [mcu 구성](Config_Reference.md#mcu)을 업데이트합니다.
예를 들면 다음과 같습니다.

```
[mcu my_can_mcu]
canbus_uuid: 11aa22bb33cc
```
