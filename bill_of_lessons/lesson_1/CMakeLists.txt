cmake_minimum_required(VERSION 3.16)
project(lesson_1 VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(Qt6 REQUIRED COMPONENTS Quick)

qt_standard_project_setup()

qt_add_executable(lesson_1
    main.cpp
)

target_link_libraries(lesson_1 PRIVATE Qt6::Quick)
