# cmake基本使用练习

### 编译
```
./build.sh
```
可以看到在bin和lib文件夹下有编译生成的结果
### 安装
```
cd build
#注意权限
sudo make install
```
可以看到软件已经按照配置安装到了`/opt`下

### 使用find_pakage
一些成熟的库会发布`<name>config.cmake`文件，称之为配置文件，find_pakage会自动搜索`${CMAKE_MODULE_PATH}`，`/usr/share`，`/usr/local/share`下是否有该模块的配置文件。如果找到，则用户能在cmake中直接使用`${<name>_INCLUDE}`、`${<name>_LIBS}`等变量，给用户带来极大的方便

例如opencv有提供OpenCVConfig.cmake文件，那么可以这么使用cmake
```cmake
find_package(OpenCV)
add_executable(cv_demo test.cpp)
target_link_libraries(cv_demo ${OpenCV_LIBS})
```
用户不需要知道OpenCV的动态库到底在哪儿，反正${OpenCV_LIBS}就是OpenCV库文件的位置。如果有多个版本，需要指定该模块的find_pakage搜索位置：

```cmake
SET(OpenCV_DIR /usr/share/OpenCV/)#这里写自己需要的opencv版本的模块位置
FIND_PACKAGE(OpenCV REQUIRED)
```
