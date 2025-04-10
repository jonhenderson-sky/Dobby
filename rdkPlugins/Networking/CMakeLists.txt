# If not stated otherwise in this file or this component's LICENSE file the
# following copyright and licenses apply:
#
# Copyright 2020 Sky UK
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

project(NetworkingPlugin)


add_library( ${PROJECT_NAME}
        SHARED
        source/NetworkingPlugin.cpp
        source/NetworkSetup.cpp
        source/BridgeInterface.cpp
        source/Netlink.cpp
        source/Netfilter.cpp
        source/DnsmasqSetup.cpp
        source/NetworkingHelper.cpp
        source/PortForwarding.cpp
        source/MulticastForwarder.cpp
        source/TapInterface.cpp
        source/StdStreamPipe.cpp
        source/IPAllocator.cpp
        source/InterContainerRouting.cpp
)

install(
        TARGETS ${PROJECT_NAME}
        LIBRARY DESTINATION lib/plugins/dobby
        NAMELINK_SKIP
)

target_link_libraries( ${PROJECT_NAME}
        DobbyRdkPluginCommonLib

        # 3rd party libraries
        Threads::Threads
        LIBNL::libnl
        LIBNL::libnl-route
)

target_include_directories( ${PROJECT_NAME}
        PUBLIC
        include

        PRIVATE
        ${LIBNL_INCLUDE_DIR}
        $<TARGET_PROPERTY:DobbyDaemonLib,INTERFACE_INCLUDE_DIRECTORIES>

)

set_target_properties( ${PROJECT_NAME} PROPERTIES SOVERSION 1 )

# uncomment to use workaround functions for libnl bridge creation
#target_compile_definitions( ${PROJECT_NAME} PRIVATE ENABLE_LIBNL_BRIDGE_WORKAROUND )
