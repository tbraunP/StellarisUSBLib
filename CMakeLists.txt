cmake_minimum_required(VERSION 2.8)

INCLUDE(CMakeForceCompiler)
 
SET(CMAKE_SYSTEM_NAME Generic)
SET(CMAKE_SYSTEM_VERSION 1)

# specify the cross compiler
CMAKE_FORCE_C_COMPILER(arm-none-eabi-gcc GNU)
CMAKE_FORCE_CXX_COMPILER(arm-none-eabi-g++ GNU)
 
find_program(LINKER arm-none-eabi-ld)
find_program(OBJCOPY arm-none-eabi-objcopy)
find_program(OBJDUMP arm-none-eabi-objdump)
SET(CMAKE_AR arm-none-eabi-ar)
SET(CMAKE_RANLIB arm-none-eabi-ranlib)

# Set compiler and linker options
SET(COMMON_FLAGS "-O0 -mcpu=cortex-m4 -mthumb -mfloat-abi=softfp -mfpu=fpv4-sp-d16 -ffunction-sections -fdata-sections -fno-common -fmessage-length=0 -Dgcc -DPART_LM4F120H5QR -DTARGET_IS_BLIZZARD_RA1 -g")
SET(CMAKE_CXX_FLAGS "${COMMON_FLAGS}  -std=gnu++0x")
SET(CMAKE_C_FLAGS "${COMMON_FLAGS} -std=gnu99")
SET(CMAKE_EXE_LINKER_FLAGS "-Wl,-gc-sections ")

# Debug Options
SET(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS}  -g")
SET(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS} -g")

# add -I Headers
include_directories ("inc")
include_directories("../StellarisCMSIS/inc")
include_directories("../StellarisDriverLib/inc")


# add source files from folder
AUX_SOURCE_DIRECTORY("src/usblib" USRCS)
AUX_SOURCE_DIRECTORY("src/usblib/device" DSRCS)
AUX_SOURCE_DIRECTORY("src/usblib/host" HSRCS)
SET(SRCS ${USRCS} ${DSRCS} ${HSRCS})

add_library(StellarisUSBLib ${SRCS})

