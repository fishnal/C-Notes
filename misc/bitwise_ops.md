# Bitwise Operators <a id="bitwiseops"></a>
+ [One's and Two's Complement](#bitwiseops-complements)

|Operator|Name|Type|Description|Example|
|:-:|:-:|:-:|:-|:-|
|`&`|AND|binary|true if both true|`3&2 = 2`|
|`\|`|OR|binary|true if one true|`3\|2 = 3`|
|`^`|XOR|binary|true if one true and one false|`3^2 = 1`|
|`~`|NOT|unary|true if false|`~3 = 0` (assuming 2-bit number)|
|`>>`|right shift|binary|shifts a number `n` bits to the right (divides and truncates by 2<sup>`n`</sup>)|`9>>3 = 1`|
|`<<`|left shift|binary|shifts a number `n` bits to the left (multiplies by 2<sup>`n`</sup>)|`9<<3 = 72`|

**Note:** Do not confuse the bitwise operators for the boolean operators!

Hexadecimal can easily be converted to binary:
```
0xfa
-> find binary of 0xf -> 1111
-> find binary of 0xa -> 1010
-> concatenate results -> 1111_1010
```
This intuition applies to any number base that's a power of 2!
```
octal example
0173 -> 123
-> find binary of 01 -> 001
-> find binary of 07 -> 111
-> find binary of 03 -> 011
-> concatenate results -> 001_111_011
```
## One's Two's Complement <a id="bitwiseops-complements"></a>
One's Complement simply involves flipping all the bits, or performing the `NOT` bitwise operation on a number.
+ i.e. 1101 -> 0010

Two's Complement is just taking the One's Complement of a number and adding 1 to the result
+ i.e. 1101 -> 0010 + 1 -> 0010

Two's Complement is significant when it comes to signed numbers. A question you may be asked is
```
What is the two's complement of -5 of a 4-bit number?
```
The approach is to
+ Find the binary representation of `5` for a 4-bit number, which is `0101`.
+ Flip it -> `1010`.
+ Add 1 -> `1011`.
+ And you're done!

This approach applies to basically any negative integer of an `n`-bit number.
