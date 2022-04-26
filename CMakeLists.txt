cmake_minimum_required(VERSION 3.18)
project(jacobi_mpi_avx)

set(CMAKE_CXX_STANDARD 17)

if (UNIX)
    find_package(MPI REQUIRED)
    include_directories(SYSTEM ${MPI_INCLUDE_PATH})
    include_directories(header)
else () # WINDOWS
    set(CMAKE_EXE_LINKER_FLAGS "-static")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=native -mavx")

    include_directories(header "C:\\Program Files (x86)\\Microsoft SDKs\\MPI\\Include")
    link_directories("c:\\Program Files (x86)\\Microsoft SDKs\\MPI\\Lib\\x86" "c:\\Program Files (x86)\\Microsoft SDKs\\MPI\\Lib\\x64")
endif ()

add_executable(jacobi_mpi_avx main.cpp header/mpi.h)

if (UNIX)
    target_link_libraries(jacobi_mpi_avx ${MPI_CXX_LIBRARIES})
else ()
    target_link_libraries(jacobi_mpi_avx msmpi.lib msmpifec.lib msmpifmc.lib)
endif ()