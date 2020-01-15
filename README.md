# PureData Bela Wekinator WiFi Servo L293D Motor Robot

- Bela Board Physical Interactive Kinetic Robot Project
- Materials: Bela Board / Wekinator / WiFi Dongle / Speaker / DC Motors / Servos
- Size: 25(H)*25(W)*25(D)cm
- Language: Pure Data
- ***More details of the project (Demo Video):*** *https://wantinghsieh.com/moving-instrumentbot/*

![image](https://wantinghsieh.com/wp-content/uploads/Moving-Instrumentbot_3_web.jpg)

---
[osc-app]: https://play.google.com/store/apps/details?id=com.hollyhook.oscHook&hl=en_US
[wekinator]: http://www.wekinator.org/
[bela]: https://bela.io/belaDiagram/
[motor-pwm]: https://howtomechatronics.com/tutorials/arduino/arduino-dc-motor-control-tutorial-l298n-pwm-h-bridge/
[servo-pwm]: https://learn.sparkfun.com/tutorials/pulse-width-modulation/all
[bela-wifi]: https://github.com/BelaPlatform/Bela/wiki/Connecting-Bela-to-wifi

## Idea
The idea is that the instrumentbot can go around by itself with its wheels just like a scary boy’s head 
attached to a spider body in the “Toy Story” animation, creating an unusual artificial monster. 
It then plays the electrical sound while it detects the changing axis of the audience’s moving smartphone 
by receiving OSC data coming from any OSC application of the smartphone via its WiFi dongle, 
which makes it look like as if it was a live creature as well as making this moving instrumentbot be controlled remotely. 
Wekinator, which is a useful machine learning toolkit, is then used here to accurately output 
which axis of the moving smartphone is and then sends that axis class to Pure Data to control a speaker, 
two servos and two DC motors on the Bela board.

## What Does It Do?
This instrumentbot is controlled remotely by a smartphone’s accelerator within the same network, which means the audience can control the robot by simply turning their smartphone’s face up, down, left or right. Smartphones then identify their orientation through the use of an accelerator, a small device made up of axis-based motion sensing.

- ### The project combines these topics:

  - Setting up the WiFi function on Linux operating system, which is run by Bela board, by editing its network interface after plugging the USB WiFi dongle into the Bela board
  - Training the axis data received from the smartphone’s OSC application by Wekinator
  - Controlling DC Motors with the L293D H-Bridge IC by generating PWM with a specific duty cycle by Pure Data to its ENABLE pins and digital high or low value to its INPUT pins
  The instrumentbot is curious about everything around it and wants to start its adventurous trip. However, it doesn’t know the correct direction to adventure and thus rely heavily on audience’s instructions to help it find out a suitable way to move.

There are there Pd patches fulfilling the entire interaction: getOSC.pd and sendOSCToWek.pd run locally whereas _main.pd runs on the Bela board.

- ### The code works like this:

  - [OSCHook App][osc-app] sends the axis data of the smartphone’s accelerator in OSC format to getOSC.pd
  - getOSC.pd unpacks the axis data and only sends the number value to sendOSCToWek.pd
  - sendOSCToWek.pd then sends pure number data receiving from getOSC.pd to Wekinator and listens to the same port(6448) as Wekinator’s
  - [Wekinator][wekinator] outputs the trained data in the format of class(1 to 5) to _main.pd run by Bela board installed on the instrumentbot
  - The robot moves when it receives commands from the _main.pd which value of the class data is received from Wekinator:
    - The instrumentbot doesn’t move but starts looking around while the phone is placed flatwise
    - The robot moves forward and stops looking around while the phone is placed upside down
    - The robot moves backward while the phone is placed vertically against a table
    - The robot turns right while the phone is placed rightward
    - The robot turns left while the phone is placed leftward
 
## Bela Board Pin Connection ([Bela Cape Diagram][bela])

![image](https://wantinghsieh.com/wp-content/uploads/Moving-Instrumentbot_Bela_1_web.jpg)

## Theory of Motor Control
* [Speed Control of DC Motor Using Pulse-Width Modulation][motor-pwm]
* [Angle Control of Servo Using Pulse-Width Modulation][servo-pwm]

## Implementation: Pd OSC Communication and Machine Learning Toolkit ([Wekinator][wekinator])

### 0. [Connecting Bela to WiFi.][bela-wifi]
  Edit Bela’s network interface by following the command lines from the above tutorial link after plugging the USB WiFi dongle into the Bela board.

### 1. Initialize Bela pin mode.
  ![image](https://wantinghsieh.com/wp-content/uploads/Moving-Instrumentbot_Pd_1_web.png)
  
### 2. Generate PWM and output it to control the angle of two servos.
  ![image](https://wantinghsieh.com/wp-content/uploads/Moving-Instrumentbot_Pd_7_web.png)
  
### 3. Generate PWM and output it to L293D’s ENABLE pins to control the speed of DC motors.
  ![image](https://wantinghsieh.com/wp-content/uploads/Moving-Instrumentbot_Pd_6_web.png)
  
### 4. Send accelerator data in OSC format from smartphone’s [OSC App][osc-app] to PC which runs getOSC Pd patch.
  ![image](https://wantinghsieh.com/wp-content/uploads/Moving-Instrumentbot_Pd_2_web.png)

### 5. PC receives axes’ data from the smartphone and reorganizes it into pure floating-point values.
  ![image](https://wantinghsieh.com/wp-content/uploads/Moving-Instrumentbot_Pd_getOSC_web.png)
