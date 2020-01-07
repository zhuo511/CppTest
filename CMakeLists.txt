cmake_minimum_required(VERSION 3.10)
project(CppTest)

set(CMAKE_CXX_STANDARD 11)

set(BUILD_TYPE X86Compile)    #CorssCompile X86Compile
# 编译参数
if (${BUILD_TYPE} STREQUAL "CorssCompile")
    set(TOOLCHAIN_DIR /opt/hi3519sdk/hisi-linux/x86-arm/arm-hisiv500-linux)
    set(CMAKE_CXX_COMPILER ${TOOLCHAIN_DIR}/bin/arm-hisiv500-linux-uclibcgnueabi-g++)
    set(CMAKE_C_COMPILER   ${TOOLCHAIN_DIR}/bin/arm-hisiv500-linux-uclibcgnueabi-gcc)

    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -O0 -fPIC -mcpu=cortex-a17.cortex-a7 -mfloat-abi=softfp -mfpu=neon-vfpv4 -mno-unaligned-access -fno-aggressive-loop-optimizations")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O0 -fPIC -mcpu=cortex-a17.cortex-a7 -mfloat-abi=softfp -mfpu=neon-vfpv4 -mno-unaligned-access -fno-aggressive-loop-optimizations")

    add_definitions(-D_GLIBCXX_USE_C99)  #C++11交叉编译失败，添加该选项可编译成功
endif ()

if (${BUILD_TYPE} STREQUAL "X86Compile")

    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -O0 -fPIC -gdwarf-2")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O0 -fPIC -gdwarf-2")

    add_definitions(-D_GLIBCXX_USE_C99)  #C++11交叉编译失败，添加该选项可编译成功
    add_definitions(-D_GLIBCXX_USE_CXX11_ABI=0)
endif ()


include_directories(
        include/
        third_party/)

# 头文件路径信息
file(GLOB PROJECT_INC
        "src/*.h"
        "include/*.h")

# 源文件路径信息
file(GLOB PROJECT_SRC
        "src/*.cpp")

set(SOURCE_FILES
        ${PROJECT_SRC}
        ${PROJECT_INC})

add_executable(cppTest ${SOURCE_FILES})
target_link_libraries(cppTest pthread)