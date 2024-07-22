CRTI_OBJ=crti.o
CRTBEGIN_OBJ=$(shell i686-elf-gcc $(CFLAGS) -print-file-name=crtbegin.o)
CRTEND_OBJ=$(shell i686-elf-gcc $(CFLAGS) -print-file-name=crtend.o)
CRTN_OBJ=crtn.o

OBJ_LINK_LIST=$(CRTI_OBJ) ${CRTBEGIN_OBJ} kernel.o boot.o $(CRTEND_OBJ) $(CRTN_OBJ)

run: myos.iso
	qemu-system-i386 -cdrom $<

all: myos.iso

myos.iso : myos.bin
	cp myos.bin isodir/boot/myos.bin
	cp grub.cfg isodir/boot/grub/grub.cfg
	grub-mkrescue -o myos.iso isodir

myos.bin : $(OBJ_LINK_LIST) linker.ld
	i686-elf-gcc -T linker.ld -o $@ $(OBJ_LINK_LIST) -ffreestanding -O2 -nostdlib -lgcc

kernel.o : kernel.c
	i686-elf-gcc -c $< -o $@ -ffreestanding -Wall -Wextra

boot.o : boot.s
	i686-elf-as $< -o $@

crti.o : crti.s
	i686-elf-as $< -o $@

crtn.o : crtn.s
	i686-elf-as $< -o $@

clean:
	rm -rf *.o *.bin *.iso isodir/boot/*.bin