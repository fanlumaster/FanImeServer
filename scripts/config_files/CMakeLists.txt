cmake_minimum_required(VERSION 3.25.0)

set(PROJECT_NAME "FanImeServer")

project(${PROJECT_NAME} VERSION 1.0)

# Boost
set(Boost_ROOT "C:/Users/19833/scoop/apps/boost/current")
set (Boost_USE_STATIC_LIBS ON)
find_package(Boost REQUIRED COMPONENTS locale json)
include_directories(${Boost_INCLUDE_DIRS})

# fmt, spdlog and sqlite3
find_package(fmt CONFIG REQUIRED)
find_package(spdlog CONFIG REQUIRED)
find_package(unofficial-sqlite3 CONFIG REQUIRED)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

add_definitions(
    /D_UNICODE=1
    /DUNICODE=1
)

# add_definitions(
#     /DFANY_DEBUG=1
# )

# For utf8cpp, make it use C++11 and later
add_compile_options(/Zc:__cplusplus)

include_directories("./")
include_directories("./src/")
include_directories("../FanImeEngine/utfcpp/source/")

set(MY_EXECUTABLE_NAME "FanImeServer")

set(SRC_FILES
    "./src/defines/defines.h"
    "./src/global/globals.h"
    "./src/ipc/event_listener.cpp"
    "./src/ipc/event_listener.h"
    "./src/ipc/ipc.cpp"
    "./src/ipc/ipc.h"
    "./src/log/fanylog.cpp"
    "./src/log/fanylog.h"
    "./src/utils/webview_utils.cpp"
    "./src/utils/webview_utils.h"
    "./src/utils/window_utils.cpp"
    "./src/utils/window_utils.h"
    "./src/utils/common_utils.cpp"
    "./src/utils/common_utils.h"
    "./src/webview2/candidate_window_webview2.cpp"
    "./src/webview2/candidate_window_webview2.h"
    "./src/window/candidate_window.cpp"
    "./src/window/candidate_window.h"
    "./src/main.cpp"
)

set(SRC_FILES_IME_ENGINE
    "./FanImeEngine/shuangpin/dictionary.cpp"
    "./FanImeEngine/shuangpin/pinyin_utils.cpp"
)

set(SRC_FILES_GOOGLEIME
    "./FanImeEngine/googlepinyinime-rev/src/share/dictbuilder.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/dictlist.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/dicttrie.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/lpicache.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/matrixsearch.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/mystdlib.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/ngram.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/pinyinime.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/searchutility.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/spellingtable.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/spellingtrie.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/splparser.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/sync.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/userdict.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/utf16char.cpp"
    "./FanImeEngine/googlepinyinime-rev/src/share/utf16reader.cpp"
)

add_executable(${MY_EXECUTABLE_NAME} WIN32
    ${SRC_FILES}
    ${SRC_FILES_IME_ENGINE}
    ${SRC_FILES_GOOGLEIME}
)

# WebView2
set_target_properties(${PROJECT_NAME} PROPERTIES VS_GLOBAL_WebView2LoaderPreference "Static")
set_target_properties(${PROJECT_NAME} PROPERTIES VS_USER_PROPS "${CMAKE_SOURCE_DIR}/FanImeServer.props")
set_target_properties(${PROJECT_NAME} PROPERTIES VS_PACKAGE_REFERENCES "Microsoft.Web.WebView2_1.0.3240.44;Microsoft.Windows.ImplementationLibrary_1.0.240803.1")

target_link_libraries(${PROJECT_NAME} PRIVATE unofficial::sqlite3::sqlite3 fmt::fmt spdlog::spdlog Boost::locale Boost::json)