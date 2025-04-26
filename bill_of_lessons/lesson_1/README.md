# Hello Qt  


<!-- TABLE OF CONTENTS -->  
<!-- AUTO-GENERATED-CONTENT:START (TOC:collapse=true&collapseText="Click to expand") -->

<details>
<summary>Table of Contents</summary>
 
- [QML Syntax](#qml-syntax)  
  * [QML](#qml)  
  * [QML Documents](#qml-documents)  
- [QML File](#qml-file)  
  * [import](#import)  
  * [QML Object Declaration](#qml-object-declaration)  
- [Integrating QML with C++](#integrating-qml-with-c)  
- [Running Individual QML Files](#running-individual-qml-files)  
- [Build System For QML](#build-system-for-qml)   
  * [qmake](#qmake)  
  * [Project Description File](#project-description-file) 
  * [Other](#other) 
- [Install lesson_1](#install-lesson_1)   
	
</details>
<!-- AUTO-GENERATED-CONTENT:END -->

<br>

## QML Syntax 
#### QML 
A declarative programing language that allows combining GUI attribute and programming logic definitions into a single object definition. It also allows attribute definitions to include imperative code to handle more complicated behavior if needed.

#### QML Documents
Standalone files of QML code. These can be used to define QML object types that can then be reused throughout an application. 

<br>

[- Visit QML Syntax Basics to learn more](https://doc.qt.io/qt-6/qtqml-syntax-basics.html)

<br>

## QML File 


Let's start by looking at a simple QML file.

```qml

// main.qml

import QtQuick 6.9

Text{
    text: "Hello World"
}  
```

<br>

### import
The first part:
```qml
import QtQuick 6.9
```

The `import` statement allows us to define to the Qt engine what resources are required by the current`.qml` file. This is like a `#include` statement in `C++` or a `import <foo> from <bar>` statement in Javascript. In this case we are using it to import a QML module. The structure for importing a QML module is:  
  
 `import <ModuleIdentifier> [<Version.Number>] [as <Qualifier>]`.  

*ModuleIdentifier = the name of the resource, like QtQuick*  
*Version.Number = version of the resource, like 6.9*  
*Qualifier = (Optional) Alias for the resource*  
  

> An import statement allows clients to tell the engine which modules, JavaScript resources and component directories are used within a QML document. 

[- Qt Documentation](https://doc.qt.io/qt-6/qtqml-syntax-imports.html#syntax-of-an-import-statement)

<br>

### QML Object Declaration
Next part:
```qml
Text{
    text: "Hello World"
}
```  
  
This is a QML object declaration. In this case we are declaring a QML object, of type `Text`, and defining its `text` property. QML code allows us to define a tree of nested QML Objects and the  properties of those QML Objects. The structure of an object declaration is:  
```qml
objectType{
    property_key: property_value
}
```    
  
*objectType = the QML objects type, like `Text`*  
*property_key = property of the QML object, like `text`*  
*property_value = value of the associated property of the QML object, like `"Hello World"`*  

<br>

## Integrating QML with C++ 

Setting up C++ with Qt is straighforward. Let's walk through a simple `.cpp` file that integrates C++ and Qt.

```cpp
#include <QtQuick>

int main(int argc, char *argv[]) {
  QGuiApplication app(argc, argv);
  QQuickView view;
  view.setSource(QUrl("qrc:/main.qml"));
  view.show();
  return app.exec();
}
```

<br>

Those familiar with C++ know the first part, including libraries/header files into the `.cpp` file. Just like with any other C++ project we need to add an `include` statement for any external dependencies the project requires. 
```cpp
#include <QtQuick>
```

> [!IMPORTANT]  
> This will require installing Qt/QML and linking it to your project.

<br>

Next:  
```cpp  
int main(int argc, char *argv[]) {
  QGuiApplication app(argc, argv);
  QQuickView view;  
  ```  
    
Again we call `main` just like any other C++ project.   
Then we initialize and create an instance of a Qt GUI Application by calling `QGuiApplication app()`. This is the core application object that manages the app's event loop and GUI lifecycle. Then we call `QQuickView view`, which creates a window that loads and displays a QML defined user interface. 

Think of it like this:   
`QGuiApplication app()` = The object that handles programming logic of the app.   
`QQuickView view` = The object that handles the graphical user interface of the app.  

<br>

Next:  
```cpp 
  view.setSource(QUrl("main.qml"));
  view.show(); 
  ```  
    
The next two lines are also straight forward. In the first line we call the `setSource()` member function of the `QQuickView` instance `view`which allows us to define the QML file/object to be loaded/used with this `QQuickView` instance. We refer to this as the  views `source`. In this case we pass `QUrl("main.qml")` which sets the source to `main.qml`. And finally we call `view.show`, this opens a native application window and shows the content loaded in `QQuickView`.    

Finally we can call `return app.exec();` to starts our event loop. The `exec()` member function is responsible for capturing and processing all interactions like button clicks, UI updates, Window Events, etc.. This is an infinite loop that runs for the entire lifetime of the app.   

> [!NOTE]
> It's best practice to keep programing logic in C++/`.cpp` files and UI in QML/`.qml`.

<br>

## Running Individual QML Files

Sometimes it can be useful to test GUI changes without having to recompile the entire application. For example:  
- Individual or small UI update   
- Programing logic not ready to be tested   
- Application takes a significant amount of time to compile      
  
In such cases there are a few options to execute individual `.qml` files without compiling any C++.   
The first option is to use `qml` CLI:  

```bash
qml main.qml
```

The second option is to use a shebang `#!` at the start of the QML file. This allows defining a binary in `PATH` to use as the interpreter for processing the `.qml` file. On linux  that would be `qml` bin in `/usr/bin/env`:  

```qml
#!/usr/bin/env qml

// ex-main.qml

import QtQuick 6.9

Text{
    text: "Hello World"
}  
```

With this change the file can now we executed directly from terminal:  
```bash
./ex-main.qml
```

<br>

## Build System For QML
### qmake

Like with C++, we will eventually need a way to simplify out build process of QML. Thankfully Qt ships with a tool called `qmake`, which can be used similarly to `make` or `cmake` and is more than enough for simple projects.   
  
Using `qmake` we can generate `Makefiles` for our project. `qmake` generates the `Makefiles` based on the project description file or `foo.po`. For example, at the root the the project source dir we can run:

```bash
qmake lesson_1.po  
```

This should generate a `.qmake.stash` and `Makefile`

<br>

### Project Description File

To run `qmake` requires a Project Description or `.po` file. This is what a basic `.po` file would look like:

```yaml
QT += quick
SOURCES += main.cpp
HEADERS += # No headers used
  
```
  
- `QT` - This section is for defining any of the Qt modules that should be included as part of the build process.   
- `SOURCES` - This section is for defning any .cpp source files.  
- `HEADERS` - This section is for defining headers that should be included as part of the build process.
  
  
It's also possible to auto-generate a Project File `.po` by using `qmake` itself. This is done by running the following command at the root of your project source directory :
```bash
qml -project
```

Example of auto-generate `.po` file:  

```yaml
######################################################################
# Automatically generated by qmake (3.1) Fri Apr 25 14:46:16 2025
######################################################################

TEMPLATE = app
TARGET = lesson_1
INCLUDEPATH += .

# You can make your code fail to compile if you use deprecated APIs.
# In order to do so, uncomment the following line.
# Please consult the documentation of the deprecated API in order to know
# how to port your code away from it.
# You can also select to disable deprecated APIs only up to a certain version of Qt.
#DEFINES += QT_DISABLE_DEPRECATED_UP_TO=0x060000 # disables all APIs deprecated in Qt 6.0.0 and earlier

# Input
SOURCES += main.cpp
QT += quick
HEADERS +=
```

Finally we can run the build commands  

```bash
qmake /path/to/projectFile.po  
make  
./main
```

<br>

### Other 
( My preffered method )    

<br>

It might be more conviniet to use other build tool-chains like CMake for more complex projects. For example, to generate `compile_commands.json` for an IDE/LSP. Refer to the [Qt Tools and utilities Documentation](https://doc.qt.io/qt-6/qt-tools-utilities.html) for information on using other build systems.    
  
This is an [example](https://doc.qt.io/qt-6/cmake-get-started.html#building-a-c-console-application) of a typical CMakeLists.txt:

```cmake
cmake_minimum_required(VERSION 3.16)

project(lesson_1 VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(Qt6 REQUIRED COMPONENTS Quick)
qt_standard_project_setup()

qt_add_executable(helloworld
    main.cpp
)

target_link_libraries(lesson_1 PRIVATE Qt6::Quick)  
```

And build commands:
```bash
# make and change to build dir
mkdir build
cd build
cmake ..
make
./lesson_1
```


## Install lesson_1

For this guide we will use CMakeLists.txt. It might seem like overkill at this point but it will be the easiest to work wwith when making changes that require linking more QML modules or C++ libraries.   
Here are the complete instructions for running lesson_1, Hello World in Qt/QML:

```bash
# make and change to build dir
mkdir build
cd build

# generate .po file 
# doesnt hurt to re-generate it if it exist
qmake ..

# Build and Install
cmake ..
make
./lesson_1
```