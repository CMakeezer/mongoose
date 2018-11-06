cmake_minimum_required(VERSION 3.0)

project(mongoose C)

add_library(mongoose
    mongoose.c)

target_include_directories(mongoose PUBLIC 
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
	$<INSTALL_INTERFACE:include/mongoose>)

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
