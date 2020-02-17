add_library(MathFunctions mysqrt.cxx)

target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR} # MathFunctions doesn't need CMAKE_CURRENT_SOURCE_DIR, but anyone consuming MathFunctions does.
          )