# Molpylot releases

Molpylot is a molecular-modeling suite in which a scientist and a language model work on the same live session,
through the same tools: structure preparation, ligand, modified-residue and metal-site parameterization, molecular
dynamics, free-energy calculations, docking and trajectory analysis. It is a desktop application, a command line and
an MCP server for AI agents.

Download the package for your system from the [latest release](https://github.com/Molpylot-Lab/releases/releases/latest):

| File | For |
|---|---|
| `Molpylot-<version>-windows-x64.zip` | Windows 10 and 11 |
| `Molpylot-<version>-linux-x86_64.tar.gz` | Linux, servers and clusters |
| `Molpylot-<version>-macos-arm64.tar.gz` | macOS 13.3 or newer, Apple Silicon |

**Versions and updates.** Only a new MAJOR version (`X.0.0`), which changes the libraries or is not compatible with
the previous one, has packages here. Every other version is a signed code update, published in
[Molpylot-Lab/updates](https://github.com/Molpylot-Lab/updates): an installed package offers it at start-up and
applies it when you accept.

**The paper.** S. Motta, A. Ranaudo, L. Callea, G. Santini. Molpylot: a shared workspace for human–AI collaboration
in molecular simulation. *bioRxiv* (2026), doi:[10.64898/2026.11.24.768932](https://doi.org/10.64898/2026.11.24.768932).
If you use Molpylot in your work, please cite it ([CITATION.cff](CITATION.cff)).

**License.** Molpylot is free for noncommercial use under the PolyForm Strict License 1.0.0, with additional
permissions ([LICENSE](LICENSE)). Commercial use needs a separate written agreement with the University of
Milano-Bicocca ([COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md)).

**Problems and questions.** Open an [issue](https://github.com/Molpylot-Lab/releases/issues), with the problem report
of *Help → Problem report…* attached.

The text below is the `README.md` of the package.

---

# Molpylot 1.0.0

Molecular-modeling suite for structure preparation, ligand, modified-residue and metal-site parameterization, molecular
dynamics, free-energy calculations, docking and trajectory analysis. It is a desktop application (`molpylot-gui`),
a command line (`molpylot`) and an MCP server for LLM clients (`molpylot-mcp`).

This folder is the **standalone package**. It holds its own Python 3.11 and every library it needs, among them OpenMM,
with CUDA 12 on Linux and Windows. It needs no conda, no Python and no administrator rights. Molpylot never modifies
this folder: extract it anywhere. Your settings, the code updates and the optional components live in Molpylot's
own folders, shared by every version you install.

## Install

**Windows 10 or 11.** The package holds about 50,000 small files: Explorer's *Extract All…* can take 20 minutes or
more, the `tar` of Windows a few. Extract it from PowerShell into a short folder outside OneDrive (which would sync
every file):

```powershell
mkdir C:\Molpylot
tar -xf "$env:USERPROFILE\Downloads\Molpylot-1.0.0-windows-x64.zip" -C C:\Molpylot
```

Then double-click **`Molpylot.exe`** in `C:\Molpylot\Molpylot-1.0.0`. If SmartScreen asks, choose *More info → Run
anyway*. To add Molpylot to the Start menu and its commands to the terminal, run this from PowerShell:

```powershell
& "C:\Molpylot\Molpylot-1.0.0\Molpylot.exe" --install-launcher
```

On a PC with *Smart App Control* turned on (Windows Security → App & browser control), Windows blocks the package.

**Linux** (x86-64, glibc 2.28 or newer: RHEL 8, Ubuntu 20.04 and newer). `--install-launcher` adds the menu entry,
and puts `molpylot`, `molpylot-gui` and `molpylot-mcp` in `~/.local/bin`:

```bash
tar -xzf Molpylot-1.0.0-linux-x86_64.tar.gz
Molpylot-1.0.0/bin/molpylot-gui --install-launcher
```

**macOS 13.3 or newer, Apple Silicon.** Extract the archive in the Terminal, then start Molpylot once from there:

```bash
tar -xzf Molpylot-1.0.0-macos-arm64.tar.gz
Molpylot-1.0.0/bin/molpylot-gui
```

Move the package's folder where it will stay before this first start. Once the window is open, Molpylot offers to add
itself to your Applications (*Add to Applications*): it writes **`~/Applications/Molpylot.app`**, with Molpylot's icon
(Launchpad, Spotlight, the Dock), and puts `molpylot`, `molpylot-gui` and `molpylot-mcp` in `~/.local/bin`; from
then on you start it like any other app. The app starts the package from where it was. *Not now* asks again at the
next start, *Don't ask again* never. The same from the Terminal, at any time:

```bash
Molpylot-1.0.0/bin/molpylot-gui --install-launcher
```

macOS quarantines a file downloaded with a web browser, and Molpylot is not notarized by Apple. A quarantined package
would make macOS ask a permission in System Settings for each of its libraries. So the first start asks once to lift
the quarantine from the package's folder, or you can lift it yourself:
`xattr -dr com.apple.quarantine Molpylot-1.0.0`. An archive downloaded in the Terminal is never quarantined:

```bash
curl -L https://github.com/Molpylot-Lab/releases/releases/download/v1.0.0/Molpylot-1.0.0-macos-arm64.tar.gz | tar -xz
```

A Mac has no CUDA: OpenMM uses OpenCL or the CPU.

**Server or cluster** (no display). Use the Linux package; it carries CUDA 12, so the nodes need only the NVIDIA
driver:

```bash
tar -xzf Molpylot-1.0.0-linux-x86_64.tar.gz
export PATH="$PWD/Molpylot-1.0.0/bin:$PATH"
molpylot --show-license          # read the license, then accept it once:
molpylot --accept-license
```

The first start of the window asks you to accept the license. `--uninstall-launcher`, then deleting the folder,
uninstalls a version.

## Then, inside Molpylot

- **Help → Components…** installs the optional components: AmberTools, OpenFE and the GPU quantum chemistry.
  `molpylot components` does the same from the command line. On Windows, AmberTools and OpenFE go inside WSL,
  Windows' own Linux (`wsl --install -d Ubuntu-24.04` in PowerShell, then restart Windows). The Windows package has
  neither docking (AutoDock Vina) nor the GPU quantum chemistry: the Linux package docks and adds the GPU quantum
  chemistry from its own *Help → Components…*, and it runs inside WSL too.
- **Help → User manual** (F1) opens the manual, with the tutorials and the reference of every tool.
- **MCP server.** Right-click *MCP: off* in the status bar → *Start MCP server (local HTTP)*. In the same menu,
  *Client configuration…* gives the token, and *Copy command* copies the Claude Code command. Other clients are in
  the manual, under *The MCP server → Connecting a client*.
- **Updates.** At start-up Molpylot offers each release's signed code update: *Update now* or *Skip*. Until it is
  installed, the sky-blue *Update available* notice at the right end of the menu bar (of the status bar on macOS)
  installs it at a click. To keep a version - a study that must not change it - tick *Stay on this version* in *Edit
  → Preferences… → Network*. *Help → Check for updates…* looks at once; without a display (a cluster) `molpylot
  update` applies it from the terminal, `molpylot update --check` only looks. When a release changes the libraries,
  download its new package.

## Documentation

The documentation of this package is its user manual: **Help → User manual** (F1). Beside the tutorials and the
reference of every tool, these chapters cover the installation:

| Chapter of the manual | |
|---|---|
| *Welcome to Molpylot → Installing and starting* | the package and the other installations, how to start on each system |
| *Settings, components and updates → Updates and versions* | how updates are checked and applied, the license without a display, several versions side by side |
| *Settings, components and updates → Where settings are kept* | the settings folder, the caches, the environments of the components |
| *Settings, components and updates → Problem report* | what **Help → Problem report…** collects, to attach to a request for help |
| *Jobs, clusters and the command line → Running on a cluster* | what must be installed there, step by step |
| *Databases, AI agents and scripting → The MCP server* | the window's server and `molpylot-mcp`, who may connect, clients |

The releases and their packages are at <https://github.com/Molpylot-Lab/releases/releases>. To report a problem, open
an issue at <https://github.com/Molpylot-Lab/releases/issues> or write an e-mail, with the problem report attached.

## License

Molpylot is free for noncommercial use under the PolyForm Strict License 1.0.0, with additional permissions
(`LICENSE.txt` in this folder). Commercial use needs a separate written agreement (`COMMERCIAL-LICENSE.md`). The
open-source libraries in this package, with their licences, are listed in `THIRD_PARTY_NOTICES.md` and
`THIRD_PARTY_LICENSES.txt` (the texts are in `THIRD_PARTY_LICENSES/`). Required Notice: Copyright (c) 2026 Università
degli Studi di Milano-Bicocca. Created and developed by Dr. Stefano Motta.
