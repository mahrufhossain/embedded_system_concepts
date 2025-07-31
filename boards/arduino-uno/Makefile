# Makefile for ATmega328P Bare-Metal Projects
#
# === Configuration ===
#
MCU = atmega328p
F_CPU = 16000000UL
BAUD = 9600
PROGRAMMER = arduino
PORT = /dev/ttyUSB0 # Adjust for your OS (e.g., COM3 on Windows, /dev/cu.usbmodemXXXX on macOS)

# === FILE NAMES ===
#
# Target executable name (without extension)
TARGET ?= main

# Automatically find all .c source files
SRCS = $(wildcard *.c)
# Generate corresponding object files
OBJS = $(SRCS:.c=.o)

# Output ELF and HEX file names
ELF = $(TARGET).elf
HEX = $(TARGET).hex

# === Compiler Settings ===
#
CC = avr-gcc
OBJCOPY = avr-objcopy
# CFLAGS:
# -mmcu=$(MCU) : Specify microcontroller
# -DF_CPU=$(F_CPU) : Define CPU frequency for _delay_ms()
# -Os : Optimize for size
# -Wall : Enable all warnings
CFLAGS = -mmcu=$(MCU) -DF_CPU=$(F_CPU) -Os -Wall

# === Default target ===
all: $(HEX)

# Declare phony targets to prevent issues with similarly named files
.PHONY: all upload clean

# Rule to compile individual .c files into .o files
# $< is the first prerequisite (the .c file)
# $@ is the target (the .o file)
%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

# Build ELF file from all object files
$(ELF): $(OBJS)
	$(CC) $(CFLAGS) -o $(ELF) $(OBJS)

# Convert ELF to HEX
# -O ihex : Output in Intel HEX format
# -R .eeprom : Remove the .eeprom section (unless you specifically want to flash EEPROM)
$(HEX): $(ELF)
	$(OBJCOPY) -O ihex -R .eeprom $(ELF) $(HEX)

# Upload with avrdude
# -c $(PROGRAMMER) : Programmer type (e.g., arduino for bootloader)
# -p $(MCU) : Microcontroller part
# -P $(PORT) : Serial port
# -b 115200 : Baud rate for communication with the bootloader (common for Uno/Nano)
# -U flash:w:$(HEX):i : Upload HEX file to flash memory
upload: $(HEX)
	avrdude -c $(PROGRAMMER) -p $(MCU) -P $(PORT) -b 115200 -U flash:w:$(HEX):i

# Clean build files
clean:
	rm -f $(OBJS) $(ELF) $(HEX)