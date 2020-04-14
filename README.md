## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```ShellSession
$ open https://cmake.org/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Выполнить инструкцию учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя> #initialize value GITHUB_USERNAME
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace #go to the workspace directory
$ pushd . #save the current working directory in memory so it can be returned to at any time
/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace /mnt/c/Users/dns/Documents/GitHub ~
$ source scripts/activate #read and execute file activate
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03 #clone the previous lab
Cloning into 'projects/lab03'...
remote: Enumerating objects: 19, done.
remote: Counting objects: 100% (19/19), done.
remote: Compressing objects: 100% (14/14), done.
remote: Total 19 (delta 1), reused 13 (delta 0), pack-reused 0
Unpacking objects: 100% (19/19), done.
$ cd projects/lab03 #go to the current lab directory
$ git remote remove origin #remove remote origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git #add remote origin form the url
```

```ShellSession
$ g++ -std=c++11 -I./include -c sources/print.cpp #make compilation file from the source file print.cpp
$ ls print.o #see the object file from the source file print.cpp
print.o
$ nm print.o | grep print #lists symbols from object file and find word "print"
0000000000000095 t _GLOBAL__sub_I__Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000000 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000026 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
$ ar rvs print.a print.o #make a static library print.a from print.o
ar: creating print.a
a - print.o
$ file print.a #output an information about the file
print.a: current ar archive  puchkovki        puchkovki                        puchkovki
$ g++ -std=c++11 -I./include -c examples/example1.cpp #make compilation file from the source file example1.cpp
$ ls example1.o #see the object file from the source file example1.cpp
example1.o
$ g++ example1.o print.a -o example1 #linking stage
$ ./example1 && echo #execute binary file and output the result on the next line
hello
```

```ShellSession
$ g++ -std=c++11 -I./include -c examples/example2.cpp #make compilation file from the source file example2.cpp
$ nm example2.o #lists symbols from object file
0000000000000000 V DW.ref.__gxx_personality_v0
                 U _GLOBAL_OFFSET_TABLE_
000000000000016f t _GLOBAL__sub_I_main
                 U _Unwind_Resume
0000000000000126 t _Z41__static_initialization_and_destruction_0ii
                 U _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
                 U _ZNSaIcEC1Ev
                 U _ZNSaIcED1Ev
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEEC1EPKcSt13_Ios_Openmode
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEED1Ev
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEED1Ev
                 U _ZNSt8ios_base4InitC1Ev
                 U _ZNSt8ios_base4InitD1Ev
0000000000000000 r _ZStL19piecewise_construct
0000000000000000 b _ZStL8__ioinit
0000000000000000 W _ZStorSt13_Ios_OpenmodeS_
                 U __cxa_atexit
                 U __dso_handle
                 U __gxx_personality_v0
                 U __stack_chk_fail
0000000000000000 T main
$ g++ example2.o print.a -o example2 ##linking stage
$ ./example2 #execute binary file
$ cat log.txt && echo #see the result on the next line
hello
```

```ShellSession
$ rm -rf example1.o example2.o print.o #delete object files
$ rm -rf print.a #delete a static library
$ rm -rf example1 example2 #delete binaries
$ rm -rf log.txt #delete output artefacts
```

```ShellSession
$ cat > CMakeLists.txt <<EOF #define the minimum required cmake version for projects building and name the project
cmake_minimum_required(VERSION 3.4)
project(print)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF #set variables CMAKE_CXX_STANDARD and CMAKE_CXX_STANDARD_REQUIRED
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF #create a static library from object file from source file print.cpp
add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF #where to find headers
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```

```ShellSession
$ cmake -H. -B_build #1-st parametr — a directory where to look for the configuration files CMakeLists, 2-nd — a directory where will be all projects and temporary files 
-- The C compiler identification is GNU 7.4.0
-- The CXX compiler identification is GNU 7.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab03/_build
$ cmake --build _build #preprocessing, compilation and linking
Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF #add targets for the building executable files
add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF #add libraries for the executable files
target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

```ShellSession
$ cmake --build _build #build modified files
-- Configuring done
-- Generating done
-- Build files have been written to: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab03/_build
[ 33%] Built target print
Scanning dependencies of target example2
[ 50%] Building CXX object CMakeFiles/example2.dir/examples/example2.cpp.o
[ 66%] Linking CXX executable example2
[ 66%] Built target example2
Scanning dependencies of target example1
[ 83%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
[100%] Linking CXX executable example1
[100%] Built target example1
$ cmake --build _build --target print #build print file (already done)
[100%] Built target print
$ cmake --build _build --target example1 #build example1
[ 50%] Built target print
[100%] Built target example1
$ cmake --build _build --target example2 #build example2
[ 50%] Built target print
[100%] Built target example2
```

```ShellSession
$ ls -la _build/libprint.a #checking the workability of the built executable files
-rwxrwxrwx 1 puchkovki puchkovki 3134 Mar 19 16:15 _build/libprint.a
$ _build/example1 && echo #checking the workability of the built executable files
hello
$ _build/example2 #checking the workability of the built executable files
$ cat log.txt && echo #checking the workability of the built executable files
hello
$ rm -rf log.txt #delete the temporary files
```

```ShellSession
$ git clone https://github.com/tp-labs/lab03 tmp #update CMake configuration file
Cloning into 'tmp'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 76 (delta 0), reused 0 (delta 0), pack-reused 73
Unpacking objects: 100% (76/76), done.
$ mv -f tmp/CMakeLists.txt . #get the CMake file
$ rm -rf tmp #delete the usefull files
```

```ShellSession
$ cat CMakeLists.txt #output CMake file
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)

project(print)

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)
if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
    target_link_libraries(${EXAMPLE_NAME} print)
    install(TARGETS ${EXAMPLE_NAME}
      RUNTIME DESTINATION bin
    )
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install #redefine variable DCMAKE_INSTALL_PREFIX
-- Configuring done
-- Generating done
-- Build files have been written to: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab03/_build
$ cmake --build _build --target install #build install file
[100%] Built target print
Install the project...
-- Install configuration: ""
-- Installing: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab03/_install/lib/libprint.a
-- Installing: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab03/_install/include
-- Installing: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab03/_install/include/print.hpp
-- Installing: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab03/_install/cmake/print-config.cmake
-- Installing: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab03/_install/cmake/print-config-noconfig.cmake
$ tree _install #show the configuration tree
_install
├ cmake
│   ├ print-config-noconfig.cmake
│   └ print-config.cmake
├ include
│   └ print.hpp
└ lib
    └ libprint.a
	
3 directories, 4 files
```

```ShellSession
$ git add CMakeLists.txt #add changes
$ git commit -m"added CMakeLists.txt" #commit changes
[master 2e7ab07] added CMakeLists.txt
 1 file changed, 36 insertions(+)
 create mode 100644 CMakeLists.txt
$ git push origin master #push changes
Username for 'https://github.com': puchkovki
Password for 'https://puchkovki@github.com':
Counting objects: 22, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (17/17), done.
Writing objects: 100% (22/22), 3.79 KiB | 73.00 KiB/s, done.
Total 22 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), done.
To https://github.com/puchkovki/lab03.git
 * [new branch]      master -> master
```

## Report

```ShellSession
$ popd #get the directory from the stack
/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace /mnt/c/Users/dns/Documents/GitHub ~
$ export LAB_NUMBER=03 #initialize the variable LAB_NUMBER
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER} #clone lab
Cloning into 'tasks/lab03'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 76 (delta 0), reused 0 (delta 0), pack-reused 73
Unpacking objects: 100% (76/76), done.
$ mkdir reports/lab${LAB_NUMBER} #make directory
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md #copy and rename file
$ cd reports/lab${LAB_NUMBER} #change directory
$ edit REPORT.md #make report
$ gist REPORT.md 
```

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2019 The ISC Authors
```
