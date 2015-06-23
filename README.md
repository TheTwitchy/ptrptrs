# Pointer Pointers
A practical C/C++ pointer reference for anyone who has ever had to deal with this crap.

## No seriously, what is it?
Long story short, in my early days of C coding, back when C was already considered a language so old that my grandfather would beat me with a cane for not using an interpreted language, I struggled to figure out some of the practical aspects of pointers. On top of that, there were never any really good sites for figuring any of this out, so in a moment born of frustration and probably alcohol, I decide to write a practical reference for deciphering other people's code, and to save time by not having to try out every possible permutation of pointer symbols while writing my own. The original was lost somewhere while moving jobs and states in the last few years, but here's what I can remember from that version.

I realize by now that anyone who finds this probably isn't writing enterprise software in C/C++ (and God help you if you are), but with the recent influx of DIYers and people looking to code for microcontrollers and embedded processors, C looks to be making a comeback in a small way with a lot of people who may not have traditional developer experience. If you have a basic understanding of C, you'll be fine.

Hopefully it's half as helpful for you as it was for me.

## Intro
Pointers are essentially a way to allocate space for a variable that will not hold the end value, but will instead hold a location in memory that has some other purpose. For example, lets say you want a variable to hold the number of limbs you currently own:

```C
int myLimbs = 3;
```

Lets set it to three, because we're all about diversity here. In most computing architectures, this allocates 32 bits of memory space to hold the value (the fairly standard size of an int).

Now, lets allocate a pointer.

```C
int * ptrMyToes = malloc(sizeof(int));
*ptrMyToes = 11;
```

For the sake of learning, hazard a guess at how many bits of memory ptrMyToes allocates.

The correct answer is "it depends". Specifically, it depends on the architecture, and even more specifically, it depends on how big your memory space is. For the x86 architecture, this is going to be 32 bits, and for a 64 bit architecture like x86_64 (which you're probably using right now), it's going to be 64 bits. Basically it's however big it takes to address all of the memory available to a given computer. For some embedded processors and microcontrollers, this could be 16 or 8 bits. Read the manual if you need to know for some given computer.

So after malloc allocates some space on the heap, and returns a pointer to ptrMyToes, we see that ptrMyToes is dereferenced to set some space in memory to 11. This means that the '*' operator will treat whatever variable it's applied to as a memory address, and instead of directly setting (or getting) that variable, it sets (or gets) the memory at the address that variable is pointing to.

If you've ever come across the phrase "dereferencing a null pointer", it essentially means that you get the end value of a pointer that is pointing at NULL or 0, which in turn generally causes a segmentation fault.

The inverse of dereferencing a pointer is called getting the address of a variable, which just means you get the address of where the variable currently resides in memory. 

```C
int n = 1;
printf("%d\n", &n);
```
So if you run the program above, as is, you'll probably get some weird number as output that probably isn't anywhere close to 1. This is because it's actually trying to print out the memory address of n, not the value. A slightly more bearable way of viewing a memory address is
``C
int n = 1;
printf("%08x\n", &n);
``
which prints the address out as a hexadecimal number, which is normally how memory addresses are formatted.

##Arrays
Coming soon.

##Structs
Coming soon.

##Pointer Gymnastics
Coming soon.

##Reference
The juicy parts for people who don't have the time to deal with my shenanigans. Assume x86 unless otherwise specified.

###The Basics
```C 
int * n;
char * c = malloc(sizeof(char));
double * d = 0xdeadb33f;
```

###Dereferencing Pointers
```C
int * n = malloc(sizeof(int));
*n = 100;
printf("%d\n", *n);
```
Output
```bash
100
```

Essentially, this returns whatever is at the memory pointed to by the pointer being dereferenced.

###Pointers and Arrays
```C
int n[2];
n[0] = 100;
n[1] = 200;
printf("n[0]=%d   n[1]=%d\n", n[0], n[1]);
printf("0x%08x\n", n);
```

Output:
```bash
n[0]=100   n[1]=200
0x501dfbd0 #This number will likely be different, but notice it's a memory location.
```

Array variable are nothing but pointers.

###Pointers and Structs
```C
typedef struct {
  int id;
}
demo_t;
  
demo_t x;
x.id = 1;
printf("x id=%d\n", x.id);
  
demo_t * y = malloc(sizeof(demo_t));
(*y).id = 2;
printf("y id=%d\n", y->id);
```
Output:
```bash
x id=1
y id=2
```

In structs and unions, this:
```C
y->id
```
is equivalent to:
```C
(*y).id
```
although the former is the shorter and much more common way to dereference a member.

###Address Operator
```C
int n = 100;
printf("0x%08x\n", &n);
```

Output:
```bash
0xe9f8a84c #This number will likely be different, but notice it's a memory location.
```

The address operator will return the address of the variable it's applied to. Sort of the reverse of dereferencing a pointer.

##Outro
For the sake of not spreading blatant lies, I've checked all code given herein, but if you notice any misspellings, see a technical error, have a question, or know any really funny politically incorrect jokes, feel free to drop me a PM through GitHub. 

That's all folks!
