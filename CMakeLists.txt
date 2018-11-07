cmake_minimum_required(VERSION 3.0)

project(mongoose C)

add_library(mongoose
    mongoose.c)

find_package(OpenSSL)
option(ENABLE_SSL "Enable SSL support" OFF)

target_include_directories(mongoose PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include/mongoose>)

if(WIN32)
    target_link_libraries(mongoose PUBLIC wsock32 ws2_32)
    target_compile_definitions(mongoose PUBLIC WIN32)
endif()
if(ENABLE_SSL)
    target_link_libraries(mongoose PUBLIC OpenSSL::SSL)
    target_compile_definitions(mongoose PUBLIC NS_ENABLE_SSL)
endif()

if(BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()

install(
        TARGETS mongoose
        EXPORT mongoose-cmake
        ARCHIVE DESTINATION lib/
        LIBRARY DESTINATION lib/
        RUNTIME DESTINATION bin/)

install(
        FILES include/mongoose.h
        DESTINATION include/mongoose)

install(
        EXPORT mongoose-cmake
        FILE mongoose-config.cmake
        NAMESPACE mongoose::
        DESTINATION share/mongoose/cmake)
