# 部署打京豆


## 简介

打京豆的脚本部署流程, 非脚本作者, 该仓库为使用说明, 每个月能打1500(不确定)左右, 如果你的计算机有安装Docker, 推荐使用[本地部署](#本地部署)方式, 没有就使用[腾讯云函数部署](#腾讯云函数部署), 不计划设计助力池.

准备工作​：
一个 Github 账号（可能需要fan qiang，推荐使用 HideU）
一个 腾讯云 账号，并 实名认证

## 腾讯云函数部署
Github准备
1. 创建一个空Github库并进入 [点我创建](https://github.com/new)
![image](https://user-images.githubusercontent.com/25003641/153543956-3720cd4f-e4b8-4ef7-90cb-9eca81693e85.png)

2. 进入刚才创建的库，输入 https://github.com/bullfly666/percollect 等待代码同步
![image](https://user-images.githubusercontent.com/25003641/153543998-dcf8ea32-dad2-47ee-b23a-e4b480e3cf97.png)
![image](https://user-images.githubusercontent.com/25003641/153544052-9605b9b9-1079-4f87-b174-0a0a39c4fce5.png)
![image](https://user-images.githubusercontent.com/25003641/153544090-07dc584a-eca4-49cc-bc42-460e7f7fec86.png)
3. 申请PAT
生成token连接[点我跳转](https://github.com/settings/tokens/new)
把repo和workflow两部分勾上，然后点击最下面的创建按钮。
![image](https://user-images.githubusercontent.com/25003641/153544389-5bed1854-7987-4f3c-a17f-a250a42bd1db.png)
![image](https://user-images.githubusercontent.com/25003641/153544425-46e090cf-925c-4490-b22d-de13b9aa6801.png)
![image](https://user-images.githubusercontent.com/25003641/153544663-7db8fa0d-d966-4919-a4a5-67b2f247c799.png)

4. 填写PAT到Secrets 然后保存
![image](https://user-images.githubusercontent.com/25003641/153544790-61734b4a-2822-4763-b9b0-de4a61de542d.png)
![image](https://user-images.githubusercontent.com/25003641/153544890-5ee63800-d411-461e-a36b-95cbb4f4dbd1.png)
name填PAT，Value填入上方申请到的PAT,保存即可
![image](https://user-images.githubusercontent.com/25003641/153545002-b7d1a270-48d9-40f6-ae86-98752026905b.png)
5. 同步仓库
在刚刚创建的库中点击Actions，执行同步任务
![image](https://user-images.githubusercontent.com/25003641/153545419-eee5f541-3a32-4f07-85d1-6413fb2e88ec.png)
设置为主分支步骤
![image](https://user-images.githubusercontent.com/25003641/153545646-5ba2ba86-ef73-49b1-ae2a-c9ccfa32dcdb.png)
![image](https://user-images.githubusercontent.com/25003641/153545752-58ebf927-4c6f-44a2-8111-5f2e6df97e95.png)
![image](https://user-images.githubusercontent.com/25003641/153545796-9a1b8617-77b1-46d3-a8e1-d72e84f8ee64.png)
![image](https://user-images.githubusercontent.com/25003641/153545910-8824a500-47e1-48eb-a60c-6bfca20f2006.png)
点下面执行相应的功能
![image](https://user-images.githubusercontent.com/25003641/153546106-1f438b61-41cd-470a-b303-91e366bd352d.png)



### 开通云函数服务

创建腾讯云账号, 依次进入 [SCF 云函数控制台](https://console.cloud.tencent.com/scf) 和 [SLS 控制台](https://console.cloud.tencent.com/sls) 开通相关服务，并创建相应[服务角色](https://console.cloud.tencent.com/cam/role)**SCF_QcsRole、SLS_QcsRole**, 腾讯云日志服务不再免费, 余额为0建议往账户里面充值1元, 防止欠费被禁用.

> 为了确保权限足够，不要使用子账户！腾讯云账户需要[实名认证](https://console.cloud.tencent.com/developer/auth)才可使用。

### 配置环境变量

通过点击**New repository secret**, 分别添加:

1. TENCENT_SECRET_ID: 进入[腾讯云密钥](https://console.cloud.tencent.com/cam/capi), 点击新建密钥后就会生成**SecretId**和**SecretKey**
2. TENCENT_SECRET_KEY
3. PT_KEY、PT_PIN: 登录后可从cookie中得到**PT_KEY**和**PT_PIN**, [获取方式](./wiki/GetJdCookie.md)
4. TENCENT_FUNCTION_NAME: 云函数名称, 任意值, 不填会有几率导致部署失败

![image](https://user-images.githubusercontent.com/27798227/153350464-52b14658-60ee-4b9c-a101-25a094e30f10.png)

### 部署

点击Actions->云函数部署, 先点击Enable WorkFlow启用, 再点击Run workflow, 等待运行完成, 没报错就是部署成功, 访问[腾讯云函数](https://console.cloud.tencent.com/scf/list), 即可查看最新部署的函数

![image](https://user-images.githubusercontent.com/6993269/99513289-6a152980-29c5-11eb-9266-3f56ba13d3b2.png)

有两种情况可能部署失败

1. 上传函数超时: 重新执行部署工作流直至成功
2. 未配置TENCENT_FUNCTION_NAME参数, 不配置会导致失败, 这是腾讯云的bug

### 日志和测试

点击云函数, 可在**日志查询**面板查看定时执行的任务日志.

![](image/README/1644476536637.png)

手动测试流程:

1. 点击云函数->函数代码->切换到旧版编辑器
2. 拉到编辑器下面, 就可以看到测试输入框了
3. 将message修改中的内容修改为脚本对应的名称, 测试多个脚本通过&符号连接.

![img](image/README/1644476708924.png)

### 自动同步代码

(非必须)自动同步你的仓库为该仓库的最新代码, 步骤:

1. [点此生成一个 token](https://github.com/settings/tokens/new) ，需勾选 `repo`和 `workflow`, 然后点击最下面的**Create Token**按钮。
2. 在仓库内**settings->secrets->Actions**中添加一个名为为**PAT**, 值为刚才创建的Token.
3. 进入Action中的同步仓库代码, 选中Enable WorkFlow, 然后就会定时执行同步Action了, 也可手动执行

![](image/README/1644497801258.png)

## 本地部署

需要熟悉Docker的使用方式

1. 安装Docker
2. 安装青龙面板(用于定时执行脚本):

   1. 运行青龙面板Docker镜像: `docker run -dit -v $PWD/ql/config:/ql/config -v $PWD/ql/log:/ql/log -v $PWD/ql/db:/ql/db -p 5600:5600 --name qinglong --hostname qinglong --restart always whyour/qinglong:latest`
   2. 在浏览器访问127.0.0.1:5600, 按照提示完成初始化
3. 在青龙面板**右上角点击新建任务**, 配置:

   - 命令: ql repo https://github.com/cweijan/JD_tencent_scf.git "src"  "icon" "^jd[^_]|USER|sendNotify|sign_graphics_validate|JDJR|JDSign|ql"
   - 定时规则: 50 0 0 * *  
     ![img](image/README/1644410122098.png)
4. 配置青龙面板

   - 添加: export PT_KEY=""和export PT_PIN="", [获取方式点这里](./wiki/GetJdCookie.md)
   - 修改GithubProxyUrl为GithubProxyUrl=""
     ![img](image/README/1644421618420.png)
5. 回到定时任务面板, 点击任务的运行按钮, 就会拉取所有的脚本, 并定时执行这些脚本, 也可手动点击脚本旁边的按钮执行.

![image](https://user-images.githubusercontent.com/27798227/153328329-b0854a0b-a279-4be9-aabe-f27fee1bb752.png)

## 特别声明

* 本仓库发布的Script项目中涉及的任何解锁和解密分析脚本，仅用于测试和学习研究，禁止用于商业用途，不能保证其合法性，准确性，完整性和有效性，请根据情况自行判断.
* 本项目内所有资源文件，禁止任何公众号、自媒体进行任何形式的转载、发布。
* lxk0301对任何脚本问题概不负责，包括但不限于由任何脚本错误导致的任何损失或损害.
* 间接使用脚本的任何用户，包括但不限于建立VPS或在某些行为违反国家/地区法律或相关法规的情况下进行传播, lxk0301 对于由此引起的任何隐私泄漏或其他后果概不负责.
* 请勿将Script项目的任何内容用于商业或非法目的，否则后果自负.
* 如果任何单位或个人认为该项目的脚本可能涉嫌侵犯其权利，则应及时通知并提供身份证明，所有权证明，我们将在收到认证文件后删除相关脚本.
* 任何以任何方式查看此项目的人或直接或间接使用该Script项目的任何脚本的使用者都应仔细阅读此声明。lxk0301 保留随时更改或补充此免责声明的权利。一旦使用并复制了任何相关脚本或Script项目的规则，则视为您已接受此免责声明.

 **您必须在下载后的24小时内从计算机或手机中完全删除以上内容.**  `</br>`

> ***您使用或者复制了本仓库且本人制作的任何脚本，则视为 `已接受`此声明，请仔细阅读***

## 特别感谢(排名不分先后)：

* [@NobyDa](https://github.com/NobyDa)
* [@chavyleung](https://github.com/chavyleung)
* [@liuxiaoyucc](https://github.com/liuxiaoyucc)
* [@Zero-S1](https://github.com/Zero-S1)
* [@uniqueque](https://github.com/uniqueque)
* [@nzw9314](https://github.com/nzw9314)
* [@JDHelloWorld](https://github.com/JDHelloWorld)
* [@smiek2221](https://github.com/smiek2221)
* [@star261](https://github.com/star261)
* [@Wenmoux](https://github.com/Wenmoux)
* [@Tsukasa007](https://github.com/Tsukasa007)
* [@Aaron](https://github.com/Aaron)
