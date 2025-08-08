---
title: 利用高德地图和小红书MCP生成旅行规划图
categories:
  - 前端
  - AI
tags:
  - 前端开发
  - HTML
  - CSS
  - Javascript
  - AI
  - MCP
date: 2025-04-30 10:27:51

cover: /img/blog/fed14.png
description: 利用高德地图和小红书MCP生成旅行规划图
---
![利用高德地图和小红书MCP生成旅行规划图](/img/blog/fed14.png)

最近网上有很多关于MCP实战的例子，我也非常感兴趣，照着网上最火的旅游规划实践例子，也玩了一下，确实非常有趣和方便，下面介绍利用**高德地图**和**小红书**MCP生成旅游规划网页的详细步骤

> 首先感谢**AI产品自由**写的攻略文档[AI + 高德MCP：10分钟搞定一份旅行攻略！](https://mp.weixin.qq.com/s/SM05H7MKnfvoVynmUfuwTA),本篇是在这个文档的基础上，再锦上添花优化一下配置的步骤

## 一、前期准备
- 最新版本的 Node.js（v18 或更新版本）
  - 通过运行检查：node --version
  - 从以下网址安装：https://nodejs.org/
- 最新版本的 Python（v3.8 或更新版本）
  - 通过运行检查：python --version
  - 从以下网址安装：https://python.org/
- UV 包管理器
  - 安装 Python 后，运行：```pip install uv```
  - 通过运行验证：```uv --version```
- 高德地图开放平台
  - 注册并认证成为开发者
  - 进入**控制台**后，找到应用管理，创建新应用
  - 添加两种key，一个是``web服务``类型，一个是``web端``类型
- 科学上网（你懂的）

> macOS系统，装了python后，命令行要加版本号，比如```pip3 install uv```

## 二、安装MCP

市面的主流的编辑器或者插件都能支持MCP，这里我用**vscode**的**cline**插件为例，来说明这个实践例子中，安装MCP的具体步骤

> MCP库网站推荐：[https://mcp.so/](https://mcp.so/)

### 安装高德地图MCP
1. 安装页面：[https://pypi.org/project/amap-mcp-server/](https://pypi.org/project/amap-mcp-server/)
2. 安装命令：```pip install amap-mcp-server``` or ```pip3 install amap-mcp-server```
3. cline插件添加高德MCP：
![高德MCP](/img/blog/amapmcp.png)
> 记得用前期准备建好的Web服务的key
![高德MCP](/img/blog/amapmcp2.png)
```json
{
  "mcpServers": {
      "amap-mcp-server": {
          "command": "uvx",
          "args": [
              "amap-mcp-server"
          ],
          "env": {
              "AMAP_MAPS_API_KEY": "高德Web服务的key"
          }
      }
  }
}
```

### 安装小红书MCP
[小红书MCP介绍](https://mcp.so/zh/server/xhs-mcp/jobsonlook)
1. 安装依赖
```bash
git clone git@github.com:jobsonlook/xhs-mcp.git
cd xhs-mcp
uv sync 
```
> 如果ssh clone有问题，也可以用``git clone https://github.com/jobsonlook/xhs-mcp.git``
2. cline插件添加小红书MCP
```
{
    "mcpServers": {
        "xhs-mcp": {
            "command": "uv",
            "args": [
                "--directory",
                "小红书mcp项目的路径",
                "run",
                "main.py"
            ],
            "env": {
                "XHS_COOKIE": "xxxx"
            }
        }
    }
}
```
小红书mcp项目的路径的获取分系统
- **macOS**: 选中刚才clone下来的小红书的mcp目录，快捷键Option+Command+C，就可以复制当前目录的路径，然后直接粘贴即可
- **windows**：比如你放在G盘的根目录下，那么路径就是``G:\\xhs-mcp``,记录路径分割线用``\\``
3. 获取小红书cookie
- 打开[小红书web端](https://www.xiaohongshu.com/explore)
- 登陆
![小红书web端](/img/blog/xhs.png)
- 登陆成功后，按F12，或鼠标右键检查，打开控制台，然后刷新一下页面
- 选择Network选项，然后筛选处选Fetch/XHR,任意选择一个请求，点击后查看里面的Cookies，复制到配置中的``XHS_COOKIE``

![小红书Cookie获取](/img/blog/xhs2.png)

## 检测MCP是否安装成功

直接cline提问即可，比如你直接提问，看看是否能成功调用mcp服务即可

```bash
根据小红书mcp，获取关于海贼王的最热门的前5个帖子
根据高德地图mcp，获取广州塔的具体经纬度
```

同时mcp服务如果运行正常，插件也会有类似的绿色提示
![MCP](/img/blog/mcp3.png)


## 三、生成规划旅行网页
> 参考AI产品自由写的提示词：[旅行规划提示词](https://o90p05z3t4.feishu.cn/wiki/Lfmuwq3jjiSozQkglU6cFEOAnjc)，我在原有提示词上做了更新

### 新建提示词文档
在项目根目录新建``提示词.md``的markdown文件，粘贴以下内容
```markdown
# 风格说明

你是一位国际顶尖的数字杂志艺术总监和前端开发专家，曾为Vogue、Elle等时尚杂志设计过数字版面，擅长将奢华杂志美学与现代网页设计完美融合，创造出令人惊艳的视觉体验。

请设计高级时尚杂志风格的手机app旅行攻略，将旅行信息以精致奢华的杂志编排呈现，让用户感受到如同翻阅高端杂志般的视觉享受。

**可选设计风格：**

构成主义风格 (Constructivism)
采用构成主义风格设计，体现20世纪早期俄国前卫艺术运动的革命性美学。必须使用大胆的几何形状和对角线元素创造动态张力，强调结构与构成。色彩应限制在红、黑两色为主，可辅以少量白色或灰色，体现革命色彩。排版是关键元素，文字应成为设计的一部分而非简单的内容载体，可使用不同大小、粗细和方向的文字创造视觉层次，标题应大胆且具冲击力，可斜向排列或分割成多行。必须使用几何形状如三角形、圆形、直线和对角线作为主要视觉元素，这些元素应相互重叠和交织。照片或图像应使用锐利的对比度和几何化处理，可使用照片蒙太奇技术。整体构图应不对称且充满张力，仿佛元素间存在力的相互作用。可添加工业元素如齿轮、工厂或机械部件的抽象表现。整体设计应呈现出前卫、动态且具有政治宣传性质的视觉效果，参考亚历山大·罗德琴科(Alexander Rodchenko)和埃尔·利西茨基(El Lissitzky)的海报设计，体现"艺术进入生活"的设计理念。

**每种风格都应包含以下元素，但视觉表现各不相同：**

1. **行程标题区**：
   - 目的地名称（主标题，醒目位置）
   - 旅行日期和总天数
   - 旅行者姓名/团队名称（可选）
   - 天气信息摘要

2. **行程概览区**：
   - 按日期分区的行程简表
   - 每天主要活动/景点的概览
   - 使用图标标识不同类型的活动

3. **详细时间表区**：
   - 以表格或时间轴形式呈现详细行程
   - 包含时间、地点、活动描述
   - 每个景点的停留时间
   - 标注门票价格和必要预订信息

4. **交通信息区**：
   - 主要交通换乘点及方式
   - 地铁/公交线路和站点信息
   - 预计交通时间
   - 使用箭头或连线表示行程路线

5. **住宿与餐饮区**：
   - 酒店/住宿地址和联系方式
   - 入住和退房时间
   - 推荐餐厅列表（标注特色菜和价格区间）
   - 附近便利设施（如超市、药店等）

6. **实用信息区**：
   - 紧急联系电话
   - 重要提示和注意事项
   - 预算摘要
   - 行李清单提醒

# 示例内容（基于上海一日游）

**目的地**：上海一日游
**日期**：2025年3月30日（星期日）
**天气**：阴，13°C/7°C，东风1-3级

**时间表**：
| 时间 | 活动 | 地点 | 备注 |
|------|------|------|------|
| 09:00-11:00 | 游览豫园 | 福佑路168号 | 门票：40元 |
| 11:00-12:30 | 城隍庙午餐 | 城隍庙商圈 | 推荐：南翔小笼包 |
| 13:30-15:00 | 参观东方明珠 | 世纪大道1号 | 门票：80元起 |
| 15:30-17:30 | 漫步陆家嘴 | 陆家嘴金融区 | 免费活动 |
| 18:30-21:00 | 黄浦江夜游 | 码头位置 | 夜游票：120元 |

**交通路线**：
- 豫园→东方明珠：地铁14号线（豫园站→陆家嘴站），约25分钟
- 东方明珠→黄浦江夜游码头：步行15分钟

**实用提示**：
- 携带雨伞，天气多变
- 避开东方明珠午间高峰
- 准备移动支付
- 注意保管随身物品

**重要电话**：
- 旅游咨询：021-12301
- 紧急求助：110/120

**小红书攻略推荐**
- 搜索和旅行规划主题相关的观看人数最多的5篇小红书文章，并列举出来

请创建一个美观且易于在手机上浏览的旅行规划长图，帮助用户清晰了解整个行程安排。长图设计应确保在手机上阅读舒适，信息层次分明。

# 技术规范：

* 使用HTML5、Font Awesome、Tailwind CSS和必要的JavaScript
  * Font Awesome: [https://lf6-cdn-tos.bytecdntp.com/cdn/expire-100-M/font-awesome/6.0.0/css/all.min.css](https://lf6-cdn-tos.bytecdntp.com/cdn/expire-100-M/font-awesome/6.0.0/css/all.min.css)
  * Tailwind CSS: [https://lf3-cdn-tos.bytecdntp.com/cdn/expire-1-M/tailwindcss/2.2.19/tailwind.min.css](https://lf3-cdn-tos.bytecdntp.com/cdn/expire-1-M/tailwindcss/2.2.19/tailwind.min.css)
  * 中文字体: [https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;500;600;700&family=Noto+Sans+SC:wght@300;400;500;700&display=swap](https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;500;600;700&family=Noto+Sans+SC:wght@300;400;500;700&display=swap)
* 确保代码简洁高效，注重性能和可维护性
* 使用CSS变量管理颜色和间距，便于风格统一

# 输出要求：

* 提供一个完整的HTML文件，包含所有设计风格的卡片
* 代码应当优雅且符合最佳实践，CSS应体现出对细节的极致追求
* 网页应使用响应式布局，在web端和移动端都有良好的视觉效果
* 行程的规划需要根据用户提供的时间的对应的天气信息进行规划，比如下雨天就尽量安排室内活动
* 每一个地点需要利用高德地图的MCP生成地点链接，点击后可以直接跳转到高德地图导航
* 每一天的时间行程需要增加一个高德地图的在线预览，将当前天的所有行程地图连接起来，方便查看今日行程，利用高德地图的js api，直接在网页上显示，不需要跳转
* 小红书的文章要给出具体帖子的链接，不需要搜索的链接
* 设计的宽度根据手机宽度自适应
* 永远用中文输出
* 注意文字的可阅读性，保持文字背景干净和字体颜色不一致
* 保证信息的完整性

请以国际顶尖杂志艺术总监的眼光和审美标准，创造风格迥异但同样令人惊艳的手机app旅行攻略，让用户感受到"这不是普通的信息卡片，而是一件可收藏的数字艺术品"。
```
### 生成页面
在编辑器中输入类似内容，让AI生成页面
![提示词示例](/img/blog/mcp.webp)
然后就能见证奇迹的发生
![Magic！！](/img/blog/qiji.jpeg)
### 美化页面
1. 再次感谢大佬整理的风格提示词：[旅行规划设计主题提示词](https://o90p05z3t4.feishu.cn/wiki/LBSDwq4z1iTUWqkEjoNcFGHvnsc)，可以选择喜欢的风格，然后粘贴到提示词文档里面的**可选设计风格**部分
2. 高德地图的js api，在页面生成后，如果发现在线地图预览有问题，则需要修改地图的js引入
> 地图接口文档：[JS API 2.0](https://lbs.amap.com/api/javascript-api-v2/guide/abc/jscode)

就用你刚开始高德控制后台新建的**Web端**的key，取key和安全密钥分别填到一下代码中，然后再继续地图的相关初始化代码
![高德地图JS API初始化](/img/blog/amap.png)
```html
<script type="text/javascript">
  window._AMapSecurityConfig = {
    securityJsCode: "「你申请的安全密钥」",
  };
</script>
<script
  type="text/javascript"
  src="https://webapi.amap.com/maps?v=2.0&key=你申请的key值"
></script>
```
例如我生成的页面中，最下面的代码就是地图的js初始化
![高德地图JS API初始化](/img/blog/amap2.png)

## 四、完成规划

最后再根据生成好的页面，进行相应的行程调整和规划，然后将生成好的html文件，丢到任意可以在线预览网页的服务器即可，恭喜你完成了你的旅行规划！！