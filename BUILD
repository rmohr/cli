load("@io_bazel_rules_go//go:def.bzl", "go_binary", "go_library")

package(default_visibility = ["//visibility:public"])

load("@bazel_gazelle//:def.bzl", "gazelle")
load("@bazel_tools//tools/build_defs/pkg:pkg.bzl", "pkg_tar")
load("@io_bazel_rules_docker//contrib:push-all.bzl", "docker_push")
load("@io_bazel_rules_docker//go:image.bzl", "go_image")
load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_bundle",
    container_repositories = "repositories",
)

gazelle(
    name = "gazelle",
    prefix = "github.com/rmohr/cli",
)

go_library(
    name = "go_default_library",
    srcs = ["main.go"],
    importpath = "github.com/rmohr/cli",
    deps = ["//cmd:go_default_library"],
)

go_binary(
    name = "cli",
    embed = [":go_default_library"],
)

go_image(
    name = "gocli",
    binary = ":cli",
    base = "@fedora//image",
    cmd = [""],
    visibility = ["//visibility:public"],
)

container_bundle(
    name = "container-bundle",
    images = {
            "index.docker.io/kubevirtci/gocli:latest": "//:gocli",
    },
)

docker_push(
    name = "push-all",
    bundle = ":container-bundle",
)
