# makefile discription.
# basic build file for mruby library (Ruby part)

# project-specific macros
# extension of the executable-file is modifiable(.exe .out ...)
BASEDIR = .
TARGET := mrbtest
LIBR := ../lib/libmruby.a
MLIB := $(TARGET).o
CLIB := $(TARGET).c
INIT := init_$(TARGET).c
DLIB := $(TARGET).ctmp
RLIB := $(TARGET).rbtmp
DEPLIB := $(TARGET).d driver.d
MRBS := $(BASEDIR)/t/*.rb
OBJS := driver.o $(MLIB)

# C compiler (gcc)
CC = gcc
LL = gcc
AR = ar
DEBUG_MODE = 1
ifeq ($(DEBUG_MODE),1)
CFLAGS = -g
else
CFLAGS = -O3
endif
INCLUDES = -I../src -I../include
ALL_CFLAGS = -Wall -Werror-implicit-function-declaration $(CFLAGS)
ifeq ($(OS),Windows_NT)
  MAKE_FLAGS = CC=$(CC) LL=$(LL) ALL_CFLAGS="$(ALL_CFLAGS)"
else
  MAKE_FLAGS = CC='$(CC)' LL='$(LL)' ALL_CFLAGS='$(ALL_CFLAGS)'
endif

# mruby compiler and test driver
ifeq ($(OS),Windows_NT)
MRBC = ../bin/mrbc.exe
EXE := $(TARGET).exe
else
MRBC = ../bin/mrbc
EXE := $(TARGET)
endif

# libraries, includes
LIBS = -lm

# compiler, linker (gcc)
CC = gcc
LL = gcc
YACC = bison
DEBUG_MODE = 1
ifeq ($(DEBUG_MODE),1)
CFLAGS = -g -O3
else
CFLAGS = -O3
endif
ALL_CFLAGS = -Wall -Werror-implicit-function-declaration $(CFLAGS)
ifeq ($(OS),Windows_NT)
  MAKE_FLAGS = CC=$(CC) LL=$(LL) ALL_CFLAGS="$(ALL_CFLAGS)"
else
  MAKE_FLAGS = CC='$(CC)' LL='$(LL)' ALL_CFLAGS='$(ALL_CFLAGS)'
endif

##############################
# generic build targets, rules

.PHONY : all
all : $(LIBR) $(EXE)
	@echo "make: built targets of `pwd`"

.PHONY : test
all : $(LIBR) $(EXE)
	./$(EXE)

# executable constructed using linker from object files
$(EXE) : $(LIBR) $(OBJS)
	$(LL) -o $@ $(CFLAGS) $(OBJS) $(LIBR) $(LIBS)

-include $(OBJS:.o=.d)

# objects compiled from source
$(OBJS) : %.o : %.c
	$(CC) $(ALL_CFLAGS) -MMD $(INCLUDES) -c $< -o $@

# C library compile
$(LIBR) :
	@$(MAKE) -C $(BASEDIR) $(MAKE_FLAGS)

# Compile C source from merged mruby source
$(CLIB) : $(RLIB) $(MRBC) $(INIT)
	$(MRBC) -Bmrbtest_irep -o$(DLIB) $(RLIB); cat $(INIT) $(DLIB) > $@

$(MRBC) : ../src/opcode.h ../src/codegen.c ../src/parse.y
	$(MAKE) -C ../tools/mrbc $(MAKE_FLAGS)

# merge mruby sources
$(RLIB) : $(MRBS)
	cat $? > $@

# clean up
.PHONY : clean
clean :
	@echo "make: removing targets, objects and depend files of `pwd`"
	-rm -f $(MRBC) $(MLIB) $(CLIB) $(RLIB) $(DLIB) $(DEPLIB) $(OBJS) $(EXE)
