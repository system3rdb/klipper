# Features

Klipper 에는 다음과 같이 몇가지 매력적인 기능이 있습니다:

* 고정밀 스테퍼 동작. Klipper 는 3D 프린터의 움직임을 계산할 때 라즈베리파이 같은 저렴한 애플리케이션 프로세서를 활용합니다. 애플리케이션 프로세서는 각 스테퍼 모터를 언제 움직여야 하는지 결정하고 해당 이벤트를 압축하여 마이크로 컨트롤러에 전송한 다음 마이크로 컨트롤러가 요청된 시간에 각 이벤트를 실행합니다. 각 스테퍼 이벤트는 25 마이크로초 이상의 정밀도로 스케줄링 됩니다. 소프트웨어는 운동학적 추정(예: Bresenham 알고리즘)을 사용하지 않습니다. 대신 가속 물리학 및 기계 운동학 물리학을 기반으로 정확한 단계 시간을 계산합니다. 더 정밀한 스테퍼 움직임은 더 조용하고 안정적인 프린터 작동으로 이어집니다.

* Best in class performance. Klipper is able to achieve high stepping
  rates on both new and old micro-controllers. Even old 8bit
  micro-controllers can obtain rates over 175K steps per second. On
  more recent micro-controllers, rates over 500K steps per second are
  possible. Higher stepper rates enable higher print velocities. The
  stepper event timing remains precise even at high speeds which
  improves overall stability.

* Klipper supports printers with multiple micro-controllers. For
  example, one micro-controller could be used to control an extruder,
  while another controls the printer's heaters, while a third controls
  the rest of the printer. The Klipper host software implements clock
  synchronization to account for clock drift between
  micro-controllers. No special code is needed to enable multiple
  micro-controllers - it just requires a few extra lines in the config
  file.

* Configuration via simple config file. There's no need to reflash the
  micro-controller to change a setting. All of Klipper's configuration
  is stored in a standard config file which can be easily edited. This
  makes it easier to setup and maintain the hardware.

* Klipper supports "Smooth Pressure Advance" - a mechanism to account
  for the effects of pressure within an extruder. This reduces
  extruder "ooze" and improves the quality of print corners. Klipper's
  implementation does not introduce instantaneous extruder speed
  changes, which improves overall stability and robustness.

* Klipper supports "Input Shaping" to reduce the impact of vibrations
  on print quality. This can reduce or eliminate "ringing" (also known
  as "ghosting", "echoing", or "rippling") in prints. It may also
  allow one to obtain faster printing speeds while still maintaining
  high print quality.

* Klipper uses an "iterative solver" to calculate precise step times
  from simple kinematic equations. This makes porting Klipper to new
  types of robots easier and it keeps timing precise even with complex
  kinematics (no "line segmentation" is needed).

* Portable code. Klipper works on ARM, AVR, and PRU based
  micro-controllers. Existing "reprap" style printers can run Klipper
  without hardware modification - just add a Raspberry Pi. Klipper's
  internal code layout makes it easier to support other
  micro-controller architectures as well.

* Simpler code. Klipper uses a very high level language (Python) for
  most code. The kinematics algorithms, the G-code parsing, the
  heating and thermistor algorithms, etc. are all written in Python.
  This makes it easier to develop new functionality.

* Custom programmable macros. New G-Code commands can be defined in
  the printer config file (no code changes are necessary). Those
  commands are programmable - allowing them to produce different
  actions depending on the state of the printer.

* Builtin API server. In addition to the standard G-Code interface,
  Klipper supports a rich JSON based application interface. This
  enables programmers to build external applications with detailed
  control of the printer.

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
