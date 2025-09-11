cmake_minimum_required(VERSION 3.10)
project(VersionedHelloWorld)

# 主版本号、次版本号
set(MAJOR_VERSION 1)
set(MINOR_VERSION 0)

# 使用 GitHub Actions 提供的 RUN_NUMBER，如果没有则用本地 build_number.txt
if(DEFINED ENV{GITHUB_RUN_NUMBER})
    set(PATCH_VERSION $ENV{GITHUB_RUN_NUMBER})
else()
    if (EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt)
        file(READ ${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt PATCH_VERSION)
        math(EXPR PATCH_VERSION "${PATCH_VERSION} + 1")
    else()
        set(PATCH_VERSION 1)
    endif()
    file(WRITE ${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt ${PATCH_VERSION})
endif()

# 生成 version.h
configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/version.h.in
    ${CMAKE_CURRENT_SOURCE_DIR}/version.h
    @ONLY
)

add_executable(hello_world main.cpp lib.cpp)
target_include_directories(hello_world PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
