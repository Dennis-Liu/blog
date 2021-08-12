### 重启jenkins
>service jenkins restart

或 

https:xxx.jenkins.com/restart

### groovy脚本批量创建用户
'''groovy
def userArr = ["YiHongxia":"YiHongxia@test.com"
,"LinShangru":"LinShangru@test.com"
,"QiuZiwen":"QiuZiwen@test.com"];
for(i in userArr){
	def user = jenkins.model.Jenkins.instance.securityRealm.createAccount(i.key, "abcd@1234");
    user.addProperty(new hudson.tasks.Mailer.UserProperty(i.value)); 
}
'''
