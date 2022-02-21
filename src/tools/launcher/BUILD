load(":win_rules.bzl", "win_cc_binary", "win_cc_library")

filegroup(
    name = "srcs",
    srcs = glob(["**"]) + ["//src/tools/launcher/util:srcs"],
    visibility = ["//src:__pkg__"],
)

config_setting(
    name = "msvc_compiler",
    flag_values = {"@bazel_tools//tools/cpp:compiler": "msvc-cl"},
)

config_setting(
    name = "clang-cl_compiler",
    flag_values = {"@bazel_tools//tools/cpp:compiler": "clang-cl"},
)

# For <filesystem>
_MSVC_COPTS = ["/std:c++17"]

# For <filesystem> and make_unique on old GCC/MinGW
_GCC_COPTS = ["-std=c++17"]

_DEFAULT_COPTS = select({
    ":msvc_compiler": _MSVC_COPTS,
    ":clang-cl_compiler": _MSVC_COPTS,
    "//conditions:default": _GCC_COPTS,
})

win_cc_binary(
    name = "launcher",
    srcs = ["launcher_main.cc"],
    copts = _DEFAULT_COPTS,
    features = ["fully_static_link"],
    linkstatic = True,
    visibility = [
        "//src:__pkg__",
        "//tools/launcher:__pkg__",
    ],
    deps = [
        ":bash_launcher",
        ":java_launcher",
        ":launcher_base",
        ":python_launcher",
        "//src/tools/launcher/util",
        "//src/tools/launcher/util:data_parser",
    ],
)

win_cc_library(
    name = "launcher_base",
    srcs = ["launcher.cc"],
    hdrs = ["launcher.h"],
    copts = _DEFAULT_COPTS,
    deps = [
        "//src/main/cpp/util:filesystem",
        "//src/tools/launcher/util",
        "//src/tools/launcher/util:data_parser",
    ],
)

win_cc_library(
    name = "java_launcher",
    srcs = ["java_launcher.cc"],
    hdrs = ["java_launcher.h"],
    copts = _DEFAULT_COPTS,
    deps = [
        ":launcher_base",
        "//src/main/native/windows:lib-process",
    ],
)

win_cc_library(
    name = "python_launcher",
    srcs = ["python_launcher.cc"],
    hdrs = ["python_launcher.h"],
    copts = _DEFAULT_COPTS,
    deps = [
        ":launcher_base",
        "//src/main/native/windows:lib-process",
    ],
)

win_cc_library(
    name = "bash_launcher",
    srcs = ["bash_launcher.cc"],
    hdrs = ["bash_launcher.h"],
    copts = _DEFAULT_COPTS,
    deps = [":launcher_base"],
)
