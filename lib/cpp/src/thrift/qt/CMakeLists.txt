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

set(thriftcppqt_SOURCES
    TQIODeviceTransport.cpp
    TQTcpServer.cpp
)

set(CMAKE_AUTOMOC ON)
if(APPLE)
    # On macOS, Qt may be installed as frameworks
    set(CMAKE_AUTOMOC_MOC_OPTIONS "-F/usr/local/qt5-mini/lib")
endif()

# Contains the thrift specific ADD_LIBRARY_THRIFT macro
include(ThriftMacros)

if(THRIFT_BUILD_QT5)
    find_package(Qt5 REQUIRED COMPONENTS Core Network)
    ADD_LIBRARY_THRIFT(thriftqt5 ${thriftcppqt_SOURCES})
    target_link_libraries(thriftqt5 PUBLIC thrift Qt5::Core Qt5::Network)
endif()

if(THRIFT_BUILD_QT6)
    find_package(Qt6 REQUIRED COMPONENTS Core Network)
    ADD_LIBRARY_THRIFT(thriftqt6 ${thriftcppqt_SOURCES})
    target_link_libraries(thriftqt6 PUBLIC thrift Qt6::Core Qt6::Network)
    
    # Work around Qt6 framework include path issue on macOS
    if(APPLE AND NOT EXISTS "/usr/local/qt5-mini/include")
        target_include_directories(thriftqt6 PRIVATE 
            /usr/local/qt5-mini/lib/QtCore.framework/Headers
            /usr/local/qt5-mini/lib/QtNetwork.framework/Headers
        )
    endif()
endif()
