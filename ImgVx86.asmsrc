; ImgVx86 - DOS image/photo viwer, Version 1.0
; Copyright 1998-2003 ardeshirv@protonmail.com, Licensed under GPLv3+

Title "ImgVx86"
.8086

SegmentGraphic	SEGMENT	AT 0A000h
bytScreen	db	64000 dup(?)
SegmentGraphic	ENDS

CodeSeg		SEGMENT PARA PUBLIC 'CODE'
  ASSUME cs:CodeSeg, ds:CodeSeg, es:CodeSeg, ss:CodeSeg
  ORG 100h

;-----------------------------------------------------------------------;
; Main Entry Point                                                      ;
;-----------------------------------------------------------------------;
Main            PROC    NEAR
		call	ClearScreen

		; Init Graphic Mode
		mov	ax, 0013h
		int	10h

		call	UpdatePalette

		; Read First Command Line Argument
		mov	dx, 82h
		mov	di, dx
		mov	ax, 13
		mov	cx, 80
		repne	scasb
		mov	BYTE PTR [di - 1], ah

		call	DrawImage
		call	WaitKeyPress

		; Restore Graphic Mode
		mov	ax, 0002h
		int	10h
		ret
Main            ENDP


;-----------------------------------------------------------------------;
; WriteStr(SI(StringZ)) 						;
;-----------------------------------------------------------------------;
;WriteStr	PROC	NEAR
;		push	ax
;		push	bx
;		push	si
;		pushf
;		mov	ah, 14
;		xor	bh, bh
;		cld
;WSLoop:	lodsb
;		or	al, al
;		jz	WSEnd
;		int	10h
;		jmp	WSLoop
;WSEnd:		popf
;		pop	si
;		pop	bx
;		pop	ax
;		ret
;WriteStr	ENDP


;-----------------------------------------------------------------------;
; DrawImage(DX(strFileNameZ))						;
; Read Image file from file and draw on screen.				;
;-----------------------------------------------------------------------;
DrawImage	PROC	NEAR
		push	ax
		push	bx
		push	cx
		push	dx
		push	si
		push	di
		push	bp
		pushf

		; Open File
		mov	ax, 03D00h
		int	21h
		mov	wrdFileHandler, ax
		jc	diFileOpenErr

		sub	cx, cx
mDrawCol:	sub	dx, dx
mLoadNext:	mov	si, dx
		mov	bp, cx

		; Read File
		mov	ah, 03Fh
		mov	bx, wrdFileHandler
		mov	cx, 100
		mov	dx, 81h
		int	21h
		jc	diFileReadErr

		; Draw pixel
		mov	cx, bp
		mov	dx, si
		mov	ah, 0ch
		mov	bx, 81h

mDrawAgain:	mov	di, cx
		mov	cl, 2
		mov	al, [bx]
		shr	al, cl
		sub	al, 03
		mov	cx, di
		int	10h
		inc	bx
		inc	dx
		cmp	dx, 100
		je	mLoadNext
		cmp	dx, 200
		jne	mDrawAgain
		inc	cx
		cmp	cx, 320
		jne	mDrawCol

		; Close File
		mov	bx, wrdFileHandler
		mov	ah, 03Eh
		int	21h
		jc	diFileCloseErr

		; Error Handlers
		jmp	diEndNormaly
diFileOpenErr:	;mov	si, OFFSET strFileOpenErr
		;call	WriteStr
		;int	20h
		;jmp	diEndNormaly
diFileCloseErr:	;mov	si, OFFSET strFileCloseErr
		;call	WriteStr
		;jmp	diEndNormaly
diFileReadErr:	;mov	si, OFFSET strFileReadErr
		;call	WriteStr
		int	20h
diEndNormaly:	popf
		pop	bp
		pop	di
		pop	si
		pop	dx
		pop	cx
		pop	bx
		pop	ax
		ret
DrawImage	ENDP


;-----------------------------------------------------------------------;
; WaitKeyPress - Wait for key press.					;
;-----------------------------------------------------------------------;
WaitKeyPress	PROC	NEAR
		push	ax
		sub	ah, ah
		int	16h
		pop	ax
		ret
WaitKeyPress	ENDP


;-----------------------------------------------------------------------;
; UpdatePalette								;
;-----------------------------------------------------------------------;
UpdatePalette	PROC	Near
		push	ax
		push	bx
		push	cx
		push	dx
		mov	ax, 1012h
		xor	bx, bx
		mov	cx, End_Palette - bytPalette
		mov	dx, OFFSET bytPalette
		int	10h
		pop	dx
		pop	cx
		pop	bx
		pop	ax
		ret
UpdatePalette	ENDP


;-----------------------------------------------------------------------;
; CleanScreen - Clear Screen Monitor.					;
;-----------------------------------------------------------------------;
ClearScreen	PROC	NEAR
		push	ax
		push	bx
		push	cx
		push	dx
		pushf
		xor	cx, cx
		mov	bh, 07h
		mov	ax, 0600h
		mov	dx, 0184Fh
		int	10h
		popf
		pop	dx
		pop	cx
		pop	bx
		pop	ax
		ret
ClearScreen	ENDP


;-----------------------------------------------------------------------;
; Data Defination: 							;
;-----------------------------------------------------------------------;
wrdFileHandler	dw	0
;strFileName	db	"Image.img", 0
;strFileOpenErr	db	"File open failed.", 0Dh, 0Ah, 0
;strFileCloseErr db	"File close failed.", 0Dh, 0Ah, 0
;strFileReadErr	db	"Read from file failed.", 0Dh, 0Ah, 0

bytPalette	db	01, 0, 0
		db	02, 0, 0
		db	03, 0, 0
		db	04, 0, 0
		db	05, 0, 0
		db	06, 0, 0
		db	07, 0, 0
		db	08, 0, 0
		db	09, 0, 0
		db	10, 0, 0
		db	11, 0, 0
		db	12, 0, 0
		db	13, 0, 0
		db	14, 0, 0
		db	15, 0, 0
		db	16, 0, 0
		db	17, 0, 0
		db	18, 0, 0
		db	19, 0, 0
		db	20, 0, 0
		db	21, 0, 0
		db	22, 0, 0
		db	23, 0, 0
		db	24, 0, 0
		db	25, 0, 0
		db	26, 0, 0
		db	27, 0, 0
		db	28, 0, 0
		db	29, 0, 0
		db	31, 0, 0
		db	32, 0, 0
		db	33, 0, 0
		db	34, 0, 0
		db	35, 0, 0
		db	36, 0, 0
		db	37, 0, 0
		db	38, 0, 0
		db	39, 0, 0
		db	40, 0, 0
		db	41, 0, 0
		db	42, 0, 0
		db	43, 0, 0
		db	44, 0, 0
		db	45, 0, 0
		db	46, 0, 0
		db	47, 0, 0
		db	48, 0, 0
		db	49, 0, 0
		db	51, 0, 0
		db	52, 0, 0
		db	53, 0, 0
		db	54, 0, 0
		db	55, 0, 0
		db	56, 0, 0
		db	57, 0, 0
		db	58, 0, 0
		db	59, 0, 0
		db	60, 0, 0
		db	61, 0, 0
		db	62, 0, 0
		db	63, 0, 0
End_Palette:


CodeSeg         ENDS
END Main

