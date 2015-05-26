cmake_minimum_required (VERSION 2.6) 
project (clustering)

IF(CMAKE_SIZEOF_VOID_P EQUAL 4)
    SET(LIB_SUFFIX "")
ELSE(CMAKE_SIZEOF_VOID_P EQUAL 4)
    SET(LIB_SUFFIX 64)
ENDIF(CMAKE_SIZEOF_VOID_P EQUAL 4)


#TESTS
SET(COMPILE_TESTS TRUE)


SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules")


IF(CMAKE_COMPILER_IS_GNUCC)
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
ELSE()
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
ENDIF()

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING
      "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel."
      FORCE)
endif(NOT CMAKE_BUILD_TYPE)

if(CMAKE_BUILD_TYPE MATCHES "Debug")
	set(LIB_NAME clusteringd)
else(CMAKE_BUILD_TYPE MATCHES "Debug")
	set(LIB_NAME clustering)
endif(CMAKE_BUILD_TYPE MATCHES "Debug")



find_package(Eigen2 REQUIRED)
include_directories(${Eigen2_INCLUDE_DIR})



file(GLOB LIB_PUBLIC_HEADERS "${CMAKE_SOURCE_DIR}/src/include/*.h")
file(GLOB LIB_SOURCES "${CMAKE_SOURCE_DIR}/src/source/*.cpp")


#add_library (${LIB_NAME}-s STATIC ${LIB_PUBLIC_HEADERS} ${LIB_SOURCES})
add_library (${LIB_NAME} SHARED ${LIB_PUBLIC_HEADERS} ${LIB_SOURCES})


if(COMPILE_TESTS)

include_directories(src/include)

add_executable(${LIB_NAME}_test test/clustering_test.cpp)
target_link_libraries(${LIB_NAME}_test ${LIB_NAME})

endif()


install(
    TARGETS ${LIB_NAME}
    LIBRARY DESTINATION lib${LIB_SUFFIX}
)

#install(
#    TARGETS ${LIB_NAME}-s
#    ARCHIVE DESTINATION lib${LIB_SUFFIX}
#)

install(
    FILES ${LIB_PUBLIC_HEADERS}
    DESTINATION include/${LIB_NAME}
)
