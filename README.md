Aseba firmware for the e-puck robot (http://www.e-puck.org)

Building
========

Pre-requisite
-------------

You need the following to compile this firmware:
 - E-Puck library (http://gna.org/svn/?group=e-puck)
 - Aseba (https://github.com/aseba-community/aseba)
 - A compiler for dsPIC, pic30-elf-gcc or MPLAB X (http://www.microchip.com/mplabx/)

You have to change the size of the UART 1 e-puck reception buffer to hold the largest possible packet (probably bytecode + header).
Otherwise if you set a new bytecode while busy (for instance while sending description), you might end up in a dead-lock.
To do so, edit `e_uart1_rx_char.s`, and change `U1RXBuf` to something like `1024` and update masks accordingly:

	e_uart1_rx_char.s:28	_U1RXBuf: .space 1024
	e_uart1_rx_char.s:57	and		#0x3ff, w0
	e_uart1_rx_char.s:86	and		#0x3ff, w2

Now, there are two ways to build:

Use pic30-elf-gcc
-----------------

If you use pic30-elf-gcc, use the provided Makefile.pic30. They might be outdated and require adaptation, we welcome patches. In that case, you have to define the environment variable EPUCKLIBROOT and ASEBAROOT before running make, and compile a library called asebaembedded composed of vm/vm.c vm/natives.c and transport/buffer/vm-buffer.c

Use MPLAB X
-----------

If you use MPLAB X IDE, create a new project, set to CPU 30F6014A.

Then configure the following settings (examples given for xc16 compiler):
 - 64 bytes of heap (Project settings -> xc16-ld -> Heap size: 64)
 - large memory model (Project settings -> xc16-gcc -> Memory model -> Data Model: Large)

Then add the files:

from Aseba:
 - `vm/vm.c`
 - `vm/natives.c`
 - `transport/buffer/vm-buffer.c`
 
from e-puck library:
 - `acc_gyro/e_lsm330.c`
 - `a_d/advance_ad_scan/e_acc.c`
 - `a_d/advance_ad_scan/e_micro.c`
 - `a_d/e_ad_conv.c`
 - `a_d/advance_ad_scan/e_ad_conv.c`
 - `a_d/advance_ad_scan/e_prox.c`
 - `camera/fast_2_timers/e_common.c`
 - `camera/fast_2_timers/e_calc_po3030k.c`
 - `camera/fast_2_timers/e_po3030k_registers.c`
 - `camera/fast_2_timers/e_calc_po6030k.c`
 - `camera/fast_2_timers/e_po6030k_registers.c`
 - `camera/fast_2_timers/e_calc_po8030d.c`
 - `camera/fast_2_timers/e_po8030d_registers.c` 
 - `camera/fast_2_timers/e_interrupt.s`
 - `camera/fast_2_timers/e_timers.c`
 - `I2C/e_I2C_master_module.c`
 - `I2C/e_I2C_protocol.c`
 - `motor_led/e_init_port.c`
 - `motor_led/advance_one_timer/e_agenda.c`
 - `motor_led/advance_one_timer/e_led.c`
 - `motor_led/advance_one_timer/e_motors.c`
 - `motor_led/advance_one_timer/e_remote_control.c`
 - `uart/e_init_uart1.s`
 - `uart/e_uart1_rx_char.s`
 - `uart/e_uart1_tx_char.s`
 - `utility/utility.c`
 
then finally:
 - `epuckaseba.c`

 
