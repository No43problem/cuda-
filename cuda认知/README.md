第一次使用cuda可以使用 which nvcc 查看cuda编译器是否安装

使用 ls -l /dev/nv* 查看gpu型号

写一段cuda程序的一般步骤为：
    
    1.创建 .cu 文件
    2. 使用nvcc编译器进行文件编译，其中编译命令为 nvcc -arch sm_20 文件.cu -o 文件 
    3. 使用命令行 ./文件名 进行运行

使用 __global__ 函数开头编写kernel function， 该限定符用于告诉系统，该函数是要在gpu上执行的

调用cuda kernel function的命令为

    函数名<<<数1，数2>>>（参数1，参数2）
  
其中函数名为kernel函数名，数1*数2为执行该函数的线程数，参数为kernel函数的参数
调用完成后使用cudaDeviceReset() 显式摧毁cuda程序占用的gpu内存资源

一个典型的cuda程序由以下5部分构成

    1.分配gpu空间
    2.把数据从cpu拷贝到gpu空间
    3.调用kernel函数操作数据
    4.数据操作完成后，将数据成gpu空间拷贝到cpu空间
    5.清理cuda程序占用的gpu空间

cuda程序将系统分为host和device两部分，二者均有各自的memory，kernel可以操作device memory，为了更好地操作device memory cuda提供了以下几个内存操作函数

    1.cudaError_t   cudamalloc（void** devptr,size_t size）//devptr表示分配的数据类型指针，size表示分配的大小
    2.cudaError_t   cudamemcpy(void* dst, const void* src, size_t count,cudaMemcpyKind kind)//src拷贝给dst count表示拷贝数据大小，kind表示拷贝类型，通常为两种「cudaMemcpyHostToDevice,cudaMemcpyDeviceToHost」
    3.cudaMemest()//cuda批量初始化函数
    4.cudaFree()//释放cuda占用的gpu内存空间
    
这里cudaError_t 指返回类型，操作成功返回cudaSuccess，失败返回cudaErrorMemoryAllocation
