# hpl-amd-centos
run hpl on amd workstation

download latest source
http://www.netlib.org/benchmark/hpl/

uncompress and go to the hpl directory

# If you dont have a Library for Matrix Operations, start with installing OpenBlas

sudo yum install -y openblas-devel.x86_64 openblas.x86_64

./configure

