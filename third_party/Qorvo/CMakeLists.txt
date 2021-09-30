#
#  Copyright (c) 2020, The OpenThread Authors.
#  All rights reserved.
#
#  Redistribution and use in source and binary forms, with or without
#  modification, are permitted provided that the following conditions are met:
#  1. Redistributions of source code must retain the above copyright
#     notice, this list of conditions and the following disclaimer.
#  2. Redistributions in binary form must reproduce the above copyright
#     notice, this list of conditions and the following disclaimer in the
#     documentation and/or other materials provided with the distribution.
#  3. Neither the name of the copyright holder nor the
#     names of its contributors may be used to endorse or promote products
#     derived from this software without specific prior written permission.
#
#  THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
#  AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
#  IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
#  ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
#  LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
#  CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
#  SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
#  INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
#  CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
#  ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
#  POSSIBILITY OF SUCH DAMAGE.
#

message(STATUS "Qorvo ${QORVO_PLATFORM} libs in ${SDK_DIR}/${QORVO_PLATFORM}")
string(TOUPPER ${QORVO_PLATFORM} QORVO_PLATFORM_UPPER)

# setting defaults for the variables
set(MBEDTLSALT "")
set(MBEDCRYPTO "")

if((OT_MTD) AND (OT_FTD))
    message(FATAL_ERROR "Building ${QORVO_PLATFORM} with both OT_MTD and OT_FTD flags set is not supported")
endif()
if (OT_MTD)
    set(QORVO_PLATFORM_LIB "${SDK_DIR}/${QORVO_PLATFORM}/lib/libQorvo${QORVO_PLATFORM_UPPER}_mtd.a")
elseif (OT_FTD)
    set(QORVO_PLATFORM_LIB "${SDK_DIR}/${QORVO_PLATFORM}/lib/libQorvo${QORVO_PLATFORM_UPPER}_ftd.a")
elseif (OT_RCP)
    set(QORVO_PLATFORM_LIB "${SDK_DIR}/${QORVO_PLATFORM}/lib/libQorvo${QORVO_PLATFORM_UPPER}_rcp.a")
else()
    message(FATAL_ERROR "Unsupported library")
endif()

# QPG6100 uses mbedtls_alt
if (QORVO_PLATFORM MATCHES "qpg6100")
    set(MBEDTLSALT "${SDK_DIR}/${QORVO_PLATFORM}/lib/libmbedtls_alt.a")
    set(MBEDCRYPTO "mbedcrypto")
endif()

message( STATUS "Building platform ${QORVO_PLATFORM} with ${QORVO_PLATFORM_LIB}" )

add_library(${QORVO_PLATFORM}-driver INTERFACE)
target_link_libraries(${QORVO_PLATFORM}-driver
    INTERFACE
        ${QORVO_PLATFORM_LIB}
        ${MBEDTLSALT}
        ${MBEDCRYPTO}
)
