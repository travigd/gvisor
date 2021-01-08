load("//tools:defs.bzl", "go_add_tags", "go_embed_data", "go_library")

package(licenses = ["notice"])

go_embed_data(
    name = "vdso_bin",
    src = "//vdso:vdso.so",
    package = "vdsodata",
    var = "Binary",
)

[
    # Generate multiple tagged files. Note that the content will always be
    # the same, but the build tags will ensure only one is selected. When we
    # generate the "Go" branch, we select all relevant architecture files
    # from the appropriate architecture build. This is a hack around some
    # starlark limitations about "out" not being a configurable attribute.
    go_add_tags(
        name = "vdso_%s" % arch,
        src = ":vdso_bin",
        out = "vdso_%s.go" % arch,
        go_tags = [arch],
    )
    for arch in ("amd64", "arm64")
]

go_library(
    name = "vdsodata",
    srcs = [
        "vdsodata.go",
        ":vdso_amd64",
        ":vdso_arm64",
    ],
    marshal = False,
    stateify = False,
    visibility = ["//pkg/sentry:internal"],
)
