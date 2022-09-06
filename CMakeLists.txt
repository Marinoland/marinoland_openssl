cmake_minimum_required(VERSION 2.8)
project (openssl)

execute_process(COMMAND git submodule init)
execute_process(COMMAND git submodule update)

set(OPENSSL_BIN_DIR ${CMAKE_CURRENT_BINARY_DIR})
IF (WIN32)
	execute_process( COMMAND perl Configure VC-WIN64A WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )
	execute_process( COMMAND nmake WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )
ELSE()
	execute_process( COMMAND ./Configure WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )
	execute_process( COMMAND make WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ext )
ENDIF()

execute_process(COMMAND cp 
	${CMAKE_CURRENT_SOURCE_DIR}/ext/libcrypto.a
	${CMAKE_CURRENT_SOURCE_DIR}/ext/libcrypto.so
	${CMAKE_CURRENT_SOURCE_DIR}/ext/libcrypto.so.3
	${CMAKE_CURRENT_SOURCE_DIR}/ext/libssl.a
	${CMAKE_CURRENT_SOURCE_DIR}/ext/libssl.so
	${CMAKE_CURRENT_SOURCE_DIR}/ext/libssl.so.3
	${OPENSSL_BIN_DIR})
