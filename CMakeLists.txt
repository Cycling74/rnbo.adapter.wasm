#based off https://gist.github.com/WesThorburn/00c47b267a0e8c8431e06b14997778e4

cmake_minimum_required(VERSION 3.19)
project(RNBO-Wasm)

option(JS_ONLY "Compiles to native JS (No WASM)" OFF)

set(RNBO_CLASS_FILE "${CMAKE_CURRENT_LIST_DIR}/../../../../examples/test/test.cpp" CACHE FILEPATH "The file that holds the generated RNBO class code")
set(RNBO_DESCRIPTION_FILE "" CACHE FILEPATH "path to description.json")

set(RNBO_CPP_DIR ${CMAKE_CURRENT_LIST_DIR}/../../)

add_definitions(-std=c++11)

if (CMAKE_BUILD_TYPE STREQUAL Release)
	add_definitions(-O3)
endif()

include_directories(
	"${RNBO_CPP_DIR}/"
	"${RNBO_CPP_DIR}/common/"
	"${RNBO_CPP_DIR}/src/"
	"${RNBO_CPP_DIR}/src/3rdparty/"
)
set(SOURCE_FILES
	${RNBO_CLASS_FILE}
	${RNBO_CPP_DIR}/adapters/wasm/RNBO_WebAssembly.cpp
)

add_executable(rnbo ${SOURCE_FILES})

set(FLAGS "--bind -s -s SINGLE_FILE=1 -s ASSERTIONS=0 -s MODULARIZE=1 -s EXPORT_NAME='rnbo_module' -s INVOKE_RUN=0 -s NO_FILESYSTEM=1 -s ALLOW_MEMORY_GROWTH=1")

if(JS_ONLY)
	set(CMAKE_EXECUTABLE_SUFFIX ".js")
	set_target_properties(rnbo PROPERTIES LINK_FLAGS "-s WASM=0 ${FLAGS}")
else(JS_ONLY)
	set(CMAKE_EXECUTABLE_SUFFIX ".wasm.js")
	set_target_properties(rnbo PROPERTIES LINK_FLAGS "-s WASM=1 ${FLAGS}")
endif(JS_ONLY)

#cloud compiler uses install to locate artifacts
install(TARGETS rnbo
	RUNTIME
	DESTINATION .
)

if (RNBO_DESCRIPTION_FILE)
	install(FILES ${RNBO_DESCRIPTION_FILE}
		DESTINATION .
	)
endif()
