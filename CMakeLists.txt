cmake_minimum_required(VERSION 3.1.0)
project(mesh_processing)

#======================= INCLUSION OF QT ======================#
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_PREFIX_PATH $ENV{QTDIR})
set(CMAKE_INCLUDE_CURRENT_DIR ON)

find_package(OpenGL)
find_package(Qt5Core REQUIRED)
find_package(Qt5Gui REQUIRED)
find_package(Qt5OpenGL REQUIRED)
find_package(Qt5Xml REQUIRED)
find_package(Qt5Widgets REQUIRED)

#======================= INCLUSION OF VTK ======================#
find_package(VTK REQUIRED)
include(${VTK_USE_FILE})

#======================= INCLUSION OF Our Code ======================#
set(FORMS_DIR "${CMAKE_SOURCE_DIR}/forms")
set(INCLUDE_DIR "${CMAKE_SOURCE_DIR}/include")
set(SOURCE_DIR "${CMAKE_SOURCE_DIR}/src")
set(RESOURCE_DIR "${CMAKE_SOURCE_DIR}/Resources")

include_directories(${FORMS_DIR})
include_directories(${INCLUDE_DIR})
include_directories(${SOURCE_DIR})
include_directories(${RESOURCE_DIR})

# Add the forms directory to the AUTOUIC search paths
set(CMAKE_AUTOUIC_SEARCH_PATHS ${CMAKE_AUTOUIC_SEARCH_PATHS} ${FORMS_DIR})

set(Srcs ${SOURCE_DIR}/main.cpp ${SOURCE_DIR}/color_table_handler.cpp ${SOURCE_DIR}/icp_algorithm.cpp ${SOURCE_DIR}/mesh_operation.cpp ${SOURCE_DIR}/mesh_processing.cpp ${SOURCE_DIR}/mesh_processing_data_model.cpp ${SOURCE_DIR}/mesh_processing_interactor_style.cpp ${SOURCE_DIR}/mesh_segmenter.cpp ${SOURCE_DIR}/vec3.cpp ${SOURCE_DIR}/vtk_widget.cpp ${SOURCE_DIR}/vtkOFFReader.cxx)
set(Hdrs ${INCLUDE_DIR}/fibonacci_heap.h ${INCLUDE_DIR}/color_table_handler.h ${INCLUDE_DIR}/icp_algorithm.h ${INCLUDE_DIR}/mesh_operation.h ${INCLUDE_DIR}/mesh_processing.h ${INCLUDE_DIR}/mesh_processing_data_model.h ${INCLUDE_DIR}/mesh_processing_interactor_style.h ${INCLUDE_DIR}/mesh_segmenter.h ${INCLUDE_DIR}/vec3.h ${INCLUDE_DIR}/vtk_widget.h ${INCLUDE_DIR}/vtkOFFReader.h ${INCLUDE_DIR}/timer.h ${INCLUDE_DIR}/min_heap.h)
set(UIs ${FORMS_DIR}/mesh_processing.ui )

# Add the executable
add_executable(mesh_processing WIN32 ${Srcs} ${Hdrs} ${UI_Srcs})

# Add the target includes
target_include_directories(mesh_processing PRIVATE ${FORMS_DIR})
target_include_directories(mesh_processing PRIVATE ${INCLUDE_DIR})
target_include_directories(mesh_processing PRIVATE ${SOURCE_DIR})

# Link libraries
target_link_libraries(mesh_processing Qt5::OpenGL)
target_link_libraries(mesh_processing Qt5::Xml)
target_link_libraries(mesh_processing Qt5::Widgets)
target_link_libraries(mesh_processing ${QT_LIBRARIES} ${OPENGL_LIBRARIES})
target_link_libraries(mesh_processing ${VTK_LIBRARIES})

# Copy resources
file(COPY ${RESOURCE_DIR} DESTINATION ${CMAKE_SOURCE_DIR}/build)
file(COPY ${RESOURCE_DIR} DESTINATION ${CMAKE_SOURCE_DIR}/build/Debug)
file(COPY ${RESOURCE_DIR} DESTINATION ${CMAKE_SOURCE_DIR}/build/Release)