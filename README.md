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


exploit the vulnerability 

I am going to debugging with GDB

![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/05a2abea-68a1-47b4-829e-4decca93be17)

The first breakpoint is before the strcpy() happens. By examining the password_buffer pointer, the debugger shows it is filled with random uninitialized data and is located at  0xbffff7a0 in memory By examining the address of the auth_flag variable, we can see both its location at 0xbffff7bc and its value of 0. The print command can be used to do arithmetic and shows that auth_flag is 28 bytes past the start of password_buffer. This relationship can also be seen in a block of memory starting at password_buffer. The location of auth_flag is shown in bold.

![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/a3eb7620-b54b-4076-918f-df12a5d5dc4f)


![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/e582ff07-6459-4f39-8926-436d66306b9a)


Continuing to the next breakpoint found after the strcpy(), these memory locations are examined again. The password_buffer overflowed into the auth_flag, changing its first two bytes to 0x41. The value of 0x00004141 might look backward again, but remember that x86 has little-endian architecture, so it's supposed to look that way. If you examine each of these four bytes individually, you can see how the memory is actually laid out. Ultimately, the program will treat this value as an integer, with a value of 16705.



----------------------------------------------------------------------------------------------------------

# Check auth_overflow code

![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/a5d965a1-ee87-4ff4-a6a4-503fbafb5fbc)

C program that includes a function vulnerable to a buffer overflow attack due to the unsafe use of strcpy.
To fix the vulnerability in code, replace strcpy with a safer alternative like strncpy, which allows you to specify the maximum number of characters to copy.


before
![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/f9686746-46ae-4b53-bd5d-7ff3f8281aaf)




After 
![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/078f292e-5d90-403e-b3b1-f80d143191a0)


----------------------------------------------------------------------------------------------------------

# Auth Overflow 2

![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/bf90515d-30f6-4a61-8b13-49d0822dda43)


Exploit_2.sh
![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/51a6ffd2-7193-42b1-8ea6-059058d975c3)

### Analyzing the GDB Output

![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/32ab4a1f-4623-48ff-8db1-f1c5d1d34c7b)

buffer might be stored near the base pointer ($bp), rather than the stack pointer, given typical function call conventions. Let's examine memory around the base pointer and look for our sequence of 'A's

Similar breakpoints are set, and an examination of memory shows that auth_flag is located before password_buffer in memory. This means auth_flag can never be overwritten by an overflow in password_buffer.

Examine Memory: The command x/40x $rbp will display the memory around the base pointer. Look for a sequence of 0x41414141 which represents 'AAAA'. This will help us identify the start of your buffer.

![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/a0a8aeb3-3c71-402d-ae56-b785be30290a)

![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/5545e821-be95-489a-9521-2cd845dbca87)


As expected, the overflow cannot disturb the auth_flag variable, since it's located before the buffer. But another execution control point does exist, even though you can't see it in the C code. It's conveniently located after all the stack variables, so it can easily be overwritten. This memory is integral to the operation of all programs, so it exists in all programs, and when it's overwritten, it usually results in a program crash.


![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/f2538cdc-2c70-4585-8bef-299eeca7842b)


![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/553e00c8-0fdf-4f82-ac62-2c466e7423b9)






### How this can be Exploit???
The current payload is likely not long enough to reach and overwrite the return address on the stack. The strcpy function does not check the length of the destination buffer (password_buffer), so if the password argument passed to check_authentication is too long, it could overflow password_buffer, leading to unintended behavior, including overwriting critical data on the stack, such as the return address.
![White-Cat-What-meme-1](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/7f16619b-0eab-4a3d-b0c8-3a87066817c7)

### What does that mean?

there's an issue with the payload length in a software exploitation context, likely in the context of a buffer overflow attack. When exploiting a buffer overflow vulnerability, attackers typically attempt to overwrite the return address on the stack with a pointer to their malicious payload.


![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/0f8fa5e4-ec4f-4cf4-ad27-5e947290d283)


### Let's fix the code!

Before code


![image](https://github.com/peterisOnIT/Stackbufferoverflow/assets/117600297/dfdbb6e6-3e24-4b2e-ba99-7f3c5be7f834)








