### 查看pod
>sudo kubectl get pod -n autotest -o wide  

### 启动
>sudo kubectl apply -f testsupport.yml

### 删除
>sudo kubectl delete pod testsupport-xxx-yyy -n support

### 查看log
>sudo kubectl logs -f --tail=10 selenium-node-c7zcq [container] -n autotest

### 进入容器
>sudo kubectl exec -it kiwitcms-xxx-yyy [--container aaa] bash -n support

### 重启yml
>kubectl replace --force -f youpod.yaml

### 如果你手头没有 yaml 文件，直接使用的 Pod 对象，我们可以稍微调整一下上面的命令，如下：
>kubectl get pod {podname} -n {namespace} -o yaml | kubectl replace --force -f -
