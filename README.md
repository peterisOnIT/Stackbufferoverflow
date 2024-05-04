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

![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/d17c4e24-c163-42e7-8e50-72dc6d3e031a)






