# Data Science Environment (Arch ↔ macOS)

This repo contains a reproducible Python data-science environment that you can switch between **Arch (Omarchy)** and **macOS (MacBook)** using **micromamba** + `environment.yml`.

> TL;DR
>
> ```bash
> micromamba create -f environment.yml -y
> micromamba activate ds
> python -m ipykernel install --user --name ds --display-name "Python (ds)"
> jupyter lab
> ```

---

## 1) Install micromamba

### Arch (bash)
```bash
cd ~
mkdir -p ~/.local/bin
curl -L https://micro.mamba.pm/api/micromamba/linux-64/latest | tar -xvj bin/micromamba
mv bin/micromamba ~/.local/bin/
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(micromamba shell hook -s bash)"' >> ~/.bashrc
exec bash -l
```

### macOS (Apple Silicon, zsh)
```bash
cd ~
mkdir -p ~/.local/bin
curl -L https://micro.mamba.pm/api/micromamba/osx-arm64/latest | tar -xvj bin/micromamba
mv bin/micromamba ~/.local/bin/
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(micromamba shell hook -s zsh)"' >> ~/.zshrc
exec zsh -l
```
> Intel Mac? Replace the URL with `osx-64`.

---

## 2) Create the environment

Assuming you have `environment.yml` at the repo root:

```bash
micromamba create -f environment.yml -y
micromamba activate ds
```

Register the kernel for Jupyter:
```bash
python -m ipykernel install --user --name ds --display-name "Python (ds)"
```

---

## 3) Launch Jupyter

```bash
jupyter lab
# or
jupyter notebook
```

Select kernel **Python (ds)** in the notebook.

---

## 4) Update dependencies

Edit `environment.yml` then run:
```bash
micromamba activate ds
micromamba install -f environment.yml -y  # applies changes
# or recreate fresh:
# micromamba deactivate
# micromamba env remove -n ds
# micromamba create -f environment.yml -y
```

Export a lock file (optional, for perfect reproducibility):
```bash
micromamba env export -n ds > environment.lock.yml
```

---

## 5) Switching machines (Arch ↔ macOS)

On each machine:
```bash
git pull
micromamba create -f environment.yml -y   # first time or after env changes
micromamba activate ds
jupyter lab
```

Don’t commit any local env directories (`.conda`, `.micromamba`, etc.). `.gitignore` already handles this.

---

## 6) Troubleshooting

- **`jupyter: command not found`**  
  Ensure micromamba shell hook is active and the env is activated:
  ```bash
  exec $SHELL -l   # reload shell (bash or zsh)
  micromamba activate ds
  ```
  Or run without activation:
  ```bash
  micromamba run -n ds jupyter lab
  ```

- **Kernel not visible in Jupyter**  
  You likely skipped kernel registration:
  ```bash
  python -m ipykernel install --user --name ds --display-name "Python (ds)"
  ```

- **Permission / PATH issues**  
  Verify:
  ```bash
  command -v micromamba
  echo $PATH
  ```

---

## 7) Extras

- Add packages by editing `environment.yml` (e.g. `plotly`, `bokeh`, `pytorch`).
- Keep data out of git (`data/`, `outputs/`); see `.gitignore` for examples.
- Consider using `jupyter lab --NotebookApp.notebook_dir=./notebooks` to open in a specific folder.

---

Happy hacking! 🚀
