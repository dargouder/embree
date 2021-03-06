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

ADD_LIBRARY(simd STATIC sse.cpp)
SET_PROPERTY(TARGET simd PROPERTY FOLDER common)
SET_PROPERTY(TARGET simd APPEND PROPERTY COMPILE_FLAGS " ${FLAGS_LOWEST}")

IF (EMBREE_STATIC_LIB)
  INSTALL(TARGETS simd EXPORT simd-targets ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR} COMPONENT devel)
  INSTALL(EXPORT simd-targets DESTINATION ${EMBREE_CMAKECONFIG_DIR} COMPONENT devel)
  SIGN_TARGET(simd)
ENDIF()

if(APPLE)
add_custom_command(
	TARGET simd 
	POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy
	 $<TARGET_FILE:simd>
	 ${CMAKE_SOURCE_DIR}/../builtDependencies/apple/${CMAKE_BUILD_TYPE}/libsimd.a)	
elseif(UNIX)
add_custom_command(
	TARGET simd 
	POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy
	 $<TARGET_FILE:simd>
	 ${CMAKE_SOURCE_DIR}/../builtDependencies/unix/${CMAKE_BUILD_TYPE}/libsimd.a)	
elseif(WIN32)
	#copying built lib to the output
	add_custom_command(
	TARGET simd 
	POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy
	 $<TARGET_FILE:simd>
	 ${CMAKE_SOURCE_DIR}/../builtDependencies/windows/$<CONFIGURATION>/simd.lib)		
endif()
