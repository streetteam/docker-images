# Docker images

This is a repository for our publicly available Docker base images.

## CircleCI base images

Images prefixed with `circleci-` are used to boot build and test jobs in our [CircleCI 2.0 workflow](https://circleci.com/).

Each image inherits from `circleci-base`, and provides an extra set of preloaded tools for building or testing our codebase.

The `.circleci/config` config defines a nightly workflow (as well as on each new commit) which rebuilds all 3 images to ensure they have the latest upstream patches from the Ubuntu repos and any pulled in PPAs.

 * `circleci-base`: this base image provides all the tools required to build our codebase.
 * `circleci-fullstack`: this extends the base image to include tools like `redis`, `rabbitmq` and `mailcatcher` that are required by unit and integration tests (`postgresql` is provided as an extra docker container from the upstream images).
 * `circleci-fullstack-chrome`: this extends the fullstack image to also include the latest stable Google Chrome and virtual framebuffer (`xvfb`) for running end-to-end browser tests.
 * `circleci-deploy`: This image **is not based on `circleci-base`** and is instead based on Bitnami's minideb base image to provide the smallest possible yet capable container for deploying a successful build to Heroku (ships with `heroku-toolbelt`).
