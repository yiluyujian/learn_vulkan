set(TARGETNAME "test1")

file(GLOB_RECURSE HEADER_FILES CMAKE_CONFIGURE_DEPENDS "*.h")
file(GLOB_RECURSE SOURCES_FILES CMAKE_CONFIGURE_DEPENDS "*.cpp")

add_executable(${TARGETNAME} ${HEADER_FILES} ${SOURCES_FILES})

find_package(glm CONFIG REQUIRED)
find_package(glfw3 CONFIG REQUIRED)
find_package(spdlog CONFIG REQUIRED)
find_package(Vulkan REQUIRED)

# if (DEFINED VULKAN_SDK_PATH)
#   set(Vulkan_INCLUDE_DIRS "${VULKAN_SDK_PATH}/Include") # 1.1 Make sure this include path is correct
#   set(Vulkan_LIBRARIES "${VULKAN_SDK_PATH}/Lib") # 1.2 Make sure lib path is correct
#   set(Vulkan_FOUND "True")
# else()
#   find_package(Vulkan REQUIRED) # throws error if could not find Vulkan
#   message(STATUS "Found Vulkan: $ENV{VULKAN_SDK}")
# endif()
# if (NOT Vulkan_FOUND)
# 	message(FATAL_ERROR "Could not find Vulkan library!")
# else()
# 	message(STATUS "Using vulkan lib at: ${Vulkan_LIBRARIES}")
# endif()

target_include_directories(${TARGETNAME} PRIVATE 
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(${TARGETNAME} PRIVATE 
    glm::glm
    glfw
    spdlog::spdlog
    Vulkan::Vulkan
)

# target_link_directories(${TARGETNAME} PRIVATE
#     ${Vulkan_LIBRARIES}
# )

add_custom_command(TARGET ${TARGETNAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${CMAKE_BINARY_DIR}/compile_commands.json
            ${CMAKE_SOURCE_DIR}/compile_commands.json
    COMMENT "Copying compile_commands.json after build"
)