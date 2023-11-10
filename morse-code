        THUMB       ; Declare THUMB instruction set
         AREA        My_code, CODE, READONLY     ;
        EXPORT      __MAIN      ; Label __MAIN is used externally q
        ENTRY


; Note: R13 is the Stack Pointer
; Note: R14 is the Link Register
; Note: R15 is the Program Counter


__MAIN

; Turn off all LEDs
        MOV         R2, #0xC000
        MOV         R3, #0xB0000000
        MOV         R4, #0x0
        MOVT        R4, #0x2009
        ADD         R4, R4, R2              ; 0x2009C000 - the base address for dealing with the ports
        STR         R3, [r4, #0x20]         ; Turn off the three LEDs on port 1
        MOV         R3, #0x0000007C
        STR         R3, [R4, #0x40]         ; Turn off five LEDs on port 2

ResetLUT
        LDR         R5, =InputLUT           ; assign R5 to the address at label LUT
NextChar                                    ; Start processing the characters
        LDRB        R0, [R5]                ; Read a character to convert to Morse Code
        ADD         R5, #1                  ; point to next value for number of delays, jump by 1 byte

        BL          LED_OFF

        TEQ         R0, #0                  ; If we hit 0 (null at end of the string) then reset to the start of lookup table
        BNE         ProcessChar             ; If we have a character process it

        MOV         R0, #4                  ; delay 4 extra spaces (7 total) between words
        BL          DELAY
        BEQ         ResetLUT

ProcessChar 
        BL          CHAR2MORSE              ; convert ASCII to Morse pattern in R1
        MOV         R0, R1                  ; Morse value moved into the register R0
        BL          LED_BLINK               ; Branch to Blink subroutine
        BL          LED_OFF
        MOV         R0, #3                  ; delay 3 spaces between chaaracters
        BL          DELAY
        B           NextChar                ; Branch to NextChar subroutine
            
;;;;;;;;;; END OF MAIN ;;;;;;;;

; Input register: R0, Output register: R1

CHAR2MORSE  
        PUSH            {LR}            ; push Link Register (return address) on stack
        SUB             R0, R0, #0x41   
        LSL             R0, R0, #0x1    
        LDR             R1, =MorseLUT   
        LDRH            R1, [R1,R0]     ; R1 points to MorseLUT, R1 gets set the morseLUT table + offset of character
        POP             {PC}            ; Pops the link register which is storing the address of where we linked from, back into the PC (R15)

LED_ON      STMFD   R13!,{R3, R4}       ; preserve R3 and R14 on the R13 stack
            MOV     R3,#0xA0000000
            STR     R3,[R4, #0x20]
            LDMFD   R13!,{R3,R4}
            BX      LR                  ; branch to the address in the Link Register.  Ie return to the caller

LED_OFF     PUSH    {R3, R14}           ; push R3 and Link Register (return address) on stack
            MOV     R3, #0xB0000000
            STR     R3, [R4, #0x20]
            POP     {R3, R15}           ; restore R3 and LR to R15 the Program Counter to return

;Input register: R0, Output register: R0

DELAY           STMFD       R13!,{R2, R8, R10, R14}

MultipleDelay   TEQ     R0, #0          ; test R0 to see if it's 0 - set Zero flag so you can use BEQ, BNE
                BEQ     exitDelay

                MOV     R10, #0x2C27    ; Initialize lower bits for countdown
                MOVT    R10, #0x000A    ; Initializes upper 16 bits

DelayLoop
                SUBS    R10, #1         ; decrement 500ms counter
                BNE     DelayLoop       ; decrement until counter is finished
                SUB     R0, #1          ; check if all 500ms delays have finished
                B       MultipleDelay   ; Branch to restart delay or exit delay

exitDelay       LDMFD   R13!,{R2, R8, R10, R15}

;Input register: R0, Output register:

LED_BLINK       STMFD R13!, {R8, R14}
       
        CLZ     R8, R0
        LSL     R0, R8
ONOFF   LSLS    R0,#0x1
        STMFD   R13!, {R0}
        LDR     R14,=REVERT
        BCS     LED_ON
        BCC     LED_OFF

REVERT
        MOV     R0, #1
        BL      DELAY
        LDMFD   R13!, {R0}
        TEQ     R0, #0               ; Tests to see if R0=0
        BNE     ONOFF                ; Loop back to Blink so long as R0 is not equal to zero
        LDMFD   R13!,{R8, R15}

        ALIGN                        ; make sure things fall on word addresses

InputLUT    DCB     "JARPS", 0       ; Input letters

        ALIGN               
MorseLUT
        DCW     0x17, 0x1D5, 0x75D, 0x75    ; A, B, C, D
        DCW     0x1, 0x15D, 0x1DD, 0x55     ; E, F, G, H
        DCW     0x5, 0x1777, 0x1D7, 0x175   ; I, J, K, L
        DCW     0x77, 0x1D, 0x777, 0x5DD    ; M, N, O, P
        DCW     0x1DD7, 0x5D, 0x15, 0x7     ; Q, R, S, T
        DCW     0x57, 0x157, 0x177, 0x757   ; U, V, W, X
        DCW     0x1D77, 0x775               ; Y, Z


LED_PORT_ADR    EQU 0x2009c000  ; Base address of the memory that controls I/O like LEDs

        END
