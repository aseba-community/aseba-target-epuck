Aseba firmware for the e-puck robot (http://www.e-puck.org)

Building
========

Pre-requisite
-------------

You need the following to compile this firmware:
 - E-Puck library (http://gna.org/svn/?group=e-puck)
 - Aseba (https://github.com/aseba-community/aseba)
 - A compiler for dsPIC, pic30-elf-gcc or MPLAB X (http://www.microchip.com/mplabx/)

You have to change the size of the UART 1 e-puck reception buffer to hold the largest possible packet (probably bytecode + header). Otherwise if you set a new bytecode while busy (for instance while sending description), you might end up in a dead-lock. To do so, edit `e_uart1_rx_char.s`, and change `U1RXBuf` to something like `530`:

	_U1RXBuf: .space 530

Now, there are two ways to build:

Use pic30-elf-gcc
-----------------

If you use pic30-elf-gcc, use the provided Makefile.pic30. They might be outdated and require adaptation, we welcome patches. In that case, you have to define the environment variable EPUCKLIBROOT and ASEBAROOT before running make, and compile a library called asebaembedded composed of vm/vm.c vm/natives.c and transport/buffer/vm-buffer.c

Use MPLAB X
-----------

If you use MPLAB X IDE, create a new project, set to CPU 30F6014A.

Then configure the following settings (examples given for xc16 compiler):
 - 512 bytes of heap (Project settings -> xc16-ld -> Heap size: 512)
 - large memory model (Project settings -> xc16-gcc -> Memory model -> Data Model: Large)

Then add the files:

from Aseba:
 - vm/vm.c
 - vm/natives.c
 - transport/buffer/vm-buffer.c
from e-puck library:
 - a_d/advance_ad_scan/e_acc.c
 - a_d/e_ad_conv.c
 - a_d/advance_ad_scan/e_ad_conv.c
 - a_d/advance_ad_scan/e_prox.c
 - camera/fast_2_timers/e_common.c
 - camera/fast_2_timers/e_calc_po3030k.c
 - camera/fast_2_timers/e_po3030k_registers.c
 - camera/fast_2_timers/e_calc_po6030k.c
 - camera/fast_2_timers/e_po6030k_registers.c
 - camera/fast_2_timers/e_interrupt.s
 - camera/fast_2_timers/e_timers.c
 - I2C/e_I2C_master_module.c
 - I2C/e_I2C_protocol.c
 - motor_led/e_init_port.c
 - motor_led/advance_one_timer/e_agenda.c
 - motor_led/advance_one_timer/e_led.c
 - motor_led/advance_one_timer/e_motors.c
 - uart/e_init_uart1.s
 - uart/e_uart1_rx_char.s
 - uart/e_uart1_tx_char.s
then finally:
 - epuckaseba.c

 
