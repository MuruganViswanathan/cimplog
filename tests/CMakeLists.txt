#   Copyright 2016 Comcast Cable Communications Management, LLC
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.

add_test(NAME Simple COMMAND simple)

set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wno-error=missing-field-initializers -W -g -fprofile-arcs -ftest-coverage -O0")
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -W  -g -fprofile-arcs -ftest-coverage -O0")
set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fprofile-arcs -ftest-coverage -O0")

add_executable(simple simple.c ../src/cimplog.c)
set_property(TARGET simple PROPERTY C_STANDARD 99)

target_link_libraries (simple gcov)
target_link_libraries (simple cunit)
target_link_libraries (simple rt)

add_test(NAME SimpleRDKLogger COMMAND simpleRDKLogger)
include_directories ( ../tests )
add_executable(simpleRDKLogger simple-rdk-logger.c ../src/cimplog-rdk-logger.c)
set_property(TARGET simpleRDKLogger PROPERTY C_STANDARD 99)

target_link_libraries (simpleRDKLogger gcov)
target_link_libraries (simpleRDKLogger cunit)
target_link_libraries (simpleRDKLogger rt)

add_test(NAME SimpleRDKLoggerErr COMMAND simpleRDKLoggerErr)
include_directories ( ../tests )
add_executable(simpleRDKLoggerErr simple-rdk-logger-error.c ../src/cimplog-rdk-logger.c)
set_property(TARGET simpleRDKLoggerErr PROPERTY C_STANDARD 99)

target_link_libraries (simpleRDKLoggerErr gcov)
target_link_libraries (simpleRDKLoggerErr cunit)
target_link_libraries (simpleRDKLoggerErr rt)

add_custom_target(coverage
                  COMMAND lcov -q --capture --directory ${CMAKE_CURRENT_BINARY_DIR}/CMakeFiles/simple.dir/__/src --output-file coverage.info
                  COMMAND lcov -q --capture --directory ${CMAKE_CURRENT_BINARY_DIR}/CMakeFiles/simpleRDKLoggerErr.dir/__/src --output-file coverage.info
                  COMMAND lcov -q --capture --directory ${CMAKE_CURRENT_BINARY_DIR}/CMakeFiles/simpleRDKLogger.dir/__/src --output-file coverage.info
                  COMMAND genhtml coverage.info
                  WORKING_DIRECTORY ${CMAKE_BINARY_DIR})
