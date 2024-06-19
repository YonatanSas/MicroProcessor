
; ------------------------------------------------------------ About the program ------------------------------------------------------------ 
; Description:
; The program simulates A "model" of a 4-bit microprocessor that includes the following units:
; ALU, I/O - LCD display, button matrix, and memory.
; The var C is for the OP CODE, and the ALU unit identifies the operation and executes the command.
; The commands are:
; 1) 000 - "ERROR" (There is no operation for this op code)
; 2) 001 - "A-B" (Sub)
; 3) 010 - "A*B" (Multiply)
; 4) 011 - "A/B" (Divide)
; 5) 100 - "A^B" (Power)
; 6) 101 - Num of "1" in B
; 7) 110 - Num of "0" in A
; 8) 111 - "ERROR" (There is no operation for this op code)

; ------------------------------------------------------------ About the program ------------------------------------------------------------ 


LIST 	P=PIC16F877
; This line essentially informs the assembler that we are using a microprocessor of this type
; so that it can adapt the instructions and resources to the characteristics of the microprocessor.


include	P16f877.inc
; Imports a definitions file and assembler for a microprocessor of this type.
; This file contains a description of all the registers, symbols, and functions for the microprocessor
; so that the assembler will know how to work with this specific hardware.

 __CONFIG _CP_OFF & _WDT_OFF & _BODEN_OFF & _PWRTE_OFF & _HS_OSC & _WRT_ENABLE_ON & _LVP_OFF & _DEBUG_OFF & _CPD_OFF
; Defines various configuration parameters of the microprocessor.


		org 0x00 ; That line specifies the starting address of the code in memory.
reset:
		goto start 
		org 0x04




;************************************************************ Start ************************************************************
; Bank 0 = 00
; Bank 1 = 01
; Bank 2 = 10
; Bank 3 = 11

start	bcf		STATUS, RP0     ; Select bank 0
		bcf		STATUS, RP1
		; Here we reset the RP bits in the Status register and since they both 0, it means we are in bank 0
		; We are 'going' to bank 0 because we want to "access" the following ports/registers

		clrf	PORTD ; Clear portD
		clrf	PORTE ; Clear portE

		; Here we turn on the RP0 bit of status and this causes us to reach bank number 1
		; because we want to "access" the ADCON1 we need to reach bank 1
		bsf		STATUS, RP0   	; Select bank 1
		movlw	0x06
		movwf	ADCON1

        movlw   0x0F		    ; 4 Low bits of PORTB are input,4 High bits output
        movwf   TRISB

        bcf     OPTION_REG,0x07 ; RBPU is ON -->Pull UP on PORTB is enabled

		clrf	TRISE	     	; PORTE output - Responsible for the display screen
		clrf	TRISD	    	; PORTD output - Responsible for the display screen

		bcf		STATUS, RP0  	; Select bank 0
		; Returning to bank 0 because we are using registers located there

		;Initialize registers

		clrf 	0x33   	    ; counter for 3 enter	

		clrf	0x69        ; temp register1
		clrf 	0x43 		; temp register2
		clrf    0x44 		; temp register3
		clrf    0x60		; result register

		clrf 	0x30   		; holds A
		clrf 	0x40   		; holds B
		clrf 	0x50   		; holds C

		call init ; Jumping to display screen initialization
		goto  user_input ; Entering the input by the user


;------------------------------------------------------ Buttons Matrix Loop ---------------------------------------------------------------------
; We disabled all the buttons except "0"/"1"
; The button matrix works as follows:
; There are 4 upper bits and 4 lower bits
; Each of the 4 upper bits in turn sends a 0 while the others are 1
; They are connected to switches where at the switch location there is actually a button at the matrix buttons
; And if the switch is closed it routes the 0 that arrives to the relevant place.
; And essentially what happens here is a loop that runs quickly and constantly scans, and the moment any key is pressed on the button matrix
; Then a specific switch is closed that routes the bit to its place and then the system knows which button was pressed

;------------------------------------------------------------------------------------------------------------
wkb:
		; This delay is so that we can press the button properly and there won't be issues of small mechanical vibrations causing multiple presses
		call 			delay		 
	    bcf             PORTB,0x4     ;Line 0 of Matrix is enabled
        bsf             PORTB,0x5
        bsf             PORTB,0x6
        bsf             PORTB,0x7
;-------------------------------------------------------------------------------------------------------------
        btfss           PORTB,0x0     ;Scan for 1,2,3,A
        goto            kb01
        btfss           PORTB,0x1
        goto            wkb 
        btfss           PORTB,0x2
        goto            wkb
        btfss           PORTB,0x3
        goto            wkb 
;-------------------------------------------------------------------------------------------------------------
        bsf             PORTB,0x4	;Line 1 of Matrix is enabled
        bcf             PORTB,0x5
;-------------------------------------------------------------------------------------------------------------
        btfss           PORTB,0x0	;Scan for 4,5,6,B
        goto            wkb
        btfss           PORTB,0x1
        goto            wkb
        btfss           PORTB,0x2
        goto            wkb
        btfss           PORTB,0x3
        goto            wkb 
;-------------------------------------------------------------------------------------------------------------
        bsf             PORTB,0x5	;Line 2 of Matrix is enabled
        bcf             PORTB,0x6
;-------------------------------------------------------------------------------------------------------------
        btfss           PORTB,0x0	;Scan for 7,8,9,C
        goto            wkb
        btfss           PORTB,0x1
        goto            wkb
        btfss           PORTB,0x2
        goto            wkb
        btfss           PORTB,0x3
        goto            wkb
;-------------------------------------------------------------------------------------------------------------
        bsf             PORTB,0x6	;Line 3 of Matrix is enabled
        bcf             PORTB,0x7
;-------------------------------------------------------------------------------------------------------------
        btfss           PORTB,0x0	;Scan for *,0,#,D
        goto            wkb 
        btfss           PORTB,0x1
        goto            kb00
        btfss           PORTB,0x2
        goto            wkb 
        btfss           PORTB,0x3
        goto            wkb 
;--------------------------------------------------------------------------------------------------------------

        goto            wkb

kb00:   movlw           0x00
        goto            disp	
kb01:   movlw           0x01
        goto            disp	
kb02:   movlw           0x02
        goto            disp	
kb03:   movlw           0x03	
        goto            disp	
	
kb04:   movlw           0x04
        goto            disp	
kb05:   movlw           0x05
        goto            disp	
kb06:   movlw           0x06
        goto            disp	
kb07:   movlw           0x07
        goto            disp	
kb08:   movlw           0x08
        goto            disp	
kb09:   movlw           0x09
        goto            disp	
kb0a:   movlw           0x0a
        goto            disp	
kb0b:   movlw           0x0b
        goto            disp	
kb0c:   movlw           0x0c
        goto            disp	
kb0d:   movlw           0x0d
        goto            disp	
kb0e:   movlw           0x0e
        goto            disp	
kb0f:   movlw           0x0f
        goto            disp	

;------------------------------------------------------ Buttons Matrix Loop ---------------------------------------------------------------------

disp:   return
	         

; Here we enter into the registers of A, B, C respectively the values
; that the user enters, make sure that he enters 3 digits for each variable,
; and of course make sure that the op code that he enters into variable C is correct,
; otherwise we display ERROR on the LCD


;----------------------------------------------------------- User Input -------------------------------------------------------------------------
user_input:

	;Insert number to 'A'
	movlw 'A'
	movwf 0x69  ;contains letters a b and c to print when start
	call enter
continue_A:
	call wkb
	call insert_A
	movf 0x43,w ; Transferring the value returned from the press to W
	call print  ; Printing the digit on the screen
	incf 0x33,f ; Advancing the counter counting how many numbers we entered into the variable
	movlw 0x03  
	subwf 0x33,w
	btfsc STATUS,Z 
	goto go_B   ; If we finished we'll move on to entering into the variable B
	goto continue_A

go_B:
	; This delay used for that the user could see the number that he entered
	call delay 
	call delay
	call delay
	call delay
	call init	
	clrf 0x33
	;Insert number to 'B'
	movlw 'B'
	movwf 0x69
	call enter
continue_B:	
	call wkb
	call insert_B
	movf 0x43,w
	call print
	incf 0x33,f
	movlw 0x03
	subwf 0x33,w
	btfsc STATUS,Z
	goto go_C
	goto continue_B

go_C
	; This delay used for that the user could see the number that he entered
	call delay
	call delay
	call delay
	call delay
	call init	
	clrf 0x33
	;Insert number to 'C
	movlw 'C'
	movwf 0x69
	call enter
continue_C:
	call wkb
	call insert_C
	movf 0x43,w
	call print
	incf 0x33,f
	movlw 0x03
	subwf 0x33,w
	btfsc STATUS,Z
	goto fucntions
	goto continue_C

;----------------------------------------------------------- User Input --------------------------------------------------------------------

	



;-------------------------------------------------------- OpCode Check ---------------------------------------------------------------------

; Here we checks what the value of C is and based on that we jump to the appropriate operator
; We will start from 0 and subtract from the variable each time and if the result is not 0 we'll increment by 1 and subtract again

fucntions:
	; This delay used for that the user could see the number that he entered
	call delay
	call delay
	call delay
	call delay
	; If C is "000" we displays "ERROR!"
	call init ; initializing the LCD screen
	clrf 0x43 ; temp register
	movf 0x50,w
	subwf 0x43,w 
	btfsc STATUS,C
	goto print_error
	
	; If C is "001" we go to the operator of sub
	movf 0x50,w
	incf 0x43,f
	subwf 0x43,w
	btfsc STATUS,C
	goto f_sub
	
	; If C is "010" we go to the operator of multiply
	movf 0x50,w
	incf 0x43,f
	subwf 0x43,w
	btfsc STATUS,C
	goto f_mult
	
	; If C is "011" we go to the operator of divide
	movf 0x50,w
	incf 0x43,f
	subwf 0x43,w
	btfsc STATUS,C
	goto f_divide
	
	; If C is "100" we go to the operator of power
	movf 0x50,w
	incf 0x43,f
	subwf 0x43,w
	btfsc STATUS,C
	goto f_power
	
	; If C is "101" we go to the operator that countes the number of "1" in B
	movf 0x50,w
	incf 0x43,f
	subwf 0x43,w
	btfsc STATUS,C
	goto f_c_ones_in_B
	
	; If C is "110" we go to the operator that countes the number of "0" in A
	movf 0x50,w
	incf 0x43,f
	subwf 0x43,w
	btfsc STATUS,C
	goto f_c_zeroes_in_A
	
	; If C is "111" we displays "ERROR!"
	movf 0x50,w
	incf 0x43,f
	subwf 0x43,w
	btfsc STATUS,C
	goto print_error
	
;------------------------------------------------------ Op Code Cheack ---------------------------------------------------------------------
	

;*** Displays " ENTER 'var' " on the LCD for each variable (A,B,C) ****

enter:
		movlw	0x84	    ;Location of the cursor (on the top row at the middle)
		movwf	0x20
		call 	lcdc
		call	mdel


		movlw	'E'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'N'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'T'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'E'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'R'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	' '			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movf	0x69,w
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	':'			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		;To move the cursor to the next display to the bottom line in the middle
		movlw	0xC6	    ;PLACE for the data on the LCD, locates the cursor to the bottom middle row
		movwf	0x20
		call 	lcdc
		call	mdel
		
		
		return


; ------------------------------------------------------ Initialization of the LCD screen ------------------------------------------------------

;* subroutine to initialize LCD *
init	movlw	0x30
		movwf	0x20
		call 	lcdc
		call	del_41

		movlw	0x30
		movwf	0x20
		call 	lcdc
		call	del_01

		movlw	0x30
		movwf	0x20
		call 	lcdc
		call	mdel

		movlw	0x01		; display clear
		movwf	0x20
		call 	lcdc
		call	mdel

		movlw	0x06		; ID=1,S=0 increment,no  shift 000001 ID S
		movwf	0x20
		call 	lcdc
		call	mdel

		movlw	0x0c		; D=1,C=B=0 set display ,no cursor, no blinking
		movwf	0x20
		call 	lcdc
		call	mdel

		movlw	0x38		; dl=1 ( 8 bits interface,n=12 lines,f=05x8 dots)
		movwf	0x20
		call 	lcdc
		call	mdel
		return


;* subroutine to write command to LCD **
lcdc	movlw	0x00		; E=0,RS=0 
		movwf	PORTE
		movf	0x20,w
		movwf	PORTD
		movlw	0x01		; E=1,RS=0
		movwf	PORTE
        call	sdel
		movlw	0x00		; E=0,RS=0
		movwf	PORTE
		return

;* subroutine to write data to LCD **

lcdd	movlw		0x02		; E=0, RS=1
		movwf		PORTE
		movf		0x20,w
		movwf		PORTD
        movlw		0x03		; E=1, rs=1  
		movwf		PORTE
		call		sdel
		movlw		0x02		; E=0, rs=1  
		movwf		PORTE
		return

;---------------------------------------------------

del_41	movlw		0xcd
		movwf		0x23
lulaa6	movlw		0x20
		movwf		0x22
lulaa7	decfsz		0x22,1
		goto		lulaa7
		decfsz		0x23,1
		goto 		lulaa6 
		return


del_01	movlw		0x20
		movwf		0x22
lulaa8	decfsz		0x22,1
		goto		lulaa8
		return


sdel	movlw		0x19		; movlw = 1 cycle
		movwf		0x23		; movwf	= 1 cycle
lulaa2	movlw		0xfa
		movwf		0x22
lulaa1	decfsz		0x22,1		; decfsz= 12 cycle
		goto		lulaa1		; goto	= 2 cycles
		decfsz		0x23,1
		goto 		lulaa2 
		return


mdel	movlw		0x0a
		movwf		0x24
lulaa5	movlw		0x19
		movwf		0x23
lulaa4	movlw		0xfa
		movwf		0x22
lulaa3	decfsz		0x22,1
		goto		lulaa3
		decfsz		0x23,1
		goto 		lulaa4 
		decfsz		0x24,1
		goto		lulaa5
		return

; ------------------------------------------------------ Initialization of the LCD screen ------------------------------------------------------


delay:					
		movlw 0x40	
		movwf 0x64
		CONT1: 
		movlw 0x65		
		movwf 0x65
		CONT2: 
		movlw 0x20	
		movwf 0x66
		CONT3:
		decfsz 0x66,f
		goto CONT3
		decfsz 0x65,f
		goto CONT2
		decfsz 0x64,f
		goto CONT1
		return	
 
; * Displays on the LCD the current number that entered by the user *
print:
		addlw	0x30
		movwf	0x20
		call 	lcdd
		call	mdel
		
		return


; ------------------------------------------------------ Insert Variables ------------------------------------------------------

; In all of the below insert_'var' functions, we take the 3 digits that the user
; entered for each variable and calculate the final value by moving the number to
; the left for each digit that is received. For example, if the user entered for the variable
; A the number 1, then our number will look like this 001 and then if the user entered
; the number 0 our number will look like this 010 (we shifts the number once to the left and adding 0)
; and if the user entered the number 1 at the end, our number will look like this 101.


insert_A: ; 0x30
		movwf 0x43    
		addwf 0x30,f  
		movlw 0x02  
		subwf 0x33,w  
 		btfsc STATUS,Z ;
		return 
		movf 0x30,w
		addwf 0x30,f
		return


insert_B: ; 0x40
		movwf 0x43
		addwf 0x40,f
		movlw 0x02
		subwf 0x33,w
		btfsc STATUS,Z
		return 
		movf 0x40,w
		addwf 0x40,f
		return

insert_C: ; 0x50
		movwf 0x43
		addwf 0x50,f
		movlw 0x02
		subwf 0x33,w
		btfsc STATUS,Z
		return 
		movf 0x50,w
		addwf 0x50,f
		return

; ------------------------------------------------------ Insert Variables ------------------------------------------------------




; ------------------------------------------------------ Functions -------------------------------------------------------------
;function to perform A-B
f_sub:
		movf 0x40,w ; Move B to W
		subwf 0x30,w ; Sub W from A (A-B)
		btfsc STATUS,C ; If the sub is positive we just print the result
		goto print_result
		; In case that the result is negative we calc the opposite (B-A) and prints '-' before the res
		movf 0x30,w
		subwf 0x40,w
		goto print_minus

;fuמction to perform A*B
f_mult:
		movlw 0x0f ; reg for check if the result is higher than 15 (out of bounds 4 bits)
		movwf 0x43
		movlw 0x00 ; clears the w reg
  mult_loop:
		addwf 0x30,w ; For exmaple: 3*4 = 3 + 3 + 3 + 3 
		decfsz 0x40,f
		goto mult_loop
		movwf 0x60 ; Moving the final value to result register 0x60
		subwf 0x43,f ; Checking if the result is out of bounds (more than 15 because we have only 4 bits)
		btfsc STATUS,C
		goto print_result
		goto print_outOfBounds


;fumction to perform A/B
; Here we are essentially performing division and returning the whole value meaning how 
; many times we managed to divide
f_divide:
	movlw 0x00
	subwf 0x40,w
	btfsc STATUS,Z ; If the var B is 0 (cant div by 0)
	goto print_div_0	
	movlw  0x00
	clrf  0x43 ; counter that checks how many times we divided
	movf  0x40,w ; Transferring the value in B to the working register
	sub_loop:
		subwf 0x30,f ; Sub from A the value in B (that now in w)
		btfsc STATUS,C ; Checking if the result is negative
		goto incCounter ; If not then incrementing the counter
		movf 0x43,w ; Transferring the counter to W
		goto print_result 
	incCounter:
		incf 0x43,f
		goto sub_loop
		
		


;fumction to perform A^B
; Since we know that our result register contains only up to 4 bits,
; we know that the highest power we can perform for 3-bit numbers (A,B) is 3,
; so we wrote separate functions for A^2/A^3 and the other powers (if A is not 0 or 1 Or if the power (B)
; is not 0 or 1) we sent straight to the OUT OF BOUNDS displayer.
; We also handled cases where A=1 B=1 A=0 B=0

f_power:
		; Edge cases
		; Checking if B is 0 (power is 0)
		movlw 0x00
		subwf 0x40,w
		btfsc STATUS,Z
		goto is_B_0
		
		; Checking if B is 1 (power is 1)
		movlw 0x01
		subwf 0x40,w
		btfsc STATUS,Z
		goto is_B_1	 	
		
		; Checking if A is 1
		movlw 0x01
		subwf 0x30,w
		btfsc STATUS,Z
		goto is_A_1
		
		movlw 0x02
		subwf 0x40,w
		btfss STATUS,Z
		goto power3
		goto power2

			 
;subroutines to deal with edge cases the power is 1/0, the base is 1
is_A_1:
		movlw 0x01
		goto print_result
			
is_B_1:
		movf 0x30,w
		goto print_result

is_B_0:
		movlw 0x01
		goto print_result
		
		

;subroutine to deal with power of 3		
power3
		movlw 0x0f ; check if the result is higher than 15 (out of bounds 4 bits)
		movwf 0x43 ; 0x43 = 15
		decf 0x40,f	; 
		clrf 0x35 ;temp register
		clrf 0x36 ;temp register
		movf 0x30,w ; 
		movwf 0x36 ;

power3_loop:
		call power_loop_helper
		decf 0x36,f
		movf 0x36,w
		movwf 0x30
		movf 0x35, w
		decfsz 0x40,f
		goto power3_loop	
		movf 0x35,w
		subwf 0x43,f
		btfsc STATUS,C
		goto print_result
		goto print_outOfBounds

;subroutine to deal with power of 2
power2
		movlw 0x0f ; check if the result is higher than 15 (out of bounds 4 bits)
		movwf 0x43
		decf 0x40,f	
		clrf 0x35
		clrf 0x36
		movf 0x30,w
		movwf 0x36

power2_loop:
		call power_loop_helper
		movf 0x36,w
		movwf 0x30
		movf 0x35, w
		decfsz 0x40,f
		goto power2_loop	
		movf 0x35,w
		subwf 0x43,f
		btfsc STATUS,C
		goto print_result
		goto print_outOfBounds


 power_loop_helper:
		addwf 0x35,f 
		decfsz 0x30,f 
		goto power_loop_helper
		return	


			

;fumction to count number of '1's in B
f_c_ones_in_B:
		clrf 0x43
		btfsc 0x40,0x02
		incf 0x43,f

		btfsc 0x40,0x01
		incf 0x43,f

		btfsc 0x40,0x00
		incf 0x43,f

		movf 0x43,0x60
		goto print_result



;fumction to count number of '0's in A
f_c_zeroes_in_A
		clrf 0x43
		btfss 0x30,0x02
		incf 0x43,f

		btfss 0x30,0x01
		incf 0x43,f

		btfss 0x30,0x00
		incf 0x43,f

		movf 0x43,0x60
		goto print_result

; ------------------------------------------------------ Functions -------------------------------------------------------------


; ------------------------------------------------------- Prints ---------------------------------------------------------------

print_error:
		movlw	0x85	    ;PLACE for the data on the LCD
		movwf	0x20
		call 	lcdc
		call	mdel


		movlw	'E'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'R'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'R'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'O'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'R'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'!'			
		movwf	0x20
		call 	lcdd
		call	mdel
		goto loop

;if the result of is negative print '-' character
print_minus
		
		movwf	0x60
		movlw	0x85	    ;PLACE for the data on the LCD
		movwf	0x20
		call 	lcdc
		call	mdel

		movlw	'-'			
		movwf	0x20
		call 	lcdd
		call	mdel

		goto continue_print

;print the actual result
;we decided to print in binary, four bits 1/0 of the result register 0x60
print_result:
	
		movwf	0x60
		movlw	0x86	    ;PLACE for the data on the LCD
		movwf	0x20
		call 	lcdc
		call	mdel

continue_print:
		
		btfss 0x60,0x03
		call print_0
		decfsz 0x44,f
		call print_1

		btfss 0x60,0x02
		call print_0
		decfsz 0x44,f
		call print_1

		btfss 0x60,0x01
		call print_0
		decfsz 0x44,f
		call print_1

		btfss 0x60,0x00
		call print_0
		decfsz 0x44,f
		call print_1

		goto loop


print_0:
		incf 0x44,f
		movlw	0x30	
		movwf	0x20
		call 	lcdd
		call	mdel
		return


print_1:
		clrf 0x44
		movlw	0x31	
		movwf	0x20
		call 	lcdd
		call	mdel
		return


;if result of an operand is more than 4 bits print 'out of bounds'
print_outOfBounds:
		movlw	0x81	    ;PLACE for the data on the LCD
		movwf	0x20
		call 	lcdc
		call	mdel


		movlw	'O'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'U'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'T'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	' '			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'O'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'F'			
		movwf	0x20
		call 	lcdd
		call	mdel
	
		movlw	' '			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	'B'			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	'O'			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	'U'			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	'N'			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	'D'			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	'S'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'!'			
		movwf	0x20
		call 	lcdd
		call	mdel
		goto loop


;if result of division is 0 print 'cant div by 0'
print_div_0:
		movlw	0x81	    ;PLACE for the data on the LCD
		movwf	0x20
		call 	lcdc
		call	mdel


		movlw	'C'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'A'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'N'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'`'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'T'			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	' '			
		movwf	0x20
		call 	lcdd
		call	mdel
	
		movlw	'D'			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	'I'			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	'V'			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	' '			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	'B'			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	'Y'			
		movwf	0x20
		call 	lcdd
		call	mdel
		
		movlw	' '			
		movwf	0x20
		call 	lcdd
		call	mdel

		movlw	'0'			
		movwf	0x20
		call 	lcdd
		call	mdel
		goto loop
		
; ------------------------------------------------------- Prints ---------------------------------------------------------------

loop:
	goto loop

	end
	
	
;************************************************************ End ***************************************************************
