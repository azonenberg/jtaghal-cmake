cmake_minimum_required(VERSION 3.3)
project(jtaghal)
set(CMAKE_BUILD_TYPE Release)

add_subdirectory("${PROJECT_SOURCE_DIR}/lib/xptools")

add_subdirectory("${PROJECT_SOURCE_DIR}/lib/log")

add_subdirectory("${PROJECT_SOURCE_DIR}/lib/jtaghal")

add_subdirectory("${PROJECT_SOURCE_DIR}/src/jtaghal-apps/svfdumper")

add_subdirectory("${PROJECT_SOURCE_DIR}/src/jtaghal-apps/jtagd")

add_subdirectory("${PROJECT_SOURCE_DIR}/src/jtaghal-apps/jtagclient")

