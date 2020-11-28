# bazelatops

Repository to play around with [bazel](https://bazel.build)

Use [Bazelisk](https://github.com/bazelbuild/bazelisk) to bazel version management.

## About

This repostiry is a playground to see how [bazel](https://bazel.build) can be used in
with monorepo and different modern frameworks

## What is here

* [Teapotstore](https://github.com/kenota/bazelatops/tree/main/web/teapotstore) a frontend application based of vue-cli

## Available commants

* `bazel run  "//web/teapotstore:prodserver"` - will build a production artifact of teapotstore frontend and serve it locally using nodejs `http-server`
* `bazel build "//web/teapotstore:package"` - build a production version of teapotstore frontend

You can see the rebuild of dependencies by bazel in action. Run `prodserver` once, and on each subsequent run it will be much faster since bazel is not rebuilding anything. Then stop it, make change in any of `web/teapotstore/src/*` files and run `prodserver` again. This time bazel will rebuild the `//web/teapotstore:package` artifact because of a source code file change.


## Additional notes and gotchas

### vue-cli

Vue CLI seems is not playing well with bazel. Vue-cli does not expect a project it is trying to build to be anywhere except current folder:

* [This issue on bazel talk about problem and workarounds](https://github.com/bazelbuild/rules_nodejs/issues/1840)
* [There is a workaround called vue-cli-launcher](https://www.npmjs.com/package/vue-cli-launcher#bazel)
* [Issue for vew-cli exist, but with no action](https://github.com/vuejs/vue-cli/issues/3150)
* [🍒 on top - you can not eject vue-cli from a project](https://github.com/vuejs/vue-cli/issues/2796)
