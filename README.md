Download Link: https://assignmentchef.com/product/solved-cs342-project-3-virtual-memory-of-a-process
<br>
In this project you will write a program <strong>app</strong> and see its virtual memory (VM) usage. You will write the application using three source files (three modules): module1.c, module2.c and module3.c. The module1.c will contain the main() function. The other files (modules) will contain some other functions and variables. It is up to you what to put into those, but you should achieve the requested memory usage behavior below.




You can compile the program with such a <strong>Makefile</strong> content (you can modify):




all:   app    module1.o     module2.o     module3.o




module1.o:    module1.c            gcc    -c     -no-pie    module1.c     module2.o:    module2.c            gcc    -c     –no-pie       module2.c       module3.o:    module2.c

gcc      –c     –no-pie       module3.c




app:   module1.o     module2.o     module3.o

gcc   -g     –Wall  –no-pie       –o     app    module1.o  module2.o                            module3.o




The program will be invoked as follows:

./app




As a result of the compilation using the Makefile, you will obtain an <strong>executable</strong> [<strong>object</strong>] <strong>file</strong> (app), and three [<strong>relocabtable</strong>] <strong>object files</strong> (module1.o, module2.o, module3.o). The C compiler  (<strong>cc</strong>) compiles each source file first into a relocatable object file. Then the linker (<strong>ld</strong>) links those object files into a single executable object file (app). The <strong>–c option</strong> of gcc causes only the compiler to run to produce a relocatable object file. If we omit  the -c option, gcc runs the linker as well, after compilation, to generate an executable object file. <strong>Linking</strong> is described very nicely in Chapter 7 [C7-CSapp] of [CSapp].




The format of the object and executable files for Unix and Linux systems is called <strong>ELF</strong> (<strong>executable and linkable format</strong>) [wElf, Elf, Elf64].




You will analyze the object files  and the executable file using <strong>objdump</strong> utility. You will see the runtime virtual memory usage of the application using <strong>pmap</strong> utility. You will see the addresses of various program elements at run-time by <strong>printing the addresses</strong> (i.e., pointer variable values) to the screen. You will compare the various addresses and address ranges that you see in the executable file’s dump, in the pmap output and the output of the program.




You will put everything, the program (source codes), its various outputs, the outputs of utilities (objdump, pmap), and your descriptions and interpretations into a report (i.e. document them) that will be submitted as  a <strong>pdf</strong> file. You will also upload the program sources and a Makefile. We should be able to just type make and obtain the object files. All these files (pdf report, README, Makefile, program sources) will be put into a folder, tarred and gziped and submitted as a single file in Moodle.




Compile the program using  <strong>–no-pie</strong> option of <strong>gcc</strong> compiler driver (so that address randomization is not used).




In the application you will define some <strong>global</strong> variables, some of which are <strong>initialized</strong>, some of which are not <strong>initialized</strong>. The initialized data should be at least 128 bytes long. The unutilized data size (at run time) should be at least 16 KB long. When the program is started, it will start with such data, without heap and stack yet.




You program should run in <strong>steps</strong>. When started, it is in step 1. In each step you will do the <strong>required things</strong> described below.  Your program should wait between the steps for you  to do these things. This waiting will be achieved in the program by waiting the user to type character <strong>‘n’</strong> (meaning <strong>next</strong>). When you type ‘n’, the program will go to the next step. To cause the program to wait until you press ‘n’, you can use <strong>sprintf</strong>() or <strong>getchar</strong>() functions.   Below, we will indicate when the program should go to the <strong>next step</strong>.




&gt;&gt;&gt; <strong> At step 1</strong>, i.e., when the program is started, the program will just have initialized and uninitialized global variables allocated space in its <strong>virtual memory</strong>. OS might have also allocated some physical frames, but we are concerned in this project with the virtual memory of a program.  Your program should print out (using <strong>printf()</strong>) the addresses of all the global variables (data) and functions (code), including the main() function,  defined in your program to screen so that you see the addresses (i.e., pointer values).




In your program, you can use <strong>printf</strong> <strong>(“%lx”,</strong> (void *) p)  to print an <strong>address</strong>  (<strong>value of a pointer</strong> p) to screen in hexadecimal format (pointer value is an <strong>unsigned long integer</strong> – <strong>64</strong> bits long).




See the program output at this step. Also run the pmap utility, and see the virtual memory usage of the program in the output of the pmap utility. Save (document into report) these outputs (you will put it into your report). Compare the range of virtual memory used by data and code (text) segments of the program with the pointer values that you see on the screen. Explain and discuss. <strong> </strong>




Now look into the executable file with the <strong>objdump </strong>utility<strong>. </strong>Use the <strong>-dx</strong> option (objdump -dx app). Save the output. Find the addresses of your global variables and functions in the objdump output. Compare them with the screen output of the program and the pmaps output. Discuss.




&gt;&gt;&gt; Now look into the object files (<strong>module1.o</strong> and <strong>module2.o</strong>) using the objdump utiliy. You need to type: objdump –dx module1.o to see the binary content of <em>relocatable</em> object file module1.o. You will analyze module2.o similarly. Look to the addresses of the functions and global variables that you defined in these modules. Then look to their addresses in the executable file dump. Compare them and interpret them. What has happened. Why a module module1.o (or  module2.o) is called <strong>relocatable object module</strong>, but the module app is called <strong>executable</strong> object module?




&gt;&gt;&gt; Find some <strong>references</strong> in the code part (text segment) of the program (app) to some of those  global variables and functions. For example, call instruction makes a reference to a function. Similarly, move instruction can make a reference to a global variable. See and document what these references are (i.e., the hex values).  Find some references to the same global variables or functions in the relocatable object files (module1.o, module2.o, module3.o). Compare the references in the executable object file and in the relocatable object files. Document your findings.




&gt;&gt;&gt; Find some references in the executable object file and the in the relocatable object files  to some<strong> standard C library </strong>routines like <strong>printf</strong>(). How do they look like. Document your finding. What is the address of printf() routine at run time while your program is running? You can print out the address of printf() in your program. Not that by default, standard C library is  <strong>dynamically linked</strong> into a program. Hence <strong>shared</strong> version of the standard C library  (<strong>libc.so</strong>) is linked at run time with the program.




&gt;&gt;&gt; Learn the pid of the program app using the <strong>ps </strong>command: ps aux app.  Then change into directory <strong>/proc/</strong>pid. What do you see there? Which files are related with memory management. See their contents using the <strong>cat</strong> command: cat filename. Document your findings.




Now, go the next step. That means you type ‘n’  at the <strong>terminal</strong> the program is running,  and the program will get ‘n’ as input and proceed to the next step.




&gt;&gt;&gt; At <strong>step  2, </strong>your program will allocate memory for dynamic data (<strong>objects</strong>) using <strong>malloc</strong>(). As a result,  the <strong>heap</strong> segment of the program (in virtual memory of the program) will grow to store these.  The program will <strong>dynamically allocate</strong> memory with multiple malloc() calls so that the total dynamically allocated (virtual) memory for the program is at least 2 MB (i.e., heap size become at least 2MB).  The program will print the addresses of some dynamically created objects to screen. Addresses are pointer values of the pointers pointing to dynamically allocated space (i.e., to dynamically created objects). A pointer is 8 bytes long in a 64 bit machine.




Look to the output of pmap again (run pmap again).




Compare the output of pmap with the output of your app program.




&gt;&gt;&gt; Find out the <strong>physical memory</strong> usage of your program at this moment by using tools such as <strong>ps</strong>, <strong>top</strong>, <strong>vmstat</strong>, or <strong>cat /proc/pid/meminfo</strong> (pid is the process id of the process). Compare the physical memory usage with virtual memory  usage. VM usage can be found from the <strong>pmap</strong> output.




Now, go to the next step (i.e., type ‘n’).




&gt;&gt;&gt; At <strong>step</strong> <strong>3</strong>, your program  will grow the <strong>stack</strong> by calling a recursive function many times. That function can be in any one of your modules. Increase in the stack should be at least 100 KB. Program will print the addresses of some local variables (pointer values) to the screen to see what they are.




Look to the pmap output again (run pmap again). Compare the pointer values (addresses) with the stack range in the pmap ouput (note that pmap has several options; you can use these options to get more info in the output).




Go to the next step.




&gt;&gt;&gt; At <strong>step 4</strong>, the program will use <strong>mmap</strong>() system call to map a file of size at least 1 MB into the virtual memory of the process. Start address where the file is mapped will be printed out.




Look to the start address that is printed out. Run pmap again and see its output. Compare them. Which part  in the pmap output is for memory mapped file?




Look to the physical memory usage of the program. Compare with virtual memory usage. Document it.




Go to the next step.




&gt;&gt; At <strong>step 5</strong>, program will read/write some section of the mapped memory region

(i.e., file).

See the physical memory usage at this time. Document it.




&gt;&gt;&gt; At this step, also look to the output of the pmap and see the segments (virtual segments) of the program. Try to explain what each is containing.  Find the total data segment size (globals + heap) in number of pages. Find the total code size in number of pages. Learn the page size using a <strong>getconf</strong> PAGESIZE command. Do the same for other segments. Document your findings.




&gt;&gt;&gt; At <strong>step 6</strong>, your program will  print the <strong>content of the page</strong> corresponding to the main function address.




Compare the output with the objdump output (main function there). Document and discuss.




Stop the program.




It is totally up to you what to put into your program to see all these. You can define some functions and (global) variables in module1.c, module2.c, and module3.c. There is no restriction in this part. If programs are written independently, there is no chance that they will be similar more than the acceptable threshold.




&gt;&gt;&gt;  Look to the size of the program on disk.  Now compile the program this time using <strong>–static</strong> option of the <strong>gcc</strong> compiler driver. Look to the size of the program again. Do objdump –dx on the executable file. What do you see. Compare with the objdump output of the program without getting compiled with static option. Document your findings.




Include all outputs and your explanations, discussions into your report.


