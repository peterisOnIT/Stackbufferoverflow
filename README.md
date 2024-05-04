# Stackbufferoverflow

A developer built a program to verify and approve access for a user. We will pretend that this is an important program with SUID. The developer did a crapshoot job. Your goal is to exploit with two different approaches (buffer overflow and stack-based buffer overflow). You will write the final exploits in the files (exploit_1.sh Download exploit_1.shand gdb_script) Download gdb_script). In other words, assume you don't know the password, how do you crack the program and get access granted (even if it segfaults eventually). Then, your final task is to fix this program so that it won't be exploitable (auth_overflow2_fixed.c). So in total:

Buffer overflow (exploit_1.sh Download exploit_1.sh) - add successful overflow parameter after $1.
Stack-based buffer overflow (gdb_script Download gdb_script) - use the template code and adjust A*N as well as the hexadecimal value.
Buffer overflow fix (auth_overflow2_fixed.c) - there are many solutions, look them up and pick the best.
To keep things interesting (meaning different assembly code for each student), use the random c function file (randomfunctions.txt Download randomfunctions.txt) using a random number generator (https://www.google.com/search?q=random+number+generator&oq=random+numberLinks to an external site.) or by rolling a dice. Pick a function and paste it in the files (auth_overflow.c Download auth_overflow.c, auth_overflow2_fixed.c Download auth_overflow2_fixed.c, auth_overflow2.c Download auth_overflow2.c). There is a comment that highlights where to copy and paste it (if there is no comment, then do not copy and paste). You do not need to call the function in main.

Then compile using (you will need Linux either native or via WSL on Windows):


gcc -no-pie -g -fno-stack-protector -o auth_overflow auth_overflow.c
gcc -no-pie -g -fno-stack-protector -o auth_overflow2 auth_overflow2.c
gcc -no-pie -g -fno-stack-protector -o auth_overflow2_fixed auth_overflow2_fixed.c

The compiled files are built using -no-pie and -fno-stack-protector. Also, -g so that we can make debugging easier. Linux's ASLR is still enabled and for good reason, but for our purposes we can exploit this program now in a deterministic way using GDB. The first exploit is actually exploitable even without GDB just through command line.








----------------------------------------------------------------------------------------------------------------------------






# How I set it up for this project

WSL: Installing Necessary Tools such as gcc for compling gdb for debudding and python3 for the scripting in my WSL
Using script named 'exploit_1.sh' that I will attempts to overflow the buffer.
I am going to use Python to generate a long string and pass it to as an argument to the C program

After I created a script in wsl as exploit_1.sh 

![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/3e6ff594-7a8f-47dc-92fb-2773cf0b34de)

This should execute the script, and see the "Running buffer overflow test" printed in your terminal followed by any output or behavir from the 'auth_overflow' porgram, depending on how it handles the over flow



Next steps to exploit the vulnerability 

I am going to debugging with GDB


![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/083d33e3-5152-4e3e-abc4-3f3fb092aa7a)

![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/ea0a6fce-2488-4cc6-8fb0-409ef9cdd283)



![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/660d22fe-66fc-4a6a-8909-e64ab082c972)


The pointer password points to the buffer that has been overflowed with 16 'A's followed by "BBBB". This shows that the buffer overflow vulnerability. This kind of overflow can lead to various security risks including unauthorized access, execution of malicious code, and data corruption, depending on what memory areas are affected beyond the overwritten buffer.

----------------------------------------------------------------------------------------------------------

# Check auth_overflow code

![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/a5d965a1-ee87-4ff4-a6a4-503fbafb5fbc)

C program that includes a function vulnerable to a buffer overflow attack due to the unsafe use of strcpy.
To fix the vulnerability in code, replace strcpy with a safer alternative like strncpy, which allows you to specify the maximum number of characters to copy.


before
![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/1f7e85b3-bddf-4e59-8692-8227051ba759)



After 
![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/078f292e-5d90-403e-b3b1-f80d143191a0)

