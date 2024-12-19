# Kalman Filters Book

[*Kalman and Bayesian Filters in Python*](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python) is a pretty good way to learn Kalman filters.

```bash
git clone https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python
```

Then, you can install the "Jupyter" extension in VSCode to read the book, or you can install Jupyter Notebook or Jupyter Lab and read it there.

## Installing dependencies

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
mamba activate KalmanFiltersBook
```

If you modify `environment.yml`, please run

```bash
mamba env update -f environment.yml
```

Occassionally, you should update all the packages to the latest versions:

```bash
mamba activate KalmanFiltersBook
mamba update --all
```

## Getting sliders working

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
