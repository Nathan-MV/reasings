# Setup the project and settings
project(reasings C)
set(PROJECT_VERSION 4.5.0)
set(API_VERSION 450)

include(GNUInstallDirs)

set(reasings_HEADER src/reasings.h)
set(reasings_SOURCE src/reasings.c)

# Read the contents of the source file
file(READ ${reasings_HEADER} FILE_CONTENTS)

# Write the contents to the destination file
file(WRITE ${reasings_SOURCE} "${FILE_CONTENTS}")

add_library(reasings STATIC ${reasings_SOURCE} ${reasings_HEADER})

set_target_properties(reasings PROPERTIES
                      PUBLIC_HEADER "${reasings_HEADER}"
                      VERSION ${PROJECT_VERSION}
                      SOVERSION ${API_VERSION}
                      )

if (WITH_PIC)
    set_property(TARGET reasings PROPERTY POSITION_INDEPENDENT_CODE ON)
endif ()

target_link_libraries(reasings PRIVATE raylib)

# Registering include directories
target_include_directories(reasings
                           PUBLIC
                           $<INSTALL_INTERFACE:include>
                           $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
                           PRIVATE
                           ${CMAKE_CURRENT_SOURCE_DIR}
                           )

# Copy the header files to the build directory for convenience
file(COPY ${reasings_HEADER} DESTINATION "include")
