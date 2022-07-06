# Buffer-Overflow-Attacks

For this assignment I completed 4 different buffer overflow attacks, increasing in difficulty as they go along. I created a virtual machine in VirtualBox running a prebuilt Ubuntu 16.04 image from SEED labs. The virutal machine image, provided files, and resources are at this link: https://seedsecuritylabs.org/Labs_16.04/Software/Buffer_Overflow/

# Attack 1
The first attack is simply running a program that launches a shell by executing shellcode stored in a buffer. The program creates an array filled with shell code, then creates a buffer and uses strcpy to copy the shellcode into the buffer. I turned off address space randomization and linked the /bin/sh shell to the /bin/zsh shell to disable some countermeasures. I compiled the program, setting the flag that allows code to be executed from the stack. I set the ownership of the program to root and changed the permissions of the program to enable the Set-UID bit. Finally, I ran the program and obtained a root shell.

<img width="970" alt="task1" src="https://user-images.githubusercontent.com/67126494/177637023-ebbb242a-6cd8-4250-aff1-69edd0888dd3.png">

# Attack 2
In the second attack I exploited a buffer overflow vulnerability in a vulnerable program. I compiled the vulnerable program, set the flag to allow executable code in the stack, and turned off StackGuard protection. I then made the program root owned and set the Set-UID bit. I found the address and offset values to complete the exploit program through debugging the vulnerable program using gdb. 

<img width="1115" alt="task2 a" src="https://user-images.githubusercontent.com/67126494/177641627-4c3a4317-805a-4097-a12a-19c415cf2fd1.png">


From the debugging results, I found the frame pointer’s value to be 0xbfffe9d8. From this, I know the return address is stored at that value plus 4, and the first value that the program could jump to is that value plus 8. The difference between the frame pointer and start of the buffer is 74, and because the return address is 4 bytes above where the frame pointer points to, the distance is 78. I set the offset to 78 so that the return address would be put in the right place and set the return address to the value of the frame pointer plus 110, which is where the program will jump to.

<img width="509" alt="exploit code" src="https://user-images.githubusercontent.com/67126494/177641704-4346bcac-eec2-4727-ba42-ee028c64b6f5.png">

After completing the exploit program I ran it and the vulnerable program and was able to obtain a root shell.

<img width="1189" alt="task2b" src="https://user-images.githubusercontent.com/67126494/177641738-d2299e9b-a988-4479-b608-e3c6108293b4.png">

# Attack 3
Attack 3 defeated the dash shells protection against the effective UID and real UID not being equal.  I added extra shellcode to call setuid(0) in the exploit program to defreat this protection. 

<img width="353" alt="shell code" src="https://user-images.githubusercontent.com/67126494/177642889-f90eeb94-dcac-4f7a-a2f7-050bdac60580.png">

I compiled the vulnerable program, turning off StackGuard and making the stack executable, set the program as root owned, and enabled the Set-UID bit. I then ran the exploit program and the vulnerable program, and was able to get a root shell.

<img width="1189" alt="task3b" src="https://user-images.githubusercontent.com/67126494/177643055-0c5dcfb2-2d09-4bf1-b572-00310489a0a8.png">


# Attack 4
Attack 4 defeats address randomization. Because I was using a 32-bit Linux machine where the stack only has 19 bits of entropy, a brute force approach was possible. I compiled the vulnerable program, turning off StackGuard and making the stack executable. I set the vulnerable program to be owned by root and enabled the Set-UID bit. I ran the exploit program and tried running the vulnerable program but received a segmentation fault because the addresses were not correct with the randomization. I then ran the script provided in the lab to brute force the attack and continually run the vulnerable program until the addresses worked, and was able to get a root shell.

<img width="969" alt="task4a" src="https://user-images.githubusercontent.com/67126494/177643515-09629633-d04d-484d-a0a3-23d1f6bdfe93.png">

<img width="1193" alt="task4b" src="https://user-images.githubusercontent.com/67126494/177643548-6b7e654d-de02-40a5-99de-41299be877bb.png">



