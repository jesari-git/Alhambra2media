------------------ LaRVaVideo microcontroller example design ------------------

                               J. Arias (2025)

-------------------------------------------------------------------------------

How to test it on an Alhambra-II board with the multimedia shield attached:

1 - Execute the "burncombi" script. This writes the flash with:
    - The FPGA configuration bitstream
    - The reset code for the RiscV core, that includes a debugger
    - A demo program for testing
  
3 - Open a serial terminal 

	iceload -d /dev/ttyUSB1 -t
	
	or use your preferred serial terminal app (i.e. GTKTerm, CoolTerm...) with
	these settings :
      Port: "/dev/ttyUSB1" (linux) or the proper COM port on Windows
      Baud rate: 115200
      Parity: None
      Flow Control: None
      CR LF: Auto
    
4 - If all goes well this message is displayed:

	LaRVa ][  debugger

	J.Arias (2025)

	And 2 seconds later the debugger screen is displayed:

	   X0       X1       X2       X3       X4       X5       X6       X7    
	--zero-- 20031490 00003BD4 00000000 00000000 00000000 00000000 00000000 
	   X8       X9       X10      X11      X12      X13      X14      X15   
	00066000 00000010 00000000 FFFE3BF4 FFFE3BE0 00000000 E0000000 000000C0 
	   brk    IRQEN
	--------    C0

	200314B4: 00066437  LUI     X8, 00066
	   PC
	200314B8: 3B440413  ADDI    X8, X8, 948
	200314BC: E00006B7  LUI     X13, E0000
	200314C0: 0206C783  LBU     X15, 32 (X13)
	200314C4: 00040513  ADDI    X10, X8, 0
	200314C8: 0FF7F793  ANDI    X15, X15, 255
	200314CC: 00179713  SLLI    X14, X15, 1
	200314D0: 0077D793  SRLI    X15, X15, 7
	200314D4: 00F767B3  OR      X15, X14, X15
	200314D8: 0FF7F793  ANDI    X15, X15, 255
	200314DC: 02F68023  SB      X15, 32 (X13)
	200314E0: B81FE0EF  JAL     X1, 20030060
	200314E4: FD9FF06F  J       200314BC
	200314E8: 20031134  ?     
	200314EC: 20031284  ?     

	hcsnrRbgxdmwl>h

	Pressing the "h" key:
	Commands:
	 h:     Help
	 c:     Continue
	 s:     execute Single instruction
	 n:     execute until Next instruction
	 r:     execute until subroutine Return (any)
	 R:     execute until Return at higher stack level
	 b:     set Breakpoint
	 B:     set Data Breakpoint
	 g:     Goto (change PC)
	 x:     change X register
	 d:     Dissasemble
	 m:     Memory dump
	 w:     memory Write
	 l:     Load code to debug
	 a:     Alternate register names
	 ctrl-c Pauses execution

	hcsnrRbgxdmwl>

	Also, a VGA video signal is generated. A monochrome white & blue image 
	displays all the internal RAM bits as pixels.

TESTING CODES

1 - RUNNING CODE on RAM:
	Pressing "l" on the serial terminal allows the loading of RAM images from
	the serial port. These images had to include a simple header with:

        .word   0x4CFFFFFF		; Sync data
        .word   start			; where to load
        .word   _edata-start	; number of bytes
        .word   start			; entry address
        
    Words are little-endian (LSB sent first) and the header isn't loaded into
    memory.
    
    The binary image file has to be sent over the serial port. (No flow control
    is required) After the loading the debugger pops again with the PC pointing
    at the first instruction of the loaded code. Then press "c" (continue)
    
    (For a funny demo try to load the "pk2" file ;)
	
2 - RUNNING THE DEMO

Enter the debugger and type 'g' (goto) followed by the hex address:

	hcsnrRbgxdmwl> g  20040000
	
This makes the PC to point to the application code in the flash. Then, type 'c'
(continue) and the application runs:

Select:
1 - WAV player from SD card

2 - Oscope / FFT

3 - PSRAM test



