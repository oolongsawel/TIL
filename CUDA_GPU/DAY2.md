#### Kernel은 어떻게 동작하는가?
-kernel
```cpp
__global__ void vecAdd(int * _a, int * _b, int * _c){
    int tID = threadIdx.x;  //각 쓰레드들의 index
    c[tID] = a[tID] + b[tID];
}
```

-kernel call
```cpp
vecAdd<<<1, NUM_DATA>>>(d_a, d_b, d_c);
```

#### CUDA Programming Model
-SIMT Architecture(Single Instruction , Multiple Threads)
Thread들을 그룹단위로 관리/실행
**_모든 Thread가 같은 프로그램 코드 공유 ->Kernel_**

#### CUDA Thread 계층구조
-**Thread** : basic processing unit
-**Warp(웝)**   : 32 Thread, 32개 쓰레드 모아놓은 그룹
-**Block**  : Group of threads (더 많은 쓰레드그룹) warp보다 더 큰 그룹ThreadIdx : 쓰레드인덱스!!!
-**Grid** : block들의 집합

![cuda_thread](/SRC/cuda_thread.PNG)


#### Thread layout 설정
demension of the grid and a block
```cpp
dim3 dimGrid(4, 1, 1);
dim3 dimBolck(8, 1, 1);
vecAdd <<< dimGrid, dimBolck>>>(d_a, d_b, d_c);
```

CheckDim.cu
```cpp
__global__ void checkIndex(void) {
    printf("threadIdx : (%d, %d, %d) 
    blockIdx : (%d, %d, %d) 
    blockDim : (%d, %d, %d)\n"
    ,threadIdx.x, threadIdx.y, threadIdx.z
    ,blockIdx.x, blockIdx.y, blockIdx.z
    ,blockDim.x , blockDim.y, blockDim.z
    ,gridDim.x , gridDim.y , gridDim.z);

}


int main(int argc, char **argv){

    dim3 block(3);
    dim3 grid((nElem + block.x - 1) / block.x );

    printf("grid.x %d gitd.y %d grid.z %d\n", grid.x , grid.y, gird.z);
    printf("block.x %d block.y %d block.z %d\n", block.x , block.y, block.z);

    checkIndex<<<grid, block>>>();
    cudaDeviceReset();

    return 0;
    
}
```


