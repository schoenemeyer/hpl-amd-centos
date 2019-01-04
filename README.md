# Install and run the HPL Linpack on a CentOS Workstation with AMD FX(tm)-6300 Six-Core Processor

- Download the latest source from   
http://www.netlib.org/benchmark/hpl/

uncompress and go to the hpl directory

If you dont have a Library for Matrix Operations, start with installing OpenBlas 
```
sudo yum install -y openblas-devel.x86_64 openblas.x86_64
./configure
```
Another option is to install the Atlas Library. This Library can be downloaded from    
https://sourceforge.net/projects/math-atlas/

You need to go through a quite lengthy installation procedure that might take more than 24 hours.
```
mkdir my_build_dir ; cd my_build_dir
   /path/to/ATLAS/configure [flags]
   make              ! tune and compile library
   make check        ! perform sanity tests
   make ptcheck      ! checks of threaded code for multiprocessor systems
   make time         ! provide performance summary as % of clock rate
   make install      ! Copy library and include files to other directories
```

The reason is the installers does detailed checking of the memory subsystem of the machine and creates an highly optimized library for your platform you are going to use, so you will see many messages like the one below   

```
BEST MU-TUNED GENERATED KERNEL FOR imf=4: NU=20, MU=2, MF=3355.25

BEGIN BASIC KERNEL TESTS:
   Kernel sr1_C.c(900000) passes basic test
DONE BASIC KERNEL TESTS:

BEGIN NU/MU EXTRACT SEARCH, imf=4:

BEGIN BASIC KERNEL TESTS:
   Kernel sr1_sse.c(900000) passes basic test
   Kernel sr1_sse.c(900000) passes basic test
   Kernel sr1_sse.c(900000) passes basic test
   Kernel sr1_sse.c(900000) passes basic test
   Kernel sr1_sse.c(900000) passes basic test
   Kernel sr1_sse.c(900000) passes basic test
   Kernel sr1_sse.c(900000) passes basic test
   Kernel sr1_sse.c(900000) passes basic test
   Kernel sr1_sse.c(900000) passes basic test
   Kernel sr1_sse.c(900000) passes basic test
DONE BASIC KERNEL TESTS:

   900000:sr1_sse.c (M=870, N=8, lda=875) gets 2551.06 MFLOPS
   900000:sr1_sse.c (M=870, N=8, lda=875) gets 2458.83 MFLOPS
   900000:sr1_sse.c (M=870, N=8, lda=875) gets 2489.47 MFLOPS
......
......
......


ATLAS install complete.  Examine 
ATLAS/bin/<arch>/INSTALL_LOG/SUMMARY.LOG for details.
make[1]: Leaving directory `/home/thomas/libs/my_build'
make clean
make[1]: Entering directory `/home/thomas/libs/my_build'
rm -f *.o x* config?.out *core*
make[1]: Leaving directory `/home/thomas/libs/my_build'
[thomas@localhost my_build]$ 
      
```

for details I recommend to read the ATLAS Guide from   
https://fossies.org/linux/atlas/doc/atlas_install.pdf

- Now change to the HPL directory

pick the most appropiate Makefile in the setup directory and adapt to your platform (Math Lib and MPI) and start the compilation:   
```
make arch=Linux_Debian_ATLAS
cd bin/Linux_Debian_ATLAS
```
You can adjust the HPL.dat to get some decent performance. Make sure that PxQ is equivalent to the number of cores for the run.
```
mpirun -np 4 ./xhpl
```



