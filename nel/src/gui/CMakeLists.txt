FIND_PACKAGE( Libwww REQUIRED )
FIND_PACKAGE( CURL REQUIRED )
FIND_PACKAGE( Lua51 REQUIRED )
FIND_PACKAGE( Luabind REQUIRED )

FILE(GLOB SRC *.cpp *.h)
FILE(GLOB HEADERS ../../include/nel/gui/*.h)

SOURCE_GROUP("include" FILES ${HEADERS})
SOURCE_GROUP("src" FILES ${SRC})

NL_TARGET_LIB(nelgui ${SRC} ${HEADERS})

SET_TARGET_PROPERTIES(nelgui PROPERTIES LINK_INTERFACE_LIBRARIES "")
NL_DEFAULT_PROPS(nelgui "NeL, Library: NeL GUI")
NL_ADD_RUNTIME_FLAGS(nelgui)

INCLUDE_DIRECTORIES(${LUA_INCLUDE_DIR} ${LUABIND_INCLUDE_DIR} ${LIBWWW_INCLUDE_DIR})

NL_ADD_LIB_SUFFIX(nelgui)

#MESSAGE( "libww libs: ${LIBWWW_LIBRARIES}" )

TARGET_LINK_LIBRARIES(  nelgui
			nelmisc
			nel3d
			${LUA_LIBRARIES}
			${LUABIND_LIBRARIES}
			${LIBXML2_LIBRARIES}
			${LIBWWW_LIBRARIES}
			${CURL_LIBRARIES}
			)

IF(WITH_PCH)
  ADD_NATIVE_PRECOMPILED_HEADER(nelgui ${CMAKE_CURRENT_SOURCE_DIR}/stdpch.h ${CMAKE_CURRENT_SOURCE_DIR}/stdpch.cpp)
ENDIF(WITH_PCH)

IF((WITH_INSTALL_LIBRARIES AND WITH_STATIC) OR NOT WITH_STATIC)
  INSTALL(TARGETS nelgui LIBRARY DESTINATION lib ARCHIVE DESTINATION lib COMPONENT libraries)
ENDIF((WITH_INSTALL_LIBRARIES AND WITH_STATIC) OR NOT WITH_STATIC)
