## Terminal
Terminal is the most basic visual user interface for a kernel, showing characters on a text console straight from code. There is no stdin or stdout, only a console which shows what is output to its memory location. Even for input, it has to be taken from serial ports/USB from the keyboard and depending on your code, it might be shown on the terminal
## Colors
There are only 8 colors for which there are lighter versions, totaling to 16 colors for text.
![[Pasted image 20260816190517.png]]
## Input
For input,  it has to be taken from the serial ports either via IRQ or straight read from it. There doesn't exist any `stdin` as we haven't created it.