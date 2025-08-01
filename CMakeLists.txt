cmake_minimum_required(VERSION 3.16)
project(mpi_bridge)

set(CMAKE_CXX_STANDARD 20)

set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/lib/hedgehog/cmake_modules")

set(Hedgehog_INCLUDE_DIR lib/hedgehog/)

# Need to defined variable Hedgehog_INCLUDE_DIR to the hedgehog path hedgehog
find_package(Hedgehog REQUIRED)
find_package(MPI REQUIRED)

set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} ${Hedgehog_CXX_FLAGS})

if (NOT MSVC)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wextra -Werror -Wuninitialized -pedantic -g")
endif (NOT MSVC)

# files
set(source_files src/main.cpp)

# executable
add_executable(mpi_bridge ${source_files})
add_definitions(-DOMPI_SKIP_MPICXX)
target_include_directories(mpi_bridge
    PUBLIC
    lib/serializer-cpp/
    lib/hedgehog/
    ${MPI_INCLUDE_PATH}
)
target_link_libraries(mpi_bridge
    PRIVATE
    ${MPI_C_LIBRARIES}
)
