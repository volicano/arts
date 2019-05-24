## ARTS

### Tip

## Packagist镜像使用方法


**修改 composer 的全局配置文件（推荐方式）**

打开命令行窗口（windows用户）或控制台（Linux、Mac 用户）并执行如下命令：

```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

上述命令将会在当前项目中的 composer.json 文件的末尾自动添加镜像的配置信息（你也可以自己手工添加）：

```
"repositories": {
    "packagist": {
        "type": "composer",
        "url": "https://packagist.phpcomposer.com"
    }
}
```

## 解除镜象：

如果需要解除镜像并恢复到 packagist 官方源，请执行以下命令：

```

composer config -g --unset repos.packagist

```

执行之后，composer 会利用默认值（也就是官方源）重置源地址。

将来如果还需要使用镜像的话，只需要根据前面的“镜像用法”中介绍的方法再次设置镜像地址即可。

