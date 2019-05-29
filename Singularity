Bootstrap: docker
From: nvidia/cuda:9.2-devel-ubuntu16.04

%files
    cmake_use_rpaths.diff /tmp

%post
    apt-get update
    apt-get install -y \
        bzip2 \
        cmake \
        git \
        libbz2-dev \
        libgsl-dev \
        libgsl2 \
        libnifti-dev \
        libtbb-dev \
        libtbb2 \
        wget \
	zlib1g-dev 
    rm -rf /var/lib/apt/lists/*
    cd /opt

    # Build and install Boost
    wget http://sourceforge.net/projects/boost/files/boost/1.58.0/boost_1_58_0.tar.gz -P /tmp 
    tar -C /opt -zxf /tmp/boost_1_58_0.tar.gz
    cd /opt/boost_1_58_0
    ./bootstrap.sh --with-libraries=atomic,date_time,exception,filesystem,iostreams,locale,program_options,regex,signals,system,test,thread,timer,log
    ./b2 --with=all install

    # Grab CUDA Samples (needed for helper_cuda.h)
    git clone https://github.com/NVIDIA/cuda-samples.git /usr/local/cuda-9.2/samples && \
    cd /usr/local/cuda-9.2/samples && \
    git checkout v9.2

    # Build and install fetalReconstruction
    cd /opt
    git clone https://github.com/bkainz/fetalReconstruction.git
    cd fetalReconstruction
    # NB need to use particular commit > tag r0.1 to get CUDA >=7.5 support
    git checkout 69c381f68cc5527650e08e926caebbffd73b7a9f
    cd /opt/fetalReconstruction/source
    # Patch CMakeLists.txt so use RPATHs for finding boost (otherwise cannot be dynamically found at runtime) 
    git apply /tmp/cmake_use_rpaths.diff
    mkdir build
    cd build
    cmake .. -DCUDA_SDK_ROOT_DIR=/usr/local/cuda-9.2/samples -DCUDA_ROOT_DIR=/usr/local/cuda-9.2 -DCUDA_HELPER_INCLUDE_DIR='/usr/local/cuda-9.2/samples/Common' -DCUDA_CUDA_LIBRARY=/usr/local/cuda-9.2/targets/x86_64-linux/lib/stubs/libcuda.so
    make
    for i in /opt/fetalReconstruction/bin/linux64/*; do chmod +x $i; ln -s $i /usr/local/bin; done
