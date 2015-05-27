cmake_minimum_required (VERSION 2.6) 


SET(PROJECT_NAME self_tunning_spectral_clustering)

project(${PROJECT_NAME})


message("Configuring ${PROJECT_NAME}")


IF(CMAKE_SIZEOF_VOID_P EQUAL 4)
    SET(LIB_SUFFIX "")
ELSE(CMAKE_SIZEOF_VOID_P EQUAL 4)
    SET(LIB_SUFFIX 64)
ENDIF(CMAKE_SIZEOF_VOID_P EQUAL 4)


#ROS USAGE
SET(USE_ROS FALSE) # Set False if you want to build this package without Catkin

#TESTS
SET(COMPILE_TESTS TRUE)


if(NOT USE_ROS)
	set(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/lib)
	set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/bin)
endif(NOT USE_ROS)


IF(CMAKE_COMPILER_IS_GNUCC)
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
ELSE()
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
ENDIF()


set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules")

message(STATUS "CMAKE_MODULE_PATH=${CMAKE_MODULE_PATH}")



find_package(Eigen2 REQUIRED)
if(NOT Eigen2_FOUND)
    message(FATAL_ERROR "Failed to find Eigen2")
else()
    message(STATUS "Eigen2_INCLUDE_DIR=${Eigen2_INCLUDE_DIR}")
endif(NOT Eigen2_FOUND)



include_directories(${Eigen2_INCLUDE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/include/)


file(GLOB LIB_PUBLIC_HEADERS "${CMAKE_CURRENT_SOURCE_DIR}/src/include/*.h")
file(GLOB LIB_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/src/source/*.cpp")


add_library(${PROJECT_NAME} STATIC ${LIB_PUBLIC_HEADERS} ${LIB_SOURCES})
#add_library (${PROJECT_NAME} SHARED ${LIB_PUBLIC_HEADERS} ${LIB_SOURCES})



if(COMPILE_TESTS)

    include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/include)

    add_executable(${PROJECT_NAME}_test ${CMAKE_CURRENT_SOURCE_DIR}/test/clustering_test.cpp)
    target_link_libraries(${PROJECT_NAME}_test ${PROJECT_NAME})

endif()


################################################################################
# Create the self_tunning_spectral_clusteringConfig.cmake file for other cmake projects.
IF(NOT USE_ROS)

	# find cmake
    	SET(self_tunning_spectral_clustering_LIBRARY_DIR ${LIBRARY_OUTPUT_PATH} )
	#Library with the full path
	SET(self_tunning_spectral_clustering_LIBRARY "${LIBRARY_OUTPUT_PATH}/lib${PROJECT_NAME}.a" )

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





