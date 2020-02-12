# OC-LuaAssembler
An generic assembler made for open computers. (Made to make C-like languages or any language really for OC) Plans to port this to CC is undetermined.


# Why?
The main purpose of LA is to make a very generic x86-like assembly language that can be used to make other compilers.
For example, the main motivation of this project was to recreate C/C++ in OC, with little overhead and with efficieny. 

# How to contribute
Because the project is so early, I need to have a basic structure. Will be updated in the future

# What do I need to know to contribute?
* Lua
* Basic compiler ideology
* C/C++ experience(optional)
* **Very** basic assembly knowledge

# Roadmap
1. Make a simple foundation to interpret the language
2. Focus on efficiency
3. Define file standards(eg: libraries, should the assembler make one file as the output or multiple)
4. Language padding(Make life easier for making a language such as C/C++)
5. Is the output considered interpreted or compiled?
6. Built-in benchmark
7. Is this even a good idea? 


# Specifics
LA will have a C/C++ context(eg: header files)
Files:
* .lua = compiled "binary"
* .asm = source file
* .asmh = header/include file

# TODO
* File structure
* Elaborate on the roadmap
* Standard stuff(eg: registers, special registers(files, stdout, etc), etc)