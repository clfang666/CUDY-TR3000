# CUDY TR3000 固件自动构建

[![OpenWrt Builder](https://github.com/clfang666/CUDY-TR3000/actions/workflows/openwrt-builder.yml/badge.svg)](https://github.com/clfang666/CUDY-TR3000/actions/workflows/openwrt-builder.yml)
[![Downloads](https://img.shields.io/github/downloads/clfang666/CUDY-TR3000/total)](https://github.com/clfang666/CUDY-TR3000/releases)

本仓库将原来的 [`tr3000`](https://github.com/clfang666/tr3000) 和 [`tr3000-open`](https://github.com/clfang666/tr3000-open) 合并为一套统一构建流程。每次构建同时生成基础版和 OpenClash 版固件，不再需要维护两套重复配置。

固件基于 [`padavanonly/immortalwrt-mt798x-6.6`](https://github.com/padavanonly/immortalwrt-mt798x-6.6) 的 `openwrt-24.10-6.6` 分支，通过 GitHub Actions 自动编译。

## 固件版本

| 版本 | 文件名后缀 | 说明 |
| --- | --- | --- |
| 基础版 | `-standard` | 不内置 OpenClash，依赖更少，适合不需要 OpenClash 的用户 |
| OpenClash 版 | `-openclash` | 内置 `luci-app-openclash` 及构建时自动解析的相关依赖 |

## 设备目标

| 配置文件 | 构建目标 |
| --- | --- |
| `TR3000V1_256M.config` | `cudy_tr3000-v1-256mb` |
| `TR3000V1_MOD.config` | `cudy_tr3000-v1-ubootmod` |

每次完整运行会生成四组固件：两个设备目标分别对应 `standard` 和 `openclash` 两种版本。

## 下载与识别

编译完成的固件会发布到 [Releases](https://github.com/clfang666/CUDY-TR3000/releases)。选择文件时同时核对：

1. 设备目标是 `256mb` 还是 `ubootmod`。
2. 需要基础版 `standard` 还是 OpenClash 版 `openclash`。
3. 文件名包含 `sysupgrade`，且与当前设备分区布局匹配。

> [!WARNING]
> 第三方固件存在变砖风险。刷写前请确认设备硬件版本、闪存/分区布局和当前 U-Boot 类型，并备份原厂固件及配置。不要在 `256mb` 与 `ubootmod` 目标之间盲目混刷。

## 自动构建

以下情况会触发构建：

- 在 Actions 页面手动运行 `OpenWrt Builder`。
- 上游源码更新检查发现新提交。
- `main` 分支中的设备配置、DIY 脚本或构建工作流发生变化。

构建流程使用一个四项矩阵并行生成固件，随后由单独的发布任务统一创建 Release，从而避免两个版本的同名文件互相覆盖。仓库默认保留最近三个 Release。

## 项目来源

- 构建模板：[P3TERX/Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt)
- 固件源码：[padavanonly/immortalwrt-mt798x-6.6](https://github.com/padavanonly/immortalwrt-mt798x-6.6)
- 基础版历史：[clfang666/tr3000](https://github.com/clfang666/tr3000)
- OpenClash 版历史：[clfang666/tr3000-open](https://github.com/clfang666/tr3000-open)

## License

[MIT](LICENSE)
