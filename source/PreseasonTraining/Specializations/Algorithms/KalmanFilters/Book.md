# Kalman Filters Book

[*Kalman and Bayesian Filters in Python*](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python) is a pretty good way to learn Kalman filters. Clone my fork of the book (I'll update the dependencies and fix any issues you might encounter there).

```bash
git clone https://github.com/ethanc8/Kalman-and-Bayesian-Filters-in-Python
```

Then, you can install the "Jupyter" extension in VSCode to read the book, or you can install Jupyter Notebook or Jupyter Lab and read it there.

## Installing dependencies

If you already have Miniforge3 installed, use those instructions. Otherwise you can use the Micromamba instructions. If you're on Windows use the Micromamba instructions.

### Micromamba

Open a Bash terminal. On Linux, this should be your default terminal. On macOS, open Terminal.app and run the `bash` command. On Windows, you can use Git Bash. (You have Git installed, right? If not, go back to [/Resources/JavaProgramming/installation.md#])

```bash
"${SHELL}" <(curl -L micro.mamba.pm/install.sh)
```

Open a new Bash terminal. Now, you can use Micromamba to install the dependencies.

```bash
micromamba env create -f environment.yml
micromamba activate kf_bf
```

If you modify `environment.yml`, please run

```bash
micromamba env update -f environment.yml
```

Occassionally, you might want to update all the packages to the latest versions:

```bash
micromamba update --all -n kf_bf
```

In VS Code, it will ask you which Jupyter kernel you want to use. Choose `kf_bf`.

### Miniforge3 (Linux or macOS)

First, please have Conda installed on your computer. If it's not installed, please install [Miniforge3](https://conda-forge.org/miniforge/), which includes Conda and a conda-forge based Python environment. You can install Miniforge3 using the following command:

```bash
wget "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh
rm Miniforge3-$(uname)-$(uname -m).sh
```

Close and reopen your shell, and run:

```bash
# Prevent Conda from polluting your environment when you're not working on Conda-managed projects.
conda config --set auto_activate_base false
```

Now, you can use Conda to install the dependencies.

```bash
mamba env create -f environment.yml
mamba activate kf_bf
```

If you modify `environment.yml`, please run

```bash
mamba env update -f environment.yml
```

Occassionally, you should update all the packages to the latest versions:

```bash
mamba activate kf_bf
mamba update --all
```

In VS Code, it will ask you which Jupyter kernel you want to use. Choose `kf_bf`.

## Getting sliders working

Only do this if the sliders aren't working; it hasn't been tested for a while.

```bash
pip3 install --user --upgrade ipywidgets
jupyter nbextension enable --py --sys-prefix widgetsnbextension
```

## Low contrast issues

In Light+ theme in VSCode, the text is too low contrast. To fix this, add this to `settings.json` (Ctrl+Shift+P -> "Open User Settings (JSON)"):
```json
    "workbench.colorCustomizations": {
        "[Default Light+]": {
            "foreground": "#111111"
        }
    },
```
