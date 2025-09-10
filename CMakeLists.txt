cmake_minimum_required(VERSION 3.10)
 project(VersionedHelloWorld)
 
 # 版本号：主版本、次版本、修订版本（原 BUILD_NUMBER 改为 PATCH_VERSION）
 set(MAJOR_VERSION 1)
 set(MINOR_VERSION 0)
 
 # 读取并递增修订版本号（原 BUILD_NUMBER 改为 PATCH_VERSION）
 if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt)
     file(READ ${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt PATCH_VERSION)
     math(EXPR PATCH_VERSION "${PATCH_VERSION} + 1")
 else()
     set(PATCH_VERSION 1)
 endif()
 
 # 保存新的修订版本号
 file(WRITE ${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt ${PATCH_VERSION})
 
 # 生成版本头文件（此时 @PATCH_VERSION@ 可被正确替换）
 configure_file(
     ${CMAKE_CURRENT_SOURCE_DIR}/version.h.in
     ${CMAKE_CURRENT_SOURCE_DIR}/version.h
     @ONLY
 )
 add_executable(hello_world main.cpp lib.cpp)
 target_include_directories(hello_world PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
