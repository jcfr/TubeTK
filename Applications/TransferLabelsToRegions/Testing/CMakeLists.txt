##############################################################################
#
# Library:   TubeTK
#
# Copyright 2010 Kitware Inc. 28 Corporate Drive,
# Clifton Park, NY, 12065, USA.
#
# All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
##############################################################################

include_regular_expression( "^.*$" )

Include(${TubeTK_SOURCE_DIR}/CMake/MIDAS3.cmake)
set( MIDAS_REST_URL http://midas3.kitware.com/midas/api/rest )
set( MIDAS_KEY_DIR ${TubeTK_SOURCE_DIR}/MIDAS_Keys)


set( TEMP ${TubeTK_BINARY_DIR}/Temporary )

set( PROJ_EXE
     ${SlicerExecutionModel_CLI_RUNTIME_OUTPUT_DIRECTORY}/${MODULE_NAME} )

set( TEXTCOMPARE_EXE
     ${TubeTK_BINARY_DIR}/bin/TextCompareCommand )

set( IMAGECOMPARE_EXE
     ${TubeTK_BINARY_DIR}/bin/ImageCompareCommand )

#######
# Test1
#######
MIDAS_ADD_TEST( ${MODULE_NAME}-Test1
                ${PROJ_EXE}
                  MIDAS{Exemplary-CVT-Tessellation.mha.md5}
                  MIDAS{SPL2012-LabelMap.nrrd.md5}
                  ${TEMP}/${MODULE_NAME}Test1-Img.mha
                  ${TEMP}/${MODULE_NAME}Test1-Map.txt )

#######
# Test1
#######
MIDAS_ADD_TEST( ${MODULE_NAME}-Test2
                ${PROJ_EXE}
                  MIDAS{Exemplary-CVT-Tessellation.mha.md5}
                  MIDAS{SPL2012-LabelMap.nrrd.md5}
                  ${TEMP}/${MODULE_NAME}Test2-Img.mha
                  ${TEMP}/${MODULE_NAME}Test2-Map.txt
                  --omitRegions 0)

###############
# Test1-Compare
###############
MIDAS_ADD_TEST( ${MODULE_NAME}-Test1-Compare-Map
                ${TEXTCOMPARE_EXE}
                  -t ${TEMP}/${MODULE_NAME}Test1-Map.txt
                  -b MIDAS{${MODULE_NAME}Test1-Map.txt.md5} )

set_property(TEST ${MODULE_NAME}-Test1-Compare-Map
               APPEND PROPERTY DEPENDS ${MODULE_NAME}-Test1 )

MIDAS_ADD_TEST( ${MODULE_NAME}-Test1-Compare-Img
                ${IMAGECOMPARE_EXE}
                  -t ${TEMP}/${MODULE_NAME}Test1-Img.mha
                  -b MIDAS{${MODULE_NAME}Test1-Img.mha.md5} )

set_property(TEST ${MODULE_NAME}-Test1-Compare-Img
               APPEND PROPERTY DEPENDS ${MODULE_NAME}-Test1 )

###############
# Test2-Compare
###############
MIDAS_ADD_TEST( ${MODULE_NAME}-Test2-Compare-Map
                ${TEXTCOMPARE_EXE}
                  -t ${TEMP}/${MODULE_NAME}Test2-Map.txt
                  -b MIDAS{${MODULE_NAME}Test2-Map.txt.md5} )

set_property(TEST ${MODULE_NAME}-Test1-Compare-Map
               APPEND PROPERTY DEPENDS ${MODULE_NAME}-Test2 )

MIDAS_ADD_TEST( ${MODULE_NAME}-Test2-Compare-Img
                ${IMAGECOMPARE_EXE}
                  -t ${TEMP}/${MODULE_NAME}Test2-Img.mha
                  -b MIDAS{${MODULE_NAME}Test2-Img.mha.md5} )

set_property(TEST ${MODULE_NAME}-Test2-Compare-Img
               APPEND PROPERTY DEPENDS ${MODULE_NAME}-Test2 )