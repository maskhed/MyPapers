---
title: 终端安全
description: 结合自己多年在工作效率提升方面的经验，分享从加速度、专注、自动化、规范性以及测试驱动开发来全方位的提高软件开发工作效率。
---

# 终端安全一站式解决方案

Feei <feei#feei.cn> 2017

## 0x01 安全背景

#### 攘内必先安其外

TODO 整体企业安全架构图

在整体入侵生命周期中的安全产品布局差不多的情况下，我们意识到人成为最大问题。

CASE1 我们各类线上防御做的再好，但攻击者只需要潜伏进公司楼下咖啡馆，就可爆破办公室中的Wi-Fi网络；甚至混入公司，一根网线就可以连入办公网络；

CASE2 在之前曾经出现过邮箱被爆破后，进一步从邮件中找到VPN配置文件，导致轻松被攻击者盗用从而直接连入内网。

CASE3 而同事的各类网站使用同一密码、密码长期不变等等问题导致仅仅账号密码的认证方式不太能适应现在的情况。

CASE4 当获取到组件类型漏洞（比如Struts2、FastJSON某个版出现本反序列化漏洞等）或常规代码安全漏洞（比如SSRF、新型SQL注入等）的安全情报时，安全团队可以通过Cobra扫描集团项目来判断漏洞影响范围，并做好应急响应。 但当获取到客户端软件的安全情报时（比如SourceTree、XCode某个版本出现本地命令执行漏洞等），安全团队无法及时主动的排查受影响的范围。

## 0x02 技术方案

在项目起始，我们也调研了几家大企业对于这些问题的解决方案，大都是常规单项解决方式。 大多数问题都是密码导致的，所以提出了一个大胆的想法，去密码。将所有密码都去掉，交由可信设备处理。 于是GuGu的雏形出来了，用一个客户端，解决上面遇到的所有问题。 整个项目涉及安全、运维、内网、IM四大团队；Mac、Windows、Android、iOS四大平台。

---

### 入网管控

通过GuGu控制密钥，使内置的VPN软件更加安全和方便，随时随地一键即可连入内网。杜绝从办公网入口渗透进入内网的可能性，也不会再出现VPN配置文件泄露导致内网漫游。 同时为了提升开发工作效率，在原有的VPN基础上增加了科学上网功能，方便开发过程中查阅资料和学习。

#### 全平台VPN方案

TODO 各VPN产品功能对比

OpenVPN、IKEv1(IPSec)、IKEv2、

TODO 最终选型方案

- Windows/macOS/Android => OpenVPN
- iOS => IKEv2

TODO 中间遇到的坑

- 下发路由表
- 下发DNS

#### VPN认证方案

![VPN认证方案](images/gugu_vpn_auth.png)

#### VPN Auth Code

将上面计算的最终的结果当成密码去连接VPN。 VPN验证服务判断密码中有四处gu/ug，且密码长度为89位的话则走GuGu SID验证逻辑。

```
# 【拼接原始数据】拼接sessionID（会话ID）、serialNumber（序列号）、timestamp（时间戳）、分隔符（gu/ug）
original = gu + sessionID(32) + ug + serialNumber(34) + gu + timestamp(10)

# 【统一平台数据交互】转换为小写
original = tolower(original)

# 【防止请求重放攻击】取original sha1的前五位，并和original拼接
result = original + ug + sha1(original)[:5]

# len = 89
2*4 + 32 + 34 + 10 + 5 = 89 
```

#### 入网架构

![](images/gugu_network_architecture.png)

####科学上网

---

### 一键免密

GuGu登陆过程中集成了多重安全策略，确保了账号的安全性。其它软件就可以信任GuGu的账号登陆状态，形成一个信任链。 登陆和使用大部分的日常系统，都会唤起GuGu的授权框，实现一键免密鉴权。 当所有系统的免密鉴权全部覆盖完成后，就不存在需要密码的系统了，届时GuGu就可以接管全部系统的密码替换为随机Token，而员工将不用记住任何密码。

#### Web端免密技术方案

![](images/gugu_password-free01.png)

#### GuGu启动HTTP Server

- 默认监听127.0.0.1的51273端口
- 若51273端口被占用，则监听51274端口
- 若51274端口被占用，则放弃启动HTTP Server服务，并上报信息给GuGu服务端

#### GuGu处理获取Token请求

- 不存在的接口将一律返回403状态码
- 不存在的HTTP Method将一律返回405
- 参数格式错误将返回1002
- 提供的为JSONP接口
- 只允许固定User-Agent和固定的域名（Referer）调用
- 一切正常将返回1001

#### 接入方客户端请求GuGu客户端接口

- 【判断是否安装GuGu】先请求127.0.0.1:51273上的获取GuGu版本接口，超时设为1s，若超时则使用51274端口进行重试，若还超时则说明GuGu未安装，返回告知用户使用备选方式登录
- 【获取Token】若安装了GuGu，则请求127.0.0.1端口上Token接口，超时设为33s（弹窗询问时间为30s）。

#### App端免密技术方案

![](images/gugu_free-password-for-mobile01.png)

![](images/gugu_free-password-for-mobile02.png)

#### 三方系统免密方案

![](images/gugu_third-party_authentication.png)

---

### 终端安全

鉴权的GuGu登陆体系和信任设备机制保障GuGu基础安全；通过预先收集电脑已安装的软件和版本号、安全配置信息等，当出现软件安全情报时能快速的确认影响范围；通过开启自动锁屏功能缓解离开电脑时未锁屏的风险。

#### 信息收集

#### 可信设备

---

### 自身加固

#### 登陆限制

预警信息包含规则名、用户名、错误次数、IP、序列号及电脑名称

- 预警单电脑爆破账号密码
- 预警多电脑单IP爆破（序列号可伪造的情况下，使用IP预警）
- 预警账号被爆破（在IP和序列号都被破解的情况下，预警账号爆破）

以下信息可在metabase动态配置

- 各规则的错误次数及封禁时间阈值（调优最佳参数）
- 公司IP白名单（为防止公司IP变动）
- 序列号白名单（为防止紧急情况下用户无法连接内网）

#### Token签名算法

将所有需要提交的参数的key取出来进行顺序排序，并拿到value 比如要提交k1=v1&k2=v2，则将k1,k2提取出来，拿到v1，v2

将所有value拼接成字符串，计算sha1，并附带在最终需要提交的数据中。 sign = sha1(v1+v2) 最终提交的数据k1=v1&k2=v2&sign=sha1(v1+v2)

```python
import hashlib

def generate_sign(params):
    """
    Generate sign by params
    :param params:
    :return:
    """
    if not isinstance(params, dict):
        raise TypeError
    values = '#%$'.join(str(params[x]) for x in sorted(params))
    sha = hashlib.sha1()
    sha.update(values.encode())
    return sha.hexdigest()

params = {
    'k2': 233,
    'k1': '.cn',
    'a': 'feei',
}
sign = generate_sign(params)
assert sign == '7344da1feae2faa432c2fa8aa70839d6bedde142'
```

```java
import java.util.List;
import java.util.Map;
import java.util.Objects;
import java.util.Set;
import java.util.stream.Collectors;

public static final String SEPERATE_CHAR = "#%$";
public static String generateSign(Map<String, Object> param) {
    Set<String> keys = param.keySet();
    //过滤掉sign
    List<String> sortedKeys = keys.stream().filter(key -> !Objects.equals(key, "sign")).sorted().collect(Collectors.toList());
    String calcSign = sortedKeys.stream().map(param::get).map(String::valueOf).collect(Collectors.joining(SEPERATE_CHAR));
    calcSign = DigestUtils.sha1Hex(calcSign);
    return calcSign;
}
```

#### Token交互流程

![](images/gugu_token.png)



## 0x03 项目成型

#### 项目场景

- 在家/公司办公：macOS/Windows 入网管控
- 外出旅游：Android/iOS 入网管控
- 查阅资料：科学上网
- 不用密码登陆各类系统：一键免密鉴权
- 离开座位锁屏：自动锁屏
- 新型高危软件漏洞应急响应：软件版本情报信息收集

#### 项目架构

TODO 整体架构图

