cmake_minimum_required(VERSION 3.15)
project(StaticRuntimePlusToolkit CUDA)

#Goal for this example:
# Validate that with cuda we can use some components of the CUDA toolkit, and
# specify the cuda runtime
find_package(CUDAToolkit REQUIRED)

add_library(Common OBJECT curand.cu nppif.cu)
target_link_libraries(Common PRIVATE CUDA::toolkit)
set_target_properties(Common PROPERTIES POSITION_INDEPENDENT_CODE ON)

#static runtime with shared toolkit libraries
add_library(SharedToolkit SHARED shared.cu)
target_link_libraries(SharedToolkit PRIVATE Common CUDA::curand CUDA::nppif )
set_target_properties(SharedToolkit PROPERTIES CUDA_RUNTIME_LIBRARY none)
target_link_libraries(SharedToolkit PUBLIC CUDA::cudart_static)

#static runtime with static toolkit libraries
add_library(StaticToolkit SHARED static.cu)
target_link_libraries(StaticToolkit PRIVATE Common CUDA::curand_static CUDA::nppif_static)

#static runtime with mixed toolkit libraries
add_library(MixedToolkit SHARED mixed.cu)
target_link_libraries(MixedToolkit PRIVATE Common CUDA::curand CUDA::nppif_static)
set_target_properties(MixedToolkit PROPERTIES CUDA_RUNTIME_LIBRARY Static)

add_executable(CudaOnlyStaticRuntimePlusToolkit main.cu)
target_link_libraries(CudaOnlyStaticRuntimePlusToolkit PRIVATE SharedToolkit StaticToolkit MixedToolkit)
