add_subdirectory(packets)

set(SOURCES
    ${COMMON_SOURCES}
    ${PACKET_SOURCES}
    data_loader.cpp
    data_loader.h
    handler.h
    main.cpp
    search_handler.h
    search_handler.cpp
    search_server.h
    search_server.cpp
)

# Temporarily remove clashing old code
# TODO: Remove this block
list(REMOVE_ITEM SOURCES
    ${CMAKE_SOURCE_DIR}/src/common/kernel.cpp
    ${CMAKE_SOURCE_DIR}/src/common/kernel.h
    ${CMAKE_SOURCE_DIR}/src/common/socket.cpp
    ${CMAKE_SOURCE_DIR}/src/common/socket.h)

if(UNIX)
    set(resource "")
else()
    set(resource "${CMAKE_SOURCE_DIR}/res/searchserver.rc")
endif()

add_executable(xi_search
    ${SOURCES}
    ${resource})

if(WIN32)
    set_target_properties(xi_search PROPERTIES OUTPUT_NAME xi_search)
endif()

if (APPLE)
    disable_lto(xi_connect)
endif()

target_compile_definitions(
    xi_search
    PUBLIC
    -DDEBUGLOGSEARCH
)

target_compile_definitions(xi_search PUBLIC DEFINE_OWN_MAIN)

target_link_libraries(xi_search
    PUBLIC
        ${EXTERNAL_LIBS}
        project_options
    PRIVATE
        project_warnings
)

target_include_directories(xi_search
    PUBLIC
        ${CMAKE_SOURCE_DIR}
        ${CMAKE_SOURCE_DIR}/src
        ${CMAKE_CURRENT_SOURCE_DIR}
)

set_target_output_directory(xi_search)
