#
# Copyright 2016 National Renewable Energy Laboratory
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

if (GENERATE_TYPES)
  generate_f90_types(src/TMD_Registry.txt ${CMAKE_CURRENT_LIST_DIR}/src/TMD_Types.f90)
  generate_f90_types(src/ServoDyn_Registry.txt ${CMAKE_CURRENT_LIST_DIR}/src/ServoDyn_Types.f90)
endif()

set(SD_SOURCES
  src/BladedInterface.f90
  src/UserSubs.f90
  src/PitchCntrl_ACH.f90
  src/TMD.f90
  src/UserVSCont_KP.f90
  src/ServoDyn.f90
  src/TMD_Types.f90
  src/ServoDyn_Types.f90
)

add_library(servodynlib ${SD_SOURCES})
target_link_libraries(servodynlib nwtclibs)

add_executable(servodyn_driver src/ServoDyn_Driver.f90)
target_link_libraries(servodyn_driver servodynlib nwtclibs ${CMAKE_DL_LIBS})

add_executable(TMD src/TMD_Driver.f90)
target_link_libraries(TMD servodynlib nwtclibs ${CMAKE_DL_LIBS})

install(TARGETS servodynlib servodyn_driver TMD
  EXPORT "${CMAKE_PROJECT_NAME}Libraries"
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib)