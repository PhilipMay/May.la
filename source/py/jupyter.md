# Jupyter & JupyterLab
- `nb_conda_kernels`: <https://github.com/Anaconda-Platform/nb_conda_kernels>

## Install JupyterLab
- `conda install jupyterlab nb_conda_kernels`

## View Jupyter Notebook online
This can be done here: <https://nbviewer.jupyter.org/>

## Add Conda Environment to Jupyter Lab
`python -m ipykernel install --user --name <conda_env_name>
--display-name "<conda_env_name>"`

## Environment settings for Jupyter
Environment settings for Jupyter are not read from `.bashrc`. You have to
specify them in a `.py` file at `~/.ipython/profile_default/startup/`

For example:
```bash
echo -e "import os\n\nos.environ[\"SOME_URL\"] = \"http://mlflow.company.tld:5000\"" > ~/.ipython/profile_default/startup/set_env.py
```
