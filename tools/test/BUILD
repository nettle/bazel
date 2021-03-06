package(default_visibility = ["//visibility:public"])

# Members of this filegroup shouldn't have duplicate basenames, otherwise
# TestRunnerAction#getRuntimeArtifact() will get confused.
# Deprecated, do not use.
filegroup(
    name = "runtime",
    srcs = ["test-setup.sh"],
)

filegroup(
    name = "test_setup",
    srcs = ["test-setup.sh"],
)

filegroup(
    name = "test_xml_generator",
    srcs = ["generate-xml.sh"],
)

filegroup(
    name = "collect_coverage",
    srcs = ["collect_coverage.sh"],
)

filegroup(
    name = "coverage_support",
    srcs = ["collect_coverage.sh"],
)

filegroup(
    name = "collect_cc_coverage",
    srcs = ["collect_cc_coverage.sh"],
)

filegroup(
    name = "coverage_report_generator",
    srcs = ["@bazel_tools//tools/test/CoverageOutputGenerator/java/com/google/devtools/coverageoutputgenerator:Main"],
)

# Test wrapper binary to run tests on Windows.
# See https://github.com/bazelbuild/bazel/issues/5508
cc_binary(
    name = "tw",
    srcs = select({
        "@bazel_tools//src/conditions:windows": ["windows/test_wrapper.cc"],
        "//conditions:default": ["empty_test_wrapper.cc"],
    }),
    linkopts = select({
        "//src/conditions:windows": [
            "-DEFAULTLIB:advapi32.lib",  # GetUserNameW
        ],
        "//conditions:default": [],
    }),
    visibility = ["//visibility:private"],
    deps = select({
        "@bazel_tools//src/conditions:windows": [
            "//src/main/cpp/util:filesystem",
            "//src/main/cpp/util:strings",
            "//src/main/native/windows:lib-file",
            "@bazel_tools//tools/cpp/runfiles",
        ],
        "//conditions:default": [],
    }),
)

filegroup(
    name = "srcs",
    srcs = glob(["**"]),
)

filegroup(
    name = "embedded_tools",
    srcs = [
        "BUILD.tools",
        "test-setup.sh",
        "generate-xml.sh",
        "collect_coverage.sh",
        "collect_cc_coverage.sh",
    ] + glob(["LcovMerger/**"]) + select({
        "@bazel_tools//src/conditions:windows": ["tw"],
        "//conditions:default": [],
    }),
    visibility = ["//tools:__pkg__"],
)

test_suite(
    name = "windows_tests",
    tags = [
        "-no_windows",
        "-slow",
    ],
    visibility = ["//visibility:private"],
)

test_suite(
    name = "all_windows_tests",
    tests = [
        ":windows_tests",
        "//tools/test/CoverageOutputGenerator/javatests/com/google/devtools/coverageoutputgenerator:all_windows_tests",
    ],
    visibility = ["//tools:__pkg__"],
)
