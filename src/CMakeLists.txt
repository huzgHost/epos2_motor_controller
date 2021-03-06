# driver source files
SET(sources Epos2.cpp)
# application header files
SET(headers Epos2.h)

# locate the necessary dependencies
FIND_PACKAGE(LibFTDI1 NO_MODULE REQUIRED)

# add the necessary include directories
INCLUDE_DIRECTORIES(.)
INCLUDE(${LIBFTDI_USE_FILE})

# create the shared library
ADD_LIBRARY(epos2 SHARED ${sources})

# link necessary libraries
TARGET_LINK_LIBRARIES(epos2 ${LIBFTDIPP_LIBRARIES})

SET_TARGET_PROPERTIES(epos2 PROPERTIES VERSION ${VERSION} SOVERSION 0)

INSTALL(TARGETS epos2
  LIBRARY DESTINATION lib${LIB_SUFFIX}
  COMPONENT sharedlibs
)

INSTALL(FILES ${headers} DESTINATION include/${PROJECT_NAME} COMPONENT headers)

SET(LIBEPOS2_INCLUDE_DIR ${includedir})
SET(LIBEPOS2_INCLUDE_DIRS ${LIBEPOS2_INCLUDE_DIR})
GET_TARGET_PROPERTY(_LIBEPOS2_LIBRARY_LOCATION epos2 LOCATION)
GET_FILENAME_COMPONENT(_LIBEPOS2_LIBRARY_NAME ${_LIBEPOS2_LIBRARY_LOCATION} NAME)
SET(LIBEPOS2_LIBRARY ${libdir}/${_LIBEPOS2_LIBRARY_NAME})
SET(LIBEPOS2_LIBRARIES ${LIBEPOS2_LIBRARY})
LIST(APPEND LIBEPOS2_LIBRARIES ${LIBUSB_LIBRARIES})
SET(LIBEPOS2_ROOT_DIR ${prefix})
SET(LIBEPOS2_VERSION_STRING ${VERSION_STRING})
SET(LIBEPOS2_VERSION_MAJOR ${MAJOR_VERSION})
SET(LIBEPOS2_VERSION_MINOR ${MINOR_VERSION})
SET(LIBEPOS2_USE_FILE ${CMAKE_INSTALL_PREFIX}/${LIBEPOS2_CMAKE_CONFIG_DIR}/UseLibEPOS2.cmake)
CONFIGURE_FILE(../LibEPOS2Config.cmake.in ${CMAKE_CURRENT_BINARY_DIR}/LibEPOS2Config.cmake @ONLY)
INSTALL(FILES ${CMAKE_CURRENT_BINARY_DIR}/LibEPOS2Config.cmake ../UseLibEPOS2.cmake DESTINATION ${LIBEPOS2_CMAKE_CONFIG_DIR})

ADD_SUBDIRECTORY(examples)
