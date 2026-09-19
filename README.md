# C++ compiler images

Docker images for building and testing C++ projects with selected GCC and Clang major versions. Images are published to the GitHub Container Registry for the `official-stockfish/compiler-images` repository.

## Available images

| Compiler | Tags |
| --- | --- |
| GCC | `gcc-10`, `gcc-11`, `gcc-12`, `gcc-13`, `gcc-14`, `gcc-15`, `gcc-16` |
| Clang | `clang-11`, `clang-12`, `clang-13`, `clang-14`, `clang-15`, `clang-16`, `clang-17`, `clang-18`, `clang-19`, `clang-20`, `clang-21`, `clang-22`, `clang-23` |

Each image is published for `linux/amd64`.

## Using an image

Pull an image by compiler and major version:

```sh
docker pull ghcr.io/official-stockfish/compiler-images:gcc-15
docker pull ghcr.io/official-stockfish/compiler-images:clang-23
```

Use the image as a build environment:

```sh
docker run --rm -v "$PWD:/src" -w /src \
  ghcr.io/official-stockfish/compiler-images:gcc-15 \
  make
```

Images are tagged both with their compiler version and the source commit that produced them:

```text
ghcr.io/official-stockfish/compiler-images:<compiler>-<version>-<commit-sha>
```

The version-only tags are convenient for consumers and may move when an image is rebuilt. Use the commit-suffixed tag when a reproducible container reference is required.

## Building locally

Build a GCC image:

```sh
docker build \
  --build-arg BASE_IMAGE=gcc:15 \
  --tag compiler-images:gcc-15 \
  gcc/
```

Build a Clang image. For Clang 11–17, use the corresponding `silkeh/clang` base image. For Clang 18–22, use `ubuntu:rolling` with `INSTALL_CLANG=true`; Clang 23 and newer additionally use the versioned LLVM package repository:

```sh
docker build \
  --build-arg BASE_IMAGE=ubuntu:rolling \
  --build-arg CLANG_VERSION=23 \
  --build-arg INSTALL_CLANG=true \
  --tag compiler-images:clang-23 \
  clang/
```

## Publishing

GitHub Actions builds and pushes the matrix of images on pushes to `main` and through manual workflow dispatch. The workflow is defined in [`.github/workflows/publish.yml`](.github/workflows/publish.yml).

The Docker build contexts are [`gcc/`](gcc/) and [`clang/`](clang/). Both images include the common command-line tools needed by the build environment: CA certificates, `curl`, `git`, and `make`.
