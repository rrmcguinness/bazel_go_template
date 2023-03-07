# Bazel Go Template

This project is a simple shell for building Go projects with Bazel.

## Building

```shell
# Build all
bazel build //...

# Test all
bazel test //...

# Code Coverage
bazel coverage //...
```

## Structure

See [Go Project Stucture](https://github.com/golang-standards/project-layout) for more details.

## API 

Is a simple directory demonstraining protocol buffer APIs.

```shell
# Build
bazel build //api/...
```

## Documents

Is a Hugo site that automatically generates the API documentation. 

```shell
# Build the site
bazel build //docs/...

# Run the site
bazel run //docs:serve
```
