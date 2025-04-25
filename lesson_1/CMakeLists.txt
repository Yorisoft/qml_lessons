cmake_minimum_required(VERSION 3.16)
project(lesson_1 LANGUAGES CXX)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(Qt6 REQUIRED COMPONENTS Quick)

qt_add_executable(lesson_1
    main.cpp
)

qt_add_resources(lesson_1 "qml"
    PREFIX "/"
    FILES main.qml
)

target_link_libraries(lesson_1 PRIVATE Qt6::Quick)
