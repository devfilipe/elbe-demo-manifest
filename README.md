# elbe-demo-manifest

Google Repo manifest for the ELBE demo workspace. This repository contains a
single `default.xml` that describes all the component repositories and their
checkout paths, allowing the entire workspace to be assembled with a single
`repo sync` command.

## Prerequisites

- Git
- [Google Repo](https://android.googlesource.com/tools/repo) tool

### Installing the `repo` tool

```bash
mkdir -p ~/.bin
PATH="${HOME}/.bin:${PATH}"
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo
chmod a+rx ~/.bin/repo
```

Add the `PATH` export to your shell profile (`~/.bashrc`, `~/.zshrc`, etc.) to
make it permanent.

## Quick start

```bash
TOPLEVEL=/path/to/your/workspace
mkdir -p "$TOPLEVEL" && cd "$TOPLEVEL"
repo init -u https://github.com/devfilipe/elbe-demo-manifest.git -b 0.1.0
repo sync
```

After `repo sync` the workspace will be populated as follows:

```
$TOPLEVEL/
├── elbe-demo-manifest/          # this repository
├── tools/
│   ├── elbe-devcontainers/      # Docker dev environment (ELBE + QEMU)
│   ├── elbe-ui/                 # Web UI for managing builds
│   └── elbe-demo-apt-repository/# Local flat APT repository tooling
├── sources/
│   └── elbe-demo-app-hello/     # Sample C application source
├── packages/
│   └── elbe-demo-pkg-hello/     # Debian packaging for the hello app
└── projects/
    └── elbe-demo-projects/      # ELBE XML image definitions
```

## Repository index

| Repository | Path in workspace | Description |
|---|---|---|
| [elbe-devcontainers](https://github.com/devfilipe/elbe-devcontainers) | `tools/elbe-devcontainers` | Docker environment with ELBE, QEMU, and Debian packaging tools |
| [elbe-ui](https://github.com/devfilipe/elbe-ui) | `tools/elbe-ui` | Web UI for managing the ELBE initvm and build pipeline |
| [elbe-demo-apt-repository](https://github.com/devfilipe/elbe-demo-apt-repository) | `tools/elbe-demo-apt-repository` | Local flat APT repository for custom packages |
| [elbe-demo-app-hello](https://github.com/devfilipe/elbe-demo-app-hello) | `sources/elbe-demo-app-hello` | Sample C application source code |
| [elbe-demo-pkg-hello](https://github.com/devfilipe/elbe-demo-pkg-hello) | `packages/elbe-demo-pkg-hello` | Debian package definition for the hello application |
| [elbe-demo-projects](https://github.com/devfilipe/elbe-demo-projects) | `projects/elbe-demo-projects` | ELBE XML files describing the target images to build |

## Next steps

Once the workspace is ready, follow the instructions in
[`tools/elbe-devcontainers`](https://github.com/devfilipe/elbe-devcontainers)
to start the development container and build your first image.

## License

See the [LICENSE](LICENSE) file.
