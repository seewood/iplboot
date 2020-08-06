# NO LONGER MAINTAINED

Because the most recent devkitPPC builds generate huge code, I can no longer
make iplboot fit in Qoob SX flash. Other modchips still have a fair amount of
headroom, but some critical bugs have demotivated me from working on this
project anymore.

Feel free to file bug reports, but beware that I will ignore them. I will review
pull requests, but I won't release any new binaries.

# iplboot

A minimal GameCube IPL

## Usage

Flash the latest build to your Qoob Pro or Viper as a BIOS.  
The Qoob SX is currently not supported because it uses a very different process
to boot, and reverse-engineering efforts so far have been unsuccessful.

After flashing, place an ipl.dol file on your SD card and turn the Cube on, it
will load it right away. The IPL also acts as a server for emu_kidid's
[usb-load](https://github.com/emukidid/gc-usb-load), should you want to use it
for development purposes.

## Building with latest DevKitPPC and latest libogc
- Dollz3
```wget http://wiibrew.org/w/images/e/ef/Dollz3.zip
unzip Dollz3.zip
chmod dollz3/dollz3
sudo mv dollz3/dollz3 /usr/local/bin
```
- dolxz 
```git clone https://github.com/FIX94/dolxz.git
cd dolxz
chmod +x bin2h/bin2h
cd loader/cube
PATH=${PATH}:/opt/devkitpro/devkitPPC/bin make
cd ../wii
PATH=${PATH}:/opt/devkitpro/devkitPPC/bin make
cd ..
PATH=${PATH}:/opt/devkitpro/devkitPPC/bin make -f Makefile.cube
PATH=${PATH}:/opt/devkitpro/devkitPPC/bin make -f Makefile.cube high=1
PATH=${PATH}:/opt/devkitpro/devkitPPC/bin make -f Makefile.wii
PATH=${PATH}:/opt/devkitpro/devkitPPC/bin make -f Makefile.wii high=1
cd ..
```
- changes in main.c from dolxz
replace the main.c with this one: 
https://github.com/seewood/iplboot/files/5034293/main.c.zip

## compile dolxz
```
gcc -Wall -static -O2 -s main.c -llzma -o dolxz
chmod +x dolxz
sudo cp dolxz /usr/local/bin
```
## doltool
```git clone https://github.com/cylgom/doltool.git
cd doltool
make
sudo cp doltool /usr/local/bin
```

## Building with minimal Code (Fits in QoobSX and ViperGC)
A specific setup is required to build iplboot:
- devkitPPC r26
- Latest libOGC compiled with dkPPC r26
- Clang (`ln -s /usb/bin/clang $DEVKITPPC/bin/powerpc-eabi-clang`)

Additionally, the only BS1 that is currently known to work is the one from PAL
1.0 IPLs (full ROM MD5: `0cdda509e2da83c85bfe423dd87346cc`).
