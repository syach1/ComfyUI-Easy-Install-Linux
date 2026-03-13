<div align="center">

# ComfyUI-Easy-Trellis2-Linux

Linux-focused **ComfyUI** fork for **CachyOS / Arch-style NVIDIA setups**  
[![GitHub Release](https://img.shields.io/github/v/release/syach1/ComfyUI-Easy-Trellis2-Linux)](https://github.com/syach1/ComfyUI-Easy-Trellis2-Linux/releases)
[![GitHub Release Date](https://img.shields.io/github/release-date/syach1/ComfyUI-Easy-Trellis2-Linux?style=flat)](https://github.com/syach1/ComfyUI-Easy-Trellis2-Linux/releases)
[![GitHub All Releases](https://img.shields.io/github/downloads/syach1/ComfyUI-Easy-Trellis2-Linux/total.svg)](https://github.com/syach1/ComfyUI-Easy-Trellis2-Linux/releases)
[![GitHub Downloads Latest](https://img.shields.io/github/downloads/syach1/ComfyUI-Easy-Trellis2-Linux/latest/total?style=flat&label=latest&color=orange)](https://github.com/syach1/ComfyUI-Easy-Trellis2-Linux/releases)

Fork maintained by **syach1**  
Based on the original work from **Tavris1**, **VenimK**, and the **Pixaroma** community.  
[![Dynamic JSON Badge](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fdiscord.com%2Fapi%2Finvites%2FgggpkVgBf3%3Fwith_counts%3Dtrue&query=%24.approximate_member_count&logo=discord&logoColor=white&label=Join%20Pixaroma%20Discord&color=FFDF00&suffix=%20users)](https://discord.com/invite/gggpkVgBf3)

</div>

---

## Fork Status

> [!IMPORTANT]
> This fork is maintained for **Linux only**.
> It was tested on **CachyOS** with an **NVIDIA RTX 4060 Ti 16GB**.
> The stable Trellis2 path in this fork is:
> - **Python 3.12**
> - **Torch 2.7.0 + cu128**
> - **flash-attn** enabled
> - **Trellis2** installed through the patched Linux add-on scripts in this repo
>
> This fork is primarily maintained for **Trellis2 on Linux**.
> Because Trellis2 has specific Torch, CUDA, and native-extension requirements, this install is best treated as a **dedicated Trellis2 environment**.
> Other custom nodes or workflows may still work, but they are **not broadly tested or officially supported** in this fork.

> [!TIP]
> On the tested Linux path, `flash-attn` is installed and validated automatically by:
> - `Add-Ons/Torch-Pack/Torch2.7.0+cu128.sh`
> - `Add-Ons/Trellis2.sh`
>
> You do not need to install it separately for the recommended Trellis2 setup.

> [!NOTE]
> This fork documents and supports the Linux path only.

## 📦 Included Components

<details>
<summary><b>Core Components</b></summary>

| 🔧 Component | 📝 Note |
|---|---|
| [Git](https://git-scm.com/) | ![Git version](https://img.shields.io/github/v/tag/git/git?label=&display_name=tag&color=blue) - Latest (will install/update if needed) |
| [Python](https://www.python.org/downloads/release/python-31210/) | ![Python version](https://img.shields.io/badge/3.12.10-blue) - Built from source |
| [ComfyUI](https://github.com/Comfy-Org/ComfyUI) | ![ComfyUI version](https://img.shields.io/github/v/release/Comfy-Org/ComfyUI?label=&display_name=tag) - Latest version |

</details>

<details>
<summary><b>Optional Add-ons Nodes and Tools</b></summary>

| 🧩 Nodes | 🛠️ Tools |
|---|---|
| [Nunchaku](https://github.com/nunchaku-ai/nunchaku) | Easy-Models-Linker |
| [SageAttention (v2.2.0)](https://github.com/woct0rdho/SageAttention) | ComfyUI-Version-Switcher |
| [FlashAttention](https://github.com/Dao-AILab/flash-attention) | Backup ComfyUI |
| [InsightFace](https://github.com/deepinsight/insightface) | Torch-Pack |
| [Trellis 2.0](https://github.com/visualbruno/ComfyUI-Trellis2) | |

</details>

## 🐧 Linux Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/syach1/ComfyUI-Easy-Trellis2-Linux.git
   ```
2. Run the top-level bootstrap:
   ```bash
   cd ComfyUI-Easy-Trellis2-Linux
   chmod +x ComfyUI-Easy-Install.sh
   ./ComfyUI-Easy-Install.sh
   ```
3. Enter the nested runtime folder and install the stable Torch runtime:
   ```bash
   cd ComfyUI-Easy-Install
   ./Add-Ons/Torch-Pack/Torch2.7.0+cu128.sh
   ```
4. Install Trellis2:
   ```bash
   cd Add-Ons
   ./Trellis2.sh
   cd ..
   ```
5. Start ComfyUI:
   ```bash
   ./run_nvidia_gpu.sh
   ```

What this path does:

- installs the tested `torch 2.7.0 + cu128` runtime
- installs and validates the pinned Linux `flash-attn` wheel used by this fork
- installs Trellis2 with the patched Linux wheel-selection and `o_voxel` fallback logic
- skips optional Linux `nvdiffrec_render` source builds when no compatible wheel is bundled

Expected success signals:

- `pytorch version: 2.7.0+cu128`
- `[SPARSE] Conv backend: flex_gemm; Attention backend: flash_attn`
- `[ATTENTION] Using backend: flash_attn`
- `Import times for custom nodes: ... ComfyUI-Trellis2`

Troubleshooting:

- **Permission Errors**: `chmod -R 755 ComfyUI-Easy-Install`
- **Trellis2 ABI/runtime issues**: use `Add-Ons/Torch-Pack/Torch2.7.0+cu128.sh`, then rerun `Add-Ons/Trellis2.sh`
- **o_voxel import issues**: run `Add-Ons/Trellis2-Build-OVoxel.sh`

Known non-fatal warnings:

- `NumbaWarning: The TBB threading layer ... is disabled`
- `_POSIX_C_SOURCE redefined`
- Open3D optional dependency warnings about `dash`, `flask`, `ipywidgets`, `nbformat`, `pandas`, or `werkzeug`
- `FETCH ComfyRegistry Data: ...`
- browser-side deprecation warnings from older ComfyUI extensions

> [!TIP]
> - Multiple ComfyUI installs are allowed without conflicts.
> - You can rename or move the nested `ComfyUI-Easy-Install` folder after installation.
> - See `ComfyUI-Easy-Install/GUIDE.md` for the full first-time walkthrough.

<details>
<summary><b>Backup ComfyUI</b></summary>

A small interactive utility to back up, restore, and manage your ComfyUI data folders.

- **Script**: `Add-Ons/backup_comfyui.sh`
- **Version**: V2.01.3 (Pixaroma Community Edition, Linux by VenimK)
- **Backup location**: `~/ComfyUI_Backups/ComfyUI_backup_YYYYMMDD_HHMMSS`
- **What gets backed up**: `user`, `input`, `output` (always) · `models` (optional)

```bash
bash Add-Ons/backup_comfyui.sh
```

</details>

---

<div align="center">

## ❤️ Support Me
Enjoy my projects? Any support is greatly appreciated!

</div>
