**Objective:** To make the receiving board send an acknowledgement to the transmitting board.



**Process:**



1\) STM32F407G-DISC1 has 3 pre-defined messages namely, msg1, msg2, and msg3.



2\) Every time the user button on the board is pressed, a new message is transmitted via UART2 to Nucleo H563ZI.



3\) Nucleo H563ZI receives the message via UART2 processes it and sends and acknowledgement message, "Received by Nucleo: \[Message]", back to STM32F407G-DISC1 via UART2.



4\) STM32F407G-DISC1 receives the message, processes it and creates a message, "Received by F407G - Received: \[Message]'.



5\) It then send the message via UART3 to PC via TTL to print on serial monitor.



6\) The process will repeat with a new message every time the user button on STM32F407G-DISC1 is pressed.



**\*The output on the serial monitor after every button press will be like: Received by F407G - Received by Nucleo: \[MESSAGE]**



**Peripheral Pins:**



*STM32F407G-DISC1:* UART2 - PA2(TX) and PA3(RX)

 		  UART3 - PA8(TX) and PA9(RX)



*NUCLEO-H563ZI:* UART2 - PD5(TX) and PA3(RX)



**Connections:**



*TTL:* 1) GND -> STM32F407G-DISC1 GND

     2) RXD -> STM32F407G-DISC1 PD8



*STM32F407G-DISC1:* 1) GND -> TTL GND

 		  2) PD8 -> RXD TTL

 		  3) PA2 -> PA3 NUCLEO-H563ZI

 		  4) PA3 -> PD5 NUCLEO-H563ZI



*NUCLEO-H563ZI:* 1) PA3 -> PA2 STM32F407G-DISC1

 	       2) PD5 -> PA3 STM32F407G-DISC1



**Challenges:**



1. (SOLVED: PULLED UP THE RX PIN (PA3) OF UART2 OF NUCLEO-H563ZI)

At the first button press the Nucleo-H563ZI seems to be receiving a null ('\\0') character before the receiving the actual message. Due to which, the acknowledgement message it sends looks like "Received: ". After the second button press the program behaves as expected. The Nucleo-H563ZI maybe picking some noise which causes the registering of the null character and subsequent calling of the 'RxCpltCallback' function.

