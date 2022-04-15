# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] — 2022-04-15

- Fixed bind-mount devcontainers failing to start due to the
  `devcontainer-volume` being marked as external, but not existing. The
  `docker-compose.yml` examples now define the `devcontainer-volume` like this:

  ```
  volumes:
    devcontainer-volume:
      name: ${WORKSPACE_CONTAINER_VOLUME_SOURCE:-not-used-in-bind-mount-workspace}
      external: ${WORKSPACE_IS_CONTAINER_VOLUME:?}
  ```

This version is tested with [Remote Containers] version `0.232.3` –
`0.232.6`.

## [1.0.0] — 2022-04-05

This version is tested with [Remote Containers] version `0.232.3` (2022-04-04).

[Remote Containers]: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers
