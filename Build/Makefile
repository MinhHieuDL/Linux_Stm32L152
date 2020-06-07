# Path to stlink folder for uploading code to board
STLINK=~/Embedded/stlink-1.6.1

# Put your source files here (*.c)
SRCS = ../App/src/main.c
SRCS += ../Platform/stm32_conf/src/system_stm32l1xx.c

# Libraries source files, add ones that you intent to use
#SRCS += stm32l1xx_rcc.c
#SRCS += stm32l1xx_gpio.c
#SRCS += stm32l1xx_usart.c

# Binaries will be generated with this name (.elf, .bin, .hex)
PROJ_NAME=stm32l152RBxx

# Put the STM32L1 library code directory here, change YOURUSERNAME to yours
STM_COMMON=../Platform

# Compiler settings. Only edit CFLAGS to include other header files.
CC=arm-none-eabi-gcc
OBJCOPY=arm-none-eabi-objcopy

# Compiler flags
CFLAGS  = -g -O2 -Wall -TSTM32L152RB_FLASH.ld
CFLAGS += -DUSE_STDPERIPH_DRIVER
CFLAGS += -mlittle-endian -mthumb -mcpu=cortex-m3 -mthumb-interwork
CFLAGS += --specs=nosys.specs
CFLAGS += -I.

# Include files from STM libraries
CFLAGS += -I$(STM_COMMON)/stm32_libs/CMSIS/Include
CFLAGS += -I$(STM_COMMON)/stm32_libs/CMSIS/Device/ST/STM32L1xx/Include
CFLAGS += -I$(STM_COMMON)/stm32_libs/STM32L1xx_StdPeriph_Driver/inc
CFLAGS += -I$(STM_COMMON)/stm32_conf/inc
CFLAGS += -I$(STM_COMMON)/utilities/STM32L-DISCOVERY

# add startup file to build
SRCS += $(STM_COMMON)/stm32_libs/CMSIS/Device/ST/STM32L1xx/Source/Templates/TrueSTUDIO/startup_stm32l1xx_md.s
OBJS = $(SRCS:.c=.o)

vpath %.c $(STM_COMMON)/stm32_libs/STM32L1xx_StdPeriph_Driver/src \

.PHONY: proj

# Commands
all: proj

proj: $(PROJ_NAME).elf

$(PROJ_NAME).elf: $(SRCS)
	$(CC) $(CFLAGS) $^ -o $@
	$(OBJCOPY) -O ihex $(PROJ_NAME).elf $(PROJ_NAME).hex
	$(OBJCOPY) -O binary $(PROJ_NAME).elf $(PROJ_NAME).bin

clean:
	rm -f *.o $(PROJ_NAME).elf $(PROJ_NAME).hex $(PROJ_NAME).bin

# Flash the STM32L1
burn: proj
	$(STLINK)/st-flash write $(PROJ_NAME).bin 0x8000000