## 更新pip报错 importerror no module named typing

原因是pip21和python2.7不配套，报错里面的typing库是python3.5才出现的，python2.7版本没有这个库。

使用get-pip.py来实现回退版本
```shell
curl -O https://bootstrap.pypa.io/pip/2.7/get-pip.py
python get-pip.py
#python -m pip install --upgrade "pip < 21.0"
```
