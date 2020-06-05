## 操作场景

Tencent Serverless Toolkit for VS Code 是腾讯云 Serverless 产品的 VS Code（Visual Studio Code）IDE 的插件。该插件可以让您更好的在本地进行 Serverless 项目开发和代码调试，并且轻松将项目部署到云端。

通过该 VS Code 插件，您可以：

- 拉取云端的云函数列表，并触发云函数。
- 在本地快速创建云函数项目。
- 使用模拟的 COS、CMQ、CKafka、API 网关等触发器事件来触发函数运行。
- 上传函数代码到云端，更新函数配置。
- 在云端运行、调试函数代码。

## 前提条件
Tencent Serverless 可在 Windows， MacOS 中安装。在安装 Tencent Serverless 之前，需要确保系统中已有以下组件/信息：
- 已注册腾讯云帐户。若未注册腾讯云账户，可 [点此](https://cloud.tencent.com/register) 进入注册页面。
- VS Code ：在 [VS Code下载页面](https://code.visualstudio.com/) 下载对应的 IDE 并安装，其**版本要求为 v1.33.0 +**。


## 操作步骤

### 安装插件
可通过以下两种方式安装 SCF VS Code 插件：  

**a. 通过插件市场直接安装**
进入 [插件市场](https://marketplace.visualstudio.com/items?itemName=tencentcloud.tencent-cloud-vscode-toolkit) 单击【install】进行安装。

**b. 通过 VS Code IDE 安装**
1. 运行 VS Code IDE。
2. 打开 VS Code 插件市场。
3. 在搜索框中输入 “Tencent Serverless”，单击搜索框下方列表中的 Tencent Serverless 插件查看详情并选择【install】。如下图所示：      
  ![](https://main.qcloudimg.com/raw/4d629d80bb03d4957213af44a4fb524c.png)    
  安装完成后，左侧栏中会展示已安装完毕的 Tencent Serverless 插件。


### 配置插件


1. 单击左侧导航栏的<img src="https://main.qcloudimg.com/raw/4395057dfb3a8f4a92c90ba7dff9b1c1.png" style="margin:-3px 0;">，打开已安装好的 Tencent Serverless 插件。
2. 单击创建一个腾讯云用户凭证。如下图所示：  
![](https://main.qcloudimg.com/raw/fca11ef6e54287f2ad400d34123872c9.png)
3. 根据提示依次输入账号的 APPID，SecretId 及 SecretKey 信息，作为插件调用云 API 时的认证信息。并在认证成功后，选择您希望部署函数的地域。配置信息获取途径如下：
 - 账号的 APPID：通过访问控制台中的 [账号信息](https://console.cloud.tencent.com/developer)，可以查看您的 APPID。
 - SecretId 及 SecretKey：通过访问控制台中的 [API 密钥管理](https://console.cloud.tencent.com/cam/capi)，获取相关密钥或创建相关密钥。
 - 地域：地域列表及对应的英文写法可 [点此](https://cloud.tencent.com/document/product/213/6091#.E4.B8.AD.E5.9B.BD.E5.A4.A7.E9.99.86.E5.8C.BA.E5.9F.9F) 参阅。

4. 为提升函数上传效率，您可以在 VS Code 中 [设置开启 COS 上传](#openCOS) 。


### 创建函数

1. 单击已配置账户函数列表上方的<img src="https://main.qcloudimg.com/raw/799da4ba342886d96d6a3e10681a2560.png" style="margin:-3px 0;">，在本地初始化新的函数项目。
2. 根据提示依次选择函数运行时 Runtime，并输入函数名。
3. 函数信息录入成功后，将开始创建。
4. 函数创建成功后，会跳转到工作区打开函数的入口文件。
5. 将 `index.py` 中的代码替换为如下内容：   
```
   import sys
   import logging
   print('Loading function')
   logger = logging.getLogger()
   def main_handler(event, context):
       logger.info("start main handler")
       print(event)
       body = 'API GW Test Success'
       response = {
           "isBase64": False,
           "statusCode": 200,
           "headers": {"Content-Type": "text",    "Access-Control-Allow-Origin": "*"},
           "body": body
       }
       return response
```

### 本地测试
进入 Tencent Serverless 插件，单击本地函数列表目标函数右侧的<img src="https://main.qcloudimg.com/raw/fef0ef2e04f094c5b3a390e6d78672c0.png" style="margin:-3px 0;">，即可对函数进行本地测试。如下图所示：
>- 当前仅支持 Python 及 Node.js 函数的本地调试能力。
>- 如果您有安装多个 Python 版本，可根据当前要调试的 runtime 在 VS Code 里 [设置 Python path](#pythonpath)。

![](https://main.qcloudimg.com/raw/23bd4100d4d720ab189d8c504f2cc4cd.png)



### 断点调试

#### 设置断点

针对 Python 函数，可以在 VS Code 插件进行本地调试。

>本地调试目前支持 Python 和 Node.js ，调试 Python 项目需要先安装 [Python 插件](https://marketplace.visualstudio.com/items?itemName=ms-Python.Python) 。如果您有安装多个 Python 版本，可根据当前要调试的 runtime 在 VS Code 里 [设置 Python path](#pythonpath)。

1. 单击左侧导航栏中的<img src="https://main.qcloudimg.com/raw/063fc1d0d23c25144d8bd883a3a89185.png" style="margin:-3px 0;">，进入本地编辑页面，给函数设置断点。如下图所示：  
   ![](https://main.qcloudimg.com/raw/7bde25c17fe9c35d25ef6126a736a2bc.png)
#### 设置调试模版

1. 单击左侧导航栏顶部的<img src="https://main.qcloudimg.com/raw/e2e619960ca0f19d1a6fd7ab1136ae8c.png" style="margin:-3px 0;">，进入调试页面（或 ctrl+shift+D）。新建调试配置文件，**并选择 SCF Debugger For Python 调试模板（Node 项目请选择 SCF Debugger For Node）**。如下图所示：
>不同的 runtime 须选择对应的调试模板，可根据您当前的调试文件类型，区分选择 Python 和 Node.js。     

![](https://main.qcloudimg.com/raw/fb49c3724b406f4ae15b7480d61580b9.png) 



#### 开始调试

单击<img src="https://main.qcloudimg.com/raw/fa6357932d0b31898ede19d74f129fb5.png" style="margin:-3px 0;">，即可看到调试信息。如下图所示：  

>   目前插件是对当前工作区打开的函数文件进行调试，为保障调试正常进行，您需要确保目标函数文件已经当前窗口打开。

![](https://main.qcloudimg.com/raw/30ae89b7e45482253cc9ffa29973f67b.png)

### 部署函数（含配置触发器）

1. 修改模板文件，配置触发器。
  由于我们的函数是基于 API 网关触发，所以需要在模板文件里（template.yaml）添加 API 网关触发事件。完整 `template.yaml` 如下：
```yaml
    Resources:
     default:
       Type: TencentCloud::Serverless::Namespace
       hello_world:
         Type: TencentCloud::Serverless::Function
         Properties:
           CodeUri: ./
           Type: Event
           Description: This is a template function
           Environment:
             Variables:
               ENV_FIRST: env1
               ENV_SECOND: env2
           Handler: index.main_handler
           MemorySize: 128
           Runtime: Pythonn2.7
           Timeout: 3
           Events:
                 hello_world_apigw:   #${FunctionName} + '_apigw'
                      Type: APIGW
                      Properties:
                             StageName: release
                             ServiceId:
                             HttpMethod: ANY
```
更多模板文件规范请参见 [腾讯云无服务器应用模型](https://cloud.tencent.com/document/product/583/36198)。  
2. 进入 Tencent Serverless 插件，单击击本地函数列表目标函数右侧的<img src="https://main.qcloudimg.com/raw/cfd7dc52f54c97eaee9025b85a4f9830.png" style="margin:-3px 0;">。如下图所示：
>如果您的函数有使用第三方依赖，则需要将依赖包放至函数目录下然后执行上传。Python 依赖安装方法可 [参考此处](<https://cloud.tencent.com/developer/article/1443081>)。

![](https://main.qcloudimg.com/raw/15e5a8e036ae36fc23e73980f0c520a1.png)		          
3. 函数上传完毕，单击云端函数右侧的<img src="https://main.qcloudimg.com/raw/6771f42abb5da560731e246810d71bf7.png" style="margin:-3px 0;">进行刷新，即可查看已上传的函数。（查看区域需切换到上传时选择的区域）如下图所示：   
![](https://main.qcloudimg.com/raw/715c67df166846c81321e83b2ade5ebb.png)  
上传成功之后，可在 VS Code 查看部署详情。如下图所示：  
![](https://main.qcloudimg.com/raw/b2ee7c648a68157d3a5e31c119916ea6.png)  
您可以将 serviceId 复制到配置文件 template.yaml 中，之后部署就不会再创建新的 API 网关。  
4. 完成部署后，可使用浏览器访问 VS Code 输出的 `subDomain` 访问路径，显示结果如下：  
![](https://main.qcloudimg.com/raw/2a14a48452c33341a0c82b46203a7656.png)  
您也可以登录 [云函数控制台](https://console.cloud.tencent.com/scf)，到相应地域查看已成功部署的函数。
>- 使用 COS 部署函数最高能提升80%的速率，大大提高了工作效率。但在部署频次、部署包很大时，可能会产生 COS 计费。
>- 现 SCF 与 COS 联合发布限时活动，开启 COS 部署即可领取代金券，请前往 [SCF 控制台](https://console.cloud.tencent.com/scf/index?rid=1?from=fromdoc) 查看活动。
>- 您可以在 VS Code 中 [设置开启 COS 上传](#openCOS) 。

### 忽略上传

实际项目中，可以自定义不想上传的文件内容，SCF 插件将会忽略这些内容进行打包上传。

1. 您需要在代码路径下，新建 `ignore` 文件夹。
2. 进入 `ignore` 文件，新建忽略配置文件 `FUNCTIONNAME.ignore` ，并在该文件下描述忽略的内容。

> 路径规范：以 template.yaml 里的 CodeUri 路径为基准 ，定义想要忽略的内容所在位置。
>
> 如下所示，template.yaml 里定义函数名为 hello，CodeUri 为`./`。

```yaml
Resources:
  default:
    Type: TencentCloud::Serverless::Namespace
    hello:
      Type: TencentCloud::Serverless::Function
      Properties:
        CodeUri: ./
        Type: Event
        Description: This is a template function
        Handler: index.main_handler
        MemorySize: 128
        Runtime: Python3.6
        Timeout: 3
```

则目录层级及 HELLO.ignore 如下图所示：

![](https://main.qcloudimg.com/raw/867949f7ba128cd078bd4b2bf400783a.png)

完成配置后，最终上传会**忽略 testmodule 目录**和**当前路径下所有 md 文件**。




### 云端测试
单击左侧列表右侧的<img src="https://main.qcloudimg.com/raw/fef0ef2e04f094c5b3a390e6d78672c0.png" style="margin:-3px 0;">，即可在页面中查看到函数在云端运行的相关信息。如下图所示：    
![](https://main.qcloudimg.com/raw/2c7fb7f915028f4187265af6aef44aaf.png)



### 更多功能

#### 查看日志

云端调用的日志会输出到 VS Code。如下图所示：  
![](https://main.qcloudimg.com/raw/83c56ff1d4e808488cffafea2867f4de.png)  
您也可以前往控制台打开函数页面选择【运行日志】，查看所有历史日志，详情请参见 [函数日志](https://cloud.tencent.com/document/product/583/36143)。

#### 下载函数

如果您已经在 [云函数控制台](https://console.cloud.tencent.com/scf/list) 创建了函数，则可以在 VS Code 插件里直接将云端函数下载到本地。  

1. 单击目标云端函数右侧的<img src="https://main.qcloudimg.com/raw/d98f76c38a8805eacb250bf40f00b695.png" style="margin:-3px 0;">，将函数导入到本地。如下图所示：   
   ![](https://main.qcloudimg.com/raw/50ede722431a430dc3af2798130d2ad1.png)
2. 选择函数下载的目标目录，下载完成后您可以选择在本窗口打开函数或者新建窗口打开。



#### 测试模板

本地调用时，可根据函数功能选择不同的测试模板，也可以自定义模板。如下图所示：  
![](https://main.qcloudimg.com/raw/ff6b012bc6a8730704d35dcdc02efba7.png)

更多测试模版相关内容，详情请参见 [触发器](https://cloud.tencent.com/document/product/583/9705)。



#### 查看监控

1. 登录 [云函数控制台](https://console.cloud.tencent.com/scf/list)，单击左侧导航栏【函数服务】。
2. 在“函数服务”页面上方选择已创建函数地域，并单击函数 ID。
3. 在已创建函数的详情页面，选择【监控信息】，即可查看函数调用次数/运行时间等情况。如下图所示：

> 监控统计的粒度最小为1分钟。您需要等待1分钟后，才可查看当次的监控记录。
>
> [](https://main.qcloudimg.com/raw/acc4d768c7a23e424fd65e065b1c043f.png)
> 更多关于监控信息请参见 [监控指标说明](https://cloud.tencent.com/document/product/583/32686)。

#### 配置告警

在已创建函数的详情页面，单击【前往新增告警】为云函数配置告警策略，对函数运行状态进行监控。如下图所示：  
![](https://main.qcloudimg.com/raw/6850e40bca71bfe7ca976004388294c8.png)
更多关于配置告警请参见 [告警配置说明](https://cloud.tencent.com/document/product/583/30133)。  

## 常见问题  

安装或使用过程中有遇到问题，可参考 [SCF 工具类常见问题](https://cloud.tencent.com/document/product/583/33456) 解决，您也可以通过 [欢迎交流](#welcome) 与我们联系。    

## 相关操作

<span id="openCOS"></span>
### 设置开启 COS 上传
1. 选择左下角的<img src="https://main.qcloudimg.com/raw/20fd46098cf037eb003dc41f1f913313.png" style="margin:-3px 0px;"/> >【Settings】。如下图所示：

    ![](https://main.qcloudimg.com/raw/e9e1f63819d29d86d8f9cae9cbb9e31a.png)
2. 在“Settings”页面，选择【Extensions】>【Scf】并勾选【Enable deployed by COS】。如下图所示：
    ![](https://main.qcloudimg.com/raw/05ca88747213e5a102747683dc20233a.png)


<span id="pythonpath"></span>
### 设置 Python path
如果您有安装多个 Python 版本，可根据当前要调试的 runtime 将 Python 2 或 Python 3 的对应路径填入 Scf 设置即可。如下图所示：
![](https://main.qcloudimg.com/raw/036d49a4388314a96880021b8a7aaff9.png)




## 欢迎交流<span id="welcome"></span>
如果您对 Tencent Serverless 感兴趣，您可以加入QQ群（537539545）与我们交流。    
![Alt text](https://main.qcloudimg.com/raw/bc881547d1cd2043ecf1b286c70f7319.png)