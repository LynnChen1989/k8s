# dashboard https

    https://github.com/kubernetes/dashboard/wiki/Dashboard-arguments 
    
    
dashboard的https端口监听在8443，http端口监听在9090， 我们后端启动dashboard的时候把他监听在9090端口即可。

具体可以参考当前目录的 kubernetes-dashboard.yaml文件， args 指定了 `--enable-insecure-login=true`。

我们通过traefix的ingress接入的时候有个坑， 你看虽然我的service指定了nodePort: 30001，但是直接通过ip:30001去访问的话， token
认证之后是没有读取k8s系统权限的（我本来想提前验证一下）。

文档中给了解释

```

When enabled, Dashboard login view will also be shown when Dashboard is not served over HTTPS. 
Still, it requires frontend to be accessed over HTTPS (i.e. secure nginx proxy).

```

意思就是，即使你后台使用了http服务，但是前端接入的时候也只能通过https的方式