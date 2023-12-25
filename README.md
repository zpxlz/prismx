# Prism X · 单兵渗透系统

## 特性

- 集渗透前置、后置于一体的轻量型跨平台系统

![pc_home](/public/static/pc_home.jpg)


## 系统结构

系统功能如下：

<Tree>
  <ul>
    <li>
      仪表盘
      <ul>
        <li>
          工作台
        </li>
      </ul>
    </li>
    <li>
      增效工具
          <ul>
            <li>资产收集</li>
            <li>邮件测试</li>
            <li>遮天平台</li>
            <li>知识文库</li>
          </ul>
      </li>
    <li>
      任务管理
          <ul>
            <li>任务列表</li>
            <li>新建任务</li>
          </ul>
    </li>
    <li>
      扫描配置
          <ul>
            <li>扫描参数</li>
            <li>插件管理</li>
          </ul>
    </li>
    <li>
      系统设定
          <ul>
            <li>基本设置</li>
            <li>资源管理</li>
            <li>用户列表</li>
          </ul>
    </li>
  </ul>
</Tree>


## 启动

<Alert type="warning">
本工具仅面向合法授权的企业资产风险检测，请严格遵守法律规定，不得危害国家安全、公共利益，不得损害个人、组织的合法权益，否则应自行承担所引起的一切法律责任。
</Alert>

下载对应 OS ARCH 的软件包 [Prism X releases](https://github.com/yqcs/heartsk_community/releases/)
，解压之后赋予可执行权限之后直接运行即可。

Linux amd64 运行示例：

```bash
$ wget https://github.com/yqcs/prismx/releases/download/1.0.10/prismx_linux_amd64.zip
$ unzip prismx_linux_amd64.zip
$ cd prismx_linux_amd64
$ chmod +x prismx
$ ./prismx
```

### WEB 模式

为了方便使用，系统提供了 CLI 命令行以及更具交互性的 WEB 模式两种运行方式。WEB 模式需提供 License 文件，运行`./prismx`
命令即可启动。 LOW BUG 版本已经签发了 WEB 模式需要的 License 及公钥文件。

运行之后访问`http://yourIP:80`即可进入登录页，使用-port 参数可指定端口。 系统默认账号`prismx/prismx@passw0rd`
，首次使用请修改账户名与密码！

<img src="/public/static/guide/login.png" alt="login Page"/>

### CLI 命令行

命令行模式无需授权及公钥文件，但是只具有基础的扫描模块，无法使用 WEB 模式的扫描配置以及信息收集等高级功能。执行-h
命令可获取相关帮助。

```bash
$ ./prismx -h
$ ./prismx -t 127.0.0.1 -p 1-500,3000-6000
```

<img src="/public/static/cli.png" alt="cli Page"  width="70%"/>

### Linux For ARM（Android）

#### 具有 Root 权限可以避免百分之九十的问题！

安卓设备为例，直接使用 adb push 推送到 `/data/local/tmp/`目录，然后使用`chmod +x `赋予可执行权限即可直接运行。该方案不便随时运行，可使用终端软件
Termux 支撑。

下载终端工具[Termux](https://termux.com/) ，打开软件之后更新软件包然后安装 wget，再下载二进制程序。

```bash
$ pkg update
$ pkg upgrade
$ pkg install wget
$ wget https://github.com/yqcs/prismx/releases/download/1.0.10/prismx_linux_arm64.zip
$ unzip prismx_linux_arm64.zip
$ cd prismx_linux_arm64
$ chmod +x prismx
$ ./prismx
```

未授予 Root 权限会出现错误：` listen tcp 0.0.0.0:80: bind: permission denied`，使用-port 参数切换绑定端口即可。

执行扫描任务时出现错误：`xx on [::1]:53: read udp [::1]:37606->[::1]:53: read: connection refused`

> 有 ROOT 权限：在手机根目录的 /etc/ 文件夹下新建一个名为 resolv.conf 的文件，内容为`nameserver 8.8.8.8`（DNS 服务器），然后重启
> Termux 之后再次运行即可。
>
> 无 ROOT
> 权限：执行`pkg install proot resolv-conf && proot -b $PREFIX/etc/resolv.conf:/etc/resolv.conf ./prismx -port 8000`
> （运行参数）
> 至此，便可成功启动，在手机浏览器访问首页：http://127.0.0.1:8000 但是并不代表可以完整使用了，以非 ROOT 权限执行任务时切记将存活检测切换为
> Ping 模式！！

<img src="/public/static/guide/phone.jpg" alt="phone Page" width="30%"/>


## 主机管理


一键生成Agent，点击获取载荷即生成客户端。

<img src="/public/static/guide/home.jpg" alt="home Page"/>

## 增效工具

> 模糊搜索：结果基于Hunter平台，使用该功需配置Hunter Api Key
>
> 子域名：该功能基于互联网系统，需确保能正常访问公网
>
> 目录扫描：扫描指定 URL 可能存在安全风险的资源地址。

![img_1.png](/public/static/guide/infoGet.png)

## 任务管理

无障碍创建向导，高级设定：

> - 存活检测：ICMP 模式速度更快，但是需要 ROOT 身份运行，在无 ROOT 权限时请手动切换为 PING。
> - 告警级别：默认选择了中危，在执行扫描任务时如检测到等级大于等于中危的漏洞时会向用户邮箱发送告警通知。通报等级：严重>高危>中危>低危>信息>无
> - 模糊存活：部分主机开启禁 PING，导致常规检测无法验证存活，此时可以启用该选项进行深度检测，默认已选中。
> - 扫描子域：此功能基于互联网系统，此选项需可访问公网。

![img_1.png](/public/static/guide/creatTask.png)

## 扫描配置

### JNDI 服务器

通常启动后首页会提示：JNDI
监控服务未启动，一些检测功能将会受到限制。解决方式：管理员账户前往 `扫描配置 —> 扫描参数 —> 外连设置`，有两种方案

> 自定义 JNDI 服务器：可在本机启动一个监听，服务器地址应当是本机内网/公网 IP 端口
>
> CEYE：配置 CEYE 平台的 Identifier 和 API Token 即可

然后选中对应的服务器模式，保存即可。未正确配置该选项会导致 log4j2 RCE 和 Fastjson RCE 等插件无法使用！

### 第三方平台

在执行子域名扫描以及信息收集的模糊搜索任务时，会依赖外界平台。请配置相关平台的身份验证信息，以保证数据的完整性。

![img.png](/public/static/guide/other.png)

### 字典配置

系统内置默认账户、密码组合。如果密码列里出现{user}占位符，则会被替换成用户名。

![img.png](/public/static/guide/dict.png)

## 插件编写

### 流程可视化创建插件

请确保程序具有读写权限以及根目录存在 lib\exploits 文件夹，插件名即是漏洞名称.yaml

![img.png](/public/static/guide/plugininfo.png)

#### 规则及语法

- Request 可视化编辑器，支持多套请求。
- 支持 CEL 函数语法，语法见教程。
- AND/OR 按钮：如果选中 AND，需要每个请求的响应均符合所设定的响应规则，如果为 OR 则只需符合其中一项即判定为具有该漏洞。

**注**：如果发送的请求是 Post Form 请求，Params 参数须先以 URL Encoded 编码转换。

![img.png](/public/static/guide/pluginRule.png)