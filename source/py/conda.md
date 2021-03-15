# Conda

## Manage Environments
  - create environment: `conda create --name <new_env_name>`
  - create environment with python 3.7: `conda create --name <new_env_name> python=3.7`
  - activate environment: `conda activate <env_name>`
  - deactivate (leave) environment: `conda deactivate`
  - list available environments: `conda info --envs`
  - remove environment: `conda remove --name <env_name> --all`

## Updates
  - update conda: `conda update -n base -c defaults conda`
  - update all packages in current environment: `conda update --all`
  - also see: <https://docs.conda.io/projects/conda/en/latest/commands/update.html>

## Remove unused cached Packages
``` bash
conda clean -a
```

also see:
<https://docs.conda.io/projects/conda/en/latest/commands/clean.html>

## conda-forge Pakete erstellen
  - conda-forge: <https://conda-forge.org/>
  - conda install conda-verify vor dem build:
    <https://github.com/conda/conda-verify>
  - Beispiele:
      - <https://github.com/conda-forge/keras-feedstock>
      - <https://github.com/conda-forge/numpy-feedstock>

## Where are the conda environments stored?
  - Mac: `/usr/local/miniconda3/envs`

## Weitere Conda-Channel konfigurieren
Wenn eine Python Bibliothek nicht im Standardverzeichnis von Conda
gepflegt wird, kann es notwendig sein weitere Channel für die
Installation zu konfigurieren.

Hierzu muss in der Conda-Bash der folgende Befehl ausgeführt werden:
`conda config --append channels <Channelname>`

Den passenden Channel findet man unter <https://anaconda.org>

Beispiel für die Bibliothek geopPy:
<https://anaconda.org/search?q=geopy>

## Conda Installation and Setup

### Disable automatic base activation:
``` bash
conda config --set auto_activate_base false
```

also see: <https://stackoverflow.com/a/54560785/271118>

### Conda installation on Linux
  - download Conda (Python 3.7, Linux, 64bit):
    <https://conda.io/miniconda.html>
  - make install file executable: `chmod +x
    Downloads/Miniconda3-latest-Linux-x86_64.sh`
  - start installation: `./Downloads/Miniconda3-latest-Linux-x86_64.sh`
  - use default settings
  - log out and back in to activate new settings

### Conda Installation auf Windows
  - Download Mini Conda (nicht Anaconda):
    <https://conda.io/en/latest/miniconda.html>
  - Für Python 3.6 und nicht 2.5 und als 64 bit Version (nicht 32 bit)
  - Proxy installieren
      - in der `.condarc` Datei folgendes einfühen (und darauf achten,
        dass der Key `ssl_verify` nicht doppelt ist), diese Datei liegt
        unter `C:\Users\<User>`

```
    proxy_servers:
      http: http://user:pass@corp.com:8080
      https: https://user:pass@corp.com:8080

    ssl_verify: False
```
