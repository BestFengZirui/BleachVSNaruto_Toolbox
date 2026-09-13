# BVN 工作箱（BVN Toolboxs）

> 一个单文件、纯前端的 **BVN 游戏数据编辑器**。用于编辑《死神 VS 火影》类同人游戏的**关卡数据、角色/辅助/地图数据、选角界面配置**，无需安装、无需后端，双击打开浏览器即用。

![tech](https://img.shields.io/badge/tech-HTML%2FCSS%2FJS-4cc9f0) ![style](https://img.shields.io/badge/style-Pixel%20Retro-e94560) ![size](https://img.shields.io/badge/single--file-true-2ecc71)

---

## 目录

- [项目简介](#项目简介)
- [功能特性](#功能特性)
- [快速开始](#快速开始)
- [模块说明](#模块说明)
  - [模块一：关卡 / 路线图编辑器](#模块一关卡--路线图编辑器)
  - [模块二：游戏数据编辑器](#模块二游戏数据编辑器)
  - [模块三：选角界面编辑器](#模块三角色选角界面编辑器)
- [数据格式说明](#数据格式说明)
  - [关卡数据 missions.json](#关卡数据-missionsjson)
  - [路线图数据 route.json](#路线图数据-routejson)
  - [游戏数据 fighter.json / assist.json / map.json](#游戏数据-fighterjson--assistjson--mapjson)
  - [选角界面 select_config.xml](#选角界面-select_configxml)
- [快捷键](#快捷键)
- [界面与交互](#界面与交互)
- [技术架构](#技术架构)
- [文件结构](#文件结构)
- [浏览器兼容性](#浏览器兼容性)
- [常见问题（FAQ）](#常见问题faq)
- [二次开发](#二次开发)

---

## 项目简介

**BVN 工作箱**是一个将游戏配置编辑工具化、可视化的单文件 Web 应用。整个应用只有一个 `index.html`，内部以「模块注册表」的方式组织三个彼此独立的功能模块，并共享一套复古像素风 UI（霓虹配色、扫描线背景、像素字体），同时适配桌面端与手机端布局。

设计目标：

- **零依赖**：单 HTML 文件，无构建步骤、无外部 JS 库，离线可运行。
- **可视化编辑**：把裸 JSON / XML 变成树形列表 + 表单编辑，降低改配置的出错率。
- **数据安全**：内置撤销栈、未保存离开拦截、导入格式校验，避免误操作丢数据。
- **可复用**：内置角色/地图字典（Dict），编辑时通过搜索选择器填写 ID，不靠手记。

---

## 功能特性

| 特性 | 说明 |
| --- | --- |
| 三合一编辑 | 关卡/路线图、角色/辅助/地图数据、选角界面 XML，一个工具全搞定 |
| 单文件部署 | 仅 `index.html`，拷贝即用，可离线运行 |
| 可视化 + 源码双模式 | 每个模块都支持「表单可视化编辑」与「原始 JSON / XML 源码编辑」 |
| 撤销 / 重做 | 全局撤销栈（上限 2MB、100 条、800ms 合并），`Ctrl+Z` 秒回退 |
| 快捷键 | `Ctrl+S` 保存、`Ctrl+O` 加载、`Ctrl+Z` 撤销、`Esc` 关闭弹窗 |
| 字典系统 | 内置角色、辅助、地图 ID 字典，选择器搜索选取，支持导入/导出自定义字典 |
| 批量调整 | 关卡编辑器支持对多个条目批量改参数 |
| 一键导出 | 单文件直接下载；多文件自动打包为 `.zip`（原生 JS 生成，无第三方库） |
| 桌面 / 手机双布局 | 自动响应式 + 顶栏手动切换，含横屏方向锁定提示 |
| 数据保护 | 未保存修改时离开页面会弹出确认；模块初始化失败有错误边界与刷新兜底 |
| 内置示例 | 每个模块都内置示例数据（示例关卡、示例路线、示例角色），一键载入学习 |

---

## 快速开始

1. 用浏览器（推荐 Chrome / Edge / Firefox 最新版）打开 `index.html`。
2. 顶部标签页切换三个模块（关卡 / 数据 / 选角）。
3. 每个模块点击 **「📂 加载」** 打开对应的 JSON / XML 文件，或点击 **「🎲 示例」** 载入内置演示数据。
4. 编辑完成后点击 **「💾 保存」** 下载文件。
5. 在手机等窄屏下，可点顶栏 **「📱」** 按钮在桌面/手机布局间手动切换。

> 提示：若窗口过窄且处于桌面布局，会出现引导提示，可拉宽窗口或点图标切换为手机模式。

---

## 模块说明

### 模块一：关卡 / 路线图编辑器

用于编辑两类数据，自动识别：

- **关卡数据（missions）**：一组关卡（`missions` 数组），每关包含地图、时限、敌等级与多波敌人（wave）。
- **路线图数据（route）**：节点顺序（`parts`）+ 解锁规则（`way`），用于描述关卡间的解锁依赖关系。

左侧为列表（支持 🔍 搜索），右侧为编辑面板。工具栏：

| 按钮 | 功能 |
| --- | --- |
| 📂 加载 | 打开 `.json` 文件（自动识别 missions / route 两种格式） |
| 💾 保存 | 以 JSON 下载当前文件 |
| ↩ 撤销 | 撤销上一步操作（`Ctrl+Z`） |
| ▾ 更多 | 原始编辑 / 批量调整 / 导入字典 / 关闭文件 |

**关卡编辑能力**

- 新增 / 删除 / 复制关卡，编辑地图、时限、敌等级。
- 波次管理：新增波次、复制上一波、删除波次、折叠展开。
- 敌人管理：添加 / 删除敌人，设置数量、是否为 BOSS、BOSS 血量。
- 敌人 ID 通过**角色选择器**选取（支持搜索 ID / 名称，可手动输入）。
- 解锁关系（prereq）编辑：单前驱 / 多前驱（全部完成才解锁）。

**路线图编辑能力**

- 四个标签页：基本属性 / 路线规则 / 节点顺序 / 可视化路线图。
- 路线规则（way）：每条规则定义节点 `P` 与其解锁前置 `N`（可为单个节点、多个节点或 `null`）。
- 可视化路线图：以树状 / 节点图形式呈现解锁依赖关系。

---

### 模块二：游戏数据编辑器

用于编辑三种类型的游戏资源数据，顶部标签切换：

| 标签 | 对应文件 | 说明 |
| --- | --- | --- |
| 角色 | `fighter.json` | 角色列表，含素材 URL、台词等 |
| 辅助 | `assist.json` | 辅助角色列表 |
| 地图 | `map.json` | 地图列表 |

工具栏：加载 / 保存 / 撤销 / **「📦 全部导出」**（三种类型一并导出为 zip）/ 关闭当前类型。

**编辑能力**

- 按「死神 / 火影 / 其他」过滤（`comic_type`），支持按 ID 或名称搜索。
- 新增条目、删除条目、编辑素材 URL 字段（文件、头像、大头像、血条头像、胜利头像等）。
- 角色台词（`says`）列表编辑。
- 原始 JSON 编辑（在弹窗中直接改源码后应用）。
- 未保存修改在切换类型时弹窗确认，避免丢失。

---

### 模块三：角色选角界面编辑器

用于编辑选角界面的 **XML 配置**（`select_config.xml`），可视化地编排角色 / 辅助在选角界面中的格子布局。

- 支持 `layout` 布局属性（x / y / width / height / left / right / top / bottom）。
- 角色列表（`char_list`）与辅助列表（`assist_list`）两个分区，每区多行、每行最多 10 个格子。
- 每个格子可设置：角色 ID、偏移量（offset）、扩展角色（moreFighter，逗号分隔多个变体）。
- 支持格子**复制 / 粘贴**（内置剪贴板），快速排布。
- 工具栏提供「{ } 源码编辑」「🔍 文件对比」（与当前 XML 文本对比差异）、「📥 导入字典」。
- 保存时输出为 `select.xml` / 自定义文件名。

---

## 数据格式说明

### 关卡数据 missions.json

```json
{
  "missions": [
    {
      "map": "xianshi",
      "time": 90,
      "enemyLevel": 5,
      "waves": [
        {
          "hold": 2,
          "repeat": false,
          "enemies": [
            { "id": "xb_ninja_1", "count": 4, "isBoss": false, "bossHp": 0 }
          ]
        },
        {
          "hold": 8,
          "repeat": false,
          "enemies": [
            { "id": "aizen_gz", "count": 1, "isBoss": true, "bossHp": 3000 }
          ]
        }
      ]
    }
  ]
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `missions[].map` | string | 地图 ID（取自字典） |
| `missions[].time` | number | 关卡时限（秒） |
| `missions[].enemyLevel` | number | 敌人等级 |
| `missions[].waves[]` | array | 波次列表 |
| `waves[].hold` | number | 出怪间隔 / 波次持续时间 |
| `waves[].repeat` | boolean | 是否循环刷怪 |
| `waves[].enemies[]` | array | 本波敌人列表 |
| `enemies[].id` | string | 敌人角色 ID |
| `enemies[].count` | number | 数量 |
| `enemies[].isBoss` | boolean | 是否 BOSS |
| `enemies[].bossHp` | number | BOSS 血量（非 BOSS 填 0） |

> 兼容：加载单个关卡对象（无 `missions` 包裹）时，会自动包一层 `{ missions: [...] }`。

### 路线图数据 route.json

```json
{
  "id": "map1",
  "name": "示例路线图",
  "way": [
    { "P": "m1", "N": null },
    { "P": "m3", "N": ["m1", "m2"] },
    { "P": "boss", "N": ["m3", "m4"] }
  ],
  "parts": ["m1", "m2", "m3", "m4", "boss"]
}
```

| 字段 | 说明 |
| --- | --- |
| `id` / `name` | 路线图标识与名称 |
| `way` | 解锁规则数组：`P` 为节点 ID，`N` 为前置节点（`null` 表示无需前置，数组表示全部完成后解锁） |
| `parts` | 节点显示顺序列表 |

### 游戏数据 fighter.json / assist.json / map.json

```json
{
  "id": "ichigo",
  "name": "黑崎一护",
  "comic_type": 0,
  "start_frame": 1,
  "urls": {
    "file": "fighter/ichigo.swf",
    "face": "face/ichigo.png",
    "face_big": "face/ichigo_big.png",
    "face_bar": "face/ichigo_bar.png",
    "face_win": "face/ichigo_win.png"
  },
  "says": ["台词一", "台词二"]
}
```

- `comic_type`：0 = 死神，1 = 火影，其他 = 其他。
- 辅助（assist）的 `urls` 一般只含 `file` / `face` / `face_big`。
- 地图（map）通常只有 `id` 与 `name`。

### 选角界面 select_config.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<select_config>
  <stage_setting>
    <layout x="0" y="0" width="800" height="600" left="15" right="15" top="50" bottom="0"/>
  </stage_setting>

  <char_list>
    <row offset="0">
      <item moreFighter="naruto_kurama" offset="0">ichigo</item>
      <item></item>
    </row>
  </char_list>

  <!-- 辅助选择界面 -->
  <assist_list>
    <row offset="0">
      <item>kon</item>
    </row>
  </assist_list>
</select_config>
```

- `layout`：界面舞台布局参数。
- `char_list` / `assist_list`：各含若干 `<row>`（`offset` 行偏移），每行最多 10 个 `<item>`。
- `<item>` 文本为角色 ID，属性 `offset` 为格子偏移，`moreFighter` 为扩展角色（多个用逗号分隔）。

---

## 快捷键

| 快捷键 | 功能 | 说明 |
| --- | --- | --- |
| `Ctrl+S`（Mac：`Cmd+S`） | 保存 | 调用当前模块的保存 |
| `Ctrl+O`（Mac：`Cmd+O`） | 加载 | 调用当前模块的加载 |
| `Ctrl+Z` | 撤销 | 输入框内不触发（避免影响文本编辑） |
| `Esc` | 关闭弹窗 | 弹窗打开时生效，视为取消 |

> 未保存修改时关闭 / 刷新页面，浏览器会弹出确认提示（`beforeunload` 守卫）。

---

## 界面与交互

- **顶栏**：像素风标题（带脉冲动画）、模块标签、桌面/手机切换按钮、当前文件名、模式徽章（关卡/路线/数据/选角）。
- **状态栏**（底部）：当前模块、当前文件、扩展信息、未保存标记（● 已修改，黄色高亮）。
- **全局组件**：`toast` 轻提示、`modal` 弹窗、`pixelConfirm` 像素风确认框、`pixelPrompt` 输入框。
- **下拉菜单**：工具栏「▾ 更多」统一收纳次要操作；点击外部自动收起。
- **响应式**：通过媒体查询自动切换桌面 / 手机布局；手机布局下模块标签转为底部导航，选角模块网格在上、编辑在下；方向锁定遮罩在竖屏使用桌面布局时提示。
- **视觉风格**：深色霓虹像素风，自定义滚动条、像素阴影按钮（按下位移）、扫描线背景。

---

## 技术架构

应用为「全局工具库 + 模块注册表」结构，全部代码在 `index.html` 内：

```
index.html
├── CSS 设计系统         像素风变量（颜色/字体/间距）、按钮、面板、弹窗、响应式
├── 全局工具库
│   ├── $ / $$           选择器封装
│   ├── escHtml / escAttr  XSS 转义
│   ├── deepClone         深拷贝
│   ├── createUndo        撤销栈（2MB / 100 条 / 800ms 合并）
│   ├── openModal / closeModal / pixelConfirm / pixelPrompt / showToast
│   ├── downloadFile      浏览器下载
│   ├── buildZip / downloadFiles   原生 JS 生成 zip（本地文件头 + 中央目录 + EOCD）
│   └── pickFile          文件选择与读取
├── Dict 字典系统         内置角色/辅助/地图字典；搜索映射；导入/导出
├── App 模块注册表        注册模块、切换模块、状态栏刷新、错误边界（模块初始化失败兜底）
├── 模块 1 LevelEditor    关卡 / 路线图编辑器
├── 模块 2 DataEditor     游戏数据编辑器（fighter / assist / map）
├── 模块 3 SelectEditor   选角界面 XML 编辑器
└── 全局事件             快捷键、beforeunload 守卫、视口模式切换
```

**关键设计点**

- **模块化**：每个模块实现 `init / activate / deactivate / getStatus / shortcuts` 接口，注册后自动生成标签与容器；懒初始化（首次切换时才构建 DOM）。
- **错误边界**：模块初始化抛错时，不白屏，展示错误信息与「刷新页面」按钮。
- **数据一致性**：所有编辑直接落在内存对象上，统一走 `updateField / updateWave / updateEnemy` 等入口，保证撤销栈快照一致。
- **安全**：所有动态插入的 ID / 文本均经过 `escHtml / escAttr` 转义，防止 XSS。

---

## 文件结构

```
BVN工作箱/
└── index.html     # 唯一文件：完整应用（含样式、字典、三个模块、工具库）
```

> 运行只需这一个文件。建议保留一份 `editor_dictionary.json` 作为自定义字典备份（可从模块一/三的「导入字典」中导出）。

---

## 浏览器兼容性

- 桌面端：Chrome / Edge / Firefox / Safari 最新版本。
- 移动端：iOS Safari / 安卓 Chrome 竖屏与横屏均可，自动切换布局。
- 需要现代特性：`DOMParser`、`Blob`、`Uint8Array`、`fetch`（文件读取走 `<input type=file>`，无需网络）。

---

## 常见问题（FAQ）

**Q1：打开页面后没有数据，是坏了吗？**
不是。应用是「编辑器」，默认空白；点「🎲 示例」可载入演示数据，或「📂 加载」打开自己的 JSON/XML 文件。

**Q2：误操作改坏了数据，能找回吗？**
可以。用 `Ctrl+Z` 撤销；未保存就离开时会弹确认，选择「取消」留在页面继续编辑。保存前建议先导出原文件备份。

**Q3：敌人 ID 记不住怎么办？**
点敌人条目右侧的选取按钮打开「选择敌人/角色」选择器，支持搜索 ID 或名称；也可在「导入字典」中导入自定义字典扩充候选项。

**Q4：导入文件报「解析失败」？**
请确认文件为 UTF-8 编码的合法 JSON / XML。可先用「原始编辑」查看格式，或对比内置示例数据的结构。

**Q5：保存后文件去哪里了？**
保存走浏览器下载，文件保存在系统「下载」目录；多文件导出为 `BVN_export_日期.zip`。

**Q6：手机横屏没反应？**
在桌面布局下过窄会提示拉宽窗口或切换手机模式；若已切换仍异常，可点顶栏「📱」按钮强制切换。

---

## 二次开发

本工具为单文件应用，改起来非常直接：

1. **改样式**：编辑 `:root` 中的 CSS 变量（颜色、字体、像素边框色）。
2. **加模块**：参考现有三个模块，实现 `{ id, title, init(root), activate?, deactivate?, getStatus?, shortcuts? }` 后调用 `App.registerModule(mod)`。
3. **扩展字典**：在 `Dict` 内置数据中追加条目，或通过界面的「导入字典」导入 `editor_dictionary.json`。
4. **改数据格式**：各模块的解析 / 生成逻辑集中在模块内的 `parseXxx / generateXxx` 函数中（如 `parseXml / generateXml`）。

> 建议：改动前先导出当前字典与数据文件备份；测试时利用各模块内置示例数据验证。

---

## License

本工具为个人同人游戏开发辅助工具，供学习与自用。数据与素材版权归原游戏及原作者所有。
