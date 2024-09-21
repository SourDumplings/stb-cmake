cmake_minimum_required(VERSION 3.5)
project(stb VERSION 0.9.0)

file(GLOB HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h)
file(GLOB SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c)

if(CMAKE_SOURCE_DIR STREQUAL PROJECT_SOURCE_DIR)
    if(NOT CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
        set(CMAKE_INSTALL_PREFIX "${CMAKE_PREFIX_PATH}" CACHE PATH "Install path prefix" FORCE)
    else()
        set(CMAKE_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/dist" CACHE PATH "Install path prefix" FORCE)
    endif()
else()
    if(NOT CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
        set(CMAKE_INSTALL_PREFIX "${CMAKE_PREFIX_PATH}" CACHE PATH "Install path prefix" FORCE)
    endif()
endif()

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

add_library(${PROJECT_NAME} STATIC ${SOURCES})

install(FILES ${HEADERS} DESTINATION include/${PROJECT_NAME})
install(TARGETS ${PROJECT_NAME}
    RUNTIME DESTINATION ${intermediate_bin_dir}    # .dll
    ARCHIVE DESTINATION ${CMAKE_LIBRARY_PATH}      # .lib
    LIBRARY DESTINATION ${CMAKE_LIBRARY_PATH}      # .so or .dylib
)

if (MSVC AND ${CMAKE_BUILD_TYPE} MATCHES "Debug")
    # Install .pdb files if using MSVC
    message("Installing pdb file...")
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/Debug/${PROJECT_NAME}.pdb DESTINATION lib${LIB_SUFFIX} CONFIGURATIONS Debug)
endif()