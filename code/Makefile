# Copyright (c) 1992-1996 The Regents of the University of California.
# All rights reserved.  See copyright.h for copyright notice and limitation 
# of liability and disclaimer of warranty provisions.
#
# This is a GNU Makefile.  It must be used with the GNU make program.
# At UW, the GNU make program is /software/gnu/bin/make.
# In many other places it is known as "gmake".
# You may wish to include /software/gnu/bin/ early in your command
# search path, so that you will be using GNU make when you type "make".
#
# About this Makefile:
# --------------------
#
#  This Makefile is used to build the Nachos system, which includes
#   the MIPS machine simulation and a simple operating system.
#
#  There is a separate Makefile, in the "test" directory, that is
#   used to build the Nachos test programs (which run on the
#   simulated machine).
#
#  There are several "build" directories, one for each type
#   of machine in the MFCF computing environment
#   (build.solaris, build.sunos, and build.ultrix), as well
#   as a build directory for Linux (build.linux) and a generic
#   build directory (build.other) for those who wish to try
#   building Nachos on other platforms.
#
#  This Makefile appears to be located in all of the build directories.
#  If you edit it in one directory, the copies in all of the other
#  directories appear to change as well.  This is the desired behaviour,
#  since this file is machine independent.  (The file actually lives
#  in build.solaris, with symbolic links from the other build directories.)
#
#  The platform-dependent parts of make's instructions are located
#  in the file Makefile.dep.
#  There is a different Makefile.dep in each build directory.
#
#  If you are in the MFCF environment, you should not have to edit
#  the Makefile.dep files by hand.  Any changes to the make instructions
#  can be made in this file (see the instructions below) - they will
#  apply no matter where you build Nachos.
#  If you are not in the MFCF environment, e.g., if you are trying
#  to build Nachos on Linux at home, you will probably need
#  to edit Makefile.dep (in the appropriate build directory) to
#  customize the make procedure to your environment.
#
# How to build Nachos for the first time:
# ---------------------------------------
#
#   (1) Make sure than you are in the build directory for the
#	type of machine you are logged in to (the "host" machine):
#
#	host type	examples		build directory
# 	-----------	-----------		----------------
#
#	sparc/SunOS	cayley,napier,		build.sunos
#	(SunOS 4.1.3)	descartes
#
#	sparc/Solaris	picard.math,		build.solaris
#	(SunOS 5.x)	hermite.math,
#			markov.math,
#			hypatia.math,
#			hume.math
#
#	mips/ULTRIX	cantor.math		build.ultrix
#	(ULTRIX 4.2)	noether.math
#
#	If you are not sure what type of machine you are on,
#	try the command "uname -a".  
#
#   (2) Type "make depend"
#		- this computes file dependencies and records them
#			at the end of the file Makefile.dep in 
#			your build directory.  Have a look...
#
#   (3) Type "make nachos"  (or just "make").
#               - make echos the commands it is executing, so that
#			you can observe its progress.  When the
#			build is finished, you should have an
#			executable "nachos" in the build directory.
#
#   (4) There is no 4th step.  You are done.  Try running "./nachos -u".
#
#
# How to Re-build Nachos after you have changed the code:
#--------------------------------------------------------
#
#     - The Nachos source code is located in the code subdirectories:
#	  threads, userprog, filesys, network, and lib.  You may
#         change the files in any of these directories, and you can
#         add new files and/or remove files. The "machine" subdirectory
#	  contains the hardware simulation (which is also part of
#	  Nachos.  You may look at it, but
#	  you may not change it, except as noted in machine/machine.h
#     - When you want to re-make Nachos, always do it in the
#	  "build" directory that is appropriate for the machine
#	  type that you are running on.  
#	DO NOT TRY TO MAKE NACHOS IN THE SOURCE CODE DIRECTORIES.
#
#     - IF all you have done is changed C++ code in existing files
#	(since the last time you made Nachos in this build directory),
#	THEN all you need to do to re-make Nachos is to type
#
#		"make nachos"
#
#	in the build directory.
#
#     - IF you have done any of the following since the last build in
#	  this directory:
#		added new .cc files or new .h files
#		added or deleted #include's from existing files
#	THEN
#		you must do
#		"make depend"
#		followed by
#		"make nachos"
#
#	in the build directory.
#
#       Note that is is always safe to do "make depend" followed by
# 	"make nachos", so if you are not sure what changes you have
#	made, do "make depend".
#
#     - IF you have added new files (.cc or .h) since the last build,
#	you should edit this Makefile before running "make depend"
# 	and "make nachos".

#	For new .h files, simply update the appropriate "_H" list below.
#       For example, if you create a file called
#       "bigfile.h" in the filesys subdirectory, you should add
#       "../filesys/bigfile.h" to FILESYS_H, which is defined below

#	For new .cc files, update the appropriate "_C" and "_O" lists.
#       For example, if you create a file called "filetable.cc" in
#	the directory "userprog", you should add
#	"../userprog/filetable.cc" to USERPROG_C,
#       and you should add "filetable.o" to USERPROG_O.
#       Note that the entry in the "_C" list includes the subdirectory
#	name, while the entry on the "_O" list does not.
#
#  Some Important Notes:
#  ---------------------
#
#       *  You can clean up all of the .o and other files left behind
#	   by make by typeing "make clean" in the build directory.
#	*  You can clean up .o and other files, as well as the nachos
#	   executable, DISK, core, SOCKET, and other files by typing
#	   make "distclean"
#
#	These are good ways to save space, but the next build that
#	you do after cleaning will take longer than usual, since
#	much of the stuff you cleaned will need to be rebuilt.
#
#       *  When you build Nachos on an ULTRIX machine (in build.ultrix),
#	   you will get lots of warning messages like this:
#
#	openfile.o: does not have gp tables for all it's sectons
#
#	   from the loader.  Ignore them.  Or better yet, figure out
#	   how to make them go away.
#
#  The Most Important Note:
#  -----------------------
#
#	* If "make" is behaving strangely and you cannot figure out
#	why, you should REBUILD the program FROM SCRATCH.
#	Yes, it is slow.
#	But, there are lots of little things that can go wrong, especially
#	with all of these different types of machines available.
#	Rebuilding from scratch at least gives you a known starting
#	place.  To rebuild from scratch, go to the appropriate
#	build directory and do:
#	
#		make distclean
#		make depend
#		make nachos
#
################################################################
#  READ THIS:   CONFIGURING NACHOS
#
# Change DEFINES (below) to
#   DEFINES = -DUSE_TLB -DFILESYS_STUB
# if you want the simulated machine to use its TLB
#
# If you want to use the real Nachos file system (based on
# the simulated disk), rather than the stub, remove
# the -DFILESYS_STUB from DEFINES.
#
# There is a a fix to the MIPS simulator to enable it to properly
# handle unaligned data access.  This fix is enabled by the addition
# of "-DSIM_FIX" to the DEFINES.  This should be enabled by default
# and eventually will not require the symbol definition
################################################################
.PHONY: clean tags fmt ubuntu print turnin

MAKE=make --no-print-directory
LPR=lpr



CPP= gcc -E
CC = g++ -m32
LD = g++ -m32
AS = as --32
MAKE = make

PROGRAM = nachos

THREAD_H =../threads/copyright.h\
	../threads/list.h\
	../threads/scheduler.h\
	../threads/synch.h \
	../threads/synchlist.h\
	../threads/system.h\
	../threads/thread.h\
	../threads/utility.h\
	../threads/mailbox.h\
	../threads/mailboxTest.h\
	../threads/whale.h\
	../threads/whaleTest.h\
	../machine/interrupt.h\
	../machine/sysdep.h\
	../machine/stats.h\
	../machine/timer.h

THREAD_C =../threads/main.cc\
	../threads/list.cc\
	../threads/scheduler.cc\
	../threads/synch.cc \
	../threads/synchlist.cc\
	../threads/system.cc\
	../threads/thread.cc\
	../threads/utility.cc\
	../threads/threadtest.cc\
	../threads/mailbox.cc\
	../threads/mailboxTest.cc\
	../threads/whale.cc\
	../threads/whaleTest.cc\
	../machine/interrupt.cc\
	../machine/sysdep.cc\
	../machine/stats.cc\
	../machine/timer.cc

THREAD_S = ../threads/switch.s

THREAD_O =main.o list.o scheduler.o synch.o synchlist.o system.o thread.o \
	utility.o threadtest.o interrupt.o stats.o sysdep.o timer.o \
	mailbox.o mailboxTest.o whale.o whaleTest.o

USERPROG_H = ../userprog/addrspace.h\
	../userprog/bitmap.h\
	../userprog/memoryManager.h\
	../userprog/table.h\
	../userprog/synchconsole.h\
	../userprog/process.h\
	../userprog/pipe.h\
	../userprog/backingstore.h\
	../filesys/filesys.h\
	../filesys/openfile.h\
	../machine/console.h\
	../machine/machine.h\
	../machine/mipssim.h\
	../machine/translate.h
	

USERPROG_C = ../userprog/addrspace.cc\
	../userprog/bitmap.cc\
	../userprog/table.cc\
	../userprog/process.cc\
	../userprog/exception.cc\
	../userprog/progtest.cc\
	../userprog/memoryManager.cc\
	../userprog/synchconsole.cc\
	../userprog/pipe.cc\
	../userprog/backingstore.cc\
	../machine/console.cc\
	../machine/machine.cc\
	../machine/mipssim.cc\
	../machine/translate.cc
	

USERPROG_O = addrspace.o bitmap.o exception.o progtest.o console.o machine.o \
	mipssim.o translate.o memoryManager.o table.o process.o synchconsole.o\
	pipe.o backingstore.o

VM_H = 
VM_C = 
VM_O = 

FILESYS_H =../filesys/directory.h \
	../filesys/filehdr.h\
	../filesys/filesys.h \
	../filesys/openfile.h\
	../filesys/synchdisk.h\
	../machine/disk.h
FILESYS_C =../filesys/directory.cc\
	../filesys/filehdr.cc\
	../filesys/filesys.cc\
	../filesys/fstest.cc\
	../filesys/openfile.cc\
	../filesys/synchdisk.cc\
	../machine/disk.cc
FILESYS_O =directory.o filehdr.o filesys.o fstest.o openfile.o synchdisk.o\
	disk.o

NETWORK_H = ../network/post.h ../machine/network.h
NETWORK_C = ../network/nettest.cc ../network/post.cc ../machine/network.cc
NETWORK_O = nettest.o post.o network.o

S_OFILES = switch.o

OFILES = $(C_OFILES) $(S_OFILES)

all:
	@ $(MAKE) depend --no-print-directory
	@ $(MAKE) $(PROGRAM) --no-print-directory

$(PROGRAM): $(OFILES)
	@ echo $@
	@ $(LD) $(OFILES) $(LDFLAGS) -o $(PROGRAM)

$(C_OFILES): %.o:
	@ echo $<
	@ $(CC) $(CFLAGS) -c $<

switch.o: ../threads/switch.s
	@ echo $<
	@ $(CPP) -P -x c $(INCPATH) $(HOST) ../threads/switch.s > swtch.s
	@ $(AS) -o switch.o swtch.s

depend: $(CFILES) $(HFILES)
	@ echo building dependencies
	@ $(CC) $(INCPATH) $(DEFINES) $(HOST) -DCHANGED -M $(CFILES) > deps.mk
	@echo '/^# DO NOT DELETE THIS LINE/+1,$$d' >eddep
	@echo '$$r makedep' >>eddep
	@echo 'w' >>eddep
	@echo 'q' >>eddep
	ed - Makefile.dep < eddep
	rm eddep deps.mk
	@echo '# DEPENDENCIES MUST END AT END OF FILE' >> Makefile.dep
	@echo '# IF YOU PUT STUFF HERE IT WILL GO AWAY' >> Makefile.dep
	@echo '# see make depend above' >> Makefile.dep

.PHONY: fmt tags clean
fmt:
	astyle *.h *.cc -nQ

tags:
	ctags -h=.h.cc -R ..

#Ming
clean:
	rm -rf ../*~ ../*/{core,nachos,DISK,*.o,swtch.s,*~} ../test/{*.coff} ../bin/{coff2flat,disassemble,out}
