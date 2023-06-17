ORG 100H

; Constants
NUM_ATTEMPTS EQU 10
SECRET_NUMBER EQU 42

; Variables
attempts DB NUM_ATTEMPTS
guess DB ?
message DB "Guess a number between 1 and 100:", 0
win_message DB "Congratulations! You guessed the correct number.", 0
lose_message DB "Game over! You couldn't guess the number.", 0
prompt DB "> ", 0

MAIN:
    MOV AH, 09H ; Print message
    MOV DX, OFFSET message
    INT 21H

    MOV AH, 0AH ; Read input
    MOV DX, OFFSET guess
    INT 21H

    SUB AL, 30H ; Convert ASCII to binary
    MOV BL, AL ; Save the guess

    MOV AH, 09H ; Print prompt
    MOV DX, OFFSET prompt
    INT 21H

    MOV AL, BL ; Compare the guess with the secret number
    CMP AL, SECRET_NUMBER
    JE WIN

    DEC attempts ; Decrement attempts
    JZ LOSE ; If zero attempts left, go to lose

    JMP MAIN ; Otherwise, continue the loop

WIN:
    MOV AH, 09H ; Print win message
    MOV DX, OFFSET win_message
    INT 21H

    JMP EXIT

LOSE:
    MOV AH, 09H ; Print lose message
    MOV DX, OFFSET lose_message
    INT 21H

EXIT:
    MOV AH, 4CH ; Exit program
    INT 21H

END
