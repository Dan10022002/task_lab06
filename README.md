# Отчёт по lab06

## Homework

1. Создаём директорию и удалённый репозиторий

_alias gsed=sed<br/>
cd Dan10022002/workspace/tasks<br/>
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

4. Создаём CPackConfig.cmake и редактируем его

_touch CPackConfig.cmake<br/>
vim CPackConfig.cmake_

```sh
include(InstallRequiredSystemLibraries)
set(CPACK_PACKAGE_CONTACT m.baykalov@mail.ru)
set(CPACK_PACKAGE_VERSION_MAJOR ${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK ${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION ${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE ${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for solving")

set(CPACK_RESOURCE_FILE_LICENSE ${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README ${CMAKE_CURRENT_SOURCE_DIR}/README.md)

set(CPACK_RPM_PACKAGE_NAME "solver-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "solver")
set(CPACK_RPM_CHANGELOG_FILE ${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)

set(CPACK_DEBIAN_PACKAGE_NAME "libsolver-dev")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)

include(CPack)
```
5. Создаём DESCRIPTION

_touch DESCRIPTION_

6. Создаём README.md и редактируем его

_gsed -i 's/lab05/lab06/g' README.md_

7. Создаём LICENSE

8. Создаём ChangeLog.md и редактируем его

```sh
touch ChangeLog.md<br/>
export DATE="`LANG=en_US date +'%a %b %d %Y'`"<br/>
cat > ChangeLog.md <<EOF<br/>
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0<br/>
- Initial RPM release<br/>
EOF
```
