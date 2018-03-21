## a quick RE write up from Angstrom CTF because @kieczkowska made me and also to test my understanding of what is going on

### Summary

This is a 80 point challenge from [AngstromCTF](https://angstromctf.com/). You could download the file from here. 
![Image](https://github.com/helenalucas96/helenalucas96.github.io/blob/master/rev2Screen.PNG)
I will be using Hexray's [IDA free](https://www.hex-rays.com/products/ida/support/download_freeware.shtml). It's free :)

After dowloading the file and dropping it into my Kali vm, running "file" on it shows us that it'a a 32-bit ELF. 
Ok sooo IDA time >:)

I loaded the file into IDA and took  look at the structure. 
We can immediately see that we have some kind of checks going on and various end outputs before the program finishes. 
![Image](helenalucas96.github.io/graphStruct.PNG)



We can also see that there are two levels, because we can see the strings that are printed in various steps. 
![Image](helenalucas96.github.io/level1&2.PNG)

Let's focus on level 1 first. 
![Image](helenalucas96.github.io/Level1.PNG)

We see _printf called (this prints the first prompt for the level 1 number) and then the program must have some way of getting our input. 
It's easy enough to conclude that this is done when ___isoc99_scanf is called (scanf is the important bit here. I assume the ___isoc99_ part is due to compiling with C89 gcc standards, but it doesn't really matter anyway).
So where does the input go? Where is it compared? 

```
call ___isoc99_scanf
add  esp, 10h
mov  eax, [ebp+var_1C]
cmp  eax, 11D7h
jz ......
```

In this bit of code we can see that we put [ebp_var1C] into eax and then compare eax to 11D7h 
(that's in hex, we can easily convert it to decimal by right clicking on the value in IDA)
![Image](helenalucas96.github.io/hextodec.png)

Bingo, level 1 solved. Let's run the program with the new info, just to check. 

![Image](helenalucas96.github.io/level1Complete.PNG)

tbc

thx to @enusecdan 
