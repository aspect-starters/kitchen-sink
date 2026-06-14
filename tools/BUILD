"""
Build supporting tools.

The tools subpackage is used to define build rules and configurations for tools
used throughout the repository. Firstparty tools not required for build support
should live elsewhere.

Of special note is that we're using the `bazel_env.bzl` tool to re-export tools
used under Bazel as a `bin/` tree which can be added to the `$PATH`. The
included `.envrc` automates this for you.

"""
load("@bazelrc-preset.bzl", "bazelrc_preset")
load("@bazel_env.bzl", "bazel_env")
load("@multitool//:tools.bzl", MULTITOOL_TOOLS = "TOOLS")

load("@gazelle//:go_tools.bzl", "GO_TOOLS")


package(default_visibility = ["//visibility:public"])

bazelrc_preset(
    name = "preset",
    doc_link_template = "https://registry.build/flag/bazel?filter={flag}",
)


load("@rules_python//python/entry_points:py_console_script_binary.bzl", "py_console_script_binary")

py_console_script_binary(
    name = "copier",
    pkg = "@pip//copier",
)



load("@npm//:defs.bzl", "npm_link_all_packages")

npm_link_all_packages(name = "node_modules")


load("@npm//tools:yo/package_json.bzl", yo_bin = "bin")
yo_bin.yo_binary(name = "yo")



bazel_env(
    name = "bazel_env",
    toolchains = {

        "jdk": "@rules_java//toolchains:current_host_java_runtime",


        "python": "@rules_python//python:current_py_toolchain",
        "uv": "@multitool//tools/uv:uv",


        "nodejs": "@nodejs_toolchains//:resolved_toolchain",
        "prettier": "//tools/format:prettier",

    },
    tools = {
        # Mapping from tool binary name ($PATH entry) to Bazel label

        "buildifier": "@buildifier_prebuilt//:buildifier",
        "format": "//tools/format:format",


        "cargo": "@rules_rust//tools/upstream_wrapper:cargo",


        "copier": ":copier",


        "go": "@rules_go//go",


        "jar": "$(JAVABASE)/bin/jar",
        "java": "$(JAVA)",


        "node": "$(NODE_PATH)",
        "npm": "$(NPM_PATH)",
        "pnpm": "@pnpm",

        "yo": ":yo",



        "python": "$(PYTHON3)",


        "bundle": "@ruby//:bundle",
        "gem": "@ruby//:gem",
        "ruby": "@ruby",

    } | MULTITOOL_TOOLS  | GO_TOOLS ,
)


load("@rules_ruby//ruby:defs.bzl", "rb_binary")

rb_binary(
    name = "grpc_tools_ruby_protoc_plugin",
    main = "@bundle//bin:grpc_tools_ruby_protoc_plugin",
    deps = ["@bundle"],
)
