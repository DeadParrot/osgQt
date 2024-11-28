IF(Qt6Widgets_FOUND)
    include(GenerateExportHeader)

    # Library name
    SET(LIB_NAME osgQOpenGL)

    # Define header and source paths
    SET(HEADER_PATH ${CMAKE_CURRENT_SOURCE_DIR}/../../include/osgQOpenGL)
    SET(SOURCE_PATH ${CMAKE_CURRENT_SOURCE_DIR})

    # List of headers (use full paths)
    SET(SOURCE_H
        ${HEADER_PATH}/osgQOpenGLWidget.h
        ${HEADER_PATH}/osgQOpenGLWindow.h
        ${HEADER_PATH}/OSGRenderer.h
    )

    # List of source files
    SET(SOURCE_CPP
        ${SOURCE_PATH}/CullVisitorEx.cpp
        ${SOURCE_PATH}/GraphicsWindowEx.cpp
        ${SOURCE_PATH}/osgQOpenGLWidget.cpp
        ${SOURCE_PATH}/osgQOpenGLWindow.cpp
        ${SOURCE_PATH}/OSGRenderer.cpp
        ${SOURCE_PATH}/RenderStageEx.cpp
        ${SOURCE_PATH}/StateEx.cpp
    )

    # Run MOC for Qt6 headers
    qt6_wrap_cpp(SOURCES_H_MOC ${SOURCE_H})

    # Add the library target
    add_library(${LIB_NAME}
        ${SOURCE_CPP}
        ${SOURCES_H_MOC}
    )

    # Include directories
    target_include_directories(${LIB_NAME} PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/../../include    # Include parent include folder
        ${CMAKE_CURRENT_BINARY_DIR}                 # For generated headers
    )

    # Link Qt6 libraries
    target_link_libraries(${LIB_NAME} PRIVATE
        Qt6::Widgets Qt6::OpenGL Qt6::OpenGLWidgets
    )

    # Generate export header
    generate_export_header(${LIB_NAME} EXPORT_FILE_NAME "${CMAKE_CURRENT_BINARY_DIR}/Export.h")

    # Installation rules (optional)
    install(TARGETS ${LIB_NAME}
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
    )
    install(DIRECTORY ${HEADER_PATH}/
        DESTINATION include/osgQOpenGL
        FILES_MATCHING PATTERN "*.h"
    )
ENDIF()
