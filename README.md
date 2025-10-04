# Serial-Controlled 28BYJ-48 Stepper Motor Project

This Arduino project demonstrates controlling a 28BYJ-48 stepper motor using the ULN2003 driver board. The motor can be rotated by entering commands in the Arduino Serial Monitor, such as `rotate 90` or `rotate -180`.

---

## Features

- Control the 28BYJ-48 stepper motor via Serial commands.
- Rotate the motor in clockwise (CW) and counter-clockwise (CCW) directions.
- Enter rotation in degrees (positive = CW, negative = CCW).
- Motor position is calculated using steps per revolution (2048).

---

## Hardware Required

- Arduino UNO
- 28BYJ-48 Stepper Motor
- ULN2003 Driver Board
- Jumper wires
- External 5V power supply (recommended for stable operation)

---

## Circuit Connections

| ULN2003 Pin | Arduino Pin |
| ----------- | ----------- |
| IN1         | 8           |
| IN2         | 9           |
| IN3         | 10          |
| IN4         | 11          |

- Connect the 28BYJ-48 motor to the ULN2003 driver board.
- Connect VCC on ULN2003 to 5V supply.
- Connect GND on ULN2003 to Arduino GND.

---

## Arduino Code

```cpp
#include <Stepper.h>

const int stepsPerRevolution = 2048; // for 28BYJ-48 (half-step mode)
const int in1Pin = 8;
const int in2Pin = 9;
const int in3Pin = 10;
const int in4Pin = 11;

Stepper myStepper(stepsPerRevolution, in1Pin, in3Pin, in2Pin, in4Pin);
float stepsPerDegree = stepsPerRevolution / 360.0;

void setup() {
  Serial.begin(9600);
  myStepper.setSpeed(10); // RPM
  Serial.println("Enter command like: rotate 90 (positive = CW, negative = CCW)");
}

void loop() {
  if (Serial.available() > 0) {
    String input = Serial.readStringUntil('\n');
    input.trim();

    if (input.startsWith("rotate")) {
      int angle = input.substring(6).toInt();
      long stepsToMove = angle * stepsPerDegree;
      myStepper.step(stepsToMove);

      Serial.print("Rotated ");
      Serial.print(angle);
      Serial.println(" degrees");
    }
  }
}
```

---

## Usage

1. Upload the code to the Arduino.
2. Open the Serial Monitor (baud rate: 9600).
3. Enter a command, for example:
   - `rotate 90` to rotate the motor 90° clockwise.
   - `rotate -180` to rotate the motor 180° counter-clockwise.
   - `rotate 360` to rotate the motor one full revolution.

---

## Essential Tips

- Use an external 5V power supply for the motor to prevent Arduino resets and ensure sufficient torque.
- Ensure a common ground connection between Arduino and the ULN2003 board.
- Keep the motor speed between 5–15 RPM to prevent missed steps.
- Introduce delays or movement planning when controlling multiple motors.

---

## Future Improvements

- Support commands with the word "degrees" included.
- Implement inverse kinematics for robotic arm applications.
- Control multiple stepper motors simultaneously.

---

## Author

[Mchiir](./https://github.com/Mchiir)

---
