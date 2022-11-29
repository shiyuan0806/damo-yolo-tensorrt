cmake_minimum_required(VERSION 3.18.0)

project(yolort_trt LANGUAGES CXX CUDA)
# 路径修改下，cudnn可以不用
set(CUDNN_DIR    "/media/ros/A666B94D66B91F4D/ros/download_zip_tar/cudnn-11.4-linux-x64-v8.2.4.15/cuda")
set(TENSORRT_DIR "/media/ros/A666B94D66B91F4D/ros/new_deploy/TensorRT-8.2.3.0.Linux.x86_64-gnu.cuda-11.4.cudnn8.2/TensorRT-8.2.3.0")
option(TENSORRT_DIR "Path to built TensorRT directory." STRING)
message(STATUS "TENSORRT_DIR: ${TENSORRT_DIR}")

find_package(OpenCV 4 REQUIRED)


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



# For TensorRT
if(EXISTS ${TENSORRT_DIR})
    message(STATUS "TensorRT DIR: " ${TENSORRT_DIR})
else()
    message(STATUS "TensorRT directory is unable to use, Please use correct TensorRT directory")
endif()

add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS} nvinfer cudart nvonnxparser nvinfer_plugin)
