cmake_minimum_required(VERSION 2.8)
project(Denigma)

set(CMAKE_CXX_STANDARD14)

include_directories(src)
include_directories(lib)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

set(CMAKE_C_COMPILER /n/helmod/apps/centos7/Comp/gcc/9.2.0-fasrc01/openmpi/4.0.2-fasrc01/bin/mpicc)
set(CMAKE_CXX_COMPILER /n/helmod/apps/centos7/Comp/gcc/9.2.0-fasrc01/openmpi/4.0.2-fasrc01/bin/mpic++)


if(OPENMP_FOUND)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS} -pg")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS} -pg")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS} -pg")
endif()


set(CMAKE_CXX_FLAGS "-fopenmp -pg")
set(CMAKE_C_FLAGS "-fopenmp -pg")


add_library(ArrayUtils SHARED src/ArrayUtilities.cpp)
add_library(Sampling SHARED src/Randomize.cpp)
link_directories(${CMAKE_SOURCE_DIR}/lib)

add_executable(Denigma
src/CypherUtilities.cpp
src/decipher.cpp
src/decipher.h
src/cipherMCMC.cpp
src/FineSearch.cpp
src/ArrayUtilities.h
src/Randomize.h)


target_link_libraries(Denigma ArrayUtils Sampling)

add_executable(Ising
src/ArrayUtilities.h
src/Randomize.h
src/Ising.cpp
src/Ising.h
src/IsingMCMC.cpp)

target_link_libraries(Ising ArrayUtils Sampling)
