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






# GRPC and Protobuf required rules
http_archive(
    name = "build_stack_rules_proto",
    urls = ["https://github.com/stackb/rules_proto/archive/b2913e6340bcbffb46793045ecac928dcf1b34a5.tar.gz"],
    sha256 = "d456a22a6a8d577499440e8408fc64396486291b570963f7b157f775be11823e",
    strip_prefix = "rules_proto-b2913e6340bcbffb46793045ecac928dcf1b34a5",
)

load("@build_stack_rules_proto//github.com/grpc/grpc-web:deps.bzl", "ts_grpc_compile")
ts_grpc_compile()

load("@io_bazel_rules_closure//closure:defs.bzl", "closure_repositories")

closure_repositories(
    omit_com_google_protobuf = True,
)

http_archive(
    name = "com_google_googleapis",
    sha256 = "fe3e2be98dfb247556ed864f781dac3464b029ba6e6cf0ac94a6862bcabe8fa2",
    strip_prefix = "googleapis-df4fd38d040c5c8a0869936205bca13fb64b2cff",
    url = "https://github.com/googleapis/googleapis/archive/df4fd38d040c5c8a0869936205bca13fb64b2cff.zip",
)

load("@com_google_googleapis//:repository_rules.bzl", "switched_rules_by_language")
switched_rules_by_language(
    name = "com_google_googleapis_imports",
    grpc = True,
)

#### GRPC-Gateway setup  #####
load("@build_stack_rules_proto//:deps.bzl", "bazel_gazelle", "io_bazel_rules_go")

io_bazel_rules_go()

load("@io_bazel_rules_go//go:deps.bzl", "go_register_toolchains", "go_rules_dependencies")

go_rules_dependencies()

go_register_toolchains()

bazel_gazelle()

load("@build_stack_rules_proto//github.com/grpc-ecosystem/grpc-gateway:deps.bzl", "gateway_grpc_compile")

gateway_grpc_compile()

load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies")

gazelle_dependencies()


# ### OpenAPI/Swagger codegen

# RULES_OPEN_API_VERSION = "f0f42afb855139ad5346659d089c32fb756d068e"
# RULES_OPEN_API_SHA256 = "9570186948f1f65c61d2c6c6006840ea70888b270f028bbd0eb736caae1cd9df"

# http_archive(
#     name = "io_bazel_rules_openapi",
#     strip_prefix = "rules_openapi-%s" % RULES_OPEN_API_VERSION,
#     url = "https://github.com/meetup/rules_openapi/archive/%s.tar.gz" % RULES_OPEN_API_VERSION,
#     sha256 = RULES_OPEN_API_SHA256
# )

# load("@io_bazel_rules_openapi//openapi:openapi.bzl", "openapi_repositories")
# openapi_repositories()