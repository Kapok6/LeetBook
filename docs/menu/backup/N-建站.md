# 使用github+jekyll搭建个人仓库

## 搭建步骤

1. github创建个人仓库

2. 选择推荐主题并发布

3. 使用自定义主题

   可以参考他人的jekyll模板，进行二次开发。（clone到个人仓，然后修改）

4. 安装Jekyll&本地调试

   1. 安装ruby环境 （jekyll是使用ruby预研开发的）[ruby下载](https://rubyinstaller.org/)

   2. 安装RubyGems（ruby包管理工具，新版ruby包中包含了rubyGem）

   3. 安装bundler （管理&安装gem依赖包及版本的工具，用`bundler install`命令会根据`Gemfile`中说明的包名臣个版本，安装所有的依赖，类似`npm insatll`）

      ```json
      ruby -v
      gem -v
      bundler -v
      ```

   4. 安装jekyll，内网可能会出现安装不成功的情况，可以改为[内网镜像](http://3ms.huawei.com/km/blogs/details/10616701)、[配置方法](http://3ms.huawei.com/km/blogs/details/6443467)、[内网windows安装Jekyll](https://pnpdjie.github.io/docs/guides/jekyll/windows_setup.html)

      ```json
      // gem配置镜像
      gem sources --add http://mirrors.tools.huawei.com/rubygems/ --remove https://rubygems.org/
      // bundle的gem镜像
      bundle config mirror.https://rubygems.org http://mirrors.tools.huawei.com/rubygems/gems/
      
      // 镜像用不了了，需要设置代理
      set http_proxy=http://proxycn2.huawei.com:8080
      set http_proxy_user=l00501673
      set http_proxy_pass=513619%40hj
      
      // 查看是否成功
      jekyll -v
      ```

   5. 启动本地jekyll项目

      ```json
      jekyll serve -w
      ```

## 推荐资源

1. jekyll模板库： http://jekyllthemes.org/page4/

   