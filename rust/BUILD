# Protobuf Rust runtime packages.

load("@rules_rust//rust:defs.bzl", "rust_library", "rust_test")
load("//third_party/bazel_skylib/rules:common_settings.bzl", "string_flag")

load("@rules_proto//proto:defs.bzl", "proto_lang_toolchain")

package(default_visibility = ["//src/google/protobuf:__subpackages__"])

rust_library(
    name = "protobuf",
    srcs = ["lib.rs"],
    rustc_flags = select({
        ":use_upb_backend": ["--cfg=upb_backend"],
        "//conditions:default": ["--cfg=cpp_backend"],
    }),
    deps = select({
        ":use_upb_backend": ["//rust/upb_backend:upb"],
        "//conditions:default": ["//rust/cpp_backend:cpp"],
    }),
)

rust_test(
    name = "protobuf_test",
    crate = ":protobuf",
    rustc_flags = select({
        ":use_upb_backend": ["--cfg=upb_backend"],
        "//conditions:default": ["--cfg=cpp_backend"],
    }),
    tags = [
        "not_build:arm",
        "notsan",
    ],
)

# TODO(b/270125787): Move to the right location once rust_proto_library is no longer experimental.
proto_lang_toolchain(
    name = "proto_lang_toolchain",
    command_line = "--rust_out=experimental-codegen=enabled,backend=" + select({
        ":use_upb_backend": "upb",
        "//conditions:default": "cpp",
    }) + ":$(OUT)",
    progress_message = "Generating Rust proto_library %{label}",
    runtime = ":protobuf",
    visibility = ["//visibility:public"],
)

# This flag controls what backend all Rust Protobufs are using in the current build.
string_flag(
    name = "rust_proto_library_backend",
    build_setting_default = "cpp",
    values = [
        "upb",
        "cpp",
    ],
)

config_setting(
    name = "use_upb_backend",
    flag_values = {
        ":rust_proto_library_backend": "upb",
    },
)
