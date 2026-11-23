# FoxesLauncher-Notices

FoxesLauncher 的**签名公告与动态配置**发布仓库。

本仓库只保存发布产物，客户端通过固定标签 `notices` 的 Release 读取内容；公告与更新 Release 完全解耦。

## Release 资产

固定标签：`notices`

| 资产 | 说明 |
| --- | --- |
| `notices.json` | 签名公告信封，客户端验签后展示 |
| `config.json` | 签名动态配置信封（联机等设置） |
| `notices.payload.json` | 未签名公告原文，便于审计 |
| `config.payload.json` | 未签名配置原文，便于审计 |

信封格式：

```json
{
  "payload": "base64(原始 JSON 字节)",
  "signature": {
    "alg": "RS256",
    "keyId": "fxl-2026-01",
    "value": "base64(签名)"
  }
}
```

客户端内置公钥，验签失败的内容会被直接丢弃。

## 客户端地址

GitHub：

```text
https://github.com/FoxesLauncher/FoxesLauncher-Notices/releases/download/notices/notices.json
https://github.com/FoxesLauncher/FoxesLauncher-Notices/releases/download/notices/config.json
```

GitCode：

```text
https://gitcode.com/FoxesLauncher/FoxesLauncher-Notices/releases/download/notices/notices.json
https://gitcode.com/FoxesLauncher/FoxesLauncher-Notices/releases/download/notices/config.json
```

中国大陆 / 中国香港、中国澳门、中国台湾优先使用 GitCode，其他区域优先使用 GitHub。

## 发布

发布使用 FoxesLauncher 主仓库外部的 `release` 工具，不在本仓库直接编辑产物：

```powershell
cd E:\Dev\FoxesLauncher\release
.\scripts\publish-notices.ps1
```

- `-DryRun`：只签名，不发布
- `-NoticesOnly` / `-ConfigOnly`：只更新公告或只更新配置
- `-SkipGitHub` / `-SkipGitCode` / `-SkipLegacy`：跳过指定发布目标

迁移期会把同一份签名产物同步到主仓库的 latest Release，供旧版本客户端继续读取。

## 注意

- 仓库必须保持 **public**，客户端匿名访问。
- GitCode 仓库必须是**普通仓库**，不能是镜像仓库，否则无法创建 Release。
- 不要手工覆盖 Release 资产；固定标签下重名资产由发布脚本先删除再上传。
