This project was created to provide an interpreter in Java language so I ported Ben Olstead's original Malbolge
interpreter written in C (available here: http://web.archive.org/web/20000815230017/http:/www.mines.edu/students/b/bolmstea/malbolge/.
).

- I wanted to evaluate how really difficult is to write a Malbolge program. So I extended the functionality of the
interpreter. 
- I added some runtime debugging to make it easier to understand what is going on in the heart of the
virtual machine. The debugger prints assembly kind of code and register values runtime.
- I found some bugs in the original interpreter and made workarounds in the Java version. It was still
extremely complex to write any useful Malbolge program.
- I experimented how to separate data and code area to remove a huge amount of complexity. First I examined other
available malbolge programs, the 'Hello World!'s (I used the runtime debugger). These programs also try to separate
code and data by updating the data register (D) as the very first instruction. However, this has a limitation: the
program cannot be long enough because the code area is limited from address 0 to address 40.
- I realised that it is more effective to manipulate the code register so there would be enough data area in the
front of the program and the code are still can be arbitrarily long.
- I also made an assembler which assembles binary Malbolge program from the language I named 'Malbolge Assembly'. This
makes programming easier because the program lines make sense (instead of the meaningless binary characters)
- This removed enough complexity that now I could write Malbolge programs which actually make sense. Here is one which
I wrote less than an hour:

```
//
// Sample Malbolge Program displays my email address.
//
// author: Miklos Sagi, 20th May, 2015
//

// The very first thing to be done is to separate code and data 'segments'.

MOV C, [D] //this jumps to address 98 which is one byte in front of the 'new code
           //segment' start

// The following operations up until address 98 could be misleading. They are not real
// operations just data used to reach the desired value in the A register.

ROTR [D]; MOV A, [D] // *
MOV C, [D] // i
NOP // o
NOP // o
EXIT // v
NOP // o
NOP // o
NOP // o
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
ROTR [D]; MOV A, [D] // *
NOP // o
EXIT // v
EXIT // v
NOP // o
NOP // o
IN A // /
NOP // o
MOV C, [D] // i
NOP // o
NOP // o
CRZ A, [D]; MOV A, [D] // p
ROTR [D]; MOV A, [D] // *
OUT A // <
NOP // o
CRZ A, [D]; MOV A, [D] // p
MOV C, [D] // i
ROTR [D]; MOV A, [D] // *
NOP // o
EXIT // v
MOV D, [D] // j
NOP // o
ROTR [D]; MOV A, [D] // *
MOV C, [D] // i
NOP // o
OUT A // <
MOV C, [D] // i
NOP // o
IN A // /
EXIT // v
NOP // o
OUT A // <
OUT A // <
NOP // o
NOP // o
NOP // o
NOP // o
EXIT // v
IN A // /
MOV C, [D] // i
NOP // o
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
NOP // o
NOP // o
NOP // o
MOV C, [D] // i
MOV D, [D] // j
EXIT // v
NOP // o
IN A // /
ROTR [D]; MOV A, [D] // *
NOP // o
MOV C, [D] //i
ROTR [D]; MOV A, [D] // *
MOV C, [D] // i
NOP // o
CRZ A, [D]; MOV A, [D] // p
EXIT // v
NOP // o
NOP // o
NOP // o
ROTR [D]; MOV A, [D] // *
ROTR [D]; MOV A, [D] // *
NOP // o
IN A // /
IN A // /
NOP // o
IN A // /
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
NOP // o
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused
NOP // o padding: unused

//address 98, the very first jump operation lands here, encrypts this byte and
//      moves on to the next op which is the 
EXIT // v
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
NOP // o padding: this byte is unused: the desired byte is unreachable from this address
ROTR [D]; MOV A, [D] // *
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
OUT A // <
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
OUT A // <
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
ROTR [D]; MOV A, [D] // *
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
OUT A // <
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
OUT A // <
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
OUT A // <
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
OUT A // <
NOP // o padding: this byte is unused: the desired byte is unreachable from this address
NOP // o padding: this byte is unused: the desired byte is unreachable from this address
NOP // o padding: this byte is unused: the desired byte is unreachable from this address
ROTR [D]; MOV A, [D] // *
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
OUT A // <
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
OUT A // <
NOP // o padding: this byte is unused: the desired byte is unreachable from this address
NOP // o padding: this byte is unused: the desired byte is unreachable from this address
ROTR [D]; MOV A, [D] // *
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
OUT A // <
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
OUT A // <
NOP // o padding: this byte is unused: the desired byte is unreachable from this address
NOP // o padding: this byte is unused: the desired byte is unreachable from this address
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
ROTR [D]; MOV A, [D] // *
CRZ A, [D]; MOV A, [D] // p
CRZ A, [D]; MOV A, [D] // p
OUT A // <
EXIT
```

This Malbolge Assembly program was assembled using the Assembler I wrote to this Malbolge binary:

```
b&`A@L>=<54z8DC54d2O0/(nK+$Gi'3g
$d@!?=|N)y98vuts!D0ohglkj)Ltf8Gc
"D~_Xj\[Z<;W)(T&LKPONMLKJIHGFEDC
BAM987[54X8xw/S-,P*)Mn&Jk#"Fgf|B
cy?wv<]s9qp6Wm3qpoQPf,Mc)gfHG]#[
Z~XWVz=SwWVONrLKo2HGkX
```