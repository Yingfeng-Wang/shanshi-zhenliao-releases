# 善识营养诊疗 · 更新发布仓库

本仓库**仅**存放 Release 产物（安装包、更新清单），**不含任何源码**。源码在私有仓库。

| 资产 | 用途 |
| --- | --- |
| `latest.json` | 更新清单：版本号、更新说明、安装包地址、minisign 签名。客户端匿名拉取 `releases/latest/download/latest.json` |
| `ShanshiNutritionZhenliao_1.0.1_x64_zh-CN.msi` | Windows 安装包（资产名须为纯 ASCII —— GitHub 会整段剔除中文等非 ASCII 字符） |
| `ShanshiNutritionZhenliao_1.0.1_x64_zh-CN.msi.sig` | 安装包的 minisign 签名。客户端下载后在本地验签，**验签不过即拒绝安装** |

## 为什么公开是安全的

安装包、更新清单、签名三者都属于**可公开的验证材料**，不是机密：

- `.sig` 是 ed25519（minisign）签名，**只能用于验证，无法反推私钥**；
- 客户端内置公钥：安装包被改动任何一个字节都会验签失败，安装被拒；
- `latest.json` 即使被人篡改，最坏结果也只是「更新不触发 / 更新失败」——其中的 `signature` 必须与所下载安装包的字节完全匹配，伪造不出来。

真正的机密是**签名私钥**，只保存在构建机上，从不入库、不上传。

## 下载后校验（可选）

安装包未做 Authenticode 代码签名，Windows 会提示「未知发布者」。如需确认文件完整：

```
certutil -hashfile ShanshiNutritionZhenliao_1.0.1_x64_zh-CN.msi SHA256
```

与对应 Release 正文列出的 SHA256 比对即可。
