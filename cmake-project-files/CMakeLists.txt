cmake_minimum_required(VERSION 3.1.0)

add_definitions(
	-D_CRT_SECURE_NO_WARNINGS
	-D_CRT_SECURE_NO_DEPRECIATE
	-DBUILD_SHARED_LIBS=false
	-DCMAKE_BINARY_DIR="${CMAKE_BINARY_DIR}/"
	-DCMAKE_SOURCE_DIR="${CMAKE_SOURCE_DIR}/"
	-DNANOVG_EXAMPLE_GLAD
)
if(WIN32)
	set(CompilerFlags
		CMAKE_CXX_FLAGS
		CMAKE_CXX_FLAGS_DEBUG
		CMAKE_CXX_FLAGS_RELEASE
		CMAKE_C_FLAGS
		CMAKE_C_FLAGS_DEBUG
		CMAKE_C_FLAGS_RELEASE)
	foreach(CompilerFlag ${CompilerFlags})
		string(REPLACE "/MD" "/MT" ${CompilerFlag} "${${CompilerFlag}}")
	endforeach()

	set(CompilerFlags
		CMAKE_C_FLAGS_MINSIZEREL
		CMAKE_CXX_FLAGS_MINSIZEREL)
	foreach(CompilerFlag ${CompilerFlags})
		string(REPLACE "/MT" "/MD" ${CompilerFlag} "${${CompilerFlag}}")
	endforeach()
else(WIN32)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -std=gnu++11 -static-libstdc++")
	set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static-libstdc++")
endif(WIN32)

include(pal_glfw-3.1.2.cmake)

include_directories(
  include/
  ../src/
  ../example/
)

add_executable(
  example_gl3
    ../src/stb_truetype.h
    ../src/fontstash.h
    ../src/nanovg.c
    ../src/nanovg.h
    ../src/nanovg_gl.h
    ../src/nanovg_gl_utils.h
    ../src/stb_image.h

    ../example/example_gl3.c
    ../example/demo.c
    ../example/perf.c

    src/glad.c
  )
  target_link_libraries(example_gl3 glfw ${GLFW_LIBRARIES})
