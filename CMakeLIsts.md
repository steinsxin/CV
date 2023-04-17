# 1.在工程下创建include文件夹存放头文件

非必需但规范，工程中头文件都放在include文件夹下

# 2.在CMakeLIsts.txt文件中导入目录

include_directories(${PROJECT_SOURCE_DIR}/include)

**${PROJECT_SOURCE_DIR}** : 当前工程下的绝对路径

# 3.在cpp文件中include引用即可