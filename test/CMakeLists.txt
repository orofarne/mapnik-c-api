add_custom_target(test)
macro(run_test test_target)
add_custom_target(${test_target}_runtest
    COMMAND ${test_target}
    DEPENDS ${test_target}
    WORKING_DIRECTORY "${CMAKE_BINARY_DIR}")
add_dependencies(test ${test_target}_runtest)
endmacro()

include_directories(${PROJECT_SOURCE_DIR})

configure_file(c-api-test-cfg.h.in ${CMAKE_CURRENT_BINARY_DIR}/c-api-test-cfg.h)
include_directories(${CMAKE_CURRENT_BINARY_DIR})

add_executable(mapnik_c_api_test c-api-test.cpp)

add_dependencies(mapnik_c_api_test
  mapnik_c_api
)
target_link_libraries(mapnik_c_api_test
  mapnik_c_api
  ${MAPNIK_LIBRARIES}
)

run_test(mapnik_c_api_test)
