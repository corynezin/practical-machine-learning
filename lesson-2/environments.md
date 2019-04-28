# Environment Management

When developing by yourself, environment management is an afterthought.  With regard to packages like TensorFlow, you can always install, uninstall, upgrade, downgrade, or restart depending on your own personal needs.  A team needs a more stable foundation on which to build its software.  Here we will discuss some basic management packages that are designed to make our lives easier.

## Python Virtual Environments

### `pip`

`pip` is a Python module can be used to install Python packages.  This provides a very easy way of using high quality open source code in your project.  

#### Example Usage

`numpy` is a performant numerical computing package for Python.  Let's install it using `pip`.

Running
```console
python -m pip install numpy
```
Yields
```console
Collecting numpy
  Downloading https://files.pythonhosted.org/packages/c1/e2/4db8df8f6cddc98e7d7c537245ef2f4e41a1ed17bf0c3177ab3cc6beac7f/numpy-1.16.3-cp36-cp36m-manylinux1_x86_64.whl (17.3MB)
     |████████████████████████████████| 17.3MB 3.7MB/s 
Installing collected packages: numpy
Successfully installed numpy-1.16.3
```
For you, it may yield something like
```console
Requirement already satisfied: numpy in ./pyenv/lib/python3.6/site-packages (1.16.3)
```
This would mean that the package `numpy` is already installed.

Running

```console
python -c 'import numpy; print(numpy.__version__)'
```
Yields
```console
1.16.3
```
For you, it may yield something like
```console
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'numpy'
```
This would mean that the package `numpy` is not installed.

### `virtualenv`

Note that the previous example had a few different possible outcomes depending on what is already installed, or not installed on your computer.  Ideally, we would like our software to work on more than just one fixed system.  We would like it if we could obtain a constant environment that we can share with our team.  This is a problem which the tool `virtualenv` helps to solve during development.

`virtualenv` allows us to create a mostly isolated Python environment where we can put all of our packages.  These packages are accessible only while that environment is "activated."  You can have as many environments on a machine as you want, and it is simple to automatically build them.

#### Example Usage
Running

`python -m virtualenv venv`

Yields
```console
Using base prefix '/usr'
New python executable in /home/nezin/Documents/practical-machine-learning/playground/venv/bin/python3.6
Also creating executable in /home/nezin/Documents/practical-machine-learning/playground/venv/bin/python
Installing setuptools, pip, wheel...
done.
```

<details><summary>Details</summary>
<p>

`virtualenv` first creates a new Python executable in the folder specified, `venv`.  By default, the version of Python, in this case 3.6, is the same as the version used in the creation of the virtual environment.  `virtualenv` then installs some basic packages necessary for installing more packages: `setuptools`, `pip`, and `wheel`.  That means we will be able to use our beloved `pip` inside the virtual environment itself.

</p>
</details>

Running

`source venv/bin/activate`

Yields nothing.  However, you should see `(venv)` appear at the start of your command prompt.  This means that the virtual environment has been activated

Running

`which python`

Yields something like

`/home/nezin/Documents/practical-machine-learning/playground/venv/bin/python`

instead of the typical

`/usr/bin/python`

This means that our Python has been replaced by a new isolated Python which we are now free to extend with all the packages we want.  You can repeat the `pip` example now, and you should obtain the same results.  In order to leave the virtual environment, you can run `deactivate`.

### Install Scripts

Now, these tools are very useful, but we still haven't solved the problem of sharing our environment with others.  If we're on the same machine, we could potentially share the virtual environment folder, but that will leave it open to tampering and if we ever want to make a change, there is no history and source control is not possible.  Instead, we can write some simple scripts to automate the building of these environments.

#### Example

```python
#requirements.txt
numpy
requests
flask
```

```bash
#!/usr/bin/bash
#install.sh
python -m pip install -r requirements.txt
```

This is quite a simple script, though it can grow more complicated as installation becomes more complicated by less standard dependencies, like large files.

## Containerization

Now you may think that all of that is a very thorough system for managing your software, but in truth it is still not enough.  Suppose for example, during development you specify an environment variable

`export CUDA_VISIBLE_DEVICES=2`

But then your buddy comes along, didn't set the same variable, and takes every single GPU, annoying everyone.

This is a very simple example, and certainly no sophisticated is needed to solve it, however there are many dependencies which cannot be installed simply using `pip` for which we do need a more sophisticated solution.

### `docker`

Docker is an open source software that allows users to create fully contained software applications while maintaining high performance.  A "container" is created by building a "Dockerfile" which fully specifies the environment that the software will run on.

#### Example Usage

```Dockerfile
#Dockerfile
FROM ubuntu:16.04
ENTRYPOINT ["echo"]
CMD ["Hello world!"]
```
Running 

`docker build .`

Yields something like

```console
Sending build context to Docker daemon    120MB
Step 1/3 : from ubuntu:16.04
 ---> a3551444fc85
Step 2/3 : entrypoint ["echo"]
 ---> Using cache
 ---> f11a57c24488
Step 3/3 : cmd ["Hello world!"]
 ---> Using cache
 ---> e43192ce46e9
Successfully built e43192ce46e9
```

Running

`Docker run e43192ce46e9`

Yields

`Hello world!`

## Services

### `flask`

### `tensorflow-serving`
