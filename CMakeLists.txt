cmake_minimum_required(VERSION 3.10) 

set(CMAKE_TOOLCHAIN_FILE ${CMAKE_CURRENT_LIST_DIR}/toolChain.cmake)

#Option Variables
set(CMAKE_VERBOSE_MAKEFILE OFF)

project(powerManager C CXX ASM)

set(CMAKE_INCLUDE_CURRENT_DIR TRUE)

set(toolChainDir "/home/maninblack/ti/ccs1040/ccs/tools/compiler/ti-cgt-msp430_20.2.5.LTS")
set(msp430Vendor "/home/maninblack/ti/ccs1040/ccs/ccs_base/msp430/include")

set(sources main.c)
message(STATUS ${CMAKE_CURRENT_LIST_DIR})

add_executable(powerManager ${sources})

target_include_directories(powerManager PRIVATE
    ${toolChainDir}/include/
    ${msp430Vendor}/
)

target_compile_definitions(powerManager PRIVATE 
__MSP430F2232__
)

target_link_options(powerManager PUBLIC
--rom_model
--ram_model
)

#${msp430Vendor}/lnk_msp430f2232.cmd
#${toolChainDir}/lib/rts430_eabi.lib
#${toolChainDir}/lib/libc.a
target_link_libraries(powerManager ${CMAKE_CURRENT_LIST_DIR}/msp430f2232.cmd ${toolChainDir}/lib/rts430_eabi.lib)

add_custom_command(TARGET powerManager
                   POST_BUILD
                   COMMAND ${toolChainDir}/bin/hex430 -memwidth=8 --romwidth=8 --diag_wrap=off --ti_txt -o "msp430CmakeDeneme.txt"  "powerManager.elf"
                   VERBATIM)