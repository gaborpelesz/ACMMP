cmake_minimum_required (VERSION 3.22)
project (ACMMP)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# include(CheckLanguage)
# check_language(CUDA)
# if (CMAKE_CUDA_COMPILER)
#     enable_language(CUDA)
#     message(" -- CUDA FOUND")
#     set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -O3 --use_fast_math --maxrregcount=128 --ptxas-options=-v -std=c++11 --compiler-options -Wall -gencode arch=compute_80,code=sm_80")
#     # set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -lineinfo") # for CUDA profiling!
# else()
#     message(FATAL_ERROR "-- CUDA NOT FOUND: Fatal error")
# endif()


find_package(CUDA REQUIRED) # For Cuda Managed Memory and c++11
find_package(OpenCV REQUIRED)
find_package(Eigen3 REQUIRED)

set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS};-O3 --use_fast_math --maxrregcount=128 --ptxas-options=-v -std=c++20 --compiler-options -Wall)
# if(CMAKE_COMPILER_IS_GNUCXX)
#     add_definitions(-pthread)
#     add_definitions(-Wall)
#     add_definitions(-Wextra)
#     add_definitions(-pedantic)
#     add_definitions(-Wno-unused-function)
#     add_definitions(-Wno-switch)
#     set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -O3 -ffast-math -march=native") # extend release-profile with fast-math
# endif()

find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()


# For compilation ...
# Specify target & source files to compile it from
cuda_add_executable(
    ACMMP
    main.h
    ACMMP.h
    ACMMP.cpp
    ACMMP.cu
    main.cpp
    )

# For linking ...
# Specify target & libraries to link it with
target_link_libraries(ACMMP
    ${OpenCV_LIBS}
)

target_include_directories(ACMMP
    PUBLIC
    ${OpenCV_INCLUDE_DIRS}
    ${EIGEN3_INCLUDE_DIRS}
)