# 皇室战争 全部徽章图鉴

一个**离线可用的单文件网页图鉴**，收录《皇室战争》游戏内可查看的**玩家徽章**（Badge）。
所有图像均由游戏内精灵文件重建渲染 —— 双击 `index.html` 即可浏览。

## 特性

| 数量 | 说明 |
| --- | --- |
| **79** | 徽章条目（带独立徽章图的系列徽章，如经典挑战 / 终极挑战 / CRL 等） |
| **387** | 等级图，同级可选、逐级变样式（图示最高等级） |
| **6** | 徽章类型（普通 / 稀有 / 史诗 / 传说 / 冠军 / 归档） |
| **123** | 卡牌精通徽章（Mastery），需「精通框 + 卡牌图标」合成，留待第二阶段 |

> 其中 4 个徽章（`CrazyArenaCompletion`、`CrazyArenaRank`、`TripleDraftLeagueCompletion`、
> `TripleDraftLeagueRank`）数据表声明了 10 级，但当前游戏素材只到第 1~2 级，
> 因此仅收录素材已有的等级，并在卡面与灯箱标注「部分收录」。

交互功能：

- **搜索** —— 名称 / 数据名 / 导出名 / TID / 进度要求模糊匹配
- **筛选** —— 徽章类型下拉、`多等级`（仅看可升级，等级数 ≥ 2）
- **排序** —— 排序值（SortOrder）/ 名称 A→Z / 按类型分组 / 按等级数
- **详情灯箱** —— 大图 + 等级缩略图条（点选切换）+ 等级一览与获得条件，`←` `→` 翻页、`↑` `↓` 换等级、`Esc` 关闭
- **中 / EN 切换** —— 名称与说明取自游戏文案表（`texts.csv` 的 CN / EN 列）

## 快速开始

无需安装、无需联网，直接双击打开 `index.html`。

## 目录结构

```
.
├── index.html              # 自包含单文件页面（数据内联）
└── assets/
    └── badges/             # 343 张 PNG（按导出名命名，170px 高）
```

原始数据表 `badges.csv` 与构建脚本、转存的 `.sc/.sctx` 同 `build/` 目录一并不入库
（见 `.gitignore`），重建时从游戏素材转储重新读取即可。

## 重建

```bash
cd build
python fetch_assets.py     # 从官方 CDN 取 sc/ui_badges.sc → ScDowngrade 转 SC1
python build_assets.py     # 解析 badges.csv + texts.csv，渲染 PNG，产出 badges.json
python build_page.py       # 注入 template.html，产出 ../index.html
python verify_page.py      # 无头 Chrome 验证（卡片数 / 图加载 / 筛选 / 灯箱 / 中英切换）
```

`build/` 内含转存的 `.sc/.sctx`（约 7 MB）与渲染脚本，**不随页面发布**。

## 数据说明

- **徽章图**来自 `sc/ui_badges.sc`（SC2 原件从官方 CDN 取，`ScDowngrade` 转 SC1 后渲染）。
  注意部分转储里的该文件是 0 字节占位，正文在 `.sctx` 中，故 `.sc` 必须走 CDN；
  外部纹理 `.sctx` 必须与 `.sc` **同一版本**，否则纹理图集不匹配会渲染出碎片错图。
- **文案**来自 `csv_client/texts.csv` 的 `EN` 与 `CN` 列，TID 一一对应。
- **数据表比素材新**：`badges.csv` 里少数徽章声明了素材中不存在的等级导出
  （见上文 4 个「部分收录」徽章）。这类缺图不静默丢弃——`build_assets.py`
  只保留能渲染的等级并在数据里写入 `missTxt`，页面据此提示；只有整只都缺图才会剔除。
- **未收录**：123 个卡牌精通徽章（`LevelsTemplate=TemplateMastery10`）没有独立徽章图，
  游戏内由「通用精通框 + 卡牌图标（按 `CustomImageOffsetX/Y`、`CustomImageScalePercent` 摆放）」合成。

## 免责声明

本项目为**非官方粉丝作品**。所有徽章图像的版权归 **Supercell** 所有，
仅用于学习与展示，不作商业用途。本项目与 Supercell 无任何关联，亦未获其授权或认可。
