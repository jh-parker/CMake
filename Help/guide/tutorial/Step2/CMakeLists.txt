cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions) # Build recurses into ./MathFunctions, outputs to build/MathFunctions
  list(APPEND EXTRA_LIBS MathFunctions)
  list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS}) # Used to be MathFunctions, but now it might be empty

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}" # i.e. this is the folder that build outputs to, but our .cxx depends on 
                           # "${PROJECT_SOURCE_DIR}/MathFunctions" # PROJECT_SOURCE_DIR == ./
                           ${EXTRA_INCLUDES} # Note above that EXTRA_INCLUDES gets conditionally defined as ${PROJECT_SOURCE_DIR}/MathFunctions
                           )
