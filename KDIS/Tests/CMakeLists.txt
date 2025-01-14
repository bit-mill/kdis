set(PROJECT_NAME_STR KDIS)

find_package(Threads REQUIRED)
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
include(FetchContent)
FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG        703bd9caab50b139428cea1aaff9974ebee5742e # release-1.10.0
)
FetchContent_MakeAvailable(googletest)

enable_testing()
set(PROJECT_TEST_NAME ${PROJECT_NAME_STR}_tests)
include_directories(${GTEST_INCLUDE_DIRS} ${COMMON_INCLUDES})
INCLUDE_DIRECTORIES(${KDIS_SOURCE_DIR})
file(GLOB TEST_SRC_FILES UnitTests/*.cpp)
add_executable(${PROJECT_TEST_NAME} ${TEST_SRC_FILES})
target_link_libraries(${PROJECT_TEST_NAME} gtest_main)
add_test(${PROJECT_TEST_NAME} ${PROJECT_TEST_NAME})

IF(BUILD_EXAMPLES_TO_LINK_TO_LIB)

    IF(EXAMPLES_USE_STATIC_OR_SHARED_LIB MATCHES STATIC)
        TARGET_LINK_LIBRARIES(${PROJECT_TEST_NAME} KDIS_LIB)
    ENDIF(EXAMPLES_USE_STATIC_OR_SHARED_LIB MATCHES STATIC)
    
    IF(EXAMPLES_USE_STATIC_OR_SHARED_LIB MATCHES SHARED)
        TARGET_LINK_LIBRARIES(${PROJECT_TEST_NAME} KDIS_DLL)
        ADD_DEFINITIONS(-D "IMPORT_KDIS")
    ENDIF(EXAMPLES_USE_STATIC_OR_SHARED_LIB MATCHES SHARED)
    
ENDIF(BUILD_EXAMPLES_TO_LINK_TO_LIB)

IF(DIS_VERSION MATCHES 6)
	ADD_DEFINITIONS(-D "DIS_VERSION=6")
ENDIF(DIS_VERSION MATCHES 6)

IF(DIS_VERSION MATCHES 5)
	ADD_DEFINITIONS(-D "DIS_VERSION=5")
ENDIF(DIS_VERSION MATCHES 5)

IF(DIS_VERSION MATCHES 7)
	ADD_DEFINITIONS(-D "DIS_VERSION=7")
ENDIF(DIS_VERSION MATCHES 7)

IF(KDIS_USE_ENUM_DESCRIPTORS)
	ADD_DEFINITIONS(-D "KDIS_USE_ENUM_DESCRIPTORS")
ENDIF(KDIS_USE_ENUM_DESCRIPTORS) 

#target_link_libraries(${PROJECT_TEST_NAME} ${CMAKE_THREAD_LIBS_INIT})



