cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
rosbuild_genmsg()
#uncomment if you have defined services
rosbuild_gensrv()

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)

set(CAN_SOURCE_DIR
 ../../..
)


include_directories(
	${CAN_SOURCE_DIR}/include
	${CAN_SOURCE_DIR}/src/libcanplusplus/SL
	${CAN_SOURCE_DIR}/src/libcanplusplus/CANSharedMemory
	${CAN_SOURCE_DIR}/src/libcanplusplus/CANMessage
	${CAN_SOURCE_DIR}/src/libcanplusplus/bus
	${CAN_SOURCE_DIR}/src/libcanplusplus/devices
	${CAN_SOURCE_DIR}/src/libcanplusplus/devices/ELMOMotor
	.
	./include
	./src/devices
	./src/state_machine
	

)
link_directories(${CAN_SOURCE_DIR}/../lib)

#MESSAGE(STATUS "${LIBRARY_OUTPUT_PATH}")
set(SRCS
	./src/hdpc_com_main.cpp
	./src/devices/DeviceELMOBaseMotor.cpp
	./src/devices/DeviceELMOSteeringMotor.cpp
	./src/devices/DeviceELMODrivingMotor.cpp
	./src/state_machine/HDPCStateMachine.cpp
	./src/state_machine/HDPCStates.cpp
)

rosbuild_add_executable(hdpc_com ${SRCS})
target_link_libraries(hdpc_com m bluetooth cpc pthread rt CAN)