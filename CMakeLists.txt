cmake_minimum_required(VERSION 3.15)

set(mathlib_VERSION 1.0.0)

project(mathlib
	VERSION ${mathlib_VERSION}
	DESCRIPTION "A simple utf-8 based unicode decoding and encoding library"
	HOMEPAGE_URL "https://github.com/ChaimMa/mathlib"
	LANGUAGES CXX)

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

add_library(mathlib "src/mathlib.cpp" "include/mathlib.h")
add_library(MyLib::mathlib ALIAS mathlib)

target_compile_features(mathlib PUBLIC cxx_std_17)
target_include_directories(mathlib
	PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
		$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

configure_package_config_file(cmake/config.cmake.in
	${CMAKE_CURRENT_BINARY_DIR}/mathlib-config.cmake
	INSTALL_DESTINATION ${CMAKE_INSTALL_DATADIR}/mathlib
	NO_SET_AND_CHECK_MACRO)
write_basic_package_version_file(
	${CMAKE_CURRENT_BINARY_DIR}/mathlib-config-version.cmake
	VERSION ${mathlib_VERSION}
	COMPATIBILITY SameMajorVersion)
install(
	FILES
		${CMAKE_CURRENT_BINARY_DIR}/mathlib-config.cmake
		${CMAKE_CURRENT_BINARY_DIR}/mathlib-config-version.cmake
	DESTINATION
		${CMAKE_INSTALL_DATADIR}/mathlib)

install(TARGETS mathlib EXPORT mathlib-targets)
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(EXPORT mathlib-targets
	NAMESPACE mathlib::
	DESTINATION ${CMAKE_INSTALL_DATADIR}/mathlib)
