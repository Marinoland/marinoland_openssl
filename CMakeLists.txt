cmake_minimum_required(VERSION 2.8)
project (openssl)

execute_process(COMMAND git submodule init)
execute_process(COMMAND git submodule update)

set(VER 3)
IF (WIN32)
	set(MAKE nmake)
	set(CONFIGURE perl Configure)
	set(STATIC_SUFFIX _static.lib)
	set(DYN_SUFFIX -${VER}-x64.dll)
ELSE()
	set(MAKE make)
	set(CONFIGURE ./Configure)
	set(STATIC_SUFFIX .a)
	set(DYN_SUFFIX .so.${VER})
ENDIF()

execute_process(COMMAND ${CONFIGURE} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )
execute_process(COMMAND ${MAKE} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )

file(COPY
	${CMAKE_CURRENT_SOURCE_DIR}/ext/libcrypto${STATIC_SUFFIX}
	${CMAKE_CURRENT_SOURCE_DIR}/ext/libcrypto${DYN_SUFFIX}
	${CMAKE_CURRENT_SOURCE_DIR}/ext/libssl${STATIC_SUFFIX}
	${CMAKE_CURRENT_SOURCE_DIR}/ext/libssl${DYN_SUFFIX}
	DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
