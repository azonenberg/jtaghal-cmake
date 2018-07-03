cmake_minimum_required(VERSION 3.3)
project(jtaghal)
set(CMAKE_BUILD_TYPE Release)

find_library(FTD2XX_LIB ftd2xx)
find_library(DJTG_LIB djtg)

set(CMAKE_C_FLAGS "-g")
set(CMAKE_CXX_FLAGS "-g")

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
