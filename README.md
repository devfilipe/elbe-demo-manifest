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

### SSH alias (required)

The manifest fetches all repositories via SSH using the host alias
`github.com-devfilipe`. This alias must be configured in `~/.ssh/config`
with the exact name shown below. Set `IdentityFile` to the SSH private key
you normally use with GitHub (e.g. `~/.ssh/id_ed25519` or `~/.ssh/id_rsa`):

```
Host github.com-devfilipe
    HostName github.com
    User git
    IdentityFile ~/.ssh/<your-github-key>
```

If you have not set up an SSH key for GitHub yet, follow the
[GitHub SSH key guide](https://docs.github.com/en/authentication/connecting-to-your-github-account/adding-a-new-ssh-key-to-your-github-account)
first.

Verify the alias resolves correctly before running `repo sync`:

```bash
ssh -T git@github.com-devfilipe
# Expected: Hi <your-username>! You've successfully authenticated...
```

## Quick start

```bash
TOPLEVEL=/path/to/your/workspace
mkdir -p "$TOPLEVEL" && cd "$TOPLEVEL"
repo init -u git@github.com-devfilipe:devfilipe/elbe-demo-manifest.git
repo sync
repo start main --all   # create local tracking branches on all projects
```

After `repo sync` the workspace will be populated as follows:

```
$TOPLEVEL/
├── tools/
│   ├── elbe-devcontainers/           # Docker dev environment (ELBE + QEMU)
│   ├── elbe-ui/                      # Web UI for managing builds
│   ├── elbe-demo-apt-repository/     # Local APT repo — pool layout (SBOM-compatible)
│   └── elbe-demo-apt-repository-flat/# Local APT repo — flat layout
├── sources/
│   └── elbe-demo-app-hello/          # Sample C application source
├── packages/
│   └── elbe-demo-pkg-hello/          # Debian packaging for the hello app
└── projects/
    └── elbe-demo-projects/           # ELBE XML image definitions
```

## Repository index

| Repository | Path in workspace | Description |
|---|---|---|
| [elbe-devcontainers](https://github.com/devfilipe/elbe-devcontainers) | `tools/elbe-devcontainers` | Docker environment with ELBE, QEMU, and Debian packaging tools |
| [elbe-ui](https://github.com/devfilipe/elbe-ui) | `tools/elbe-ui` | Web UI for managing the ELBE initvm and build pipeline |
| [elbe-demo-apt-repository](https://github.com/devfilipe/elbe-demo-apt-repository) | `tools/elbe-demo-apt-repository` | Local APT repository — pool layout, required for SBOM generation |
| [elbe-demo-apt-repository-flat](https://github.com/devfilipe/elbe-demo-apt-repository-flat) | `tools/elbe-demo-apt-repository-flat` | Local APT repository — flat layout, simpler setup without SBOM support |
| [elbe-demo-app-hello](https://github.com/devfilipe/elbe-demo-app-hello) | `sources/elbe-demo-app-hello` | Sample C application source code |
| [elbe-demo-pkg-hello](https://github.com/devfilipe/elbe-demo-pkg-hello) | `packages/elbe-demo-pkg-hello` | Debian package definition for the hello application |
| [elbe-demo-projects](https://github.com/devfilipe/elbe-demo-projects) | `projects/elbe-demo-projects` | ELBE XML files describing the target images to build |

## Next steps

Once the workspace is ready, follow the instructions in
[`tools/elbe-devcontainers`](https://github.com/devfilipe/elbe-devcontainers)
to start the development container and build your first image.

## License

See the [LICENSE](LICENSE) file.
