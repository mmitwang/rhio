###
### Rhoban Input Output Libray
### Rhoban 2015
###

cmake_minimum_required(VERSION 2.8)
project(RhIO)

#Option for compiling Tests
option(BUILD_RHIO_TESTS "Build RhIO Tests" ON)  

#Include sources directory
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/Server)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/Common)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/Client)

#Include ZMQ (Network Library)
set(ZMQ_DIRECTORY "ZeroMQ" CACHE STRING "ZMQ lib directory")
include_directories(${ZMQ_DIRECTORY}/include)
link_directories(${ZMQ_DIRECTORY}/lib)

#Sources files
set(SOURCES_FILES_SERVER
    Server/IONode.cpp
    Server/RhIO.cpp
    Server/ServerRep.cpp
    Server/ValueNode.cpp
)
set(SOURCES_FILES_COMMON
    Common/DataBuffer.cpp
)
set(SOURCES_FILES_CLIENT
    Client/ClientReq.cpp
)

#Needed Libraries
set(RHIO_LIBRARIES
    zmq
    pthread
)

#Enable C++11
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
#Enable compiler Warning
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -W -Wall")

#Build Server and Client as static library
add_library(RhIO STATIC ${SOURCES_FILES_SERVER} ${SOURCES_FILES_COMMON})
target_link_libraries(RhIO ${RHIO_LIBRARIES})
add_library(RhIOClient STATIC ${SOURCES_FILES_CLIENT} ${SOURCES_FILES_COMMON})
target_link_libraries(RhIOClient ${RHIO_LIBRARIES})

#Build Tests
if (BUILD_RHIO_TESTS)
    add_executable(testTree Tests/testTree.cpp)
    target_link_libraries(testTree RhIO)
    add_executable(testValue Tests/testValue.cpp)
    target_link_libraries(testValue RhIO)
    add_executable(testServerRep Tests/testServerRep.cpp)
    target_link_libraries(testServerRep RhIO)
    add_executable(testClientReq Tests/testClientReq.cpp)
    target_link_libraries(testClientReq RhIOClient)
endif (BUILD_RHIO_TESTS)

