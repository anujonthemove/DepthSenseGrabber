cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

project(DepthSenseGrabberOpenCV)


FIND_PACKAGE( OpenCV REQUIRED )

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

set(DEPTHSENSESDK_DIR /opt/softkinetic/DepthSenseSDK)
set(DEPTHSENSESDK_INCLUDE_DIR ${DEPTHSENSESDK_DIR}/include)
set(DEPTHSENSESDK_LIBRARY_DIR ${DEPTHSENSESDK_DIR}/lib)
set(DEPTHSENSESDK_LIBRARIES ${DEPTHSENSESDK_LIBRARY_DIR}/libDepthSense.so ${DEPTHSENSESDK_LIBRARY_DIR}/libDepthSensePlugins.so ${DEPTHSENSESDK_LIBRARY_DIR}/libturbojpeg.so ${DEPTHSENSESDK_LIBRARY_DIR}/libusb-1.0.so)

#set(DEPTHSENSEGRABBER_DIR /home/anuj/Desktop/DepthSenseGrabber/build/DepthSenseGrabberCore)

find_package(Boost 1.46.0 COMPONENTS system thread REQUIRED)
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})


add_library(DepthSenseGrabberCore
	../DepthSenseGrabberCore/DepthSenseGrabberCore.hxx
	../DepthSenseGrabberCore/DepthSenseGrabberCore.cxx

	../shared/ConversionTools.cxx
	../shared/ConversionTools.hxx
)

include_directories(${DEPTHSENSESDK_INCLUDE_DIR})
link_directories(${DEPTHSENSESDK_LIBRARY_DIR})




add_executable (DepthSenseGrabberOpenCV
	DepthSenseGrabberOpenCV.cxx
	DepthSenseGrabberOpenCV.hxx
	../DepthSenseGrabberCore/DepthSenseGrabberCore.hxx
	../shared/ConversionTools.cxx
	../shared/ConversionTools.hxx
)

target_link_libraries (DepthSenseGrabberOpenCV ${OpenCV_LIBS} DepthSenseGrabberCore  ${DEPTHSENSESDK_LIBRARIES} ${Boost_LIBRARIES})