cmake_minimum_required(VERSION 3.18.0)
# set(CMAKE_CUDA_COMPILER "/usr/local/cuda-11.6/bin/nvcc")
project(yolort_trt LANGUAGES CXX CUDA)
# cmake -DCMAKE_CUDA_COMPILER=/usr/local/cuda-11.6/bin/nvcc
# set(CUDA_TOOLKIT_ROOT_DIR     "/usa/local/cuda-11.6")
set(CUDNN_DIR    "/media/ros/A666B94D66B91F4D/ros/download_zip_tar/cudnn-11.4-linux-x64-v8.2.4.15/cuda")
# set(TENSORRT_DIR "/media/ros/A666B94D66B91F4D/ros/new_deploy/TensorRT-8.5.1.7.Linux.x86_64-gnu.cuda-11.8.cudnn8.6/TensorRT-8.5.1.7")
set(TENSORRT_DIR "/media/ros/A666B94D66B91F4D/ros/new_deploy/TensorRT-8.2.3.0.Linux.x86_64-gnu.cuda-11.4.cudnn8.2/TensorRT-8.2.3.0")
option(TENSORRT_DIR "Path to built TensorRT directory." STRING)
message(STATUS "TENSORRT_DIR: ${TENSORRT_DIR}")

find_package(OpenCV 4 REQUIRED)

# set(PROTOBUF_DIR "/media/ros/A666B94D66B91F4D/ros/project/protobuf_my")

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
    add_compile_options(-Wall)
else()
    add_compile_options(-Wextra -Wall -Wno-unused -Wno-deprecated-declarations)
endif()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# For CUDA
find_package(CUDA)
message(STATUS "CUDA_DIR: ${CUDA_TOOLKIT_ROOT_DIR}")

include_directories(
        ${OpenCV_INCLUDE_DIRS}
        ${CUDA_DIR}/include
        ${TENSORRT_DIR}/include
        ${CUDNN_DIR}/include
)

link_directories(
        ${TENSORRT_DIR}/lib
        ${CUDA_TOOLKIT_ROOT_DIR}/lib64
        ${CUDNN_DIR}/lib
)

#include_directories(${CUDA_TOOLKIT_ROOT_DIR}/include)
#if (CMAKE_SYSTEM_NAME MATCHES "Linux")
#    link_directories(${CUDA_TOOLKIT_ROOT_DIR}/lib64)
#elseif (CMAKE_SYSTEM_NAME MATCHES "Windows")
#    link_directories(${CUDA_TOOLKIT_ROOT_DIR}/lib/x64)
#else()
#    link_directories(${CUDA_TOOLKIT_ROOT_DIR}/lib64)
#endif()

# For TensorRT
if(EXISTS ${TENSORRT_DIR})
    message(STATUS "TensorRT DIR: " ${TENSORRT_DIR})
else()
    message(STATUS "TensorRT directory is unable to use, Please use correct TensorRT directory")
endif()

add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS} nvinfer cudart nvonnxparser nvinfer_plugin)
