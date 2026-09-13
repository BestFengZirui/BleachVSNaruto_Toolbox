BVN工作箱
BVN ToolBoxs — BVN游戏关卡、路线、角色选角可视化编辑器，纯前端单HTML静态工具，无需后端，浏览器直接运行，可部署在 GitHub Pages。
✨ 项目介绍

BVN工作箱是一款面向BVN模组开发者的可视化编辑工具，全部功能封装在单个 index.html 文件内。

• 🎯 关卡编辑器：编辑missions关卡JSON，配置地图、时间、敌人波次、BOSS血量，可视化波次预览，支持批量修改、撤销

• 🗺️ 路线图编辑器：编辑way/parts解锁路线数据，DAG路线树可视化，校验节点数据一致性

• 👤 游戏数据编辑器：维护角色/辅助/地图字典，编辑ID、名称、资源路径、语录、BGM配置

• 🎮 选角界面编辑器：可视化编辑选角XML配置，拖拽式网格编辑，布局预览，变体角色配置

• 💾 全部在浏览器本地运行，所有文件读写都是本地，数据不会上传服务器

• 📱 完整响应式：支持桌面端 + 手机移动端自适应布局

• ⌨️ 快捷键支持：Ctrl+S保存、Ctrl+O打开文件、Ctrl+Z撤销

🚀 在线预览(GitHub Pages部署)
将本仓库开启GitHub Pages，访问你的pages地址即可直接使用
示例地址：
https://你的用户名.github.io/bvn-workbox/index.html
⚠️注意：GitHub Pages只是静态网页托管，文件全部保存在你的浏览器本地，网页不会读写你的github仓库文件。编辑完成后点击保存下载JSON/XML到本地电脑。
📂 文件结构
bvn-workbox/
├── index.html      # 主程序，全部代码在此单文件
├── 128x128.png     # 网页左上角程序图标（必须，缺失会图标不显示）
└── README.md       # 本说明文档
项目为单HTML应用，不需要js/css分离，不需要构建，无依赖。
🛠️ 本地使用方法

1. 把 index.html 和 128x128.png 放在同一个文件夹

2. 直接双击 index.html 使用；

3. 或者使用浏览器打开该html文件。
本地使用完全不需要服务器，直接打开本地文件即可运行全部功能。
📦 GitHub Pages部署步骤

1. 在GitHub新建公开仓库，仓库名建议：bvn‑workbox

2. 将两个文件上传到仓库根目录：

◦ index.html

◦ 128x128.png

3. 打开仓库设置：Settings → Pages

4. Source选择：Deploy from a branch

5. Branch选择 main，文件夹选择 / (root)，保存。

6. 等待几分钟，生成你的Pages访问链接，直接浏览器打开链接即可使用编辑器。
重要提醒：
GitHub Pages只是运行网页，不能直接修改仓库里的json/xml！
你所有编辑操作完成后，点击编辑器内【保存】按钮，下载文件到你的电脑，如果你想要更新仓库数据，手动上传下载后的文件到github仓库。
📝 使用说明

1. 打开网页，顶部切换模块：关卡编辑器 / 游戏数据 / 选角界面

2. 点击【📂加载】，从本地硬盘选择你的json/xml配置文件

3. 在可视化界面修改你的配置

4. 修改完成，点击【💾保存】，文件会下载到本地磁盘

5. 内置演示数据：点击🎲示例可以直接加载演示内容体验全部功能

6. 支持撤销操作 ↩撤销 / Ctrl+Z

支持导入导出文件类型

• 关卡模式：missions.json

• 路线模式：包含way、parts字段的json

• 游戏数据：fighter.json / assist.json / map.json

• 选角配置：select.txt / select.xml

⚠️ 注意事项

1. 所有数据仅在浏览器内存，刷新页面会丢失未导出修改，务必随时保存下载文件。

2. 网页不会自动保存到github，编辑后必须手动下载文件，需要更新仓库就手动上传下载好的文件。

3. 图标文件128x128.png不可缺失，缺失会顶部标题图标显示异常。

4. 不支持大文件无限撤销，撤销栈有内存上限。

5. 导出zip打包功能使用浏览器原生web api，老旧浏览器可能不支持。

📄 开源协议

MIT License
Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

💡 开发说明

• 整个项目仅原生HTML、CSS、JavaScript，无第三方框架，无npm，无需编译构建

• 修改源码直接编辑index.html即可

• 兼容现代浏览器（Chrome、Edge、Firefox），不兼容IE
