cmake_minimum_required(VERSION 3.2)
PROJECT(indi-astrolink4usb CXX C)

set (ASTROLINK4_VERSION_MAJOR 1)
set (ASTROLINK4_VERSION_MINOR 0)

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake_modules/")

SET(CMAKE_CXX_STANDARD 11)

find_package(INDI REQUIRED)

include_directories( ${CMAKE_CURRENT_BINARY_DIR})
include_directories( ${CMAKE_CURRENT_SOURCE_DIR})
include_directories( ${INDI_INCLUDE_DIR})

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/config.h.cmake ${CMAKE_CURRENT_BINARY_DIR}/config.h )
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/indi_astrolink4usb.xml.cmake ${CMAKE_CURRENT_BINARY_DIR}/indi_astrolink4usb.xml )

################ AstroLink4 ################

set(indi_astrolink4usb_SRCS
        ${CMAKE_CURRENT_SOURCE_DIR}/indi_astrolink4usb.cpp
   )

add_executable(indi_astrolink4usb ${indi_astrolink4usb_SRCS})
target_link_libraries(indi_astrolink4usb indidriver)

install(TARGETS indi_astrolink4usb RUNTIME DESTINATION bin )
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/indi_astrolink4usb.xml DESTINATION ${INDI_DATA_DIR})

