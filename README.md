# Overview
Simple de-bounce library for STM32Cube HAL.
Tested with NUCLEO-F303K8 and STM32CubeIDE 1.7.0.

# Demo Run

- Connect switch to the left side of nucleo as follows:

```
     [PA9]
     [PA10]
     [NRST]
 +-- [GND]
SW   [PA12]
 +-- [PB0]
     [PB7]
     [PB6]
     [PB1]
     [PF0]
     [PF1]
     [PA8]
 +-- [PA11]
SW   [PB5]
 +-- [PB4]
```

Run program, push PB0 and/or PA11 button. Built-in LED will lit as follows:

- [PA11] only ... 1 time
- [PB0] only ... 2 times
- [PB11] and [PB0] ... 3 times

Using terminal software through USB connecter, can see more information.

# How to use .h/.c

- Define the numbers of button in ```PushButtons.c```
- Call `Button_Init()` before main loop to define PORT and PIN.
- Buttons must be connected ground, and GPIO has to be set to Pull-Up Digital Input.
- Call `uint32_t Button_Loop()` to get the return code. If 0, no key detected.

Return code structure in this sample:
```
bit31 bit30 ... bit2 bit1 bit0
  0     0        0   PB0  PA11
```

If bit1==1, PB0 was pushed.
If bit0==1, PA11 was pushed.

Please notice that the logic is inversed as actual DC level.

- Arrange  ```#define BUTTON_TIME 75 // in msec``` in ```PushButtons.c``` according to switch feeling. This decides de-bounce and release time loughly.

# Limitation

This sample ***can't understand:***
- Repeat
- Rolling:push [A], long time after, push [B]
-  Double Click: may be recognize as 2 times seperately

Only for "On-Trigger."

And HAL_GetTick() overflow is not understand.

# Test environment
- 