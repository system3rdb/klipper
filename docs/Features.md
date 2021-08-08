# Features

Klipper 에는 다음과 같이 몇가지 매력적인 기능이 있습니다:

* <b>고정밀 스테퍼 동작.</b> Klipper 는 3D 프린터의 움직임을 계산할 때 라즈베리파이 같은 저렴한 애플리케이션 프로세서를 활용합니다. 애플리케이션 프로세서는 각 스테퍼 모터를 언제 움직여야 하는지 결정하고 해당 이벤트를 압축하여 마이크로 컨트롤러에 전송한 다음 마이크로 컨트롤러가 요청된 시간에 각 이벤트를 실행합니다. 각 스테퍼 이벤트는 25 마이크로초 이상의 정밀도로 스케줄링 됩니다. 소프트웨어는 운동학적 추정(예: Bresenham 알고리즘)을 사용하지 않습니다. 대신 가속 물리학 및 기계 운동학 물리학을 기반으로 정확한 단계 시간을 계산합니다. 더 정밀한 스테퍼 움직임은 더 조용하고 안정적인 프린터 작동으로 이어집니다.

* <b>동급 최고의 성능.</b> Klipper는 신규 및 기존 마이크로 컨트롤러 모두에서 높은 스테핑 속도를 달성할 수 있습니다. 심지어 오래된 8비트 마이크로 컨트롤러도 초당 175K 단계 이상의 속도를 얻을 수 있습니다. 최신 마이크로 컨트롤러에서는 초당 500K 단계 이상의 속도가 가능합니다. 더 빠른 스테퍼 속도는 더 빠른 인쇄 속도를 가능하게 합니다. 스테퍼 이벤트 타이밍은 고속에서도 정밀하게 유지되어 전반적인 안정성을 향상시킵니다.

* <b>Klipper는 다수개의 마이크로 컨트롤러가 있는 프린터를 지원합니다.</b> 예를 들어, 하나의 마이크로 컨트롤러는 압출기를 제어하는데 사용되고 다른 하나는 프린터의 히터를 제어하고 세 번째는 나머지 프린터를 제어하는데 사용할 수 있습니다. Klipper 호스트 소프트웨어는 마이크로 컨트롤러간의 클럭 드리프트를 계산하기 위해 클록 동기화를 구현합니다.여러 마이크로 컨트롤러를 활성화하는데 특별한 코드는 필요하지 않습니다. 구성 파일에 몇 줄만 추가하면 됩니다.

* <b>간단한 config 파일을 통한 구성.</b> 설정을 변경하기 위해 마이크로 컨트롤러를 다시 펌웨어 업데이트할 필요가 없습니다. Klipper의 모든 구성은 쉽게 편집할 수 있는 표준 config 파일에 저장됩니다. 이렇게 하면 하드웨어를 더 쉽게 설정하고 유지 관리할 수 있습니다.

* <b>Klipper는 압출기 내 압력의 영향을 설명하는 메커니즘인 "Smooth Pressure Advance"를 지원합니다.</b> 이것은 압출기의 "얼룩"을 줄이고 인쇄 모서리의 품질을 향상시킵니다. Klipper의 구현은 전반적인 안정성과 견고성을 향상시키는 즉각적인 압출기 속도 변경을 도입하지 않습니다.

* <b>Klipper는 인쇄 품질에 대한 진동의 영향을 줄이기 위해 "Input Shaping"을 지원합니다.</b> 이렇게 하면 인쇄물에서 "진동형 무늬" ("고스팅", "에코잉" 또는 "리플링"이라고도 함)을 줄이거나 제거할 수 있습니다. 또한 높은 인쇄 품질을 유지하면서 더 빠른 인쇄 속도를 얻을 수 있습니다.

* <b>Klipper는 "iterative solver"를 사용하여 간단한 운동 방정식에서 정확한 단계 시간을 계산합니다.</b> 이를 통해 Klipper를 새로운 유형의 로봇에 쉽게 이식할 수 있으며 복잡한 운동학에서도 정확한 타이밍을 유지할 수 있습니다("Line segmentation"이 필요하지 않음).
 
* <b>이식 가능한 코드.</b> Klipper는 ARM, AVR 및 PRU 기반 마이크로 컨트롤러에서 작동합니다. 기존 "reprap" 스타일 프린터는 하드웨어 수정 없이 Klipper를 실행할 수 있습니다. Raspberry Pi를 추가하기만 하면 됩니다. Klipper의 내부 코드 레이아웃을 통해 다른 마이크로 컨트롤러 아키텍처도 쉽게 지원할 수 있습니다.

* <b>더 간단한 코드.</b> Klipper는 대부분의 코드에 대해 매우 높은 수준의 언어(Python)를 사용합니다. 운동학 알고리즘, gcode 해석, 가열 및 온도센서 알고리즘 등은 모두 Python으로 작성되었습니다. 이를 통해 새로운 기능을 더 쉽게 개발할 수 있습니다.

* <b>사용자 정의 가능한 매크로.</b> 새로운 gcode 명령을 프린터 구성 파일에서 정의할 수 있습니다 (코드 변경이 필요하지 않음). 이러한 명령은 프로그래밍할 수 있으므로 프린터 상태에 따라 다른 작업을 생성할 수 있습니다.

* <b>내장 API 서버.</b> 표준 gcode 인터페이스 외에도 Klipper는 풍부한 JSON 기반 애플리케이션 인터페이스를 지원합니다. 이를 통해 프로그래머는 프린터를 세부적으로 제어하여 외부 응용 프로그램을 구축할 수 있습니다.

## Additional features

Klipper supports many standard 3d printer features:

* Works with Octoprint. This allows the printer to be controlled using
  a regular web-browser. The same Raspberry Pi that runs Klipper can
  also run Octoprint.

* Standard G-Code support. Common g-code commands that are produced by
  typical "slicers" are supported. One may continue to use Slic3r,
  Cura, etc. with Klipper.

* Support for multiple extruders. Extruders with shared heaters and
  extruders on independent carriages (IDEX) are also supported.

* Support for cartesian, delta, corexy, corexz, rotary delta, polar,
  and cable winch style printers.

* Automatic bed leveling support. Klipper can be configured for basic
  bed tilt detection or full mesh bed leveling. If the bed uses
  multiple Z steppers then Klipper can also level by independently
  manipulating the Z steppers. Most Z height probes are supported,
  including BL-Touch probes and servo activated probes.

* Automatic delta calibration support. The calibration tool can
  perform basic height calibration as well as an enhanced X and Y
  dimension calibration. The calibration can be done with a Z height
  probe or via manual probing.

* Support for common temperature sensors (eg, common thermistors,
  AD595, AD597, AD849x, PT100, PT1000, MAX6675, MAX31855, MAX31856,
  MAX31865, BME280, HTU21D, and LM75). Custom thermistors and custom
  analog temperature sensors can also be configured.

* Basic thermal heater protection enabled by default.

* Support for standard fans, nozzle fans, and temperature controlled
  fans. No need to keep fans running when the printer is idle.

* Support for run-time configuration of TMC2130, TMC2208/TMC2224,
  TMC2209, TMC2660, and TMC5160 stepper motor drivers. There is also
  support for current control of traditional stepper drivers via
  AD5206, MCP4451, MCP4728, MCP4018, and PWM pins.

* Support for common LCD displays attached directly to the printer. A
  default menu is also available. The contents of the display and menu
  can be fully customized via the config file.

* Constant acceleration and "look-ahead" support. All printer moves
  will gradually accelerate from standstill to cruising speed and then
  decelerate back to a standstill. The incoming stream of G-Code
  movement commands are queued and analyzed - the acceleration between
  movements in a similar direction will be optimized to reduce print
  stalls and improve overall print time.

* Klipper implements a "stepper phase endstop" algorithm that can
  improve the accuracy of typical endstop switches. When properly
  tuned it can improve a print's first layer bed adhesion.

* Support for measuring and recording acceleration using an adxl345
  accelerometer.

* Support for limiting the top speed of short "zigzag" moves to reduce
  printer vibration and noise. See the [kinematics](Kinematics.md)
  document for more information.

* Sample configuration files are available for many common printers.
  Check the [config directory](../config/) for a list.

To get started with Klipper, read the [installation](Installation.md)
guide.

## Step Benchmarks

Below are the results of stepper performance tests. The numbers shown
represent total number of steps per second on the micro-controller.

| Micro-controller                | Fastest step rate | 3 steppers active |
| ------------------------------- | ----------------- | ----------------- |
| 16Mhz AVR                       | 154K              | 102K              |
| 20Mhz AVR                       | 192K              | 127K              |
| Arduino Zero (SAMD21)           | 234K              | 217K              |
| "Blue Pill" (STM32F103)         | 387K              | 360K              |
| Arduino Due (SAM3X8E)           | 438K              | 438K              |
| Duet2 Maestro (SAM4S8C)         | 564K              | 564K              |
| Smoothieboard (LPC1768)         | 574K              | 574K              |
| Smoothieboard (LPC1769)         | 661K              | 661K              |
| Beaglebone PRU                  | 680K              | 680K              |
| Duet2 Wifi/Eth (SAM4E8E)        | 686K              | 686K              |
| Adafruit Metro M4 (SAMD51)      | 761K              | 692K              |
| BigTreeTech SKR Pro (STM32F407) | 922K              | 711K              |

On AVR platforms, the highest achievable step rate is with just one
stepper stepping. On the SAMD21 and STM32F103 the highest step rate is
with two simultaneous steppers stepping. On the SAM3X8E, SAM4S8C,
SAM4E8E, LPC176x, and PRU the highest step rate is with three
simultaneous steppers. On the SAMD51 and STM32F4 the highest step rate
is with four simultaneous steppers. (Further details on the benchmarks
are available in the [Benchmarks document](Benchmarks.md).)
