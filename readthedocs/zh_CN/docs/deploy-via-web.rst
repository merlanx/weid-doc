.. role:: raw-html-m2r(raw)
   :format: html

.. _deploy-via-web:

使用 WeIdentity 部署工具完成部署（可视化部署方式）
=====================================================================

准备: 打开 WeIdentity 部署工具的 Web 页面
""""""""""""""""""""""""""""""""""""""""""""""""""""""

通过安装“WeIdentity 部署工具”的服务器的 IP 访问 Web 页面 :code:`http://IP:6102` 以进行 WeIdentity 的部署。

.. note::
     1. 在使用之前, 请确保已安装“WeIdentity 部署工具”, 详见文档：\ `安装 WeIdentity 部署工具 <./weidentity-installation.html>`_\。
     2. 若无法使用 Web 页面, 可以使用命令行的方式完成部署, 详见文档：\ `部署文档(命令行部署方式) <./deploy-via-commandline.html>`_\。
     3. 因为“WeIdentity 部署工具”没有账号登录机制，所以必须确保整个环境只有在内网（不会被其他外部用户访问到的网络）可以访问，公网的其他用户不能访问。

第0步: 选择角色
"""""""""""""""""""""""""""

此步骤可选择部署时所用的角色, 包括 “联盟链委员会管理员” 和 “非联盟链委员会管理员”, 如下图所示。

   .. image:: images/deploy-via-web-guide-choose-role.png
      :alt: deploy-via-web-guide-choose-role.png

.. note::
     什么是“联盟链委员会管理员”?
       一条联盟链中，选取一家机构来作为联盟链委员会管理员，此机构将会管理和运维此联盟链，并负责
       完成 WeIdentity 智能合约的部署。举个例子，一条联盟链有4个机构，其中一个机构可以作为联盟链委员会管理员，其他则是联盟链委员会普通成员。

第1步: 配置区块链节点
"""""""""""""""""""""""""""

此步骤将配置需连接的区块链节点, 如下图所示。

   .. image:: images/deploy-via-web-guide-setup-blockchain.png
      :alt: deploy-via-web-guide-setup-blockchain.png

   - 机构名称
      * 配置说明：机构名称用于标识机构唯一性, 类似域名的作用，在同一条联盟链上，先注册先得。例如微众，可以填入"webank"作为其机构名称。
      * 配置要求：建议使用机构的英文名称或简称, 并确保机构名称在联盟链成员中唯一。

   - AMOP 通讯 ID
      * 配置说明：此 ID 将作为节点间 AMOP 通讯所需要的Topic来进行监听。AMOP 通讯可在不同机构的节点间通讯, 亦可在同一机构内的不同节点间通讯。
      * 配置要求：建议使用英文, 并确保 ID 机构唯一或 VPC 唯一。\ `「什么是 AMOP ?」 <https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/docs/manual/amop_protocol.html?highlight=amop>`_\

   .. - 配置部署环境
      * 配置说明：目前支持三种部署环境, 生产环境, 测试环境和开发环境。不同环境可使用同一条区块链, 亦可各自使用独立的链。
      * 配置要求：请根据实际需要选择, 并确保联盟链成员的环境一致。

   - 配置区块链节点 IP 和 Channel 端口
      * 配置说明：填入集成 WeIdentity Java SDK 的service（或者部署 WeIdentity Rest Service），所需连接的区块链节点的内网或公网 IP。Channel 端口为该节点的Channel端口。 \ `「什么是 Channel 端口?」 <https://mp.weixin.qq.com/s/XZ0pXEELaj8kXHo32UFprg>`_\
      * 配置要求：格式为区块链节点 IP:Channel 端口。如需使用多个区块链节点, 请用半角逗号","分隔。
      * 连接单个节点的配置示例：10.10.4.1:20200
      * 连接多个节点的配置示例：10.10.4.1:20200,10.10.4.2:20200,127.0.0.1:20200

   - 配置 SDK 证书
      * 配置说明：连接区块链节点时需要使用的 SDK 证书。
      * 配置要求：登录区块链节点服务器, 生成/拷贝证书文件, 并下载到本机后上传。SDK 证书文件包括 ca.crt, node.crt 和 node.key。

.. note::
     1. \ `「如何获得 SDK 证书文件?」 <https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/docs/enterprise_tools/operation.html#get-sdk-file>`_\
     2. \ `「区块链中的各种证书」 <https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/docs/manual/certificates.html>`_\

第2步: 选择主群组
"""""""""""""""""""""""""""

如下图所示。

   .. image:: images/deploy-via-web-guide-choose-group-id.png
      :alt: deploy-via-web-guide-choose-group-id.png

   - 选择主群组 ID
      * 配置说明：主群组是 WeIdentity 智能合约部署的群组，需要协调所有部署了区块链节点的机构选择一个群组作为主群组，所有机构的所有区块链节点都需要加入这个主群组，这样才能保证所有的WeID都是相互可见的。
      * 如果您使用到了多群组的架构，例如整个联盟链部署了 ID 为98，101，102三个区块链群组，然后所有部署了区块链节点的机构协商选择 98 作为主群组，则这个步骤所有机构都选择98即可；
      * 如果整个联盟链只有一个群组，则这里就选择这个唯一的群组作为主群组。

.. note::
   \ `「如何查看群组?」 <https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/docs/manual/console.html#getgrouplist>`_\

第3步: 配置数据库(可选)
"""""""""""""""""""""""""""

此步骤将配置所需连接的数据库环境, 请提前自行安装数据库并创建数据库实例及用户。

   - 配置数据库
      * 配置说明：当需要使用 Transportation, Envidence 异步存证,Persistence 数据存储等相关功能组件时, 数据库才是必须的。

   .. image:: images/deploy-via-web-guide-setup-database.png
      :alt: deploy-via-web-guide-setup-database.png

第4步: 创建管理员 WeID
""""""""""""""""""""""""""""""""""""""""""

此步骤将配置您在 weid-build-tools 里面的 Admin 账户, 后续的部署等操作将使用该账户（请妥善保管私钥, 谨防丢失）。

   .. image:: images/deploy-via-web-guide-create-admin-weid.png
      :alt: deploy-via-web-guide-create-admin-weid.png

第5步: 部署 WeIdentity 智能合约（仅联盟链委员会管理员需要执行这一步骤）
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

此步骤将部署 WeIdentity 智能合约 到指定的区块链上, 如图所示。

   .. image:: images/deploy-via-web-guide-deploy-weid-contract.png
      :alt: deploy-via-web-guide-deploy-weid-contract.png

   - 配置链 ID (chain-id)
         * 配置说明：\ `「什么是链 ID (Chain Id) ?」 <./weidentity-spec.html#id4>`_\
         * 如果是为了测试或者体验部署工具流程，可以填入一个随意的数字，例如1000


最后
""""""""""""""""""""""""""""""""""""""""""

至此，配置和部署已经完成，在 `./output/admin/` 目录下生成 Admin 密钥文件, 用于后续注册权威机构等管理操作, 请妥善保管。
