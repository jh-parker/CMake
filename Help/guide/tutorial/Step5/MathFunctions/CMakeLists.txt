add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR} # INTERFACE tells dependants on this library about where the *.h's this library produces are.
          PRIVATE ${CMAKE_BINARY_DIR} # PRIVATE is for this library. It gives this library paths to includes it needs.
          )

# install rules
install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)
