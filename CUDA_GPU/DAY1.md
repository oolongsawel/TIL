---
# _GPU프로그램의 이해_

### HOST와 DEVICE
host와 device(GPU card)는 독립적인 하드웨어
서로다른 메모리영역, 서로 비동기적으로 동시에 실행가능

-host code
```cpp
int main (void) {
    printf("hello World");
    return 0;
}
```

-device code
```cpp
__global__ void mykernel (void) {
    printf("hello Cuda World");
}

int main (void) {
    mykernel<<< 1, 1>>>();
    return 0;
}
```

### CUDA C/C++ KEYWORDS 
cuda-> c언어를 확장해서 사용
kernel함수 <<<>>> : host가 gpu를 호출, device thread의 동작을 정의하는 c function
```cpp
1.  __host__ : 가장 default , CPU가 호출할 수 있는 코드

2. __device__ : gpu가 부를 수 있는 함수

3. __global__ : host에서 gpu를 이용하기 위한 code
```

```cpp
process

1. GPU device공간할당 
cudaMalloc() 함수

2. CPU--->GPU 메모리복사
cudaMemcpy(des, src, size, cudaMemcpyHostToDevice) 함수

3. GPU연산처리
kernel<<< , >>>() 커널호출

4. GPU-->CPU메모리복사
cudaMemcpy(des, src, size, cudaMemcpyDeviceToHost) 함수

5. GPU 메모리해제
cudaFree() 함수
```

### 성능측정
어느곳을 측정해야하는가?
Computation + Data Trans For Overhead
커널함수수행시간 + 데이터전송시간(CPU->GPU, GPU->CPU메모리복사시간)

이때 GPU수행시간동안 CPU연산진행될 수 있음 독립적인 메모리&구조이기때문에..
cudaDeviceSyncronize() 