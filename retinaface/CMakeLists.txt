cmake_minimum_required(VERSION 2.6)

project(retinaface)

add_definitions(-std=c++11)

option(CUDA_USE_STATIC_CUDA_RUNTIME OFF)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_BUILD_TYPE Debug)

find_package(CUDA REQUIRED)

set(CUDA_NVCC_PLAGS ${CUDA_NVCC_PLAGS};-std=c++11;-g;-G;-gencode;arch=compute_30;code=sm_30)

include_directories(${PROJECT_SOURCE_DIR}/include)
include_directories(/usr/local/cuda-9.0/targets/aarch64-linux/include)
link_directories(/usr/local/cuda-9.0/targets/aarch64-linux/lib)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -Ofast -Wfatal-errors -D_MWAITXINTRIN_H_INCLUDED")

#cuda_add_library(leaky ${PROJECT_SOURCE_DIR}/leaky.cu)
#cuda_add_library(yololayer ${PROJECT_SOURCE_DIR}/yololayer.cu)

find_package(OpenCV)
include_directories(OpenCV_INCLUDE_DIRS)

add_executable(retina_50 ${PROJECT_SOURCE_DIR}/retina_r50.cpp)
target_link_libraries(retina_50 nvinfer nvinfer_plugin)
target_link_libraries(retina_50 cudart)
#target_link_libraries(retina yololayer)
target_link_libraries(retina_50 ${OpenCV_LIBRARIES})

add_definitions(-O2 -pthread)

