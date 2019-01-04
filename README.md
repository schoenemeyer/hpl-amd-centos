# Install and Run the HPL Linpack on a CentOS Workstation with AMD FX(tm)-6300 Six-Core Processor

- Download the latest source from   
http://www.netlib.org/benchmark/hpl/

uncompress and go to the hpl directory

If you dont have a Library for Matrix Operations, start with installing OpenBlas
```
sudo yum install -y openblas-devel.x86_64 openblas.x86_64
./configure
```
Another Option is to install the Atlas Library. This Library can be downloaded from    
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



