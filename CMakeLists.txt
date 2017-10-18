cmake_minimum_required(VERSION 3.8)
project(RaceEstimator)

set(CMAKE_CXX_STANDARD 17)

set(GENERATED_SOURCE_FILES
        generated/fonts/src/ApplicationFontProvider.cpp
        generated/images/src/BitmapDatabase.cpp
        generated/texts/src/TypedTextDatabase.cpp
        generated/texts/src/Texts.cpp
        )

set(GENERATED_HEADER_FILES
        generated/fonts/include/fonts/ApplicationFontProvider.hpp
        generated/images/include/BitmapDatabase.hpp
        generated/texts/include/texts/TypedTextDatabase.hpp
        generated/texts/include/texts/TextKeysAndLanguages.hpp
        )

set(GUI_SOURCE_FILES
        gui/src/common/FrontendApplication.cpp
        gui/src/model/Model.cpp
        gui/src/template_screen/TemplateView.cpp
        gui/src/template_screen/TemplatePresenter.cpp
)

set(GUI_HEADER_FILES
        gui/include/gui/template_screen/TemplateView.hpp
        gui/include/gui/model/ModelListener.hpp
        gui/include/gui/model/Model.hpp
        gui/include/gui/common/FrontendHeap.hpp
        gui/include/gui/common/FrontendApplication.hpp
        )

set(SOURCE_FILES main.cpp
        touchgfx/framework/source/platform/hal/simulator/sdl2/HALSDL2.cpp
        touchgfx/framework/source/platform/hal/simulator/sdl2/HALSDL2_icon.cpp
        touchgfx/framework/source/platform/hal/simulator/sdl2/OSWrappers.cpp
        touchgfx/framework/source/platform/driver/touch/SDL2TouchController.cpp
        )


add_definitions(-DSIMULATOR)
include_directories(touchgfx/framework/include)
include_directories(touchgfx/framework/include/platform/hal/simulator/sdl2/vendor)
include_directories(touchgfx/framework/include/platform/hal/simulator)
include_directories(generated/texts/include)
include_directories(generated/images/include)
include_directories(generated/fonts/include)
include_directories(gui/include)

link_directories(${CMAKE_SOURCE_DIR}/touchgfx/lib/linux)
link_directories(${CMAKE_SOURCE_DIR}/touchgfx/lib/sdl2/linux64)

add_custom_target(
        BuildAssets
        DEPENDS assets/texts/texts.xlsx
        COMMAND make -f simulator/gcc/Makefile assets
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)


add_executable(RaceEstimator ${SOURCE_FILES} ${GUI_SOURCE_FILES} ${GUI_HEADER_FILES} ${GENERATED_HEADER_FILES} ${GENERATED_SOURCE_FILES})
target_link_libraries(${PROJECT_NAME} touchgfx SDL2 SDL2_image rt m pthread dl)

add_dependencies(${PROJECT_NAME} BuildAssets)