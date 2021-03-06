# Marlin Firmware Arduino Project Makefile
#
# Makefile Based on:
# Arduino 0011 Makefile
# Arduino adaptation by mellis, eighthave, oli.keller
# Marlin adaption by Daid
# Marlin 2.0 support and RELOC_WORKAROUND by @marcio-ao
#
# This has been tested with Arduino 0022.
#
# This makefile allows you to build sketches from the command line
# without the Arduino environment (or Java).
#
# Detailed instructions for using the makefile:
#
#  1. Modify the line containing "ARDUINO_INSTALL_DIR" to point to the directory that
#     contains the Arduino installation (for example, under Mac OS X, this
#     might be /Applications/Arduino.app/Contents/Resources/Java).
#
#  2. Modify the line containing "UPLOAD_PORT" to refer to the filename
#     representing the USB or serial connection to your Arduino board
#     (e.g. UPLOAD_PORT = /dev/tty.USB0).  If the exact name of this file
#     changes, you can use * as a wild card (e.g. UPLOAD_PORT = /dev/tty.usb*).
#
#  3. Set the line containing "MCU" to match your board's processor.
#     Older one's are atmega8 based, newer ones like Arduino Mini, Bluetooth
#     or Diecimila have the atmega168.  If you're using a LilyPad Arduino,
#     change F_CPU to 8000000. If you are using Gen7 electronics, you
#     probably need to use 20000000. Either way, you must regenerate
#     the speed lookup table with create_speed_lookuptable.py.
#
#  4. Type "make" and press enter to compile/verify your program.
#
#  5. Type "make upload", reset your Arduino board, and press enter to
#     upload your program to the Arduino board.
#
# Note that all settings at the top of this file can be overriden from
# the command line with, for example, "make HARDWARE_MOTHERBOARD=71"
#
# To compile for RAMPS (atmega2560) with Arduino 1.6.9 at root/arduino you would use...
#
#   make ARDUINO_VERSION=10609 AVR_TOOLS_PATH=/root/arduino/hardware/tools/avr/bin/ \
#   HARDWARE_MOTHERBOARD=33 ARDUINO_INSTALL_DIR=/root/arduino
#
# To compile and upload simply add "upload" to the end of the line...
#
#   make ARDUINO_VERSION=10609 AVR_TOOLS_PATH=/root/arduino/hardware/tools/avr/bin/ \
#   HARDWARE_MOTHERBOARD=33 ARDUINO_INSTALL_DIR=/root/arduino upload
#
# If uploading doesn't work try adding the parameter "AVRDUDE_PROGRAMMER=wiring" or
# start upload manually (using stk500) like so:
#
#   avrdude -C /root/arduino/hardware/tools/avr/etc/avrdude.conf -v -p m2560 -c stk500 \
#   -U flash:w:applet/Marlin.hex:i -P /dev/ttyUSB0
#
# Or, try disconnecting USB to power down and then reconnecting before running avrdude.
#

# This defines the board to compile for (see boards.h for your board's ID)
HARDWARE_MOTHERBOARD ?= 11

# Arduino source install directory, and version number
# On most linuxes this will be /usr/share/arduino
ARDUINO_INSTALL_DIR  ?= ${HOME}/Arduino
ARDUINO_VERSION      ?= 106

# You can optionally set a path to the avr-gcc tools. Requires a trailing slash. (ex: /usr/local/avr-gcc/bin)
AVR_TOOLS_PATH ?=

#Programmer configuration
UPLOAD_RATE        ?= 57600
AVRDUDE_PROGRAMMER ?= arduino
# on most linuxes this will be /dev/ttyACM0 or /dev/ttyACM1
UPLOAD_PORT        ?= /dev/ttyUSB0

#Directory used to build files in, contains all the build files, from object files to the final hex file
#on linux it is best to put an absolute path like /home/username/tmp .
BUILD_DIR          ?= applet

# This defines whether Liquid_TWI2 support will be built
LIQUID_TWI2        ?= 0

# this defines if Wire is needed
WIRE               ?= 0

# this defines if U8GLIB is needed (may require RELOC_WORKAROUND)
U8GLIB             ?= 1

# this defines whether to include the Trinamic TMC2630Stepper
TMC2630            ?= 1

############
# Try to automatically determine whether RELOC_WORKAROUND is needed based
# on GCC versions:
#   http://www.avrfreaks.net/comment/1789106#comment-1789106

CC_MAJ:=$(shell $(CC) -dM -E - < /dev/null | grep __GNUC__ | cut -f3 -d\ )
CC_MIN:=$(shell $(CC) -dM -E - < /dev/null | grep __GNUC_MINOR__ | cut -f3 -d\ )
CC_PATCHLEVEL:=$(shell $(CC) -dM -E - < /dev/null | grep __GNUC_PATCHLEVEL__ | cut -f3 -d\ )
CC_VER:=$(shell echo $$(( $(CC_MAJ) * 10000 + $(CC_MIN) * 100 + $(CC_PATCHLEVEL) )))
ifeq ($(shell test $(CC_VER) -lt 40901 && echo 1),1)
  @echo This version of GCC is likely broken. Enabling relocation workaround.
	RELOC_WORKAROUND = 1
endif

############################################################################
# Below here nothing should be changed...

# Here the Arduino variant is selected by the board type
# HARDWARE_VARIANT = "arduino", "Sanguino", "Gen7", ...
# MCU = "atmega1280", "Mega2560", "atmega2560", "atmega644p", ...

#Gen7
ifeq ($(HARDWARE_MOTHERBOARD),10)
HARDWARE_VARIANT ?= Gen7
MCU              ?= atmega644
F_CPU            ?= 20000000
else ifeq  ($(HARDWARE_MOTHERBOARD),11)
HARDWARE_VARIANT ?= Gen7
MCU              ?= atmega644p
F_CPU            ?= 20000000
else ifeq  ($(HARDWARE_MOTHERBOARD),12)
HARDWARE_VARIANT ?= Gen7
MCU              ?= atmega644p
F_CPU            ?= 20000000
else ifeq  ($(HARDWARE_MOTHERBOARD),13)
HARDWARE_VARIANT ?= Gen7
MCU              ?= atmega1284p
F_CPU            ?= 20000000

#RAMPS
else ifeq  ($(HARDWARE_MOTHERBOARD),3)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),33)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),34)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),35)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),36)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),38)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),43)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),44)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),45)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),46)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),48)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560

#RAMPS equivalents
else ifeq  ($(HARDWARE_MOTHERBOARD),143)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),144)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),145)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),146)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),148)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),77)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),78)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),79)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),401)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),402)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),40)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),41)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),47)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),53)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),504)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),37)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),42)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),52)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),49)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),72)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),80)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),503)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),431)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),343)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),243)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560

#Other ATmega1280, ATmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),111)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),112)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),2)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),21)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),200)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),70)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),701)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),703)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),704)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),302)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),303)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),304)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),21)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),999)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),310)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),321)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),74)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),75)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560

#Gen6
else ifeq  ($(HARDWARE_MOTHERBOARD),5)
HARDWARE_VARIANT ?= Gen6
MCU              ?= atmega644p
else ifeq  ($(HARDWARE_MOTHERBOARD),51)
HARDWARE_VARIANT ?= Gen6
MCU              ?= atmega644p

#Sanguinololu
else ifeq  ($(HARDWARE_MOTHERBOARD),6)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega644p
else ifeq  ($(HARDWARE_MOTHERBOARD),62)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega644p
else ifeq  ($(HARDWARE_MOTHERBOARD),63)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega644p
else ifeq  ($(HARDWARE_MOTHERBOARD),64)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega1284p
else ifeq  ($(HARDWARE_MOTHERBOARD),65)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega1284p
else ifeq  ($(HARDWARE_MOTHERBOARD),66)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega1284p
else ifeq  ($(HARDWARE_MOTHERBOARD),69)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega1284p
else ifeq  ($(HARDWARE_MOTHERBOARD),89)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega1284p
else ifeq  ($(HARDWARE_MOTHERBOARD),92)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega1284p
else ifeq  ($(HARDWARE_MOTHERBOARD),505)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega1284p
else ifeq  ($(HARDWARE_MOTHERBOARD),601)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega1284p

#Ultimaker
else ifeq  ($(HARDWARE_MOTHERBOARD),7)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),71)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega1280

#ATmega1281, ATmega2561
else ifeq  ($(HARDWARE_MOTHERBOARD),702)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega1281
else ifeq  ($(HARDWARE_MOTHERBOARD),25)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega1281

#Teensylu
else ifeq  ($(HARDWARE_MOTHERBOARD),8)
HARDWARE_VARIANT ?= Teensy
MCU              ?= at90usb1286
else ifeq  ($(HARDWARE_MOTHERBOARD),81)
HARDWARE_VARIANT ?= Teensy
MCU              ?= at90usb1286
else ifeq  ($(HARDWARE_MOTHERBOARD),811)
HARDWARE_VARIANT ?= Teensy
MCU              ?= at90usb1286
else ifeq  ($(HARDWARE_MOTHERBOARD),82)
HARDWARE_VARIANT ?= Teensy
MCU              ?= at90usb646
else ifeq  ($(HARDWARE_MOTHERBOARD),83)
HARDWARE_VARIANT ?= Teensy
MCU              ?= at90usb1286
else ifeq  ($(HARDWARE_MOTHERBOARD),84)
HARDWARE_VARIANT ?= Teensy
MCU              ?= at90usb1286
else ifeq  ($(HARDWARE_MOTHERBOARD),88)
HARDWARE_VARIANT ?= Teensy
MCU              ?= at90usb1286

#Gen3+
else ifeq  ($(HARDWARE_MOTHERBOARD),9)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega644p

#Gen3 Monolithic Electronics
else ifeq  ($(HARDWARE_MOTHERBOARD),22)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega644p

#Megatronics
else ifeq  ($(HARDWARE_MOTHERBOARD),70)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560

#Alpha OMCA board
else ifeq  ($(HARDWARE_MOTHERBOARD),90)
HARDWARE_VARIANT ?= SanguinoA
MCU              ?= atmega644

#Final OMCA board
else ifeq  ($(HARDWARE_MOTHERBOARD),91)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega644p

#Sethi 3D_1
else ifeq  ($(HARDWARE_MOTHERBOARD),20)
HARDWARE_VARIANT ?= Sanguino
MCU              ?= atmega644p

#Rambo
else ifeq  ($(HARDWARE_MOTHERBOARD),301)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560

#Azteeg
else ifeq  ($(HARDWARE_MOTHERBOARD),67)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560
else ifeq  ($(HARDWARE_MOTHERBOARD),68)
HARDWARE_VARIANT ?= arduino
MCU              ?= atmega2560

endif

# Be sure to regenerate speed_lookuptable.h with create_speed_lookuptable.py
# if you are setting this to something other than 16MHz
# Set to 16Mhz if not yet set.
F_CPU ?= 16000000

# Arduino contained the main source code for the Arduino
# Libraries, the "hardware variant" are for boards
# that derives from that, and their source are present in
# the main Marlin source directory

TARGET = $(notdir $(CURDIR))

# VPATH tells make to look into these directory for source files,
# there is no need to specify explicit pathnames as long as the
# directory is added here

# The Makefile for previous versions of Marlin used VPATH for all
# source files, but for Marlin 2.0, we use VPATH only for arduino
# library files.

VPATH = .
VPATH += $(BUILD_DIR)
VPATH += $(ARDUINO_INSTALL_DIR)/hardware/arduino/avr/cores/arduino

VPATH += $(ARDUINO_INSTALL_DIR)/hardware/arduino/avr/libraries/SPI
VPATH += $(ARDUINO_INSTALL_DIR)/hardware/arduino/avr/libraries/SPI/src
VPATH += $(ARDUINO_INSTALL_DIR)/libraries/LiquidCrystal/src
ifeq ($(LIQUID_TWI2), 1)
VPATH += $(ARDUINO_INSTALL_DIR)/libraries/Wire
VPATH += $(ARDUINO_INSTALL_DIR)/libraries/Wire/utility
VPATH += $(ARDUINO_INSTALL_DIR)/libraries/LiquidTWI2
endif
ifeq ($(WIRE), 1)
VPATH += $(ARDUINO_INSTALL_DIR)/libraries/Wire
VPATH += $(ARDUINO_INSTALL_DIR)/libraries/Wire/utility
endif
ifeq ($(NEOPIXEL), 1)
VPATH += $(ARDUINO_INSTALL_DIR)/libraries/Adafruit_NeoPixel
endif
ifeq ($(U8GLIB), 1)
VPATH += $(ARDUINO_INSTALL_DIR)/libraries/U8glib
VPATH += $(ARDUINO_INSTALL_DIR)/libraries/U8glib/clib
endif
ifeq ($(TMC2630), 1)
VPATH += $(ARDUINO_INSTALL_DIR)/libraries/TMC2130Stepper/src
VPATH += $(ARDUINO_INSTALL_DIR)/libraries/TMC2130Stepper/src/source
endif

ifeq ($(HARDWARE_VARIANT), arduino)
HARDWARE_SUB_VARIANT ?= mega
VPATH += $(ARDUINO_INSTALL_DIR)/hardware/arduino/avr/variants/$(HARDWARE_SUB_VARIANT)
else
ifeq ($(HARDWARE_VARIANT), Sanguino)
VPATH += $(HARDWARE_DIR)/marlin/avr/variants/sanguino
else
HARDWARE_SUB_VARIANT ?= standard
VPATH += $(HARDWARE_DIR)/$(HARDWARE_VARIANT)/variants/$(HARDWARE_SUB_VARIANT)
endif
endif
LIB_SRC = wiring.c \
	wiring_analog.c wiring_digital.c \
	wiring_pulse.c \
	wiring_shift.c WInterrupts.c hooks.c

ifeq ($(HARDWARE_VARIANT), Teensy)
LIB_SRC = wiring.c
VPATH += $(ARDUINO_INSTALL_DIR)/hardware/teensy/cores/teensy
endif

LIB_CXXSRC = WMath.cpp WString.cpp Print.cpp SPI.cpp Tone.cpp

ifeq ($(NEOPIXEL), 1)
LIB_CXXSRC += Adafruit_NeoPixel.cpp
endif

ifeq ($(LIQUID_TWI2), 0)
LIB_CXXSRC += LiquidCrystal.cpp
else
LIB_SRC += twi.c
LIB_CXXSRC += Wire.cpp LiquidTWI2.cpp
endif

ifeq ($(WIRE), 1)
LIB_SRC += twi.c
LIB_CXXSRC += Wire.cpp
endif

ifeq ($(U8GLIB), 1)
LIB_CXXSRC += U8glib.cpp
LIB_SRC += u8g_ll_api.c u8g_bitmap.c u8g_clip.c u8g_com_null.c u8g_delay.c u8g_page.c u8g_pb.c u8g_pb16h1.c u8g_rect.c u8g_state.c u8g_font.c u8g_font_data.c
endif

ifeq ($(TMC2630), 1)
LIB_CXXSRC += TMC2130Stepper.cpp TMC2130Stepper_COOLCONF.cpp TMC2130Stepper_DRV_STATUS.cpp TMC2130Stepper_IHOLD_IRUN.cpp TMC2130Stepper_CHOPCONF.cpp TMC2130Stepper_GCONF.cpp  TMC2130Stepper_PWMCONF.cpp SW_SPI.cpp
endif

ifeq ($(RELOC_WORKAROUND), 1)
LD_PREFIX=-nodefaultlibs
LD_SUFFIX=-lm -lgcc -lc -lgcc
endif

#Check for Arduino 1.0.0 or higher and use the correct source files for that version
ifeq ($(shell [ $(ARDUINO_VERSION) -ge 100 ] && echo true), true)
LIB_CXXSRC += main.cpp
else
LIB_SRC += pins_arduino.c main.c
endif

FORMAT = ihex

# Name of this Makefile (used for "make depend").
MAKEFILE = Makefile

# Debugging format.
# Native formats for AVR-GCC's -g are stabs [default], or dwarf-2.
# AVR (extended) COFF requires stabs, plus an avr-objcopy run.
DEBUG = stabs

OPT = s

DEFINES ?=

# Program settings
CC = $(AVR_TOOLS_PATH)avr-gcc
CXX = $(AVR_TOOLS_PATH)avr-g++
OBJCOPY = $(AVR_TOOLS_PATH)avr-objcopy
OBJDUMP = $(AVR_TOOLS_PATH)avr-objdump
AR  = $(AVR_TOOLS_PATH)avr-ar
SIZE = $(AVR_TOOLS_PATH)avr-size
NM = $(AVR_TOOLS_PATH)avr-nm
AVRDUDE = avrdude
REMOVE = rm -f
MV = mv -f

# Place -D or -U options here
CDEFS    = -DF_CPU=$(F_CPU) ${addprefix -D , $(DEFINES)}
CXXDEFS  = $(CDEFS)

ifeq ($(HARDWARE_VARIANT), Teensy)
CDEFS  += -DUSB_SERIAL
LIB_SRC    += usb.c pins_teensy.c
LIB_CXXSRC += usb_api.cpp
endif

# Add all the source directories as include directories too
CINCS = ${addprefix -I ,${VPATH}}
CXXINCS = ${addprefix -I ,${VPATH}}

# Compiler flag to set the C/CPP Standard level.
CSTANDARD = -std=gnu99
CXXSTANDARD = -std=gnu++11
CDEBUG = -g$(DEBUG)
CWARN = -Wall -Wstrict-prototypes
CTUNING = -funsigned-char -funsigned-bitfields -fpack-struct \
	-fshort-enums -w -ffunction-sections -fdata-sections \
	-flto \
	-DARDUINO=$(ARDUINO_VERSION)
ifneq ($(HARDWARE_MOTHERBOARD),)
CTUNING += -DMOTHERBOARD=${HARDWARE_MOTHERBOARD}
endif
#CEXTRA = -Wa,-adhlns=$(<:.c=.lst)
CEXTRA = -fno-use-cxa-atexit -fno-threadsafe-statics

CFLAGS := $(CDEBUG) $(CDEFS) $(CINCS) -O$(OPT) $(CWARN) $(CEXTRA) $(CTUNING) $(CSTANDARD)
CXXFLAGS :=         $(CDEFS) $(CINCS) -O$(OPT) -Wall    $(CEXTRA) $(CTUNING) $(CXXSTANDARD)
#ASFLAGS = -Wa,-adhlns=$(<:.S=.lst),-gstabs
LDFLAGS = -lm


# Programming support using avrdude. Settings and variables.
AVRDUDE_PORT = $(UPLOAD_PORT)
AVRDUDE_WRITE_FLASH = -Uflash:w:$(BUILD_DIR)/$(TARGET).hex:i
ifeq ($(shell uname -s), Linux)
AVRDUDE_CONF = /etc/avrdude/avrdude.conf
else
AVRDUDE_CONF = $(ARDUINO_INSTALL_DIR)/hardware/tools/avr/etc/avrdude.conf
endif
AVRDUDE_FLAGS = -D -C$(AVRDUDE_CONF) \
	-p$(MCU) -P$(AVRDUDE_PORT) -c$(AVRDUDE_PROGRAMMER) \
	-b$(UPLOAD_RATE)

# Since Marlin 2.0, the source files may be distributed into several
# different directories, so it is necessary to find them recursively

SRC    = $(shell find src -name '*.c'   -type f)
CXXSRC = $(shell find src -name '*.cpp' -type f)

# Define all object files.
OBJ  = ${patsubst %.c,   $(BUILD_DIR)/arduino/%.o, ${LIB_SRC}}
OBJ += ${patsubst %.cpp, $(BUILD_DIR)/arduino/%.o, ${LIB_CXXSRC}}
OBJ += ${patsubst %.S,   $(BUILD_DIR)/arduino/%.o, ${LIB_ASRC}}
OBJ += ${patsubst %.c,   $(BUILD_DIR)/%.o, ${SRC}}
OBJ += ${patsubst %.cpp, $(BUILD_DIR)/%.o, ${CXXSRC}}

# Define all listing files.
LST = $(LIB_ASRC:.S=.lst) $(LIB_CXXSRC:.cpp=.lst) $(LIB_SRC:.c=.lst)

# Combine all necessary flags and optional flags.
# Add target processor to flags.
ALL_CFLAGS = -mmcu=$(MCU) -I. $(CFLAGS)
ALL_CXXFLAGS = -mmcu=$(MCU) $(CXXFLAGS)
ALL_ASFLAGS = -mmcu=$(MCU) -x assembler-with-cpp $(ASFLAGS)

# set V=1 (eg, "make V=1") to print the full commands etc.
ifneq ($V,1)
 Pecho=@echo
 P=@
else
 Pecho=@:
 P=
endif

# Create required build hierarchy if it does not exist

$(shell mkdir -p $(dir $(OBJ)))

# Default target.
all: sizeafter

build: elf hex

elf: $(BUILD_DIR)/$(TARGET).elf
hex: $(BUILD_DIR)/$(TARGET).hex
eep: $(BUILD_DIR)/$(TARGET).eep
lss: $(BUILD_DIR)/$(TARGET).lss
sym: $(BUILD_DIR)/$(TARGET).sym

# Program the device.
# Do not try to reset an Arduino if it's not one
upload: $(BUILD_DIR)/$(TARGET).hex
ifeq (${AVRDUDE_PROGRAMMER}, arduino)
	stty hup < $(UPLOAD_PORT); true
endif
	$(AVRDUDE) $(AVRDUDE_FLAGS) $(AVRDUDE_WRITE_FLASH)
ifeq (${AVRDUDE_PROGRAMMER}, arduino)
	stty -hup < $(UPLOAD_PORT); true
endif

	# Display size of file.
HEXSIZE = $(SIZE) --target=$(FORMAT) $(BUILD_DIR)/$(TARGET).hex
ELFSIZE = $(SIZE) --mcu=$(MCU) -C $(BUILD_DIR)/$(TARGET).elf; \
          $(SIZE)  $(BUILD_DIR)/$(TARGET).elf
sizebefore:
	$P if [ -f $(BUILD_DIR)/$(TARGET).elf ]; then echo; echo $(MSG_SIZE_BEFORE); $(HEXSIZE); echo; fi

sizeafter: build
	$P if [ -f $(BUILD_DIR)/$(TARGET).elf ]; then echo; echo $(MSG_SIZE_AFTER); $(ELFSIZE); echo; fi


# Convert ELF to COFF for use in debugging / simulating in AVR Studio or VMLAB.
COFFCONVERT=$(OBJCOPY) --debugging \
	--change-section-address .data-0x800000 \
	--change-section-address .bss-0x800000 \
	--change-section-address .noinit-0x800000 \
	--change-section-address .eeprom-0x810000


coff: $(BUILD_DIR)/$(TARGET).elf
	$(COFFCONVERT) -O coff-avr $(BUILD_DIR)/$(TARGET).elf $(TARGET).cof


extcoff: $(TARGET).elf
	$(COFFCONVERT) -O coff-ext-avr $(BUILD_DIR)/$(TARGET).elf $(TARGET).cof


.SUFFIXES: .elf .hex .eep .lss .sym
.PRECIOUS: .o

.elf.hex:
	$(Pecho) "  COPY  $@"
	$P $(OBJCOPY) -O $(FORMAT) -R .eeprom $< $@

.elf.eep:
	-$(OBJCOPY) -j .eeprom --set-section-flags=.eeprom="alloc,load" \
		--change-section-lma .eeprom=0 -O $(FORMAT) $< $@

# Create extended listing file from ELF output file.
.elf.lss:
	$(OBJDUMP) -h -S $< > $@

# Create a symbol table from ELF output file.
.elf.sym:
	$(NM) -n $< > $@

	# Link: create ELF output file from library.

$(BUILD_DIR)/$(TARGET).elf: $(OBJ) Configuration.h
	$(Pecho) "  CXX   $@"
	$P $(CC) $(LD_PREFIX) $(ALL_CXXFLAGS) -Wl,--gc-sections,--relax -o $@ -L. $(OBJ) $(LDFLAGS) $(LD_SUFFIX)

# Object files that were found in "src" will be stored in $(BUILD_DIR)
# in directories that mirror the structure of "src"

$(BUILD_DIR)/%.o: %.c Configuration.h Configuration_adv.h $(MAKEFILE)
	$(Pecho) "  CC    $<"
	$P $(CC) -MMD -c $(ALL_CFLAGS) $< -o $@

$(BUILD_DIR)/%.o: %.cpp Configuration.h Configuration_adv.h $(MAKEFILE)
	$(Pecho) "  CXX   $<"
	$P $(CXX) -MMD -c $(ALL_CXXFLAGS) $< -o $@

# Object files for Arduino libs will be created in $(BUILD_DIR)/arduino

$(BUILD_DIR)/arduino/%.o: %.c Configuration.h Configuration_adv.h $(MAKEFILE)
	$(Pecho) "  CC    $<"
	$P $(CC) -MMD -c $(ALL_CFLAGS) $< -o $@

$(BUILD_DIR)/arduino/%.o: %.cpp Configuration.h Configuration_adv.h $(MAKEFILE)
	$(Pecho) "  CXX   $<"
	$P $(CXX) -MMD -c $(ALL_CXXFLAGS) $< -o $@

# Target: clean project.
clean:
	$(Pecho) "  RMDIR $(BUILD_DIR)/"
	$P rm -rf $(BUILD_DIR)


.PHONY:	all build elf hex eep lss sym program coff extcoff clean depend sizebefore sizeafter

# Automaticaly include the dependency files created by gcc
-include ${patsubst %.o, %.d, ${OBJ}}
