# LAB2

**Student ID:** 22110038
**Name:** Phan Triệu Huy 

## Delete File dummyfile:
create file dummy and setup:

**change file_del.asm**:

Because eax is 32 bits register so when we move a small value into eax, the high bits of this register (24 bits) will automatically be filled with 0.

We need to edit a few lines of code in file_del.asm:

  Use al register. Because this is the lowest 8-bit part of the eax register. When you move a value into al, only 8-bits (1 byte) are changed, not affecting the rest of eax.

  Clear the eax register using command:

xor eax, eax

  Replace move eax, 10 with:

mov al, 8

add al, 2









