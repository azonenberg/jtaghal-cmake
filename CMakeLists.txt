cmake_minimum_required(VERSION 3.3)
project(jtaghal)
set(CMAKE_BUILD_TYPE Release)

# Find JTAG drivers
find_library(FTD2XX_LIB ftd2xx)
find_library(DJTG_LIB djtg)

#Search for Google protocol buffers
find_package(Protobuf 3 REQUIRED)
include_directories(${PROTOBUF_INCLUDE_DIRS})

#Custom target to run static analysis
add_custom_target(
	analysis
	WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
	COMMAND cppcheck . --enable=style,performance,portability,missingInclude --force -I ${CMAKE_SOURCE_DIR}/lib/jtaghal/ -I ${CMAKE_BINARY_DIR}/protobufs --suppress=missingIncludeSystem -q --check-config)

set(CMAKE_C_FLAGS "-g -Wall -Wextra -fsanitize=address")
set(CMAKE_CXX_FLAGS "-g -Wall -Wextra -fsanitize=address")

if(FTD2XX_LIB)
    message(STATUS "Enabling FTDI backend (using D2XX blob)")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DHAVE_FTD2XX")
    set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -DHAVE_FTD2XX")
endif()

if(DJTG_LIB)
    message(STATUS "Enabling Digilent integrated programmer backend (using Digilent blob)")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DHAVE_DJTG")
    set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -DHAVE_DJTG")
endif()

add_subdirectory("${PROJECT_SOURCE_DIR}/lib/xptools")
add_subdirectory("${PROJECT_SOURCE_DIR}/lib/log")
add_subdirectory("${PROJECT_SOURCE_DIR}/lib/jtaghal")

add_subdirectory("${PROJECT_SOURCE_DIR}/src/jtaghal-apps/svfdumper")
add_subdirectory("${PROJECT_SOURCE_DIR}/src/jtaghal-apps/jtagd")
add_subdirectory("${PROJECT_SOURCE_DIR}/src/jtaghal-apps/jtagclient")
add_subdirectory("${PROJECT_SOURCE_DIR}/src/jtaghal-apps/jtagsh")
