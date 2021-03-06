
# Makefile inspired from jasegs Klingel.

CC=avr-gcc

DEVICE=atmega328p

CFLAGS = -g2 -mmcu=$(DEVICE) -Os
CFLAGS+= -Os -ffunction-sections -fdata-sections -fpack-struct
CFLAGS+= -fno-move-loop-invariants -fno-tree-scev-cprop
CFLAGS+= -fno-inline-small-functions
CFLAGS+= -Wall -Wno-pointer-to-int-cast


LDFLAGS = -Wl,-u,vfprintf -lprintf_flt

BAUDRATE=57600

# BAUDRATE=115200


# OS Detection
UNAME=$(shell uname)
ifeq ($(UNAME), Darwin)
  PROG_PRT=cu.usbmodem1411
else
  PROG_PRT=ttyUSB0
endif

all: main.elf

uart.o: uart.c
	$(CC) $(CFLAGS) -c -o uart.o uart.c

main.o: main.c
	$(CC) $(CFLAGS) -c -o main.o main.c

main.elf: uart.o main.o 
	$(CC) $(CFLAGS) $(LDFLAGS) -o main.elf main.o uart.o
	avr-objcopy  -O ihex -R .eeprom main.elf main.hex

flash:	main.elf
	sudo avrdude -c arduino -P /dev/$(PROG_PRT) -b $(BAUDRATE) -p $(DEVICE) -U flash:w:main.hex



clean:
	rm -f *.o
	rm -f *.elf
	rm -f *.map
	rm -f *.hex

.PHONY: clean

