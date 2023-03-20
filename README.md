<!-- Please do not change this logo with link -->

[![MCHP](images/microchip.png)](https://www.microchip.com)

# PIC18F25Q71 Curiosity High Pin Count (HPC) Lab Code

The following demo project contains labs designed for Microchip’s Curiosity High Pin Count (HPC) Development board, whih supports Microchip's 28 and 40-pin 8-bit PIC® microcontrollers (MCUs). Each lab includes a brief description of its purpose and offers exercises that demonstrate basic PIC device capabilities, as well as peripherals and registers. These can also be used to test the condition of the board. MPLAB Code Configurator (MCC) was used for developing these examples. MCC is an easy-to-use plugin tool for MPLAB X IDE which can be used to configure the peripherals and generate drivers to use in the application code. All labs are written in C language and are compatible with the latest XC8 compilers.

##### Curiosity HPC Development Board:
![Curiosity High Pin Count (HPC) Development Board](images/hpc-board.PNG)
## Related Documentation

- [MPLAB Code Configurator](https://www.microchip.com/en-us/development-tools-tools-and-software/embedded-software-center/mplab-code-configurator)
- [PIC18-Q71 Family Product Page](https://www.microchip.com/en-us/products/microcontrollers-and-microprocessors/8-bit-mcus/pic-mcus/pic18-q71)

## Software Used

- [MPLAB X IDE 6.0.5](https://www.microchip.com/en-us/development-tools-tools-and-software/mplab-x-ide?utm_source=GitHub&utm_medium=TextLink&utm_campaign=MCU8_MMTCha_MPAE_Examples&utm_content=pic18f46q71-curiosity-hpc-demo-code-github) or newer
- [MPLAB XC8 2.41.0](https://www.microchip.com/en-us/development-tools-tools-and-software/mplab-xc-compilers?utm_source=GitHub&utm_medium=TextLink&utm_campaign=MCU8_MMTCha_MPAE_Examples&utm_content=pic18f46q71-curiosity-hpc-demo-code-github) or newer compiler
- [MPLAB Melody Library 2.2.37](microchip.com/mplab/mplab-code-configurator) or newer
- [Microchip PIC18F-Q Series Device Support (1.16.368)](packs.download.microchip.com/) or newer 

## Hardware Used

- [PIC18F25Q71 Microcontroller](https://www.microchip.com/wwwproducts/en/PIC18F25Q71)
- [Curiosity High Pin Count (HPC) Development Board](https://www.microchip.com/developmenttools/ProductDetails/PartNO/DM164136)  

## Labs
The labs in this project are presented in the same order as they appear on the programmed labs. Progress through each of the labs by pressing the S1 button on the board.

- Lab 1: Hello World (Turn On an LED) <br />
- Lab 2: Blink <br />
- Lab 3: Rotate (Moving the Light Across LEDs) <br />
- Lab 4: Analog-to-Digital Conversion (ADC) <br />
- Lab 5: Variable Speed Rotate <br />
- Lab 6: Pulse-Width Modulation (PWM) <br />
- Lab 7: Timers <br />
- Lab 8: Interrupts <br />
- Lab 9: Wake-up from Sleep Using Watchdog Timer (WDT) <br />
- Lab 10: EEPROM <br />

## Inputs and Display
- Push Button Switch – One on-board push button switch (S1) is utilized. S1 is connected to the PIC MCU RB4 pin and is used to switch to the next lab. <br />
- Potentiometer – A 10kΩ potentiometer connected to the RA0 pin is used in labs requiring analog inputs <br />
- LEDs - The Curiosity HPC Development Board has four LEDs (D2 through D5) that are connected to I/O ports RA4 through RA7, respectively. These LEDs are used to display the output of the different labs. <br/>

### <u>Lab 1: Hello World</u>
#### Introduction
This lab shows how to turn on an LED.

#### Hardware Effects
LED D2 will light up and stays that way.

#### Summary
The LEDs are connected to the input-output (I/O) pins. First, the I/O pin must be configured to be an output. In this case, when one of these pins is driven high (LED_D2 = 1), the LED will turn on. These two logic levels are derived from the power pins of the PIC MCU. Since the PIC's power pin (V<sub>DD</sub>) is connected to 5V or 3.3V and the source (V<sub>SS</sub>) to ground (0V), a logic level of `‘1’` is equivalent to 5V or 3.3V, and a logic level of `‘0’` is 0V.


### <u>Lab 2: Blink</u>
#### Introduction
This lab shows how to blink an LED.

#### Hardware Effects
LED D2 blinks at a rate of approximately 1.5s.

#### Summary
One way to create a delay is to spend time decrementing a value. In assembly, the timing can be accurately programmed since the user will have direct control on how the code is executed. In C, the compiler takes the C and compiles it into assembly before creating the file to program to the actual PIC MCU (HEX file). Because of this, it is hard to predict exactly how many instructions it takes for a line of C to execute. For a more accurate timing in C, this lab uses the Timer1 (TMR1) MCU module to produce the desired delay. Timer1 is discussed in Lab 7: Timers.


### <u>Lab 3: Rotate</u>
#### Introduction
This lab is built on Lab 1 and 2, which showed how to light up an LED and then make it blink using loops. This lab incorporates four on-board LEDs (D2, D3, D4 and D5) and the program lights up each LED in turn.

#### Hardware Effects
LEDs D2, D3, D4 and D5 light up in turn every 500 milliseconds. Once D5 is lit, D2 lights up and the pattern repeats.

#### Summary
In C, we use Binary Left Shift and Right Shift Operators (<< and >>, respectively) to move bits around in the registers. To create a rotating pattern on the LEDs, a temporary variable is declared and initialized with a value of `'1'`. Every 500 ms, the value of this variable is left-shifted by one. If the value exceeds `` `b1000``, the variable is reset to a value of `'1'`. The value is then written to the uppoer MSbs of LATA by right shifting the value again by four. The upper four MSbs of LATA control the LEDs on the HPC board.


### <u>Lab 4: Analog-to-Digital Conversion (ADC)</u>
#### Introduction
This lab shows how to configure the ADC, run a conversion, read the analog voltage controlled by the on-board potentiometer, print the conversion result on the Universal Asynchronous Receiver and Transmitter (UART) and display the high-order four bits on the display.

#### Hardware Effects
The four MSbs of the ADC result are reflected onto each of the four LEDs respectively. Rotate the potentiometer to change the display. <br />
The ADC value will be printed on UART TX pin which is connected to pin RC6 through PPS. Connect this pin to the Virtual COM port's TX pin using a jumper wire to use the onboard serial to USB feature.

#### Summary
The PIC18-Q71 family of devices has an on-board 12-bit differential Analog-to-Digital Converter with Computation (ADCC) and Context Switching. The converter can be referenced to the device’s V<sub>DD</sub> or an external voltage reference. This lab references it to VDD. The result from the ADC is represented by a ratio of the voltage to the reference.


### <u>Lab 5: Variable Speed Rotate</u>
#### Introduction
This lab combines all ones to produce a variable speed rotating LED display that is proportional to the ADC value. The ADC value and LED rotate speed are inversely proportional to each other.
#### Hardware Effects
Rotate the on-board potentiometer to change the speed of the LEDs shift. <br />
The ADC value will be printed on UART TX pin which is connected to pin RC6 through PPS. Connect this pin to the Virtual COM port's TX pin using a jumper wire to use the onboard serial to USB feature.
#### Summary
A crucial step in this lab is to check if the ADC value is `'0'`. If it does not perform the zero check, and the ADC result is zero, the LEDs will rotate at an incorrect speed. This is an effect of the delay value underflowing from 0 to 255.

###### Program Flow
![Lab 5 Program Flow](images/lab5-program-flow.PNG)

### <u>Lab 6: Pulse-Width Modulation (PWM)</u>
#### Introduction
In this lab, the PIC MCU generates a PWM signal that lights an LED with the potentiometer, thereby controlling the brightness.
#### Hardware Effects
Rotate the potentiometer to adjust the brightness of LED D5. <br />
The ADC value will be printed on UART TX pin which is connected to pin RC6 through PPS. Connect RC6 to the TX pin of the Virtual COM port through a jumper wire to use the on-board USB-to-UART serial converter.
#### Summary
PWM is a scheme that provides power to a load by quickly switching between fully ON and fully OFF states. The PWM signal resembles a square wave, where the high portion of the signal is considered the ON state and the low portion of the signal is considered the OFF state. The high portion, also known as the pulse width, can vary in time and is defined in steps. A longer, high ON time will illuminate the LED brighter. The frequency or period of the PWM does not change. The PWM period is defined as the duration of one cycle or the total amount of ON and OFF time combined. Another important term to take note of is the PWM duty cycle, which is the ratio of the pulse width to the period and is often expressed in percentage. A lower duty cycle corresponds to less power applied and a higher duty cycle corresponds to more power applied.


### <u>Lab 7: Timers</u>
#### Introduction
This lab produces the same output as Lab 3: ROTATE. The only difference is that this version uses Timer1 to provide the delay routine.
#### Hardware Effects
LEDs rotate from right to left, similar to Lab 3.
#### Summary
Timer1 is a counter module that uses two 8-bit paired registers (TMR1H:TMR1L) to implement a 16-bit timer/counter in the processor. It can be used to count instruction cycles or external events that occur at or below the instruction cycle rate. <br />
This lab configures Timer1 to count instruction cycles and to set a flag when it rolls over. This frees up the processor to do meaningful work rather than wasting instruction cycles in a timing loop. Using a counter provides a convenient method of measuring time or delay loops, as it allows the processor to work on other tasks rather than counting instruction cycles.


### <u>Lab 8: Interrupts</u>
#### Introduction
This lab discusses all about interrupts – their purpose, capabilities and how to set them up. Most interrupts are sourced from MCU peripheral modules. Some I/O pins can also be configured to generate interrupts whenever a change in state is detected. Interrupts usually signal events that require servicing by the software’s Interrupt Service Routine (ISR). Once an interrupt occurs, the program counter immediately jumps to the ISR and once the Interrupt flag is cleared, resumes what it was doing before. It is a rather more efficient way of watching out for events than continuously polling a bit or register.
#### Hardware Effects
LEDs D5, D4, D3 and D2 rotate from left to right at a constant rate of 499.712 ms.
#### Summary
This lab demonstrates the advantage of using interrupts over polling. An interrupt is generated whenever the Timer0 (TMR0) register reaches 0xFF and goes back to Reset value. This indicates that 500 ms have passed and it is time to rotate the light. This interrupt is serviced by the TMR0_ISR() function. Note that this is the same as for Lab 7: Timer1, but this time we are not continuously watching the TMR1IF flag.


### <u>Lab 9: Wake-up from Sleep Using Watchdog Timer (WDT)</u> 
#### Introduction
This lab introduces Sleep mode. The `SLEEP()` function is used to put the device into a low-power Standby mode.
#### Hardware Effects
Once this lab is in Running state, the WDT will start counting. While in Sleep mode, LEDs D2/D4 and LEDs D3/D5 are turned ON and OFF respectively. Pressing the switch will not move to the next lab since the PIC is in Sleep mode. After the WDT has reached its period, which is approximately 4s for this lab, the PIC exits sleep mode and the four LEDs, D2 through D5, are toggled.
#### Summary
The Power-Down mode is entered by executing the `SLEEP` instruction. Upon entering Sleep mode, there are different conditions that can exist such as:

- WDT will be cleared but keeps running, if enabled for operation during Sleep
- PD bit of the STATUS register is cleared
- TO bit of the STATUS register is set
- CPU clock is disabled

Different PICs have different conditions once they enter Sleep mode so it is recommended that the reader refer to the datasheet to know more of these conditions. <br />
The Watchdog Timer (WDT) is a system timer that generates a Reset if the firmware does not issue a CLRWDT instruction within the time-out period. WDT is typically used to recover the system from unexpected events. When the device enters Sleep, the WDT is cleared. If the WDT is enabled during Sleep, the WDT resumes counting. When the device exits Sleep, the WDT is cleared again. When a WDT time-out occurs while the device is in Sleep, no Reset is generated.


### <u>Lab 10: EEPROM</u>
#### Introduction
This lab provides code for writing and reading a single byte onto the on-board EEPROM. EEPROM is nonvolatile memory, meaning that it does not lose its value when power is shut off. This is unlike RAM, which will lose its value when no power is applied. The EEPROM is useful for storing variables that must still be present during no power. A good use case is to store calibration data for the user application and have it loaded on every boot-up.
#### Hardware Effects
The top four MSbs of the ADC are written to EEPROM. These are read afterwards and displayed on the LEDs. Rotating the potentiometer changes the value of the ADC to be written to and read from EEPROM.
#### Summary
This lab is similar to Lab 4: ADC, but instead of directly moving the ADC result directly onto the LEDs, it performs a simple “write” and “read” on the EEPROM. As shown on figure below, the top four MSbs of the ADC result are first written to EEPROM, and retrieved later from the same address before moving onto the LEDs.

###### Program Flow
![Lab 5 Program Flow](images/lab10-program-flow.PNG)
