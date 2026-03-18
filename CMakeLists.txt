cmake_minimum_required(VERSION 3.20)

project(ImguiFramework)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_subdirectory(ThirdParty/glfw)
add_subdirectory(ThirdParty/glm)
add_subdirectory(ThirdParty/Imgui)

set(PROJECT_SOURCES
    main.cpp
)

add_executable(${PROJECT_NAME} ${PROJECT_SOURCES})

target_include_directories(${PROJECT_NAME} PRIVATE
    ThirdParty/glfw/include
    ThirdParty/glm
    ThirdParty/Imgui/Imgui
    ThirdParty/Imgui/ImguiNodeEditor
    ThirdParty/Imgui/ImGuizmo
    ThirdParty/Imgui/ImPlot
    ThirdParty/Imgui/ImPlot3D
    ThirdParty/Imgui/RenderBackend
    ThirdParty/VulkanMemoryAllocator
)

target_include_directories(${PROJECT_NAME} PRIVATE
    ThirdParty/Imgui/Imgui
    ThirdParty/Imgui/ImguiNodeEditor
    ThirdParty/Imgui/ImGuizmo
    ThirdParty/Imgui/ImPlot
    ThirdParty/Imgui/ImPlot3D
    ThirdParty/Imgui/RenderBackend
    ThirdParty/glfw/include
    ThirdParty/glm
    ThirdParty/VulkanMemoryAllocator
)
target_link_libraries(${PROJECT_NAME} PRIVATE
    Imgui
    glfw
    glm
)
if (WIN32)
    target_include_directories(${PROJECT_NAME} PRIVATE
        ThirdParty/Vulkan/Win/vk_video
        ThirdParty/Vulkan/Win
    )
    target_link_directories(${PROJECT_NAME} PRIVATE
        ThirdParty/Vulkan/Win/Lib
    )
    target_link_libraries(${PROJECT_NAME} PRIVATE
        vulkan-1
    )
elseif (UNIX AND NOT APPLE)
    target_include_directories(${PROJECT_NAME} PRIVATE
        ThirdParty/Vulkan/Linux/vk_video
        ThirdParty/Vulkan/Linux
    )
    target_link_directories(${PROJECT_NAME} PRIVATE
        ThirdParty/Vulkan/Linux/Lib
    )
    target_link_libraries(${PROJECT_NAME} PRIVATE
        libvulkan.so
    )
elseif (APPLE)
    target_include_directories(${PROJECT_NAME} PRIVATE
        ThirdParty/Vulkan/Mac/vk_video
        ThirdParty/Vulkan/Mac
    )
    target_link_directories(${PROJECT_NAME} PRIVATE
        ThirdParty/Vulkan/Mac/Lib
    )
    target_link_libraries(${PROJECT_NAME} PRIVATE
        libvulkan.dylib
    )
endif()

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        ${CMAKE_CURRENT_SOURCE_DIR}/ThirdParty/Font/SourceHanSansSC-Regular.otf
        ${CMAKE_BINARY_DIR}/Font/SourceHanSansSC-Regular.otf
)
