#CXX|C|JAVA
project(learncmake CXX) 
cmake_minimum_required(VERSION 2.8)
add_definitions(-DDEBUG)
add_definitions(-std=c++11)

#build-in var
#CMAKE_INCLUDE_PATH
#CMAKE_LIBRARY_PATH
#PROJECT_BINARY_DIR
#PROJECT_SOURCE_DIR
#EXECUTABLE_OUTPUT_PATH
#LIBRARY_OUTPUT_PATH

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
include_directories(${PROJECT_SOURCE_DIR}/include)
#link_directories()
aux_source_directory(${PROJECT_SOURCE_DIR}/source MY_SRC_CORE)


#STATUS|SEND_ERROR|FATAL_ERROR
message(STATUS "hello word")
add_library(add SHARED ${MY_SRC_CORE})
set_target_properties(add PROPERTIES VERSION 1.2 SOVERSION 1)
add_executable(hello ${PROJECT_SOURCE_DIR}/demo/hello.cpp)
#rename target
set_target_properties(hello PROPERTIES OUTPUT_NAME newname)
target_link_libraries(hello add)


#config 'make install'
#cmake -DCMAKE_INSTALL_PREFIX=/opt
install(TARGETS hello add
	RUNTIME DESTINATION bin
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib)
install(PROGRAMS scripts.sh DESTINATION .)
install(FILES README.md DESTINATION .)
install(DIRECTORY doc DESTINATION .)


