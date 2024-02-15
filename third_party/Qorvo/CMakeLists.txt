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

# mbedtls
if (${QORVO_MBEDTLS})
    if (DEFINED ENV{QORVO_OT_MBEDTLS_SDK})
        SET(QORVO_MBEDTLS_DIR "$ENV{QORVO_OT_MBEDTLS_SDK}")
    else()
        SET(QORVO_MBEDTLS_DIR "${SDK_DIR}")
    endif()
    set(MBEDTLSALT "${SDK_DIR}/${QORVO_PLATFORM}/lib/libmbedtls_alt.a")
    set(QORVO_MBEDTLS_SDK_DIR "${QORVO_MBEDTLS_DIR}/qorvo-mbedtls/")
    if(NOT EXISTS ${QORVO_MBEDTLS_SDK_DIR}/mbedtls/v3.3.0/include/mbedtls)
        message(FATAL_ERROR "'${QORVO_MBEDTLS_SDK_DIR}/mbedtls/v3.3.0/include/mbedtls' does not exist. Try setting 'QORVO_OT_SDK' or 'QORVO_OT_MBEDTLS_SDK'")
    endif()
    message(STATUS "Including Qorvo MBEDTLS - ${QORVO_MBEDTLS_SDK_DIR}")

    include(${QORVO_MBEDTLS_SDK_DIR}/mbedtls.cmake)

    list(APPEND OT_PUBLIC_INCLUDES "${QORVO_MBEDTLS_SDK_DIR}/mbedtls/v3.3.0/include/mbedtls")
    list(APPEND OT_PUBLIC_INCLUDES "${QORVO_MBEDTLS_SDK_DIR}/inc")

    set(OT_PUBLIC_INCLUDES ${OT_PUBLIC_INCLUDES} PARENT_SCOPE)
else()
    set(MBEDCRYPTO "mbedcrypto")
endif()

foreach(TYPE "FTD" "MTD")
    string(TOLOWER ${TYPE} ftd)
    set(QORVO_PLATFORM_LIB_${TYPE} "${SDK_DIR}/${QORVO_PLATFORM}/lib/libQorvo${QORVO_PLATFORM_UPPER}_${ftd}.a")

    add_library(${PLATFORM_DRIVER_${TYPE}} INTERFACE)
    target_link_libraries(${PLATFORM_DRIVER_${TYPE}}
        INTERFACE
            ${QORVO_PLATFORM_LIB_${TYPE}}
            ${MBEDTLSALT}
            ${MBEDCRYPTO}
    )
endforeach()

