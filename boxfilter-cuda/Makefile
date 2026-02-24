#===============================================================================
# User Options
#===============================================================================

# Compiler can be set below, or via environment variable
CC        = nvcc
OPTIMIZE  = yes
DEBUG     = no
ARCH      = sm_60
LAUNCHER  =

#===============================================================================
# Program name & source code list
#===============================================================================


program = main

obj = main.o shrUtils.o cmd_arg_reader.o reference.o

#===============================================================================
# Sets Flags
#===============================================================================

# Standard Flags
CFLAGS := $(EXTRA_CFLAGS) -std=c++17 -Xcompiler -Wall -arch=$(ARCH)

# Linker Flags
LDFLAGS = 

# Debug Flags
ifeq ($(DEBUG),yes)
  CFLAGS += -g
  LDFLAGS  += -g
endif


# Optimization Flags
ifeq ($(OPTIMIZE),yes)
  CFLAGS += -O3
endif


#===============================================================================
# Targets to Build
#===============================================================================

$(program): $(obj)
	$(CC) $(CFLAGS) $(obj) -o $@ $(LDFLAGS)

main.o: main.cu shrUtils.h
	$(CC) $(CFLAGS) -c $< -o $@

shrUtils.o : shrUtils.cpp shrUtils.h cmd_arg_reader.h exception.h
	$(CC) $(CFLAGS) -c $< -o $@

cmd_arg_reader.o : cmd_arg_reader.cpp cmd_arg_reader.h exception.h
	$(CC) $(CFLAGS) -c $< -o $@

reference.o : reference.cpp
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -rf $(program) $(obj)

run: $(program)
	$(LAUNCHER) ./$(program) ./data/lenaRGB.ppm 10000
