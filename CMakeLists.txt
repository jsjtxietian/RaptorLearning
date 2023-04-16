cmake_minimum_required(VERSION 3.0.0)

project(RaptorEngine VERSION 0.1.0)

find_package(Vulkan REQUIRED)

set(CMAKE_SUPPRESS_REGENERATION true)

add_library(RaptorFoundation STATIC
    source/foundation/array.hpp
    source/foundation/assert.cpp
    source/foundation/assert.hpp
    source/foundation/bit.cpp
    source/foundation/bit.hpp
    source/foundation/blob_serialization.cpp
    source/foundation/blob_serialization.hpp
    source/foundation/blob.hpp
    source/foundation/camera.cpp
    source/foundation/camera.hpp
    source/foundation/color.cpp
    source/foundation/color.hpp
    source/foundation/data_structures.cpp
    source/foundation/data_structures.hpp
    source/foundation/file.cpp
    source/foundation/file.hpp
    source/foundation/gltf.cpp
    source/foundation/gltf.hpp
    source/foundation/hash_map.hpp
    source/foundation/log.cpp
    source/foundation/log.hpp
    source/foundation/memory_utils.hpp
    source/foundation/memory.cpp
    source/foundation/memory.hpp
    source/foundation/numerics.cpp
    source/foundation/numerics.hpp
    source/foundation/platform.hpp
    source/foundation/process.cpp
    source/foundation/process.hpp
    source/foundation/relative_data_structures.hpp
    source/foundation/resource_manager.cpp
    source/foundation/resource_manager.hpp
    source/foundation/serialization.cpp
    source/foundation/serialization.hpp
    source/foundation/service_manager.cpp
    source/foundation/service_manager.hpp
    source/foundation/service.cpp
    source/foundation/service.hpp
    source/foundation/string.cpp
    source/foundation/string.hpp
    source/foundation/time.cpp
    source/foundation/time.hpp
)

set_property(TARGET RaptorFoundation PROPERTY CXX_STANDARD 17)

target_compile_definitions(RaptorFoundation PRIVATE
    _CRT_SECURE_NO_WARNINGS

    TRACY_ENABLE
    TRACY_ON_DEMAND
    TRACY_NO_SYSTEM_TRACING
)

target_include_directories(RaptorFoundation PRIVATE
    source
    source/raptor
)

add_library(RaptorApp STATIC
    source/application/application.cpp
    source/application/application.hpp
    source/application/game_camera.cpp
    source/application/game_camera.hpp
    source/application/game_application.cpp
    source/application/game_application.hpp
    source/application/input.cpp
    source/application/input.hpp
    source/application/keys.cpp
    source/application/keys.hpp
    source/application/window.cpp
    source/application/window.hpp
)

set_property(TARGET RaptorApp PROPERTY CXX_STANDARD 17)

target_compile_definitions(RaptorApp PRIVATE
    _CRT_SECURE_NO_WARNINGS

    TRACY_ENABLE
    TRACY_ON_DEMAND
    TRACY_NO_SYSTEM_TRACING
)

target_include_directories(RaptorApp PRIVATE
    source
    )

if (WIN32)
    target_include_directories(RaptorApp PRIVATE
        binaries/SDL2-2.0.18/include)
else()
    target_include_directories(RaptorApp PRIVATE
        ${SDL2_INCLUDE_DIRS})
endif()

add_library(RaptorExternal STATIC
    source/external/tlsf.c

    source/external/json.hpp

    source/external/imgui/imconfig.h
    source/external/imgui/imgui_draw.cpp
    source/external/imgui/imgui_impl_sdl.cpp
    source/external/imgui/imgui_impl_sdl.h
    source/external/imgui/imgui_internal.h
    source/external/imgui/imgui_memory_editor.h
    source/external/imgui/imgui_tables.cpp
    source/external/imgui/imgui_widgets.cpp
    source/external/imgui/imgui.cpp
    source/external/imgui/imgui.h
    source/external/imgui/imstb_rectpack.h
    source/external/imgui/imstb_textedit.h
    source/external/imgui/imstb_truetype.h
    source/external/imgui/stb_image.h
    source/external/imgui/TextEditor.cpp
    source/external/imgui/TextEditor.h

    source/external/tracy/tracy/Tracy.hpp
    source/external/tracy/tracy/TracyVulkan.hpp
    source/external/tracy/TracyClient.cpp

    source/external/enkiTS/LockLessMultiReadPipe.h
    source/external/enkiTS/TaskScheduler.cpp
    source/external/enkiTS/TaskScheduler.h

    source/external/meshoptimizer/allocator.cpp
    source/external/meshoptimizer/clusterizer.cpp
    source/external/meshoptimizer/indexcodec.cpp
    source/external/meshoptimizer/indexgenerator.cpp
    source/external/meshoptimizer/meshoptimizer.h
    source/external/meshoptimizer/overdrawanalyzer.cpp
    source/external/meshoptimizer/overdrawoptimizer.cpp
    source/external/meshoptimizer/simplifier.cpp
    source/external/meshoptimizer/spatialorder.cpp
    source/external/meshoptimizer/stripifier.cpp
    source/external/meshoptimizer/vcacheanalyzer.cpp
    source/external/meshoptimizer/vcacheoptimizer.cpp
    source/external/meshoptimizer/vertexcodec.cpp
    source/external/meshoptimizer/vertexfilter.cpp
    source/external/meshoptimizer/vfetchanalyzer.cpp
    source/external/meshoptimizer/vfetchoptimizer.cpp
)

if (WIN32)
    target_sources(RaptorExternal PRIVATE
        source/external/StackWalker.cpp)
endif()

target_compile_definitions(RaptorExternal PRIVATE
    TRACY_ENABLE
    TRACY_ON_DEMAND
    TRACY_NO_SYSTEM_TRACING
)

if (WIN32)
    target_include_directories(RaptorExternal PRIVATE
        binaries/SDL2-2.0.18/include
        source
    )
else()
    target_link_libraries(RaptorExternal PRIVATE
        dl
    )
    target_include_directories(RaptorExternal PRIVATE
        ${SDL2_INCLUDE_DIRS})
endif()

set_property(TARGET RaptorExternal PROPERTY CXX_STANDARD 17)

add_subdirectory(source/main)
