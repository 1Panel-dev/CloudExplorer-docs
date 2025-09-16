## 本地开发

### 克隆本仓库

```bash
git clone https://github.com/1Panel-dev/CloudExplorer-docs
```

### 安装依赖

```bash
cd docs
pip install -r requirements/requirements.txt
```

### 修改文档内容

本文档的文档结构定义在 `mkdocs.yml` 文件中，文档的具体内容均在 `docs` 目录中。
```yaml
..........
nav:
    - 产品介绍: index.md
    - 快速入门: quick_start.md
    - 系统架构: system_arch.md
    - 安装部署:
        - 在线安装: installation/online_installation.md
        - 离线安装: installation/offline_installation.md
        - 在线升级: installation/online_upgrade.md
        - 离线升级: installation/offline_upgrade.md
        - 命令行工具: installation/cli.md
    - 功能手册:
        - 通用功能: user_manual/general.md
        - 主页: user_manual/homepage.md
        - 管理中心:
            - 云账号: 
                - 添加云账号: user_manual/management/add_cloudaccount.md
                - 数据同步设置: user_manual/management/sysn_setting.md
           - 用户与租户: 
                - 用户管理: user_manual/management/user_management.md
                - 角色管理: user_manual/management/role_management.md
                - 组织管理: user_manual/management/org_management.md
            - 日志管理: user_manual/management/log.md
            - 模块管理: user_manual/management/modoule.md
            - 系统设置: user_manual/management/system_management.md

        - 云主机管理:
            - 创建云主机: user_manual/cloud-server/create_server.md
            - 云主机管理: user_manual/cloud-server/server.md
            - 磁盘管理: user_manual/cloud-server/disk.md
            - 任务: user_manual/cloud-server/task.md
            - 回收站: user_manual/cloud-server/Recycling_station.md
        - 云账单:
            - 账单总览: user_manual/finance-management/bill_overview.md
            - 账单明细: user_manual/finance-management/bill_detail.md
            - 自定义账单: user_manual/finance-management/custom_bill.md
            - 分账设置: user_manual/finance-management/ledger_setting.md 
        - 运营分析:
            - 总览: user_manual/operation-analytics/overview.md
            - 基础资源分析: user_manual/operation-analytics/infrastructure_analysis.md
            - 云主机分析: user_manual/operation-analytics/server_analysis.md
            - 磁盘分析: user_manual/operation-analytics/disk_analysis.md
            - 资源优化: user_manual/operation-analytics/resource optimization.md
     
        - 安全合规:
            - 总览: user_manual/security-compliance/overview.md
            - 扫描检查: user_manual/security-compliance/compliance_scan.md
            - 规则设置: 
                - 合规规则: user_manual/security-compliance/rule_setting.md
                - 规则组: user_manual/security-compliance/rule_group.md
            - 风险条例: user_manual/security-compliance/risk.md
    - 开发文档:
        - 环境准备: dev_manual/dev_manual_prepare.md
        - 新增一个扩展模块: dev_manual/dev_manual_new_module.md
        - 开发指南: dev_manual/dev_manual.md
    - 更新日志: update_log.md
    - 联系我们: contact.md
..........
```

文档内容使用 markdown 语法编写，若要添加新的文档，需要先在 `mkdocs.yml` 文件中的 `nav` 部分增加对应章节导航。

### 本地调试文档

```bash
mkdocs serve
```

执行上述命令后，可通过 `http://127.0.0.1:8000` 地址查看生成的文档内容，当修改文档后，页面内容会自动更新。

### 构建文档

```bash
mkdocs build
```

执行上述命令后，会在 `site` 目录下生成文档站点的静态文件，将目录中的内容复制到任意 HTTP 服务器上即可完成文档的部署。

## 帮助完善文档

### Fork 文档仓库

点击仓库右上角的 `fork` 按钮，复制本仓库到自己的 github 账号。

### 克隆 fork 后的仓库

```bash
git clone https://github.com/your-github-account/docs.git
```

### 本地修改并调试

### Push 修改内容到 GitHub 仓库

### 提交 Pull Request 到本仓库
