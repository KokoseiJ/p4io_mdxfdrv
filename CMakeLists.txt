cmake_minimum_required(VERSION 3.11)
project(p4io_mdxfdrv VERSION 0.1 LANGUAGES C)

set(CMAKE_SYSTEM_NAME Windows)

set(toolchain i686-w64-mingw32)
set(CMAKE_SYSROOT /usr/${toolchain})
set(CMAKE_C_COMPILER ${toolchain}-gcc)

add_compile_definitions(WIN32_LEAN_AND_MEAN
                        WINVER=0x0601
                        _WIN32_WINNT=0x0601
                        COBJMACROS
                        # AC_IO_MSG_LOG
)

include_directories(bemanitools/src/main)

add_library(util STATIC bemanitools/src/main/util/log.c
                        bemanitools/src/main/util/mem.c
                        bemanitools/src/main/util/str.c
+                       bemanitools/src/main/util/hex.c
)

add_library(aciodrv STATIC bemanitools/src/main/aciodrv/device.c
                           bemanitools/src/main/aciodrv/port.c)
target_link_libraries(aciodrv util)

add_executable(mdxftest src/main.c src/mdxf.c)
target_link_libraries(mdxftest aciodrv)

