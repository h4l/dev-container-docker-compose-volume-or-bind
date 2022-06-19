# Dev Container Example: Docker Compose with workspace in volume or bind mount

## Summary

An example [Dev Container](https://code.visualstudio.com/docs/remote/containers)
configuration for a Docker Compose project with workspace code accessed from the
local filesystem via a bind mount, or with the project code in a Docker
container volume.

The project can be opened in Visual Studio Code via the command palette
**Remote-Containers: Clone Repository in Container Volume...** or by opening a
local checkout of the repository.

Supporting both container volume workspaces and bind mount workspaces in a
single Docker Compose dev container configuration is not easy at present.
Hopefully this example will become redundant in the near future if the Remote
Containers plugin provides a way to consistently access workspace content from
volumes and bind mounts in Docker Compose services.

## Using this example

1. If this is your first time using a development container, please see getting started information on [setting up](https://aka.ms/vscode-remote/containers/getting-started) Remote-Containers.

2. To open it from a container volume:

    1. Start VS Code, press <kbd>F1</kbd> and select **Remote-Containers: Clone
       Repository in Container Volume...** and enter this repository's URL

   Or to open it from the local filesystem:

    1. Clone this repository on your computer

    2. Start VS Code and open this project folder.

    3. Press <kbd>F1</kbd> and run **Remote-Containers: Reopen Folder in Container**

To use it in your own project, copy the `.devcontainer` directory from this
repository into your project and edit the `devcontainer.json` and
`docker-compose.yml` as required. Make sure the
`.devcontainer/gen-docker-compose-workspace-env.sh` script is marked executable
(`chmod +x ...`).

The key parts are:

* In `devcontainer.json` run the envar-generation script in `initializeCommand`:

    ```json5
    "initializeCommand": ".devcontainer/gen-docker-compose-workspace-env.sh --container-workspace-folder '${containerWorkspaceFolder}' --local-workspace-folder '${localWorkspaceFolder}'"
    ```
* In `docker-compose.yml`:

    * Access the workspace files in a service by defining the `volumes` entry:

    ```yaml
    volumes:
      - ${WORKSPACE_SOURCE:?}:${WORKSPACE_TARGET:?}
    ```

    * And define this top-level volume:

    ```yaml
    volumes:
      devcontainer-volume:
        name: ${WORKSPACE_CONTAINER_VOLUME_SOURCE:-not-used-in-bind-mount-workspace}
        external: ${WORKSPACE_IS_CONTAINER_VOLUME:?}
    ```

## Compatibility

* The [Remote Containers] plugin has changed its behaviour between updates in
  incompatible ways several times. Future updates could break this example
  again.
* The example has not been tested with GitHub Codespaces as I've not got an
  account with access to it.

[Remote Containers]: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers

## How it works

The [`devcontainer.json`] `"initializeCommand"` runs
[`gen-docker-compose-workspace-env.sh`] which generates several environment
variables in `.devcontainer/.env` which vary depending on whether the workspace
is in a container volume or a local bind mount.
[`.devcontainer/docker-compose.yml`] references these environment variables when
defining its service volumes, which allows one Compose file to consistently
handle bind mounts and volume workspaces.

Run [`gen-docker-compose-workspace-env.sh`] with `--help` or just read its help
text for more details.

[`devcontainer.json`]: .devcontainer/devcontainer.json
[`gen-docker-compose-workspace-env.sh`]: .devcontainer/gen-docker-compose-workspace-env.sh
[`.devcontainer/docker-compose.yml`]: .devcontainer/docker-compose.yml
