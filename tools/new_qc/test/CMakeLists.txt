file(MAKE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/testinput")
execute_process(COMMAND ${CMAKE_COMMAND} -E create_symlink
  "${CMAKE_CURRENT_SOURCE_DIR}/testinput/qc_autogenerated.yaml"
  "${CMAKE_CURRENT_BINARY_DIR}/testinput/qc_autogenerated.yaml")

file(MAKE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/Data")
execute_process(COMMAND ${CMAKE_COMMAND} -E create_symlink
  "${CMAKE_CURRENT_SOURCE_DIR}/../../../test/testinput/filters/filters_testdata.nc4"
  "${CMAKE_CURRENT_BINARY_DIR}/Data/filters_testdata.nc4")

# Test that the generated filter can be run.
ecbuild_add_test(TARGET  test_autogeneratedfilter
                 SOURCES TestAutogeneratedFilter.cc $<TARGET_OBJECTS:autogeneratedfilter>
                 ARGS    "testinput/qc_autogenerated.yaml"
                 ENVIRONMENT OOPS_TRAPFPE=1
                 LIBS    ufo
                 #[[Make it possible to include files from the top-level test directory and the directory with the generated sources]]
                 INCLUDES "${CMAKE_CURRENT_SOURCE_DIR}/../../.."
                          "${CMAKE_CURRENT_BINARY_DIR}/..")
