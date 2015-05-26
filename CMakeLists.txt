cmake_minimum_required (VERSION 2.6) 
project (self_tunning_spectral_clustering)

IF(CMAKE_SIZEOF_VOID_P EQUAL 4)
    SET(LIB_SUFFIX "")
ELSE(CMAKE_SIZEOF_VOID_P EQUAL 4)
    SET(LIB_SUFFIX 64)
ENDIF(CMAKE_SIZEOF_VOID_P EQUAL 4)


#ROS USAGE
SET(USE_ROS FALSE) # Set False if you want to build this package without Catkin

#TESTS
SET(COMPILE_TESTS TRUE)


SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules")


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
	set(LIB_NAME self_tunning_spectral_clustering)
else(CMAKE_BUILD_TYPE MATCHES "Debug")
	set(LIB_NAME self_tunning_spectral_clustering)
endif(CMAKE_BUILD_TYPE MATCHES "Debug")



find_package(Eigen2 REQUIRED)
include_directories(${Eigen2_INCLUDE_DIR})



file(GLOB LIB_PUBLIC_HEADERS "${PROJECT_SOURCE_DIR}/src/include/*.h")
file(GLOB LIB_SOURCES "${PROJECT_SOURCE_DIR}/src/source/*.cpp")


add_library (${LIB_NAME} STATIC ${LIB_PUBLIC_HEADERS} ${LIB_SOURCES})
#add_library (${LIB_NAME} SHARED ${LIB_PUBLIC_HEADERS} ${LIB_SOURCES})


if(COMPILE_TESTS)

include_directories(src/include)

add_executable(${LIB_NAME}_test test/clustering_test.cpp)
target_link_libraries(${LIB_NAME}_test ${LIB_NAME})

endif()


################################################################################
# Create the self_tunning_spectral_clusteringConfig.cmake file for other cmake projects.
IF(NOT USE_ROS)

	# find cmake
    	SET(self_tunning_spectral_clustering_LIBRARY_DIR ${LIBRARY_OUTPUT_PATH} )
	#Library with the full path
	SET(self_tunning_spectral_clustering_LIBRARIES "${LIBRARY_OUTPUT_PATH}/lib${PROJECT_NAME}.a" )

	SET(self_tunning_spectral_clustering_INCLUDE_DIR
		${PROJECT_SOURCE_DIR}/src/include
		)

	CONFIGURE_FILE( ${CMAKE_CURRENT_SOURCE_DIR}/cmake/self_tunning_spectral_clusteringConfig.cmake.in
		${CMAKE_CURRENT_BINARY_DIR}/cmake/self_tunning_spectral_clusteringConfig.cmake @ONLY IMMEDIATE )
		export( PACKAGE self_tunning_spectral_clustering )

	# TODO installation
	#install(
	#    TARGETS ${LIB_NAME}
	#    LIBRARY DESTINATION lib${LIB_SUFFIX}
	#)

	#install(
	#    TARGETS ${LIB_NAME}-s
	#    ARCHIVE DESTINATION lib${LIB_SUFFIX}
	#)

	#install(
	#    FILES ${LIB_PUBLIC_HEADERS}
	#    DESTINATION include/${LIB_NAME}
	#)
ENDIF()





