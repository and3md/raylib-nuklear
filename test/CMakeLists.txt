# raylib-nuklear-test
add_executable(raylib-nuklear-test raylib-nuklear-test.c)
if (MSVC)
target_compile_options(raylib-nuklear-test PRIVATE -Wall)
else()
target_compile_options(raylib-nuklear-test PRIVATE -Wall -Wextra -Wconversion -Wsign-conversion)
endif()
target_link_libraries(raylib-nuklear-test PUBLIC raylib raylib_nuklear)

# Copy the resources
file(GLOB resources resources/*)
set(test_resources)
list(APPEND test_resources ${resources})
file(COPY ${test_resources} DESTINATION "resources/")

# Set up the test
list(APPEND CMAKE_CTEST_ARGUMENTS "--output-on-failure")
add_test(NAME raylib-nuklear-test COMMAND raylib-nuklear-test)
