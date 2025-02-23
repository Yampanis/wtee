Wtee
====

The wtee tool does two things:

- Duplicates standard input to standard output.
- Starts a local http server on which the piped data can be viewed.

In other words, much like the unix ``tee`` utility, *wtee* duplicates
standard input to standard output, while also making the piped data
viewable on a web page. For example::

  tail -f /var/log/debug | wtee | nl

Documentation:
    http://wtee.readthedocs.io/en/latest/

Development:
    https://github.com/gvalkov/wtee

Package:
    http://pypi.python.org/pypi/wtee

Changelog:
    http://wtee.readthedocs.io/en/latest/changelog.html

How to Build
====

##### First, we need to install pyenv for setting up old version of Python.

https://github.com/pyenv/pyenv?tab=readme-ov-file#how-it-works

In bash,

```
curl https://pyenv.run | bash
```

First, add the commands to ~/.bashrc by running the following in your terminal:
```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
```

Then, if you have ~/.profile, ~/.bash_profile or ~/.bash_login, add the commands there as well. If you have none of these, add them to ~/.profile.

- to add to ~/.profile:
```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.profile
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.profile
echo 'eval "$(pyenv init -)"' >> ~/.profile
```
- to add to ~/.bash_profile:
```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
```
###### Restart your shell

for the PATH changes to take effect.
```
exec "$SHELL"
```

#### Install Python build dependencies

###### Install python 3.5 using pyenv

```
pyenv install 3.6.5
```
if you got trouble in installing python 3.6.5, please refer this article.

https://stackoverflow.com/questions/3905615/zlib-module-missing

```
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libreadline-dev libbz2-dev libsqlite3-dev wget curl llvm libncurses5-dev
```


```
pyenv versions
```
There should be 3.6.5 in python.

###### Set python version to 3.6.5

```
pyenv global 3.6.5
```

###### install dependencies for the wtee project (only if compiling from source, not needed to run executable)

Go to the root directory of source project.
```
git clone https://github.com/yampanis/wtee.git
cd ./wtee
```

Create a virtual environment ()

```
python -m venv env
```

Activate the Virtual Environment

```
source env/bin/activate
```
Install wtee from source

```
pip install -e .
```

Test wtee

```
tail -f ./screenlog.1 | wtee | nl
```
You can see that a server is running on port 8080 by default.
Visit http://localhost:8080 to check if server is running.


Build a standalone executable

```
pex . -r requirements.txt -e wtee.main:main -o wtee.pex
```

```
###### run wtee executalble
```
tail -f ./screenlog.1 | ./wtee.pex -b localhost:8000 | nl
```
