### 查看所有pod
>sudo kubectl get pod -n autotest -o wide  

### 查看pod详细情况
>sudo kubectl describe pod kiwitcms-84cd884b85-n6wvm -n support

### 启动
>sudo kubectl apply -f testsupport.yml

### 删除
>sudo kubectl delete pod testsupport-xxx-yyy -n support

### 查看log
>sudo kubectl logs -f --tail=10 selenium-node-c7zcq [container] -n autotest  
>sudo kubectl logs --since=9h selenium-hub-76db798c6b-w4pkb -n autotest > /tmp/dennis/selenium-hub.log

### 进入容器
>sudo kubectl exec -it kiwitcms-xxx-yyy [--container aaa] bash -n support

### 重启yml
>sudo kubectl replace --force -f youpod.yaml

### 如果你手头没有 yaml 文件，直接使用的 Pod 对象，我们可以稍微调整一下上面的命令，如下：
>sudo kubectl get pod {podname} -n {namespace} -o yaml | kubectl replace --force -f -

### 单文件映射（使用subPath,不会使文件夹内的其他文件消失）
```yaml
- name: kiwitcms-bugs-custom
    mountPath: /venv/lib64/python3.8/site-packages/tcms/bugs/api.py
    subPath: api.py
```
