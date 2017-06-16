#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# This makefile is for cortex-M4 build system supporting HOST and MSP432 
# platform
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      
#		<FILE>.i - Builds <FILE>.i preprocessed file
#		<FILE>.asm - Builds <FILE>.asm assembly file 
#		<FILE>.o - Builds <FILE>.o object file without linking
#		compile-all - Compliles all object files without linking
#		build : Builds and links all the source files
#		clean : Removes all generated files
#
# Platform Overrides:
#		PLATFORM - Target platform (HOST, MSP432)
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
PLATFORM = HOST
TARGET = c1m2


# Architectures Specific Flags
LINKER_FILE = ../msp432p401r.lds
CPU = cortex-m4
ARCH = armv7e-m
SPECS = nosys.specs



ifeq ($(PLATFORM),HOST)
	CC = gcc
	CFLAGS = -Wall -Werror -g -O0 -std=c99
	CPPFLAGS = -DHOST
	LDFLAGS = -Wl,-Map=$(TARGET).map
	OBJS = $(SRCS_H:.c=.o) 
	LD = $(INCLUDES_H)

else
	CC = arm-none-eabi-gcc
	CFLAGS = -Wall -Werror -g -O0 -std=c99 -mcpu=$(CPU) -march=$(ARCH) \
			-mfloat-abi=hard -mthumb -mfpu=fpv4-sp-d16 --specs=$(SPECS)
	CPPFLAGS = -DMSP432
	LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
	OBJS = $(SRCS:.c=.o)
	LD = $(INCLUDES)

endif

%.i : %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) $(LD) -E $< -o $@

%.asm : %.c 
	$(CC) $(CFLAGS) $(CPPFLAGS) $(LD) -S $< -o $@


%.o : %.c 
	$(CC) $(CFLAGS) $(CPPFLAGS) $(LD) -c $< -o $@ 

.PHONY : compile-all build clean

compile-all : $(OBJS)

build : $(TARGET).out
	
$(TARGET).out : $(OBJS)
	$(CC) $(CFLAGS) $(CPPFLAGS) $(LDFLAGS) $^ -o $@

clean:
	rm -f $(OBJS) $(TARGET).out *.asm *.i $(TARGET).map