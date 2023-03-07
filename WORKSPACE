# TODO - Rename Project
workspace(name = "project_name")

###############################################################################
# Begin: Build Variables
#
# These variable represent the major flags that MAY be changed to support
# different environments.
###############################################################################

# Go Language Variables
RULES_GO_VERSION = "1.20.1"


# Hugo Documentation Variables
RULES_HUGO_COMMIT = "02234789fa9f2112807c1642eacb9f9728fc179d"

RULES_HUGO_SHA256 = "4ce20c981ad50ac0c956e85ef991e59b204778bde59d81e40be05450259ae969"

RULES_HUGO_VERSION = "0.101.0"

HUGO_THEME_SHA256 = "7fdd57f7d4450325a778629021c0fff5531dc8475de6c4ec70ab07e9484d400e"

HUGO_THEME_URL = "https://github.com/thegeeklab/hugo-geekdoc/releases/download/v0.34.2/hugo-geekdoc.tar.gz"

###############################################################################
# End: Build Variables
###############################################################################

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

###############################################################################
# GRPC Tool Chain
###############################################################################

git_repository(
    name = "com_google_googleapis",
    commit = "59b73bd6f7c00dc5af895414c444b08055849bdf",
    remote = "https://github.com/googleapis/googleapis",
    shallow_since = "1665087085 -0700",
)

load("@com_google_googleapis//:repository_rules.bzl", "switched_rules_by_language")

switched_rules_by_language(
    name = "com_google_googleapis_imports",
    go = True,
    grpc = True,
)

http_archive(
    name = "rules_proto_grpc",
    sha256 = "fb7fc7a3c19a92b2f15ed7c4ffb2983e956625c1436f57a3430b897ba9864059",
    strip_prefix = "rules_proto_grpc-4.3.0",
    urls = ["https://github.com/rules-proto-grpc/rules_proto_grpc/archive/4.3.0.tar.gz"],
)

load("@rules_proto_grpc//:repositories.bzl", "rules_proto_grpc_repos", "rules_proto_grpc_toolchains")

rules_proto_grpc_toolchains()

rules_proto_grpc_repos()

load("@rules_proto//proto:repositories.bzl", "rules_proto_dependencies", "rules_proto_toolchains")

rules_proto_dependencies()

rules_proto_toolchains()

load("@rules_proto_grpc//doc:repositories.bzl", rules_proto_grpc_doc_repos = "doc_repos")

rules_proto_grpc_doc_repos()

###############################################################################
# GO Tool Chain
###############################################################################

http_archive(
    name = "io_bazel_rules_go",
    sha256 = "dd926a88a564a9246713a9c00b35315f54cbd46b31a26d5d8fb264c07045f05d",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_go/releases/download/v0.38.1/rules_go-v0.38.1.zip",
        "https://github.com/bazelbuild/rules_go/releases/download/v0.38.1/rules_go-v0.38.1.zip",
    ],
)

http_archive(
    name = "bazel_gazelle",
    sha256 = "ecba0f04f96b4960a5b250c8e8eeec42281035970aa8852dda73098274d14a1d",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-gazelle/releases/download/v0.29.0/bazel-gazelle-v0.29.0.tar.gz",
        "https://github.com/bazelbuild/bazel-gazelle/releases/download/v0.29.0/bazel-gazelle-v0.29.0.tar.gz",
    ],
)

load("@rules_proto_grpc//:repositories.bzl", "bazel_gazelle", "io_bazel_rules_go")  # buildifier: disable=same-origin-load

io_bazel_rules_go()

load("@rules_proto_grpc//go:repositories.bzl", rules_proto_grpc_go_repos = "go_repos")

rules_proto_grpc_go_repos()

load("@io_bazel_rules_go//go:deps.bzl", "go_register_toolchains", "go_rules_dependencies")

go_rules_dependencies()

go_register_toolchains(version = RULES_GO_VERSION)

###############################################################################
# Gazelle Dependencies
###############################################################################
load("//:build/go_deps.bzl", "go_dependencies")

# gazelle:repository_macro build/go_deps.bzl%go_dependencies
go_dependencies()


load("@com_github_grpc_grpc//bazel:grpc_deps.bzl", "grpc_deps")

grpc_deps()

###############################################################################
# Rules Package for Tar
###############################################################################
http_archive(
    name = "rules_pkg",
    sha256 = "eea0f59c28a9241156a47d7a8e32db9122f3d50b505fae0f33de6ce4d9b61834",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_pkg/releases/download/0.8.0/rules_pkg-0.8.0.tar.gz",
        "https://github.com/bazelbuild/rules_pkg/releases/download/0.8.0/rules_pkg-0.8.0.tar.gz",
    ],
)

load("@rules_pkg//:deps.bzl", "rules_pkg_dependencies")

rules_pkg_dependencies()

###############################################################################
# Hugo Tool Chain
###############################################################################

# Update these to latest
# Use a modified version of the popular rules_hugo, this modified version
# allows modern Hugo themes to be used.
http_archive(
    name = "build_stack_rules_hugo",
    sha256 = RULES_HUGO_SHA256,
    strip_prefix = "rules_hugo-%s" % RULES_HUGO_COMMIT,
    url = "https://github.com/rrmcguinness/rules_hugo/archive/%s.zip" % RULES_HUGO_COMMIT,
)

load("@build_stack_rules_hugo//hugo:rules.bzl", "hugo_repository")

#
# Load hugo binary itself
#
# Optionally, load a specific version of Hugo, with the 'version' argument
hugo_repository(
    name = "hugo",
    extended = True,
    version = RULES_HUGO_VERSION,
)

# Create a readable archive from a GitHub Hugo Theme that DOES NOT support the theme layout.
http_archive(
    name = "theme_geekdoc",
    build_file_content = """
filegroup(
    name = "files",
    srcs = glob(["**"]),
    visibility = ["//visibility:public"],
)
    """,
    sha256 = HUGO_THEME_SHA256,
    url = HUGO_THEME_URL,
)

# DO NOT MOVE THESE LINES
load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies")

gazelle_dependencies()
