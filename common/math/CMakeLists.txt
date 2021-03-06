## ======================================================================== ##
## Copyright 2009-2020 Intel Corporation                                    ##
##                                                                          ##
## Licensed under the Apache License, Version 2.0 (the "License");          ##
## you may not use this file except in compliance with the License.         ##
## You may obtain a copy of the License at                                  ##
##                                                                          ##
##     http://www.apache.org/licenses/LICENSE-2.0                           ##
##                                                                          ##
## Unless required by applicable law or agreed to in writing, software      ##
## distributed under the License is distributed on an "AS IS" BASIS,        ##
## WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. ##
## See the License for the specific language governing permissions and      ##
## limitations under the License.                                           ##
## ======================================================================== ##

ADD_LIBRARY(math STATIC constants.cpp)
SET_PROPERTY(TARGET math PROPERTY FOLDER common)
SET_PROPERTY(TARGET math APPEND PROPERTY COMPILE_FLAGS " ${FLAGS_LOWEST}")

IF (EMBREE_STATIC_LIB)
  INSTALL(TARGETS math EXPORT math-targets ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR} COMPONENT devel)
  INSTALL(EXPORT math-targets DESTINATION ${EMBREE_CMAKECONFIG_DIR} COMPONENT devel)
  SIGN_TARGET(math)
ENDIF()

if(APPLE)
	#copying built lib to the output
	add_custom_command(
	TARGET math 
	POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy
	 $<TARGET_FILE:math>
	 ${CMAKE_SOURCE_DIR}/../builtDependencies/apple/${CMAKE_BUILD_TYPE}/libmath.a)	
elseif(UNIX)
	#copying built lib to the output
	add_custom_command(
	TARGET math 
	POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy
	 $<TARGET_FILE:math>
	 ${CMAKE_SOURCE_DIR}/../builtDependencies/unix/${CMAKE_BUILD_TYPE}/libmath.a)	
elseif(WIN32)
	#copying built lib to the output
	add_custom_command(
	TARGET math 
	POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy
	 $<TARGET_FILE:math>
	 ${CMAKE_SOURCE_DIR}/../builtDependencies/windows/$<CONFIGURATION>/math.lib)		
endif()
