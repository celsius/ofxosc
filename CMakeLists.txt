# cmake file to get a libofxosc.so lib to use with openFrameworks apps

PROJECT(OFXOSC)

#CMake generalities
CMAKE_MINIMUM_REQUIRED (VERSION 2.6.2)

IF(COMMAND cmake_policy)
	IF(${CMAKE_VERSION} STRLESS "2.8.0")
		#AM: don't know which is the best, but must be < CMP0008 for cmake 2.6.0 and < CMP0012 for cmake 2.6.4
		cmake_policy(SET CMP0003 NEW) 
	ELSE()
		cmake_policy(SET CMP0014 NEW)
	ENDIF()
ENDIF()

#build options
OPTION(BUILD_SHARED_LIBS "Build Shared libraries" OFF)

# OS-SPECIFIC SUBPROJECTS COMPILATION for 64 bits (fix compilation problem on x64 platform). 
#TODO: check: there is some flag in cmake >= 2.8.9 for this)
IF(UNIX)
	SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
	SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
ENDIF()

# Build type (default is Release)
# NB: Release mode defines a macro NDEBUG and sets the optimization flag to -O3 (i.e. gcc -DNDEBUG -O3)
IF(NOT CMAKE_BUILD_TYPE)
  SET(CMAKE_BUILD_TYPE Release CACHE STRING
      "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel."
      FORCE)
ENDIF()

MESSAGE("Files will be built in ${PROJECT_SOURCE_DIR}/bin/${CMAKE_BUILD_TYPE}")
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin/${CMAKE_BUILD_TYPE})
SET(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin/${CMAKE_BUILD_TYPE})

# this sets the path to the openframeworks folder containing "addons", "apps", "examples", "libs," ... (actually only "libs" is used)
IF(NOT OF_ROOT_PATH)
	# TODO find a way to install openframework in /usr/local/something 
	# and write FindOpenFrameworks.cmake to avoid this idiotic hardcoded path
	MESSAGE("*** Path to OpenFrameworks is undefined. Setting it to /opt/openframeworks.\r
*** You can change this either within 'ccmake .' or using 'cmake ../.. -DOF_ROOT_PATH=/path/to/openframeworks'")
	SET(OF_ROOT_PATH "/opt/openframeworks")
ENDIF()
#specific flags for use with openFrameworks
SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D_REENTRANT -DOF_USING_GTK -DOF_USING_MPG123 -Wall -fexceptions -march=native -mtune=native -MMD -MP")
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D_REENTRANT -DOF_USING_GTK -DOF_USING_MPG123 -Wall -fexceptions -march=native -mtune=native -MMD -MP")

# OFXOSC stuffs

SET(LIB_NAME ofxosc)

FILE(GLOB_RECURSE SRC ${SRC} *.cpp *.c)
FILE(GLOB_RECURSE HDR ${HDR} *.hpp *.h)

INCLUDE_DIRECTORIES(./src)
INCLUDE_DIRECTORIES(./libs/oscpack/src)
INCLUDE_DIRECTORIES(./libs/oscpack/src/osc)
INCLUDE_DIRECTORIES(./libs/oscpack/src/ip)
INCLUDE_DIRECTORIES(./libs/oscpack/src/ip/posix)

INCLUDE_DIRECTORIES(/usr/include/gstreamer-0.10)
INCLUDE_DIRECTORIES(/usr/include/glib-2.0)
INCLUDE_DIRECTORIES(/usr/lib/glib-2.0/include)
INCLUDE_DIRECTORIES(/usr/include/libxml2/)

#certainly not the cleanest way to do it, but it matches openframeworks' gcc command from the Makefile
#besides ... it works
#[OPENFRAMEWORKS]
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/glu/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/FreeImage/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/assimp/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/assimp/include/Compiler)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/cairo/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/cairo/include/cairo)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/cairo/include/pixman-1)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/cairo/include/libpng15)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/fmodex/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/freetype/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/freetype/include/freetype2)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/freetype/include/freetype2/freetype)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/freetype/include/freetype2/freetype/config)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/freetype/include/freetype2/freetype/internal)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/freetype/include/freetype2/freetype/internal/services)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/glew/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/glew/include/GL)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/kiss/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/portaudio/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/rtAudio/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/tess2/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/poco/include)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/math)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/gl)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/video)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/3d)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/types)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/graphics)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/app)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/events)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/sound)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/utils)
INCLUDE_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworks/communication)

LINK_DIRECTORIES(${OF_ROOT_PATH}/libs/openFrameworksCompiled/lib/linux64/)
LINK_DIRECTORIES(${OF_ROOT_PATH}/libs/poco/lib/linux64)
LINK_DIRECTORIES(${OF_ROOT_PATH}/libs/fmodex/lib/linux64)
LINK_DIRECTORIES(${OF_ROOT_PATH}/libs/freetype/lib/linux64)
LINK_DIRECTORIES(${OF_ROOT_PATH}/libs/glew/lib/linux64)
LINK_DIRECTORIES(${OF_ROOT_PATH}/libs/kiss/lib/linux64)
LINK_DIRECTORIES(${OF_ROOT_PATH}/libs/portaudio/lib/linux64)
LINK_DIRECTORIES(${OF_ROOT_PATH}/libs/rtAudio/lib/linux64)
LINK_DIRECTORIES(${OF_ROOT_PATH}/libs/tess2/lib/linux64)
#[/OPENFRAMEWORKS]

ADD_LIBRARY(${LIB_NAME} ${SRC} ${HDR})
TARGET_LINK_LIBRARIES(${LIB_NAME} pthread m)
TARGET_LINK_LIBRARIES(${LIB_NAME} gstreamer-0.10)

#[OPENFRAMEWORKS]
TARGET_LINK_LIBRARIES(${LIB_NAME} openFrameworks)
TARGET_LINK_LIBRARIES(${LIB_NAME} PocoNet)
TARGET_LINK_LIBRARIES(${LIB_NAME} PocoXML)
TARGET_LINK_LIBRARIES(${LIB_NAME} PocoUtil)
TARGET_LINK_LIBRARIES(${LIB_NAME} PocoFoundation)
TARGET_LINK_LIBRARIES(${LIB_NAME} freetype)
TARGET_LINK_LIBRARIES(${LIB_NAME} kiss)
TARGET_LINK_LIBRARIES(${LIB_NAME} tess2)
TARGET_LINK_LIBRARIES(${LIB_NAME} RtAudio)
TARGET_LINK_LIBRARIES(${LIB_NAME} fmodex)
TARGET_LINK_LIBRARIES(${LIB_NAME} GLEW)
TARGET_LINK_LIBRARIES(${LIB_NAME} gtk-x11-2.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} gdk-x11-2.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} atk-1.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} gio-2.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} pangoft2-1.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} gdk_pixbuf-2.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} pangocairo-1.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} cairo)
TARGET_LINK_LIBRARIES(${LIB_NAME} pango-1.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} fontconfig)
TARGET_LINK_LIBRARIES(${LIB_NAME} gobject-2.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} gmodule-2.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} gthread-2.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} rt)
TARGET_LINK_LIBRARIES(${LIB_NAME} glib-2.0)
TARGET_LINK_LIBRARIES(${LIB_NAME} mpg123)
TARGET_LINK_LIBRARIES(${LIB_NAME} glut)
TARGET_LINK_LIBRARIES(${LIB_NAME} GL)
TARGET_LINK_LIBRARIES(${LIB_NAME} asound)
TARGET_LINK_LIBRARIES(${LIB_NAME} openal)
TARGET_LINK_LIBRARIES(${LIB_NAME} sndfile)
TARGET_LINK_LIBRARIES(${LIB_NAME} vorbis)
TARGET_LINK_LIBRARIES(${LIB_NAME} FLAC)
TARGET_LINK_LIBRARIES(${LIB_NAME} ogg)
TARGET_LINK_LIBRARIES(${LIB_NAME} freeimage)
#[/OPENFRAMEWORKS]

TARGET_LINK_LIBRARIES(${LIB_NAME} portaudio)
TARGET_LINK_LIBRARIES(${LIB_NAME} jack)

#installation in /usr/local/lib
INSTALL(TARGETS ofxosc ARCHIVE DESTINATION /opt/ofxosc/)
