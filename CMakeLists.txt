cmake_minimum_required(VERSION 3.8...3.14)

if(${CMAKE_VERSION} VERSION_LESS 3.12)
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

project(WxTest LANGUAGES C CXX)

option(WXWIDGETS_VERSION "Version of wxWdigets package to use" 3.1.1)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_LIST_DIR}/cmake")
include(conan)

conan_check(VERSION 1.0.0 REQUIRED)
conan_add_remote(NAME bincrafters URL https://api.bintray.com/conan/bincrafters/public-conan)

conan_cmake_run(REQUIRES 
                    wxwidgets/${WXWIDGETS_VERSION}@bincrafters/stable
                BASIC_SETUP CMAKE_TARGETS
                BUILD missing)

add_executable(WxTest src/main.cpp)

target_link_libraries(WxTest 
	PRIVATE
		CONAN_PKG::wxwidgets
)