# AVR Assembly

## Registers

- A register is one byte long (8 bits)
- *words* are two registers next to each other (i.e. `r1:0` or `r25:24`)
- There are 32 registers in AVR assembly, from `r0` to `r31`
- `r1` is known as the zero register (because `r0` wouldn't make any sense to be the zero register)

## Pointer registers

- Special register pairs (so special that they have their own short names)
	- `r27:r26 -> X`
	- `r29:r28 -> Y`
	- `r31:r30 -> Z`
	- Can reference higher byte of the pair by using `H` and lower using `L`
		- i.e. `ldi YH, hi(256)` (stores hi 8 bits of 256 into higher byte of `Y`, or effectively into `r29`)
- `ld` read access for pointer registers, `st` write access for pointer registers
- `X` simply reads/writes from the address `X`, pointer doesn't change
	- `ld r1, x` or `st x, r1`
- `X+` reads/writes from/to address `X` then increments pointer forward by one
	- `ld r1, x+` or `st x+, r1`
- `-X` first decrement pointer by one then read/write from/to new address
	- `ld r1, -x` or `st -x, r1`
- Note: you can use `Y` or `Z` in place of `X`

## Functions

- Calling functions via `call` creates a new stack frame
	- Do NOT confuse this with jumping to labels.
- When calling and writing functions, be wary of callee-save and caller-save registers:
	- Caller-save `r18-r27;r30-r31`: saved by whoever called this function, so this function can freely change these
	- Callee-save `r2-r17; r28-r29`: saved by the function that's called, so the caller should expect these to have to the same values after the function returns
- Be wary of `r0` and `r1`:
	- `r0` is a garbage like register, so anything can be in there. Don't expect any function to be considerate of what you put in there. With actual reasoning, when multiplying two registers, the product is stored in `r1:0`. This is because the product of two 8-bit numbers can be 16-bits. These registers are meant to be messed around with (well not so much `r1`, but more on that later), so it's safer for the program to store the product in here.
	- `r1` is the zero register. So if this gets modified in any way whatsoever, you need to make sure you clear this register before leaving your subroutine. Everyone's assuming that `r1` will be 0 when they work with it. So if you multiply, remember to clear `r1` after you're done with the product.
- Parameters are passed via `r25` to `r8` (left to right)

	```c
	uint8_t function(uint8_t i, uint8_t j);
	/* i would be in `r25:r24` (w/8-bit value in `r24`) */
	/* j would be in `r23:r22` (w/8-bit value in `r22`) */
	```

	- If we have many parameters, then they're pushed onto the stack

- Return values are stored in registers `r25:18` depending on how big the return value is
	- 8-bit: `r24`
	- 16-bit: `r24-r25`
	- 32-bit: `r22-r25`
	- 64-bit: `r18-r25`

## SRAM

- Static RAM (hence **S**RAM)
- Useful for pushing variables to stack
	- Contents of a register (caller-save registers in particular)
	- Return address prior or after a subroutine
- Useful for recursion

## Recursion

- Before calling a recursive function, save any registers you will need to use (especially caller-save ones)  after the recursion finishes.
- Make sure you write your base cases properly
	- There's no less than or equal to, there's only less than
	- No greater than, only greater than or equal to