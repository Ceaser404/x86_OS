ASM=nasm
CC=gcc

SRC_DIR=src
TOOLS_DIR=tools
BUILD_DIR=build

.PHONY: all floppy_image kernel bootloader clean always tools_fat

all: floppy_image
#
# Floppy image
#
floppy_image: $(BUILD_DIR)/main_floppy.img

# build floppy disk image taking binary file built from bootloader padding with zeros until 1.44 MB
# create file system using mkfs.fat command -F 12 (FAT 12)
# put bootloader in first sector of disk (must truncate file to avoid losing rest of image)
# mcopy copies bin file for kernel to disk
$(BUILD_DIR)/main_floppy.img: bootloader kernel
	dd if=/dev/zero of=$(BUILD_DIR)/main_floppy.img bs=512 count=2880
	mkfs.fat -F 12 -n "NBOS" $(BUILD_DIR)/main_floppy.img
	dd if=$(BUILD_DIR)/bootloader.bin of=$(BUILD_DIR)/main_floppy.img conv=notrunc
	mcopy -i $(BUILD_DIR)/main_floppy.img $(BUILD_DIR)/kernel.bin "::kernel.bin"



all: floppy_image2
#
# Floppy image
#
floppy_image2: $(BUILD_DIR)/main_floppy2.img

# build floppy disk image taking binary file built from bootloader padding with zeros until 1.44 MB
# create file system using mkfs.fat command -F 12 (FAT 12)
# put bootloader in first sector of disk (must truncate file to avoid losing rest of image)
# mcopy copies bin file for kernel to disk
$(BUILD_DIR)/main_floppy2.img: bootloader kernel
	dd if=/dev/zero of=$(BUILD_DIR)/main_floppy2.img bs=512 count=2880
	mkfs.fat -F 12 -n "NBOS" $(BUILD_DIR)/main_floppy2.img
	dd if=$(BUILD_DIR)/bootloader.bin of=$(BUILD_DIR)/main_floppy2.img conv=notrunc
	mcopy -i $(BUILD_DIR)/main_floppy2.img $(BUILD_DIR)/kernel.bin "::kernel.bin"



#
# Bootloader
#
bootloader: $(BUILD_DIR)/bootloader.bin

# build bootloader.asm program and output in a binary format
$(BUILD_DIR)/bootloader.bin: always
	$(ASM) $(SRC_DIR)/bootloader/boot.asm -f bin -o $(BUILD_DIR)/bootloader.bin

#
# Kernel
#
kernel: $(BUILD_DIR)/kernel.bin

$(BUILD_DIR)/kernel.bin: always
	$(ASM) $(SRC_DIR)/kernel/main.asm -f bin -o $(BUILD_DIR)/kernel.bin

#
# Kernel2
#
kernel2: $(BUILD_DIR)/kernel2.bin

$(BUILD_DIR)/kernel2.bin: always
	$(ASM) $(SRC_DIR)/kernel2/main2.asm -f bin -o $(BUILD_DIR)/kernel2.bin

#
# Tools
#
tools_fat: $(BUILD_DIR)/tools/fat
$(BUILD_DIR)/tools/fat: always tools/fat/fat.c
	mkdir -p $(BUILD_DIR)/tools
	$(CC) -g -o $(BUILD_DIR)/tools/fat $(TOOLS_DIR)/fat/fat.c


#
# Always
#
always:
	mkdir -p $(BUILD_DIR)

#
# Clean
#
	rm -rf $(BUILD_DIR)/*