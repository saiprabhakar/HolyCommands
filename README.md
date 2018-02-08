# python_custom_hooks
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
