#learncmake是项目名
#CXX|C|JAVA是项目语言，择其一
project(learncmake CXX)

#断言cmake的最低版本
#cmake_minimum_required这种是大小写不区分的，但是像VERSION这种必须大写
cmake_minimum_required(VERSION 2.8)
#加编译宏
add_definitions(-DDEBUG)
#加编译选项
add_definitions(-std=c++11)

#下面是一些内置变量
#CMAKE_INCLUDE_PATH 头文件搜索路径
#CMAKE_LIBRARY_PATH 库文件搜索路径
#PROJECT_BINARY_DIR
#PROJECT_SOURCE_DIR 就是CMakeLists.txt文件所在的目录
#EXECUTABLE_OUTPUT_PATH 定义编译生成的可执行文件存放目录
#LIBRARY_OUTPUT_PATH 定义编译生成的库文件存放目录

#set函数对变量赋值，变量被赋值时不用$符号，但是引用变量需要${}符号
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#配置项目的头文件和库文件搜索路径
include_directories(${PROJECT_SOURCE_DIR}/include)
#link_directories()
aux_source_directory(${PROJECT_SOURCE_DIR}/source MY_SRC_CORE)


#相当于print，有STATUS|SEND_ERROR|FATAL_ERROR三个选项，严重级别不同
message(STATUS "hello word")

#生成库文件，SHARED表示动态库，在cmake中可执行文件和库文件都是target
#此例中add就是一个target
add_library(add SHARED ${MY_SRC_CORE})
#对target进行各种属性配置，设置库版本，API版本
set_target_properties(add PROPERTIES VERSION 1.2 SOVERSION 1)

#生成可执行文件，hello就是一个target
add_executable(hello ${PROJECT_SOURCE_DIR}/demo/hello.cpp)
#对target属性进行配置，修改文件名，但target的唯一标识仍然是hello
set_target_properties(hello PROPERTIES OUTPUT_NAME newname)
#可执行文件链接到库库文件
target_link_libraries(hello add)

#配置make install的行为
#cmake -DCMAKE_INSTALL_PREFIX=/opt ..这条语句配置安装目录
#分别配置可执行文件、动态库、静态库安装到系统的位置
install(TARGETS hello add
	RUNTIME DESTINATION bin
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib)
#配置脚本程序的安装位置
install(PROGRAMS build.sh DESTINATION .)
#配置文本文件的安装位置
install(FILES README.md DESTINATION .)
#配置目录的安装位置
install(DIRECTORY doc DESTINATION .)
