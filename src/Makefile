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
LINKER_FILE = msp432p401r.lds
CPU = cortex-m4
ARCH = armv7e-m
SPECS = nosys.specs

#Object Files
OBJS = $(SOURCES:.c=.o)

ifeq ($(PLATFORM),HOST)
	CC = gcc
	CFLAG = -Wall -Werror -g -O0 -std=c99
	CPPFLAG = -DHOST
	LDFLAGS = Wl, -Map=$(TARGET).map 

else
	CC = arm-none-eabi-gcc
	CFLAG = -Wall -Werror -g -O0 -std=c99 -mcpu=$(CPU) -march=$(ARCH) \
			-mfloat-abi=hard -mthumb -mfpu=fpv4-sp-d16 --specs=$(SPECS)
	CPPFLAG = -DMSP432
	LDFLAGS = -Wl, -T $(LINKER_FILE) -Map=$(TARGET).map

endif

