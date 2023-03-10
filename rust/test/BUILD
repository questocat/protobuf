load("//rust:defs.bzl", "rust_proto_library")
load("@rules_rust//rust:defs.bzl", "rust_test")

rust_proto_library(
    name = "unittest_rust_proto",
    testonly = True,
    deps = ["//third_party/protobuf:unittest_proto"],
)

rust_test(
    name = "unittest_proto_test",
    srcs = ["unittest_proto_test.rs"],
    # TODO(b/270274576): Enable testing on arm once we have a Rust Arm toolchain.
    tags = [
        "not_build:arm",
        "notsan",
    ],
    deps = [":unittest_rust_proto"],
)

proto_library(
    name = "parent_proto",
    srcs = ["parent.proto"],
)

proto_library(
    name = "child_proto",
    srcs = ["child.proto"],
    exports = [":parent_proto"],
    deps = [":parent_proto"],
)

rust_proto_library(
    name = "parent_rust_proto",
    deps = [":parent_proto"],
)

rust_proto_library(
    name = "child_rust_proto",
    deps = [":child_proto"],
)

rust_test(
    name = "child_parent_test",
    srcs = ["child_parent_test.rs"],
    # TODO(b/270274576): Enable testing on arm once we have a Rust Arm toolchain.
    tags = [
        "not_build:arm",
        "notsan",
    ],
    deps = [
        ":child_rust_proto",
        ":parent_rust_proto",
    ],
)
