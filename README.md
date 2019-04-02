## python_custom_hooks
Include in all python projects  
Inspired from https://dmerej.info/blog/post/how-i-lint/#fn:1 

1. pip install pylint, invoke
2. create tasks.py with the below mentioned content
2. optional: edit pylint according to https://dmerej.info/blog/post/some-pylint-tips/
3. pip install pycodestyle
4. add below lines in setup.cfg (include virtual environment directories)
```
[pycodestyle]
exclude=<some directory>
```
5. vim checkcode.sh
```
#!/bin/bash -x

pycodestyle .
invoke pylint
```
6. chmod 777 checkcode.sh
7. usage 
```./checkcode.sh```

optional to launch seperately:
- pylint: ```invoke pylint```
- pycodestyle: ```pycodestyle .```

tasks.py:
```
import os
import invoke


def get_pylint_args():
    for file in os.listdir("."):
        if file.endswith(".py") or os.path.exists(file + '/__init__.py'):
            yield file


@invoke.task
def pylint(ctx):
    del ctx
    invoke.run("pylint " + " ".join(get_pylint_args()), echo=True)
```


Jupyter extension:
- pip install jupyter_contrib_nbextensions
- pip install jupyter_nbextensions_configurator
- (needed?) jupyter nbextensions_configurator enable --user
- jupyter-contrib-nbextension install --system

## Docker commands
```
Docker ps
Docker images
Docker run -it <> bash # (-p 8888:8888)(-d)
docker exec -it upbeat_panini /bin/bash #  (go into a running docker container)
docker commit -p cocky_goldberg tf_sai_test # (commiting changes)
docker rm $(docker ps -aq) # (remove all containers but not images and not the running ones)
docker run -it -p 0.0.0.0:6006:6006 -p 8888:8888 <TF docker image> # (docker with jupyter and tensorboard enabled)
Docker rmi <image name># (remove an image)
docker rmi $(docker images -f "dangling=true" -q)# (remove none dockers)
docker-compose -f <>.yml build --parallel
docker-compose -f <>.yml up
docker-compose -f <>.yml up -d --no-deps --build <service_name> # update one container defined as servicename fast
docker stats -a --format   'CPU: {{.CPUPerc}}\tMEM: {{.MemPerc}}' `docker ps -q`  > log_mem_AE # log memory
cat log_mem_AE | sed 's/\x1b\[[0-9;]*[a-zA-Z]//g' | cut -d" " -f3 | cut -d "%" -f1 | sort -nr | head -n1 # find peak memory
```
## Docker tensorflow on Bash on windows (you are unfortunate)

- Enable windows subsystem for linux
- Install docker on windows
- Launch it and enable hyper-V and virtualization if and when it asks for
- Follow https://blog.jayway.com/2017/04/19/running-docker-on-bash-on-windows/ to enable
- Done

## AWS guide:
AWS mounting EBS: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html 

## Minikube:
Install as per: https://kubernetes.io/docs/tasks/tools/install-minikube/

## Virtual env 
```apt-get update
apt-get install python-pip git wget curl lsb_release vim
pip install --upgrade pip
pip install virtualenv

sudo pip install virtualenv
virtualenv -p python3.5 venv1
. venv1/bin/activate
pip --no-cache-dir install -r req.txt
Deactivate
pip freeze -r requirements.txt
```

## Jupyter:
```
pip3 install jupyter
pip install jupyter
jupyter-notebook --ip=0.0.0.0 --allow-root
sudo pip install jupyter_contrib_nbextensions jupyter_nbextensions_configurator # system wide installation
jupyter nbextensions_configurator enable --user
sudo jupyter-contrib-nbextension install --system
```
- Todo: Running secure Jupyternotebook: http://jupyter-notebook.readthedocs.io/en/stable/public_server.html
- Jupyter themes:
```
# activate virtualenv then execute the below commands
pip install --upgrade jupyterthemes
jt -t onedork -fs 95 -tfs 11 -nfs 115 -cellw 88% -T -N
```
