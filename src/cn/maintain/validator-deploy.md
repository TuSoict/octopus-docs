## 搭建验证节点

章鱼网络提供了一键部署验证节点的服务。这是为了简化部署过程，但它不是唯一的方法，验证节点运营商也可以[手动搭建验证节点](./validator-deploy-manually.md)。

**关于验证节点的硬件配置**

> 在测试网络中，我们的一键部署工具使用AWS EC2实例**t3.small**，默认配置为CPU 2核，内存 2G，SSD存储 80G。如果你手动搭建验证节点，可以参考这个配置。

### 自动搭建验证节点

**注**：目前一键部署服务仅支持部署验证节点到 AWS 服务器。

验证人访问章鱼网络([Mainnet](https://mainnet.oct.network) 和 [Testnet](https://testnet.oct.network))，在应用链列表中，选择要成为验证人的应用链，点击进入应用链页面，进行以下操作：

1. 在应用链页面中，点击`Deploy Tool`，在弹出页面中，选择`Deploy Node`；

![deploy node](../../maintain/validator_deploy_node.jpg)

2. 在弹出页面中，输入你的`AWS Access Key`，点击`Enter`；

![deploy login](../../maintain/validator_deploy_login.jpg)

3. 点击`Deploy New`，在弹出页面中，选择`Base Image`，点击`Deploy`；

![deploy new](../../maintain/validator_deploy_new.jpg)

4. 完成初始化后，点击`Apply`，在弹出的窗口中，输入你的`AWS Access Secret`，点击`Apply`；

![deploy apply](../../maintain/validator_deploy_apply.jpg)

**注**：AWS Secret Key 仅会被用于此次部署，并且不会在任何地方被存储，帐户的风险非常低。

5. 部署过程大约持续3-5分钟，然后刷新页面查看状态，部署成功如下图所示。记录 AWS 实例的登录信息，并点击`RSA`下载用于 SSH 登录 AWS 实例的密钥文件。

![deploy success](../../maintain/validator_deploy_success.jpg)

### 检查节点是否完成同步

1. 登录 AWS 实例

```bash
ssh -i <id_rsa文件路径> ubuntu@<AWS实例的IP地址>
```

2. 输入以下命令检查验证节点的 Docker 日志

```bash
docker logs seashell
```

是否有类似的输出如下：

```bash
2021-09-21 00:12:09 ✨ Imported #54411 (0x3566…3b0e)
2021-09-21 00:12:12 ✨ Imported #54412 (0xdf36…2c87)
2021-09-21 00:12:12 [54412] 🐙 Current block: 54412 (parent hash: 0x9cc7f31a20793f50cf885835de0e3977a1e080431ebc002469aa176046ba094a)
......
2021-09-21 00:13:18 ✨ Imported #54434 (0xba36…ee68)
2021-09-21 00:13:18 [54434] 🐙 Current block: 54434 (parent hash: 0x84aa3d1b6455859f9503d6ecc70b50b183141fe08f5b0695357e00fe1d24d915)
2021-09-21 00:13:18 💤 Idle (6 peers), best: #54434 (0xba36…ee68), finalized #54431 (0xd194…b319), ⬇ 22.0kiB/s ⬆ 21.9kiB/s
```

**备注**：使用下载的密钥文件 SSH 登录到 AWS 服务器实例时，如果遇到错误`Permissions 0644 for 'id_rsa' is too open`，请执行以下命令：

```bash
chmod 400 ~/.ssh/id_rsa
```


