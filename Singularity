Bootstrap: docker
From: nvidia/cuda:9.2-devel-ubuntu16.04

%labels

   AUTHOR w.furnass@sheffield.ac.uk

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
    cd /opt

    # Build and install Boost
    wget http://sourceforge.net/projects/boost/files/boost/1.58.0/boost_1_58_0.tar.gz -P /tmp 
    tar -C /opt -zxf /tmp/boost_1_58_0.tar.gz
    cd /opt/boost_1_58_0
    ./bootstrap.sh --with-libraries=atomic,date_time,exception,filesystem,iostreams,locale,program_options,regex,signals,system,test,thread,timer,log
    ./b2 --with=all install
    ldconfig

    # Grab CUDA Samples (needed for helper_cuda.h)
    git clone --branch v9.2 --depth 1 https://github.com/NVIDIA/cuda-samples.git /usr/local/cuda-9.2/samples && \

    # Build fetalReconstruction
    mkdir /opt/fetalReconstruction
    cd /opt/fetalReconstruction
    # Parsimoneous checkout of commit of interest
    # (need to use particular commit > tag r0.1 to get CUDA >=7.5 support)
    git init
    git remote add origin https://github.com/bkainz/fetalReconstruction.git
    git fetch --depth 1 origin 69c381f68cc5527650e08e926caebbffd73b7a9f
    git checkout FETCH_HEAD

    mkdir source/build
    cd source/build
    cmake .. -DCUDA_SDK_ROOT_DIR=/usr/local/cuda-9.2/samples -DCUDA_ROOT_DIR=/usr/local/cuda-9.2 -DCUDA_HELPER_INCLUDE_DIR='/usr/local/cuda-9.2/samples/Common' -DCUDA_CUDA_LIBRARY=/usr/local/cuda-9.2/targets/x86_64-linux/lib/stubs/libcuda.so
    make

    # Ensure built binaries are executable and  on the PATH
    for i in /opt/fetalReconstruction/bin/linux64/*; do chmod +x $i; ln -s $i /usr/local/bin; done

    # Cleanup to reduce image size
    apt-get purge -y \
        cmake \
        git \
        libbz2-dev \
        libgsl-dev \
        libnifti-dev \
        libtbb-dev \
        wget \
	zlib1g-dev 
    apt-get --purge -y autoremove
    rm -rf /var/lib/apt/lists/*
    rm -r /opt/boost_1_58_0
    rm -rf /opt/fetalReconstruction/source

%runscript
    /usr/local/bin/PVRreconstructionGPU  
