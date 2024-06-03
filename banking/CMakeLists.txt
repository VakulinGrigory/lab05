cmake_minimum_required(VERSION 3.4)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(banking)

# Создаем статическую библиотеку account из исходного файла Account.cpp
add_library(account STATIC ${CMAKE_CURRENT_SOURCE_DIR}/Account.cpp)
# Добавляем текущую директорию в список директорий для поиска заголовочных файлов.
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# Создаем статическую библиотеку transaction из исходного файла Transaction.cpp
add_library(transaction STATIC ${CMAKE_CURRENT_SOURCE_DIR}/Transaction.cpp)
# Устанавливаем зависимость библиотеки transaction от библиотеки account
target_link_libraries(transaction account)
