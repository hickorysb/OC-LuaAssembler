
# OC-LuaAssembler

A generic assembler made for open computers. (Made to make C-like languages or really any language for OpenComputers) Plans to port this to ComputerCraft are undetermined.





# Why?

The main purpose of LA is to make a very generic x86-like assembly language that can be used to make other compilers.

For example, the main motivation of this project was to recreate C/C++ in OC, with little overhead and efficieny. 



# How to contribute

Because the project is so early, I need to have a basic structure. More information on contribution will be added in the future





# Roadmap

1. Have an idea of what the language will look & work like

2. Make a simple foundation to interpret the language

3. Focus on efficiency

4. Define file standards(eg: libraries, should the assembler make one file as the output or multiple)

5. Language padding(Make life easier for making a language such as C/C++)



# Specifics
LA will have a C/C++ context(eg: header files)
Files:
* .lua = compiled "binary"
* .asm = source file
* .asmh = header/include file

LA will have two modes of operation:
1. Interpreted, this version will be updated more frequetly with new features due to it's simplicity.
2. A "Compiled" version that will be assembly converted directly into lua for little to no overhead. Due to the more complex nature of compiling to directly to Lua, the compiled version of the language will lag behind the interpreted version in the realm of features. We will make an attempt to make bug fixes available for both versions as quickly as possible.

# TODO
* File structure
* Standard stuff(eg: registers, special registers(files, stdout, etc), etc)

# What do I need to know to contribute?

* Lua

* Basic compiler ideology

* C/C++ experience(optional)

* **Very** basic assembly knowledge

### What it will (probably) look like
#### Instruction Ruleset
    instructions that don't use a register or stack by default will be called like so:
    [instruction] [destination], [source]
    And instructions that use a register/stack by default(such as the %math stack & instructions):
    [instruction] [source], [destination]
    This will allow for easy categorization and simplifacation, eg: "add 5 and %math together, and put the result here"
    
#### Stacks / Registers
    To modify a stack, use the % modifier
    To modify a stack element you use the $ modifier
    For example:
    pop $math, garbage! will put the top most element into the garbage collector
    pop %math, garbage! will put the math stack into the garbage collector(including all elements in the stack) and forcefully reinit the stack

#### Memory Management
    Whenever you create a variable you are forced to give it a set size in bytes(unable to allocate a specific number of bits)
    alloc myVariable, size(2)! ; two byte variable ;
    If the size of the variable(somehow) exceeds this, OCLA will throw an error. Otherwise it will simply overflow the variable.
    eg:
    1111 1111 ; trying to force this to become 2 bytes will result in: ;
    0000 0000
    
    **Unsigned and signed variables will not be supported by default. The only non-int variable format supported will be strings for convenience. If you want unsigned integers, floats, or doubles, either you or a compiler will implement it yourself**  
    
#### Comments
    Comments are simply made with two semi colons, and are completely ignored by the assembly
    ;comment;
    The spaces in the example code are what I prefer
    
#### Registers
    Registers will be the main way to interact with components and hardware in your computer. If you want to directly access screen memory you can with %screen and its counter parts
    eg(writing to the screen instead of using mov stdout, 3)
    
    mov %screen_x, 5!
    mov %screen_y, 2! ; This may change so that you can access directly into the screen register without using 3 calls ;
    mov %screen, "3"!
    
    This would make the 5th character on the second row "3"
    
    Calling functions for specific components however will most likely be implemented with syscalls using arguments provided in %arg
    eg:
    
    syscall [number for redstone getInput function], [variable to store the output into]
    If the component is not available it will return zero, with syscalls to check if the device exists


#### etc details
    pop %stack, garbage will automatically turn into pop %stack, _GC to clear out said stack
    syscall 0x0 will automatically turn into syscall 0x0, _GC (This way you can call a function that doesn't return anything, or you just don't want the output), otherwise output will be redirected to the appropriate

# What it looks like in theory

#### default.lld
    ; default.lld example ;
    __MAIN: ; Declare the "main function" of the linker
    start init! ; declares the "entry point" of the program ;
    GLOBAL DEFAULT_LINKER! ; declares a "macro" or global variable that will be pre defined ;
    set OPTIMIZATION 0! ; Optimizations for higher level languages, assume none for default ;
    
    INFO:
    ; Contain information related to the software ;
    ; eg: set LANGUAGE "C" ;
    set LANGUAGE "default"!
    set VERSION "0.0"!
    set VERSION_ASM_NUM_MAJOR 0!! ; !! has the same effect as GLOBAL, specifically for variables ;
    set VERSION_ASM_NUM_MINOR 0!! ; eg: printf("version major: %d", VERSION_ASM_NUM_MAJOR)\; in C ;
    
    CALLS: ; Allow the user to make aliases or overrides for default system calls ; 
    (0x0)[shell] : clear! ; syscall 0 will call program clear via the shell ;
    (0x1)[lua, 1] : clear_lua! ; directly run 1 line after clear_lua as lua ;
    (0x2)[lua, file] : clear.lua! ; Run a lua file with this filename, check path for this file etc ;
    ; etc default syscalls (Will be in docs) ;
    clear_lua:
    gpu.fill(1, 1, gpu.getResolution(), " ")
    
    RET SUCCESS!

#### hello_world.lasm
    init:
	alloc hw<string>, "Hello World\!"! ; Make a variable called "hw" with 13 bytes ;
	alloc inp, size(4)! ; allocate a variable with a maximum size of 4 bytes ;
	alloc int_x<int>, size(1)! ; explicitly specify variable type instead of a generic size ;
	alloc int_y<int>, size(1)!
	alloc dynamic_size, size(runtime)! ; May be removed: allows size to be set as it as a value put into it at runtime ;
	call main! ; calls main function using the $arg stack for arguments

    main: ; declare main entry point ;
    alloc argv, size(runtime)!
    alloc argc, size(1)!
    pop $arg, argv!
    pop $arg, argc!
    pop %arg, garbage! ; garbage collect everything in $arg stack :
    
    ; print arguments in argv based off of argc ;
    
    
    
    
    mov stdout, hw! ; prints "Hello World!" into stdout, minus a newline ;
    mov inp, stdin! ; get input from the user and store in inp ;
    mov stdout, create("You said: ")! ; create(generic) will make a temp var, will most likely be size(runtime) ;
    mov stdout, inp!
    
    ; basic math ;
    push $math, int_x! ; push int_x onto the math stack ;
    mut int_y, stdout! ; Multiply int_x by int_y and print it out ;
    div int_y, stdout! ; Divide int_y by int_x and print out ;
    add int_y, stdout! ; etc ;
    sub int_y, stdout! ; etc ;
    
    syscall 0x0! ; Calls function 0x0 defined in default.lld, which clears the screen ; 
    
    pop %math, _GC! ; pop math stack into the garbage collector ;
    ; alternative: pop %math, garbage!, _GC will be explicit ;
    
    
