# Отчёт по lab06

## Homework

1. Создаём директорию и удалённый репозиторий

_cd Dan10022002/workspace/tasks<br/>
mkdir task_lab06<br/>
cd task_lab06_

2. Клонируем lab03 и удаляем вс файлы, кроме CMakeLists.txt, LIСENSE, заголовочных файлов и фалов реализаций

_git clone https://github.com/Dan10022002/timp_task_lab03.git<br/>
git remote remove origin<br/>
git remote add origin https://github.com/Dan10022002/task_lab06_

3. Переходим в папку solver_application и редактируем файл CMakeLists.txt

_cd solver_application<br/>
touch CMakeLists.txt<br/>
vim CMakeLists.txt_

```sh
cmake_minimum_required(VERSION 3.4)
project(solver)

set(PRINT_VERSION_MAJOR 0)
set(PRINT_VERSION_MINOR 1)
set(PRINT_VERSION_PATCH 0)
set(PRINT_VERSION_TWEAK 0)
set(PRINT_VERSION
  ${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})
set(PRINT_VERSION_STRING "v${PRINT_VERSION}")

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(myExe equation.cpp)

add_library(formatter STATIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib/formatter.cpp)
add_library(formatter_ex STATIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/formatter_ex.cpp)
add_library(solver_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib/solver.cpp)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib)

target_link_libraries(formatter_ex formatter)

target_link_libraries(myExe formatter_ex)
target_link_libraries(myExe solver_lib)

install(TARGETS solver_lib formatter formatter_ex
	EXPORT solver_lib-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib/ DESTINATION include)
install(EXPORT solver_lib-config DESTINATION cmake)

include(CPackConfig.cmake)
```
