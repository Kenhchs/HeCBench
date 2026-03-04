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

obj = main.o io.o process.o

#===============================================================================
# Sets Flags
#===============================================================================

# Standard Flags
CFLAGS := $(EXTRA_CFLAGS) -std=c++17 -Xcompiler -Wall -arch=$(ARCH)

# Linker Flags
LDFLAGS = 

# Debug Flags
ifeq ($(DEBUG),yes)
  CFLAGS += -g -DDEBUG
  LDFLAGS  += -g
endif

# Optimization Flags
ifeq ($(OPTIMIZE),yes)
  CFLAGS += -O3
endif
#===============================================================================
# Targets to Build
#===============================================================================

$(program): $(obj) Makefile
	$(CC) $(CFLAGS) $(obj) -o $@ $(LDFLAGS)

%.o: %.cpp utils.h Makefile
	$(CC) -x cu $(CFLAGS) -c $< -o $@

process.o: process.cu kernels.h utils.h Makefile
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -rf $(program) $(obj)

run: $(program)
	@if [ ! -d "query" ]; then \
		echo "Dataset not found. Downloading via gdown..."; \
		python3 -c "import gdown; gdown.download_folder(id='1JJ8UVJgYO2iANTlV5oEF_7z4jURtGX84', use_cookies=False)"; \
		mkdir -p query reffiles-tab Results; \
		unzip -j cudaMap/cudaMap-all.zip "cudaMap/queries/*" -d query; \
		unzip -j cudaMap/cudaMap-all.zip "cudaMap/reffiles-tab/*" -d reffiles-tab; \
	fi
	echo -e "\n\n\n" | $(LAUNCHER) ./$(program)

