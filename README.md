# 璐恺十二钗

一部**搞笑校园视觉小说（galgame）**：主角是同学杨璐恺的 12 张表情照，每一张对应一位"钗"——同一个人被切成十二种人格（皮肤）。她们在同一间教室里争宠、整活，你负责做选择、刷好感，最后按好感结算结局。

* 单文件网页：`index.html`（HTML + CSS + 原生 JS，**零依赖、无构建、无 CDN、无外部字体、无音频文件**）
* 双击 `index.html` 即可离线运行（`file://` 下也能存档）
* 移动端、桌面端均可玩
* 基调：纯搞笑、干净、适合发同学群。"后宫"只是玩梗，全篇没有任何露骨内容

> 谨以此，纪念那些被我们笑过、也被我们记住的大学时光。

---

## 一、怎么玩

| 操作 | 桌面端 | 移动端 |
| --- | --- | --- |
| 推进对话 | `空格` / `回车` / 鼠标点击画面 | 点击画面 |
| 跳过打字机（补全整句） | 在正在打字时点一下 / 按一下空格 | 点一下 |
| 选择选项 | `1` `2` `3` `4` 数字键，或直接点击 | 点击选项按钮 |
| 存档 | 右上角「存档」按钮 / `S` 键 | 右上角「存档」按钮 |
| 快进（自动播放） | 右上角「快进」按钮 | 同左（再点一次关闭） |
| 静音 | 右上角 🔊 按钮 / `M` 键 | 右上角按钮 |
| 好感度面板 | 右上角「好感 ♥」/ `A` 键 | 右上角按钮 |
| 关闭弹层 | `Esc` | 点面板外区域 |

* 游戏**每推进一句自动存档**，标题页的「继续游戏」会从上次那一句接着播。
* 点击正在逐字显示的句子时，会**先把整句补全**，再点一下才推进——不会漏字。
* 存档键：`galgame-yanglukai:save:v1`（进度 + 好感 + 已解锁结局 + 静音状态）
* 结局收集键：`galgame-yanglukai:ends:v1`（结局图鉴）

---

## 二、剧本结构

### 2.1 全局规模

| 项目 | 数量 |
| --- | --- |
| 剧本节点总数 | **99** |
| 其中独立台词节点 | **94 句**（另有 5 句"选项提问"，合计 99 句显示文本） |
| 选项节点 | **5 个**（每个 3~4 个按钮，共 17 个按钮） |
| 结局 | **13 个**（12 位钗的专属结局 + 1 个隐藏「十二钗齐聚」后宫结局） |
| 场景背景 | 8 种（教室 / 食堂 / 操场 / 礼堂 / 宿舍 / 表白墙 / 毕业照 / 校门） |
| 通关时长 | 约 5~10 分钟（取决于阅读速度） |

十二钗出场句数：幼年恺 19、生日恺 5、比心恺 3、Wink恺 3、托腮恺 6、拼好恺 4、奖牌恺 4、鬼脸恺 7、军训恺 5、绷绷恺 7、怼脸恺 4、奶龙恺 5，旁白 27。

### 2.2 章节流程

```
标题页
  └─ 序章 · 开学第一天（24 句 + 1 个选项）
        · 幼年恺自称"十二钗里最乖的"
        · 其余十一钗依次登场抢戏，每人一句自我介绍 + 一个梗
        · 选项：先跟谁打招呼（3 个）
  └─ 第一幕 · 排座位（7 句 + 1 个选项）
        · 选项：选谁当同桌（3 个）
  └─ 第二幕 · 中午十二点的第一食堂（8 句 + 1 个选项）
        · 选项：午饭怎么办（4 个）
  └─ 第三幕 · 毕业季（含两次关键选择）
        · 6 句 + 选项「表白墙上写下什么」（4 个）
        · 3 句 + 选项「毕业照站哪边」（3 个）
  └─ 结局结算页（按好感度自动判定）
```

### 2.3 好感度与结局判定

* 开局所有人好感 = **1**（序章你和十二钗都打过招呼了）。
* 每次选择会给 **指定几位钗加好感**（`aff`），或者给 **全部 12 位一起加**（`affAll`）。
* 结算规则（`computeEnding()`）：
  1. **若 12 位的好感全部 ≥ 7**（`HAREM_THRESHOLD`）→ 触发隐藏结局 **《十二钗齐聚》**。这条线要求 5 次选择**全部**选"雨露均沾"选项，走完是 12 人各 8 点。
  2. 否则 → **好感最高的那位钗**获得专属结局。
     并列时按"最后一次涨好感的先后"决胜（`gainSeq`），所以最后一次选择往往最关键。
* 单角色好感上限恰为 8（`AFF_MAX`），心形与进度条按 8 归一化显示。
* 结局页会给出：结局名、结局正文、好感排行（12 行）、达成时间、结局收集进度。

### 2.4 十二钗一览（顺序固定）

| # | 名字 | 梗 | 图片 |
| --- | --- | --- | --- |
| 01 | 幼年恺 | 自称"十二钗里最乖的那个"，其实最能装 | `assets/kai_01.jpg` |
| 02 | 生日恺 | 每天都过生日，日历只有一页 | `assets/kai_02.jpg` |
| 03 | 比心恺 | 连被罚站都在比心 | `assets/kai_03.jpg` |
| 04 | Wink恺 | 说话不带眨眼会卡壳 | `assets/kai_04.jpg` |
| 05 | 托腮恺 | 一托腮就想通了人生 | `assets/kai_05.jpg` |
| 06 | 拼好恺 | 左半边内向、右半边社牛，左右拼接的合体人 | `assets/kai_06.jpg` |
| 07 | 奖牌恺 | 走一步叮当三步，奖牌挂满身 | `assets/kai_07.jpg` |
| 08 | 鬼脸恺 | 毕业照上永远不正经 | `assets/kai_08.jpg` |
| 09 | 军训恺 | 军训结束三年，口令还没结束 | `assets/kai_09.jpg` |
| 10 | 绷绷恺 | 一般不笑，笑起来很吓人 | `assets/kai_10.jpg` |
| 11 | 怼脸恺 | 距离越近，照片越对 | `assets/kai_11.jpg` |
| 12 | 奶龙恺 | 已经成年了，但说话有点奶 | `assets/kai_12.jpg` |

---

## 三、怎么改台词 / 加分支

打开 `index.html`，找到 `/* 3. 剧本数据 */` 下面的 `var SCRIPT = [ ... ]`，全部内容都是**数据**，改字不用碰引擎。

### 3.1 一个节点长这样

```js
{ id:'p03', who:0, bg:'class', text:'哟，新同学？我叫杨璐恺——不过在这间教室里，你得叫我"幼年恺"。' },
```

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `id` | ✅ | 节点唯一 id（字符串），选项靠它跳转 |
| `who` | ✅ | 说话者：`0`~`11` 对应十二钗；`-1` 表示旁白 |
| `text` | ✅ | 台词正文 |
| `bg` | ❌ | 场景 id（见下表）。省略则沿用上一句的场景 |
| `chapter` | ❌ | `true` 时按"章节标题"样式居中放大显示 |
| `next` | ❌ | 下一句的 id。**省略时自动播放下一个数组元素**；写 `'@end'` 表示进入结局结算 |
| `choices` | ❌ | 选项数组，见下 |

场景 id：`class`（教室）、`canteen`（食堂）、`field`（操场）、`stage`（礼堂）、`dorm`（宿舍）、`wall`（表白墙）、`photo`（毕业照）、`gate`（校门）。
想加新场景：在 `var BGS = {...}` 里加一项，并在 CSS 里加一条 `.bg-你的id{background:linear-gradient(...)}`。

### 3.2 选项长这样

```js
{ id:'aChoice', who:0, text:'你要选谁当同桌？', choices:[
  { label:'跟拼好恺坐——左边放书，右边放他', aff:{5:3, 4:1}, next:'aA1' },
  { label:'把课桌拼成一个圈，让十二钗轮流来坐同桌', affAll:1, next:'aC1' }
]},
```

* `label`：按钮文字（建议 ≤ 22 个汉字，窄屏会自动换行）。
* `aff`：`{钗下标: 加多少}`。下标就是上表的 `#-1`，例如 `{5:3}` = 拼好恺 +3。
* `affAll`：给全部 12 位一起加（用来做"雨露均沾"路线）。可以只写 `affAll`，也可以和 `aff` 同时写。
* `next`：选完跳去哪个节点 id。

### 3.3 加一条新分支的步骤

1. 在 `SCRIPT` 里给新节点起 id（例如 `aD1`、`aD2`），把节点写在你希望的**物理位置**（顺序播放是按数组顺序来的）。
2. 在新分支的**最后一句**上写 `next:'xxxx'`，指回主线（否则会顺延到下一个数组元素）。
3. 在对应 `choices` 里加一个按钮，`next` 指向新分支的第一句。
4. 记得给按钮写 `aff` / `affAll`。

> 小技巧：用 `__skip()` + 浏览器控制台可以快速把新分支跑一遍，见第五节。

### 3.4 加一个新结局

在 `var ENDINGS = [ ... ]` 里加一项：

```js
{ id:'kai13', who:-1, name:'《结局名》', text:'结局正文，\n 用 \\n 换行。' },
```

* `who` 是结局页右上角展示的头像下标（`-1` 表示不显示头像）。
* 再把 `computeEnding()` 里挑结局的逻辑改一改即可（比如增加"某两位钗都 > 6 触发三角结局"）。
* 图鉴条目数、`结局收集 X / N` 会自动跟着 `ENDINGS.length` 变，不用改文案。

---

## 四、本地运行

**方式一：直接双击**（推荐，零依赖）

```
双击 index.html
```

`file://` 协议下所有功能都可用（图片、localStorage 存档、WebAudio 音效）。

**方式二：起个静态服务器**（想部署到 GitHub Pages 时用）

```bash
cd 杨璐恺十二钗
python -m http.server 8000
# 浏览器打开 http://localhost:8000/
```

目录里已经放好空的 `.nojekyll`，可直接丢到 GitHub Pages 的仓库根目录（或用 `scut-game-main` 的 hall 页收录）。

**清空存档**：浏览器控制台执行

```js
localStorage.removeItem('galgame-yanglukai:save:v1');
localStorage.removeItem('galgame-yanglukai:ends:v1');
```

---

## 五、技术要点

* **单文件零依赖**：整份 HTML 里只有一个 `<script>` 标签，没有任何 `http(s)://` 外链、没有 `@import`、没有字体文件、没有音频文件。
* **打字机**：`setTimeout` 逐字（26ms/字），点击/空格时若正在打字则**先补全整句**；每 2 个字触发一次 WebAudio 短音（节流，不吵）。
* **背景**：每种场景一个 CSS 渐变，上面叠一张 `assets/campus.jpg`（`opacity:.13`，`object-fit:cover`）做校园底纹。
* **立绘**：`assets/kai_XX.jpg`（512×512），圆角卡片 + 白色描边 + 柔和红色光晕，切换时用 `popIn` 关键帧做轻微弹跳入场（先移除 class、强制回流、再加回来，保证动画重播）。
* **旁白**：不显示单人头像，改为展示 12 张缩略图拼成的"群像卡"。
* **音效**：全部用 WebAudio 现场合成（`OscillatorNode` + `GainNode` 包络），共 5 类——逐字音、点击音、选择音（双音）、存档音、结局音（7 音小旋律）。静音开关会记住在存档里。
* **响应式**：`viewport-fit=cover` + `env(safe-area-inset-*)` 适配刘海屏；字号用 `clamp()` 自适应；桌面端内容区最大 760px 居中。
* **存档**：`localStorage`，**每推进一句自动存**；存档结构里带 `savedAt` 时间戳。`localStorage` 被禁用（隐私模式）时用 `try/catch` 静默降级，不报错、不影响游玩。
* **结局结算**：`computeEnding()` 先判后宫阈值，再取最高好感；并列按"最后涨好感顺序"决胜。
* **无 `requestAnimationFrame`**：全部动画交给 CSS + `setTimeout`，因此在 `--virtual-time-budget` 的无头 Chrome 里也能正常跑测试。

### 5.1 测试钩子（不影响正常玩法）

```js
window.__state()
// => { scene, who, whoIndex, text, fullText, typing, aff:[...12], choices:[...],
//      ended, ending, endingName, savedAt, screen, muted, unlocked, bg, total }
//   scene   : 当前节点 id（如 'p01'）
//   who     : 当前说话者名字（字符串，旁白为 '旁白'）；whoIndex 是对应的 0~11 / -1
//   text    : 当前已显示的文本；fullText 是整句；typing 表示是否正在逐字
//   choices : 当前选项的按钮文字数组（没有选项时为 []）

window.__goto(sceneId)   // 跳到任意节点（用于调试/测试；节点不存在返回 false）
window.__choose(i)       // 选中第 i 个选项（0 开始），会真的改好感度并跳转
window.__skip()          // 若正在打字 → 补全整句并返回 true；否则推进到下一句
window.__auto(true/false)// 开关快进（自动播放）
window.__save()          // 手动存档，返回 savedAt 时间戳
window.__affAll(n)       // 调试用：把 12 位好感全部设为 n
```

> `__skip()` 的语义：**在打字中调用 = 跳过打字机（补全整句）；不在打字中调用 = 推进一句**。
> 所以从一句的开头连续调用两次 `__skip()` 才会跨过该句（第一次补全、第二次推进）。写自动化脚本时按这个节奏循环即可。

### 5.2 用无头 Chrome 自测（开发时用）

本作全部动画都用 CSS + `setTimeout`（没有 `requestAnimationFrame` 依赖），所以在 `--virtual-time-budget` 下也能跑完整流程。
关键点：**强制 `document.hidden = false`、把 `requestAnimationFrame` 垫成 `setTimeout` 的 shim，必须插在游戏 `<script>` 之前**才生效。

```html
<!-- 注入到 <title> 之前 -->
<script>
Object.defineProperty(document,'hidden',{get:function(){return false;}});
window.requestAnimationFrame=function(cb){return setTimeout(cb,16);};
window.onerror=function(m,s,l,c){window.__errs.push(m+'@'+l);};
</script>

<!-- 注入到 </body> 之前 -->
<pre id="probe"></pre>
<script>
(function(){
  document.getElementById('btnStart').click();
  var g=0; while(!window.__state().ended && g++<1500){
    var s=window.__state();
    if(s.choices.length) window.__choose(0); else window.__skip();
  }
  document.getElementById('probe').textContent =
    'PROBE_JSON_START'+JSON.stringify({st:window.__state(), errs:window.__errs})+'PROBE_JSON_END';
})();
</script>
```

```bash
chrome --headless=new --disable-gpu --virtual-time-budget=15000 --window-size=440,900 \
       --dump-dom "file:///…/杨璐恺十二钗/_probe.html" > out.html
# 再用正则从 out.html 里取 <pre id="probe"> 的内容
```

注意事项：
* `file://` 下 `canvas.getImageData` 会被跨源污染 —— 要验证"截图非空白"请用 `--screenshot=out.png` + Pillow 统计（标准差 / 非白像素比例），不要读 canvas 像素。
* 无头 Chrome 在 Windows 上会把窗口宽度夹到约 500px，想量 360/390 这种真窄屏，可以用 `srcdoc` iframe 把整页塞进去再 `contentDocument` 里量。
* `--dump-dom` 抓出来的中文在 Windows 控制台可能乱码，属终端编码问题，写入文件按 UTF-8 读就正常。

---

## 六、文件清单

```
杨璐恺十二钗/
├── index.html              # 单文件游戏（引擎 + 剧本 + 样式）
├── README.md               # 本文件
├── .nojekyll               # 空文件，GitHub Pages 用
└── assets/
    ├── kai_01.jpg ~ kai_12.jpg   # 十二钗立绘（512×512，顺序固定）
    └── campus.jpg                # 背景校园照（低透明度叠底）
```

素材来源：`十二钗素材/01_幼年恺.jpg … 12_奶龙恺.jpg`、`合成杨璐恺/assets/scut-campus.jpg`，仅做重命名与复制，未做二次修改。
