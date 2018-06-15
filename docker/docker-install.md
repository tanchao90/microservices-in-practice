
# Docker Install


## 遇到的问题
#### `Ubuntu` Language Problem
执行 `sudo systemctl enable docker` 产生下面的警告
```
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = (unset),
	LC_CTYPE = "zh_CN.UTF-8",
	LANG = "en_US.utf8"
    are supported and installed on your system.
perl: warning: Falling back to a fallback locale ("en_US.utf8").
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = (unset),
	LC_CTYPE = "zh_CN.UTF-8",
	LANG = "en_US.utf8"
    are supported and installed on your system.
perl: warning: Falling back to a fallback locale ("en_US.utf8").
```

相关命令:

- `locale` 查看当前系统的语言环境
- `locale -a` 列出所有可用的公共语言环境的名称
- `sudo locale-gen` 更新语系至最新
- `sudo locale-gen zh_CN.UTF-8` 下载指定语系

解决办法：

- `export LANGUAGE="en_US.UTF-8"`
- `echo 'LANGUAGE="en_US.UTF-8"' >> /etc/default/locale`
- `echo 'LC_ALL="en_US.UTF-8"' >> /etc/default/locale`

or

- `export LANGUAGE="en_US.UTF-8"`
- `sudo vim /etc/default/locale` 直接编辑配置文件

修改之后 `/etc/default/locale` 文件内容如下：
```
LANG=en_US.utf8
LANGUAGE="en_US.UTF-8"
LC_ALL="en_US.UTF-8"
```

参考资料：

- [Language Problem on Ubuntu 14.04](https://www.digitalocean.com/community/questions/language-problem-on-ubuntu-14-04)
- [Ubutun使用记录——语系错误](https://www.douban.com/note/362250557/)


