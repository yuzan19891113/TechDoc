# 引擎构建与验证

**引擎修改的一般步骤：**

![](../../.gitbook/assets/image%20%28240%29.png)

**具体步骤和链接如下：**

1.  先提交代码测试分支: [http://tc-svn.tencent.com/qqspeed/nssunity5\_proj/trunk/Unity2019.4\_Test](http://tc-svn.tencent.com/qqspeed/nssunity5_proj/trunk/Unity2019.4_Test)  
2. 使用测试分支构建引擎：[http://devops.oa.com/console/pipeline/speedmobile/p-96d5ee32a21c43eb9f231632cdfd800a/preview](http://devops.oa.com/console/pipeline/speedmobile/p-96d5ee32a21c43eb9f231632cdfd800a/preview)  
3. 使用测试引擎构建手机版本：  
 Android: [http://devops.oa.com/console/pipeline/speedmobile/p-91371783f2d84f36a0ad87d28aab2174/history](http://devops.oa.com/console/pipeline/speedmobile/p-91371783f2d84f36a0ad87d28aab2174/history)  
 iOS: [http://devops.oa.com/console/pipeline/speedmobile/p-7e1290a94d514e4d896528e844ff4f38/history](http://devops.oa.com/console/pipeline/speedmobile/p-7e1290a94d514e4d896528e844ff4f38/history)  
4. 验证完毕，将代码合回主干：[http://tc-svn.tencent.com/qqspeed/nssunity5\_proj/trunk/Unity2019.4.22f1](http://tc-svn.tencent.com/qqspeed/nssunity5_proj/trunk/Unity2019.4.22f1)  
5. 构建主干dev引擎：[http://devops.oa.com/console/pipeline/speedmobile/p-cf7e37dfa397459b80eaaa6598766996/preview](http://devops.oa.com/console/pipeline/speedmobile/p-cf7e37dfa397459b80eaaa6598766996/preview)  
6. 使用dev引擎构建手机版本：  
 Android: [http://devops.oa.com/console/pipeline/speedmobile/p-14b9f856a1b84335b2cc6d40073458bf/history](http://devops.oa.com/console/pipeline/speedmobile/p-14b9f856a1b84335b2cc6d40073458bf/history)  
 iOS: [http://devops.oa.com/console/pipeline/speedmobile/p-f849d75118634edaac28a75a42491187/history](http://devops.oa.com/console/pipeline/speedmobile/p-f849d75118634edaac28a75a42491187/history)  
7. 发布dev引擎到release: [http://devops.oa.com/console/pipeline/speedmobile/p-ec059983bf1f48b8bb4c55e1b62622b0/history](http://devops.oa.com/console/pipeline/speedmobile/p-ec059983bf1f48b8bb4c55e1b62622b0/history)  
8. 主干出包并验证

