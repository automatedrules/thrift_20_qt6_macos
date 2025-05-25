#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements. See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership. The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License. You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied. See the License for the
# specific language governing permissions and limitations
# under the License.
#

set(CMAKE_AUTOMOC ON)

set(TQTcpServerTest_SOURCES
    TQTcpServerTest.cpp
)

if(WITH_QT5)
    find_package(Qt5 REQUIRED COMPONENTS Test Network)
    add_executable(TQTcpServerTest_Qt5 ${TQTcpServerTest_SOURCES})
    target_link_libraries(TQTcpServerTest_Qt5 testgencpp_cob thriftqt5 thrift Qt5::Test Qt5::Network)
    add_test(NAME TQTcpServerTest_Qt5 COMMAND TQTcpServerTest_Qt5)
endif()

if(WITH_QT6)
    find_package(Qt6 REQUIRED COMPONENTS Test Network)
    add_executable(TQTcpServerTest_Qt6 ${TQTcpServerTest_SOURCES})
    target_link_libraries(TQTcpServerTest_Qt6 testgencpp_cob thriftqt6 thrift Qt6::Test Qt6::Network)
    add_test(NAME TQTcpServerTest_Qt6 COMMAND TQTcpServerTest_Qt6)
endif()
