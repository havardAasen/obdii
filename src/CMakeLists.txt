add_executable(obdii_cli cli.c)
target_link_libraries(obdii_cli obdii)

set(LIBRARY_SOURCES
        OBDII.c
        OBDIICommunication.c)

if(BUILD_SHARED)
    add_library(obdii_shared SHARED ${LIBRARY_SOURCES})
    set_target_properties(obdii_shared PROPERTIES OUTPUT_NAME obdii)
    target_link_directories(obdii_shared PUBLIC
                            "${PROJECT_BINARY_DIR}"
                            "${PROJECT_SOURCE_DIR}/src")
endif()


if(BUILD_STATIC)
    add_library(obdii_static STATIC ${LIBRARY_SOURCES})
    set_target_properties(obdii_static PROPERTIES OUTPUT_NAME obdii)
    target_link_directories(obdii_static PUBLIC
                            "${PROJECT_BINARY_DIR}"
                            "${PROJECT_SOURCE_DIR}/src")
endif()


install(FILES OBDII.h OBDIICommunication.h
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR} COMPONENT DEV)
if(BUILD_SHARED)
    install(TARGETS obdii_shared
            LIBRARY COMPONENT LIB
            ARCHIVE COMPONENT DEV
            RUNTIME COMPONENT LIB)
endif()

if(BUILD_STATIC)
    install(TARGETS obdii_static COMPONENT DEV)
endif()
