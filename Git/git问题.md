



- push的时候总是出现：

```
fatal: remote error:
You can't push to git://github.com/user_name/user_repo.git
Use git@github.com:user_name/user_repo.git
```

经过高人的点播，找到了原因：

如果在git clone的时候用的是git://github.com:xx/xxx.git 的形式, 那么就会出现这个问题，因为这个protocol是不支持push的

用git clone [git@github.com:lujinjianst/myNCCL.git](mailto:git@github.com:lujinjianst/myNCCL.git)

就可以用git push了。





from https://xvideos.blog.csdn.net/article/details/52367128



```
https://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@github.com/conversekuang/supplyments.git


```

