
# OC-LuaAssembler

An generic assembler made for open computers. (Made to make C-like languages or any language really for OC) Plans to port this to CC is undetermined.





# Why?

The main purpose of LA is to make a very generic x86-like assembly language that can be used to make other compilers.

For example, the main motivation of this project was to recreate C/C++ in OC, with little overhead and with efficieny. 



# How to contribute

Because the project is so early, I need to have a basic structure. Will be updated in the future





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

# TODO
* File structure
* Standard stuff(eg: registers, special registers(files, stdout, etc), etc)

# What do I need to know to contribute?

* Lua

* Basic compiler ideology

* C/C++ experience(optional)

* **Very** basic assembly knowledge

### What it will (probably) look like
#### default.lld
    ; default.lld example ;
    __MAIN: ; Declare the "main function" of the linker
    start main! ; declares the "entry point" of the program ;
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
	alloc hw, "Hello World\!"! ; Make a variable called "hw" with 13 bytes ;
	alloc inp, size(4)! ; allocate a variable with a maximum size of 4 bytes ;
	alloc int_x<int>, size(1)! ; explicitly specify variable type instead of a generic size ;
	alloc int_y<int>, size(1)!
	alloc dynamic_size, size(runtime)! ; May be removed: allows size to be set as it as a value put into it at runtime ;

    main: ; declare main entry point ;
    ; [func] [dest], [src] ;
    mov stdout, hw! ; prints "Hello World!" into stdout, minus a newline ;
    mov inp, stdin! ; get input from the user and store in inp ;
    mov stdout, create("You said: ")! ; create(generic) will make a temp var ;
    mov stdout, inp!
    push $math, int_x! ; push int_x onto the math stack ;
    mut int_y, stdout! ; Multiply int_x by int_y and print it out ;
    div int_y, stdout! ; Divide int_y by int_x and print out ;
    add int_y, stdout! ; etc ;
    sub int_y, stdout! ; etc ;
    pop $math, _GC! ; pop math stack into the garbage collector ;
    ; alternative: pop $math, garbage!, _GC will be explicit ;
    
    