# Pointer Pointers
A practical C/C++ pointer reference for anyone who has ever had to deal with this crap.

## No seriously, what is it?
Long story short, in my early days of C coding, back when C was already considered a language so old that my grandfather would beat me with a cane for not using an interpreted language, I struggled to figure out some of the practical aspects of pointers. On top of that, there were never any really good sites for figuring any of this out, so in a moment born of frustration and probably alcohol, I decide to write a practical reference for deciphering other people's code, and to save time by not having to try out every possible permutation of pointer symbols while writing my own. The original was lost somewhere while moving jobs and states in the last few years, but here's what I can remember from that version.

I realize by now that anyone who finds this probably isn't writing enterprise software in C/C++ (and God help you if you are), but with a recent giant influx of DIYers and people looking to code for microcontrollers and embedded processors, C looks to be making a comeback in a small way with a lot of people who may not have traditional developer experience.

Hopefully it's half as helpful for you as it was for me.

## Intro
Pointers are essentially a way to allocate space for a variable that will not hold the value, but will instead hold a location in memory that has some other purpose. For example, lets say you want a variable to hold the number of limbs you currently own:

```C
int myLimbs = 3;
```

Lets set it to three, because we're all about diversity here. In most computing architectures, this allocates 32 bits of memory space to hold the value. That's 4 bytes for you math wizards.

Lets allocate a pointer.

```C
int* ptrMyLimbs;
```

So we haven't actually set this to anything yet, but for the learning, hazard a guess at how many bytes that allocates.

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
