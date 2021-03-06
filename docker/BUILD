load("@bazel_tools//tools/build_defs/pkg:pkg.bzl", "pkg_tar")
load("@io_bazel_rules_docker//docker:docker.bzl", "docker_build")
load("//docker:mixer_docker.bzl", "mixer_docker_build")

# Use "manual" target tag to skip rules in the wildcard expansion

pkg_tar(
    name = "mixer_tar",
    extension = "tar.gz",
    files = [
        "//cmd/server:mixs",
    ],
    mode = "0755",
    package_dir = "/usr/local/bin/",
    tags = ["manual"],
)

mixer_docker_build(
    cmd = [
        "--configStoreURL=fs:///etc/opt/mixer/configroot",
        "--logtostderr",
        "--v=2",
    ],
    entrypoint = [
        "/usr/local/bin/mixs",
        "server",
    ],
    images = [
        {
            "name": "mixer",
            "base": "@docker_ubuntu//:xenial",
        },
        {
            "name": "mixer_debug",
            "base": "@ubuntu_xenial_debug//file",
        },
    ],
    ports = [
        "9091",
        "9093",
        "9094",
        "42422",
    ],
    repository = "istio",
    tags = ["manual"],
    tars = [
        ":mixer_tar",
        "//testdata:configs_tar",
    ],
)
