cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(banking_app)

# Добавляем путь к модулям CMake для поддержки покрытия кода (codecov)
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/third-party/CMake-codecov/cmake" ${CMAKE_MODULE_PATH})
# Добавляем директорию banking в список директорий для поиска заголовочных файлов.
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/banking)
# Добавляем поддиректорию banking для сборки.
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/banking)

if (BUILD_TESTS)
  # Добавляем поддиректорию third-party/gtest, содержащую Google Test
  add_subdirectory(third-party/gtest)
  # Включаем поддержку тестирования в проекте
  enable_testing()
  include_directories(${gtest_SOURCE_DIR}/include ${gtest_SOURCE_DIR})
  include_directories(${gmock_SOURCE_DIR}/include ${gmock_SOURCE_DIR})  
  file(GLOB ACCOUNT_TEST_SOURCES tests/*.cpp)
  add_executable(check ${ACCOUNT_TEST_SOURCES})
  target_link_libraries(check gtest transaction gtest_main account gmock)
  add_test(NAME check COMMAND check)

  if (BUILD_COVERAGE)
    set(ENABLE_COVERAGE ON CACHE BOOL "Enable coverage build." FORCE)
    list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
    find_package(codecov)
    add_coverage(check)
    list(APPEND LCOV_REMOVE_PATTERNS "'${PROJECT_SOURCE_DIR}/tests/*'")
    coverage_evaluate()
  endif ()
endif ()
