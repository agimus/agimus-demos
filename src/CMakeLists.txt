# Copyright 2020, 2021 CNRS - Airbus SAS
# Author: Florent Lamiraux, Joseph Mirabel
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are
# met:

# 1. Redistributions of source code must retain the above copyright
# notice, this list of conditions and the following disclaimer.

# 2. Redistributions in binary form must reproduce the above copyright
# notice, this list of conditions and the following disclaimer in the
# documentation and/or other materials provided with the distribution.

# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
# A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
# HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
# LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
# DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
# THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


python_install_on_site(agimus_demos __init__.py)
python_install_on_site(agimus_demos in_state_planner.py)
python_install_on_site(agimus_demos tools_hpp.py)

python_install_on_site(agimus_demos/calibration __init__.py)
python_install_on_site(agimus_demos/calibration tooltip_calibration.py)
python_install_on_site(agimus_demos/calibration hand_eye_calibration.py)
python_install_on_site(agimus_demos/calibration play_path.py)

python_install_on_site(agimus_demos/talos tools_hpp.py)
python_install_on_site(agimus_demos/talos __init__.py)

python_install_on_site(rqt_agimus tooltip_calibration.py)
python_install_on_site(rqt_agimus contact_calibration.py)
python_install_on_site(rqt_agimus camera_calibration.py)

SET(LIBRARY_NAME ${PROJECT_NAME})
if(USE_SOT)
  ADD_LIBRARY(${LIBRARY_NAME}
    SHARED
    contact-simulation.cc
    contact-simulation.hh
    )
  target_link_libraries (${LIBRARY_NAME} PUBLIC
      dynamic-graph-python::dynamic-graph-python
      )

  add_library(wrap MODULE python-module-py.cc)
  file(MAKE_DIRECTORY ${PROJECT_BINARY_DIR}/src/agimus_demos/sot/${SUBMODULENAME})
  set_target_properties(wrap
    PROPERTIES PREFIX ""
    LIBRARY_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/src
    OUTPUT_NAME agimus_demos/sot/wrap)
  target_compile_options(wrap PRIVATE "-Wl,--no-as-needed")
  target_link_libraries(wrap PUBLIC
    dynamic-graph-python::dynamic-graph-python
    ${PYTHON_LIBRARY} ${LIBRARY_NAME})
  target_include_directories(wrap PRIVATE ${PYTHON_INCLUDE_PATH})
  install(TARGETS wrap
    DESTINATION ${PYTHON_SITELIB}/agimus_demos/sot)
  install(
    FILES ${CMAKE_CURRENT_SOURCE_DIR}/agimus_demos/sot/__init__.py
    DESTINATION ${PYTHON_SITELIB}/agimus_demos/sot)
else()
  ADD_LIBRARY(${LIBRARY_NAME} INTERFACE)
endif()

INSTALL(TARGETS ${LIBRARY_NAME}
EXPORT ${TARGETS_EXPORT_NAME}
DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
)
