#### eigen在ros中使用 

在Cmakelist.txt中加入以下三行：

find_package(cmake_modules REQUIRED)
find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})
