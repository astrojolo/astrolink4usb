cmake_minimum_required(VERSION 3.10)
project(indi-astrolink4usb VERSION 1.0 LANGUAGES CXX C)

# Expose major/minor for compatibility
set(ASTROLINK4_VERSION_MAJOR ${PROJECT_VERSION_MAJOR})
set(ASTROLINK4_VERSION_MINOR ${PROJECT_VERSION_MINOR})

# Modules and install dirs
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake_modules/" ${CMAKE_MODULE_PATH})
include(GNUInstallDirs)

# Use modern C++ standard settings
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(INDI REQUIRED)

# Configure files
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/config.h.cmake ${CMAKE_CURRENT_BINARY_DIR}/config.h)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/indi_astrolink4usb.xml.cmake ${CMAKE_CURRENT_BINARY_DIR}/indi_astrolink4usb.xml)

include(CMakeCommon)

################ AstroLink4 ################

set(indi_astrolink4usb_SRCS
    ${CMAKE_CURRENT_SOURCE_DIR}/indi_astrolink4usb.cpp
)

add_executable(indi_astrolink4usb ${indi_astrolink4usb_SRCS})

# Modern include handling
target_include_directories(indi_astrolink4usb
    PRIVATE
        ${CMAKE_CURRENT_BINARY_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${INDI_INCLUDE_DIR}
)

# Link against indidriver target if available, otherwise use provided INDI libraries variable
if(TARGET indidriver)
    target_link_libraries(indi_astrolink4usb PRIVATE indidriver)
elseif(DEFINED INDI_LIBRARIES)
    target_link_libraries(indi_astrolink4usb PRIVATE ${INDI_LIBRARIES})
else()
    message(WARNING "Could not find indidriver target or INDI_LIBRARIES; linking may fail.")
endif()

# Install
install(TARGETS indi_astrolink4usb RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/indi_astrolink4usb.xml DESTINATION ${INDI_DATA_DIR})