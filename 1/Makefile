run: myos.iso
	qemu-system-i386 -cdrom $<

all: myos.iso

myos.iso : myos.bin
	cp myos.bin isodir/boot/myos.bin
	cp grub.cfg isodir/boot/grub/grub.cfg
	grub-mkrescue -o myos.iso isodir

myos.bin : kernel.o boot.o linker.ld
	/opt/cross/bin/i686-elf-gcc -T linker.ld -o $@ -ffreestanding -O2 -nostdlib boot.o kernel.o -lgcc

kernel.o : kernel.c
	/opt/cross/bin/i686-elf-gcc -c $< -o $@ -ffreestanding -Wall -Wextra

boot.o : boot.s
	/opt/cross/bin/i686-elf-as $< -o $@

clean:
	rm -rf *.o *.bin *.iso isodir/boot/*.bin