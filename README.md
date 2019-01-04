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
The result should look like the lines below. The result was achieved with Openmpi 1.10.7 and Atlas 3.10.3:   
```
--------------------------------------------------------------------------------

- The matrix A is randomly generated for each test.
- The following scaled residual check will be computed:
      ||Ax-b||_oo / ( eps * ( || x ||_oo * || A ||_oo + || b ||_oo ) * N )
- The relative machine precision (eps) is taken to be               1.110223e-16
- Computational tests pass if scaled residuals are less than                16.0

================================================================================
T/V                N    NB     P     Q               Time                 Gflops
--------------------------------------------------------------------------------
WR11C2R4       16152   192     3     2              75.74              3.710e+01
HPL_pdgesv() start time Fri Jan  4 16:43:20 2019

HPL_pdgesv() end time   Fri Jan  4 16:44:36 2019

--VVV--VVV--VVV--VVV--VVV--VVV--VVV--VVV--VVV--VVV--VVV--VVV--VVV--VVV--VVV-
Max aggregated wall time rfact . . . :               1.92
+ Max aggregated wall time pfact . . :               0.73
+ Max aggregated wall time mxswp . . :               0.58
Max aggregated wall time update  . . :              73.92
+ Max aggregated wall time laswp . . :               8.10
Max aggregated wall time up tr sv  . :               0.12
--------------------------------------------------------------------------------
||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=        0.0012292 ...... PASSED
================================================================================

Finished      1 tests with the following results:
              1 tests completed and passed residual checks,
              0 tests completed and failed residual checks,
              0 tests skipped because of illegal input values.
--------------------------------------------------------------------------------

End of Tests.
================================================================================
[thomas@localhost Linux_Debian_ATLAS]$ 
```

