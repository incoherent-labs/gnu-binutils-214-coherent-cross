# gnu-binutils-214-coherent-cross
Set of patches for GNU Binutils 2.14 (20030612) enabling target support for Mark-Williams Coherent 4.2 (i386-pc-coherent) COFF executable files.  These patches define Coherent sections and alignment, update configure scripts, and fix a trio of COFF relocation bugs in BFD that caused memory corruption issues under Coherent.

Due to the 14-character filename limit of CoherentFS, this package cannot be built under Coherent 4.2.14 or earlier.  It also triggers a slew of compile errors under recent versions of FreeBSD (even without the patches).  It compiles fine under FreeBSD 4.11 and the resulting executable files are compatible with FreeBSD 13.5 and 14.4 (i386) with the port `misc/compat4x` installed and `options COMPAT_FREEBSD4` enabled in the kernel (true with GENERIC).

Apply against contents of [binutils-2.14a.tar.bz2](https://ftp.gnu.org/gnu/binutils/binutils-2.14.tar.bz2) (SHA1 ccee4124a19508e68f5447bf3e0c43203f0c060e).

To patch, build, and install (change $PREFIX to your desired destination):
```
export PREFIX=$HOME/opt
tar -xvjf binutils-2.14a.tar.bz2
cd binutils-2.14
patch -p1 -E <../binutils-2.14.diff
cd ..

mkdir build-binutils-2.14
cd build-binutils-2.14
../binutils-2.14/configure  --target=i386-pc-coherent --prefix=$PREFIX --enable-64-bit-bfd=no --enable-targets=i386-pc-linux,i386-coff,i386-pc-aout --disable-nls
gmake all
gmake install
export PATH=$PATH:$PREFIX/bin
```

Files touched are:
```
bfd/coff-i386.c
bfd/coffcode.h
bfd/cofflink.c
bfd/config.bfd
bfd/configure
bfd/configure.in
binutils/rename.c
config.sub
configure
configure.in
gas/configure
gas/configure.in
ld/Makefile.am
ld/Makefile.in
ld/configure.host
ld/configure.tgt
ld/emulparams/i386coh.sh
ld/scripttempl/i386coff.sc
```

Required dependencies for building under FreeBSD 4.11:
```
gmake-3.80_2
perl-5.8.5
```
