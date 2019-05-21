Makefile extension (make.inc)
=============================

Simple Makefile extension to compile C sources.

'make.inc' is an include file for your source Makefile which already contains compilation methods for a C binary, library and
archive files including dependency tracking and hardened gcc cflags.

This helps developers easily define what to compile and helps easily generate binaries, shared libraries (.so), archives (.a) etc.
with simple definitions. make.inc automatically tracks dependencies, compiles binaries and libraries with advanced compiler flags
and defines own clean program. The program uses standard GCC compatible compiler flags will not override existing CFLAGS and LDFLAGS,
but make use of it and extends to add your custom flags so that make.inc goes well with any buildsystem.

Use make.inc in your Makefile by adding 'include make.inc' in the end of source Makefile.

Syntax to define compile targets:-
Assign your binaries and libraries to 'bins :=' macro.

Example:-
    bins := bin1 bin2 .. binN libXX1.so libXX2.so .. libXXN.so
Order above based on dependency. Example if binary 'abc' have a dependency on 'libXYZ.so', then
order like:-
    bins := libXYZ.so abc

Also supports options to pass additional sources (objs), cflags and ldflag for the binary/library.
Example:-
   bins := abc libXXN.so
   abc_sources := a.c b.c c.c d.c
   abc_cflags := -I./include/ -O2 -g
   
   abc_ldflags := -ldl -lncurses
   libXXN.so_sources := x.c y.c z.c
   libXXN.so_cflags := -DDEBUG -I./include/
   libXXN.so_ldflags := -L./xyz/ -ldl

make.inc additionally adds strict Werror flags via 'opt_flags'. You can either override them by
redefining opt_flags:= variable or remove unneeded flags by passing them to opt_no_flags:= in your Makefile.


Sample Makefile
===============

PKG_NAME := myapp

CFLAGS := -DAPP_DEBUG

bins := libs/libmyapp.so myapp libs/libmyapp.so libs/libtest.a testapp

myapp_sources := myapp.c supporting.c
myapp_cflags := -DFEATURE_TEST -O2
myapp_ldflags := -ldl -lpthread -lmyapp

libmyapp.so_sources := myappext.c libtest/user.c pluggins/*.c
libmyapp.so_cflags := -DFEATURE_LOGGING
libmyapp.so_ldflags := -lcap

libtest.a_sources := test.c testext/hello.c

include make.inc


Commands supported
==================
	make
	make clean
	make dist    (creates tarball if PKG_NAME and PKG_VERSION is defined or a VERSION file is found)

