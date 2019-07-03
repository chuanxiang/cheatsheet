### Install pyenv/pyenv-virtualenv
```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
brew install pyenv-virtualenv
```

### Shell Setting
```bash
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"

if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
  eval "$(pyenv virtualenv-init -)"
fi
```

### Create/Delete virtual env
```bash
pyenv virtualenv env_name
pyenv uninstall env_name

# Below commands DOES NOT work with pyenv
conda create --name env_name
conda create --name myclone --clone myenv
conda create --name env_name --file requirement.txt
conda env create -f environment.yml
conda remove --name env_name --all
```

### Activate/Deactivate env
```bash
conda activate env_name
conda deactivate

# Below commands DOES NOT work with pyenv
pyenv activate env_name
source activte env_name
```

### List of virtual env
```bash
conda env list
```

### Export conda source pakage
```bash
conda env export > environment.yml
conda list --export > conda_requirement.txt
```

### Install source packages from file
```bash
conda install --name env_name -f environment.txt
conda install --file requirement.txt
```

### Other commands
```bash
conda info --envs
conda list
conda list --export
```
