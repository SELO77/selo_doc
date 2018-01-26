Date: January 15th, 2018
Tags: python, pyenv, virtualenv
Permalink: pyenv-virtualenv-install-and-venv-setup


# pyenv-vritualenv install and venv setup

##### clone pyenv-virtualenv
```
git clone https://github.com/pyenv/pyenv-virtualenv
```

```
$ brew install pyenv-virtualenv
```

##### add following script to your shell profile
```
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

##### check the versions installed
```
$ pyenv versions
* system (set by /Users/selochanlee/.pyenv/version)
```

##### install specific version
```
# check the installable version list
$ pyenv install --list
...
# install specific version
$ pyenv install 3.6.1
...
# confirm it whether installed or not
$ pyenv versions
...
```

##### create virtualenv with pyen and activate it
```
$ pyenv virtualenv 2.7.13 <venv_name>
...
# activate the installed version
$ pyenv activate <venv_name>
...
```


