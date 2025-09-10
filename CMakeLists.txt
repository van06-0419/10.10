cmake_minimum_required(VERSION 3.10)
project(VersionedHelloWorld)

# 版本号主版本和次版本
set(MAJOR_VERSION 1)
set(MINOR_VERSION 0)

# 读取并递增构建号
if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt)
    file(READ ${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt BUILD_NUMBER)
    math(EXPR BUILD_NUMBER "${BUILD_NUMBER} + 1")
else()
    set(BUILD_NUMBER 1)
endif()

# 保存新的构建号
file(WRITE ${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt ${BUILD_NUMBER})

# 生成版本头文件
configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/version.h.in
    ${CMAKE_CURRENT_SOURCE_DIR}/version.h
    @ONLY
)

# 添加可执行文件
add_executable(hello_world main.cpp lib.cpp)

# 指定头文件目录
target_include_directories(hello_world PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
