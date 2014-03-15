A simple virtual machine project.
It contains a dalvik virtual machine and a java virtual machine.
Work environment: ubuntu 12.04 64bits.


How to run it?
==============

	1. Build and verify
	-------------------
		$ make
		$ make check

	2. A generated file "output-dvm" will stores the execution results.
	-------------------------------------------------------------------
		$ cat output-dvm


Use simple dvm/jvm like a benchmark tool
========================================

	1. Write a test program
	-----------------------
		use java.lang.System.currentTimeMillis() to calculate the execution time of the program. You can find tests/Main.java as an example.

	2. Compile the program
	----------------------
		compile the program to .class format for jvm and .dex format for dvm.
		$ javac tests/Main.java
		$ dx --dex --output=test/Main.dex tests/Main.class
	
	3. Run it with dvm/jvm
	----------------------
		$ ./simple_dvm/dvm tests/Main.dex
		$ ./simple_jvm/jvm tests/Main.class

	4. The output will show the execution time with different virtual machine
	-------------------------------------------------------------------------
		% execution time = 17457ms. count = 100000000


Use Google performance tool to analysis the performance of virtual machine
==========================================================================

	1. Install the google performance tool
	--------------------------------------
		you can reference the instruction from http://code.google.com/p/gperftools/. Here is the instructions I used
		$ ./configure --enable-frame-pointers
		$ make
		$ make install

	2. Add -lprofiler to the link-time step for your executable.
	------------------------------------------------------------
		add -lprofiler to LDFLAGS in simple_dvm/Makefile

	3. Add ProfilerStart() and ProfilerStop() to source code
	--------------------------------------------------------
		These functions are declared in <gperftools/profiler.h>.ProfilerStart() will take the profile-filename as an argument.

	4. Rebuild the dvm/jvm
	----------------------
		$ make

	5. Run your program with profiler
	---------------------------------
		$ env CPUPROFILE=/tmp/prof.out <PATH_TO_YOUR_PROGRAM> [PROGRAM args]

	6. Use pprof to visualize the output
	------------------------------------
		$ pprof <PATH_TO_YOUR_PROGRAM> /tmp/prof.out
		The output format information is in http://gperftools.googlecode.com/svn/trunk/doc/cpuprofile.html

