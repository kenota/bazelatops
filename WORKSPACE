workspace(
    # How this workspace would be referenced with absolute labels from another workspace
    name = "bazelatops",
    # Map the @npm bazel workspace to the node_modules directory.
    # This lets Bazel use the same node_modules as other local tooling.
    managed_directories = {"@teapotstore_npm": ["web/teapotstore/node_modules"]},
)

# Install the nodejs "bootstrap" package
# This provides the basic tools for running and packaging nodejs programs in Bazel
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
http_archive(
    name = "build_bazel_rules_nodejs",
    sha256 = "121f17d8b421ce72f3376431c3461cd66bfe14de49059edc7bb008d5aebd16be",
    urls = ["https://github.com/bazelbuild/rules_nodejs/releases/download/2.3.1/rules_nodejs-2.3.1.tar.gz"],
)

# The npm_install rule runs yarn anytime the package.json or package-lock.json file changes.
# It also extracts any Bazel rules distributed in an npm package.
load("@build_bazel_rules_nodejs//:index.bzl", "npm_install", "node_repositories")
node_repositories()

npm_install(
    # Name this npm so that Bazel Label references look like @npm//package
    name = "teapotstore_npm",
    package_json = "//web/teapotstore:package.json",
    package_lock_json = "//web/teapotstore:package-lock.json",
)


# GoLang & Bazelle
# v0.24.7 of rules_go uses Go 1.15.5
http_archive(
    name = "io_bazel_rules_go",
    sha256 = "207fad3e6689135c5d8713e5a17ba9d1290238f47b9ba545b63d9303406209c6",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_go/releases/download/v0.24.7/rules_go-v0.24.7.tar.gz",
        "https://github.com/bazelbuild/rules_go/releases/download/v0.24.7/rules_go-v0.24.7.tar.gz",
    ],
)

http_archive(
    name = "bazel_gazelle",
    sha256 = "b85f48fa105c4403326e9525ad2b2cc437babaa6e15a3fc0b1dbab0ab064bc7c",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-gazelle/releases/download/v0.22.2/bazel-gazelle-v0.22.2.tar.gz",
        "https://github.com/bazelbuild/bazel-gazelle/releases/download/v0.22.2/bazel-gazelle-v0.22.2.tar.gz",
    ],
)

load("@io_bazel_rules_go//go:deps.bzl", "go_register_toolchains", "go_rules_dependencies")
load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies")

go_rules_dependencies()

go_register_toolchains()

gazelle_dependencies()


# Rules GRPC
# 1. Generic rules_grpc deps:


http_archive(
    name = "rules_proto_grpc",
    urls = ["https://github.com/rules-proto-grpc/rules_proto_grpc/archive/2.0.0.tar.gz"],
    sha256 = "d771584bbff98698e7cb3cb31c132ee206a972569f4dc8b65acbdd934d156b33",
    strip_prefix = "rules_proto_grpc-2.0.0",
)

load("@rules_proto_grpc//:repositories.bzl", "rules_proto_grpc_toolchains", "rules_proto_grpc_repos", "io_bazel_rules_go", "bazel_gazelle")
rules_proto_grpc_toolchains()
rules_proto_grpc_repos()

load("@rules_proto//proto:repositories.bzl", "rules_proto_dependencies", "rules_proto_toolchains")
rules_proto_dependencies()
rules_proto_toolchains()

# 2. GRPC-go specific rules_grpc rules:

io_bazel_rules_go()
bazel_gazelle()

load("@rules_proto_grpc//go:repositories.bzl", rules_proto_grpc_go_repos="go_repos")

rules_proto_grpc_go_repos()