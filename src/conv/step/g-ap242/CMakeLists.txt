set(AP242_INCLUDE_DIRS
  ${CMAKE_CURRENT_BINARY_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}/../g-step
  ${STEPCODE_INCLUDE_DIRS}
  ${OPENNURBS_INCLUDE_DIRS}
  )

link_directories(${CMAKE_BINARY_DIR}/${LIB_DIR})

BRLCAD_INCLUDE_DIRS(AP242_INCLUDE_DIRS)

set(STEP_SCHEMA_FILE ${CMAKE_SOURCE_DIR}/src/other/ext/stepcode/data/ap242/ap242e1.exp)
add_definitions(-DAP242)

GENERATE_SCHEMA_INPUTS(${STEP_SCHEMA_FILE} gap242)

set(gap242_SOURCES
  ${express_srcs}
  ../BRLCADWrapper.cpp
  ../g-step/AP203.cpp
  ../g-step/Default_Geometric_Context.cpp
  ../g-step/ON_Brep.cpp
  ../g-step/ON_NurbsCurve.cpp
  ../g-step/ON_NurbsSurface.cpp
  ../g-ap214/Shape_Definition_Representation.cpp
  ../g-step/Shape_Representation.cpp
  ../g-step/Shape_Representation_Relationship.cpp
  g-ap242.cpp
  )

set(gap242_LIBS
  libwdb
  librt
  libbrep
  libbn
  libbu
  steputils
  stepeditor
  stepdai
  stepcore
  ${OPENNURBS_LIBRARIES}
  )
list(APPEND gap242_LIBS ${gap242_LIBS})

BRLCAD_ADDEXEC(g-ap242 "${gap242_SOURCES}" "${gap242_LIBS}" NO_STRICT FOLDER Conv)
add_dependencies(g-ap242 step-express-${SCHEMA_NAME}-gap242)

if (HIDE_INTERNAL_SYMBOLS)
  set(gap242_IMPORTS
    BU_DLL_IMPORTS
    BN_DLL_IMPORTS
    RT_DLL_IMPORTS
    WDB_DLL_IMPORTS
    SC_CORE_DLL_IMPORTS
    SC_DAI_DLL_IMPORTS
    SC_EDITOR_DLL_IMPORTS
    SC_UTILS_DLL_IMPORTS
    )
  list(APPEND gap242_IMPORTS ${gap242_IMPORTS})
  set_property(TARGET g-ap242 APPEND PROPERTY COMPILE_DEFINITIONS "${gap242_IMPORTS}")
  set_property(SOURCE
    ${CMAKE_CURRENT_BINARY_DIR}/AP242_MANAGED_MODEL_BASED_3D_ENGINEERING_MIM_LF/SdaiAP242_MANAGED_MODEL_BASED_3D_ENGINEERING_MIM_LF.cc
    APPEND PROPERTY COMPILE_FLAGS "/bigobj")
endif (HIDE_INTERNAL_SYMBOLS)

set(IGNORE_FILES
  CMakeLists.txt
  AP242.h
  )
CMAKEFILES(${IGNORE_FILES})

# Local Variables:
# tab-width: 8
# mode: cmake
# indent-tabs-mode: t
# End:
# ex: shiftwidth=2 tabstop=8
