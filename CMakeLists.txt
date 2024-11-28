cmake_minimum_required(VERSION 2.8.3)

set(CXX_DISABLE_WERROR TRUE)

set(PROJECT_NAME agimus_demos)
set(PROJECT_DESCRIPTION "Demonstration of Agimus with various robots")
set(PROJECT_URL "https://github.com/agimus/agimus-demos")
set(PROJECT_USE_CMAKE_EXPORT TRUE)

include(cmake/base.cmake)
include(cmake/test.cmake)
include(cmake/python.cmake)

compute_project_args(PROJECT_ARGS LANGUAGES CXX)
project(${PROJECT_NAME} ${PROJECT_ARGS})

OPTION (USE_SOT "Wether to trigger dependency to the Stack Of Tasks" OFF)

if(USE_SOT)
  add_project_dependency (dynamic-graph-python 4 REQUIRED)
  add_project_dependency (dynamic_graph_bridge 3 REQUIRED)
endif()

findpython()
find_package(Eigen3)

OPTION(USE_FRANKA "Install files for franka robot" OFF)
OPTION(USE_TALOS "Install files for talos robot" OFF)
OPTION(USE_TIAGO "Install files for tiago robot" OFF)
OPTION(USE_UR3 "Install files for UR3 robot" OFF)
OPTION(USE_UR5 "Install files for UR5 robot" OFF)
OPTION(USE_UR10 "Install files for UR10 robot" OFF)

IF(USE_FRANKA)
  ADD_SUBDIRECTORY(franka)
ENDIF()
IF(USE_TALOS)
  ADD_SUBDIRECTORY(talos)
ENDIF()
IF(USE_TIAGO)
  ADD_SUBDIRECTORY(tiago)
  add_subdirectory(tests)
ENDIF()
IF(USE_UR3)
  ADD_SUBDIRECTORY(ur3)
ENDIF()
IF(USE_UR5)
  ADD_SUBDIRECTORY(ur5)
ENDIF()
IF(USE_UR10)
  ADD_SUBDIRECTORY(ur10)
ENDIF()

add_subdirectory(src)

# Install other files with the same hierarchy as here.
install(DIRECTORY urdf srdf meshes worlds
  DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME}
  FILES_MATCHING
  PATTERN "*"
  PATTERN "*.launch" EXCLUDE
)

INSTALL(FILES plugin.xml
    DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME})

install(PROGRAMS
  scripts/gazebo_object_to_tf.py
  scripts/visual_tag_initializer.py
  scripts/calibration_frames_grabber.py
  scripts/publish_constants.py
  scripts/group_of_tags.py
  scripts/publishwmc.py
  scripts/publish_camera_extrinsics.py
  scripts/rqt_tooltip_calibration
  scripts/rqt_contact_calibration
  scripts/rqt_camera_calibration
  DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME}/scripts)
