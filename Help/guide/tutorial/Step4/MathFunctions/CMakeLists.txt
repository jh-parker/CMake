add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR} # CMAKE_CURRENT_SOURCE_DIR refers to ./ relative to this CMakeLists.txt, i.e. MathFunctions/. MathFunctions doesn't need the *.h's from MathFunctions/, but anyone consuming MathFunctions does.
          )
