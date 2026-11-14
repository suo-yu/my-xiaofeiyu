# 皮肤目录

五套都是「小肥鱼」的不同穿搭，共用同一套骨骼与交互逻辑。切换入口：右键 →「穿什么呢」，选择会写进 `data/userdata/settings.json`。

| id | 显示名 | 画布 | 层数 | 形态 | 尾巴 | 动作菜单 |
|---|---|---|---|---|---|---|
| `feiyu-01` | 今天元气一点 | 1024² | 19 | 半身前倾，手被画布裁断 | — | 无（只有窗口操作） |
| `feiyu-02` | 今天少女一点 | 1280² | 21 | 全身站姿，独立腿 + 鞋 | ✅ 鲸尾 | 摇尾巴 / 摸摸尾巴 / 摸摸头 / 跳一下 / 眨个眼 / 说句话 |
| `feiyu-03` | 今天Q弹一点 | 1280² | 19 | Q 版半身，双手分开 | ✅ 鲸尾 | 同上 + 果冻挤压拉伸 |
| `feiyu-04` | 今天穿暖和点 | 1280² | 20 | 全身近景，冬装毛领兜帽、双手揣在一起、鲸尾 | ✅ 静态鲸尾 | 摸摸头 / 戳一下 / 跳一下 / 眨个眼 / 说句话 |
| `feiyu-05` | 今天硬气一点 | 1280² | 18 | Q 版全身，扣着汤盆、独立腿 + 鞋 | — | 摸摸头 / 戳一下 / 跳一下 / 眨个眼 + 果冻挤压拉伸 |

## 每套的特性

### feiyu-01 · 今天元气一点
- 鱼鳍耳、女仆发箍，脸比较大，适合近景
- **闭眼方案**：这版 `eyelash` 图层里原本夹着下眼睑线，闭眼时会跟着下移，已在图层里把那段清掉（原始文件备份在 `skins/_archive/feiyu-01/`）；
  内眼角还有一小段多余睫毛/眼眶，也一并清掉了（`_archive/feiyu-01/17_eyelash-r.before-inner-fragment.png`）
- **嘴部**：w 型猫嘴。张嘴时先整层收掉 w 闭嘴边（`mouth.hideBase`），只留 `art/11_mouth_open.png`
  这张完整的张开嘴；上缘按最新红线截平（左边再压低了一点）、轮廓按红线重画（左侧鼓、底边更平、右壁更直），闭嘴时 w 再淡回来。
  张嘴素材整体左上微调 `(-3, -4)` 并顺时针转 `15°`（`openOffset` / `openRotation`）；角度是按视觉调的，上唇位置会跟着转。
  只动嘴，不做脸型/脸颊收缩，头发和下巴都不动
- 没有尾巴层，所以菜单里不出现尾巴动作
- 右键菜单有「张个嘴」，可以单独看一遍张嘴动作

### feiyu-02 · 今天少女一点
- 全身站姿，裙摆、腿、鞋分层，尾巴独立
- 头部位移收得很小（6px），眼睛小，眨眼位移 12px
- **嘴部**：原图是张开的笑，`mouth.rest = open`；说话/「张个嘴」时把嘴层纵向压扁到 `squashMin` 来闭嘴，静息仍是原来的笑。
  压到最闭时再逆时针转 `10°`（`squashRotation: -10`）并往下挪 `3px`（`squashOffset: [0, 3]`），压出来的闭嘴线才不会歪
- 双手是一整层（不能单独挥手）

### feiyu-03 · 今天Q弹一点
- Q 版，头大身小，最适合挤压拉伸
- `jelly: true`：点击 / 拖拽 / 摸尾巴都会压扁再回弹
- **嘴部**：原图是闭合的 w 线，`mouth.rest = closed`；说话时淡入 `art/10_mouth_open.png` 的嘴洞，
  嘴洞整体逆时针转 `5°`（`openRotation: -5`），用 `10_mouth_open_cover.png` 把原 w 线擦掉。两张图都由 `tools/mouth-art.py` 生成
- 双手左右分开（以后可以做挥手）
- **发卡**：分层导出时漏了，已从原图抠回来补成 `art/20_headwear-frill.png`，作为最上层随头部移动；导出包见 `artifacts/feiyu-03-fixed.zip`

### feiyu-04 · 今天穿暖和点
- 冬装全身：毛领厚外套 + 白绒兜帽，双手揣在胸前，裙摆上有鲸鱼围裙，右侧鲸尾
- 眼睛比前几套大，眨眼靠睫毛下移 24 / 30px 闭合（左右眼高度不同，所以两边位移不一样）
- 嘴是张开的，`mouthTalk` 打开
- **嘴部**：原图是张开的「o」嘴，`mouth.rest = open`；说话时把嘴层纵向压扁来闭嘴，静息仍是原图；
  压到最闭时顺时针转 `5°`（`squashRotation: 5`）
- **下半身是补出来的**：原始导出只分离到腰部（`layers/` 里没有裙摆、鲸尾、左袖），下半身是从同人设的完整原图（`_sources/feiyu-04/original-full.jpg`）里抠回来补的：
  - `art/20_body.png`：y>462 的静态下半身（外套、双袖、围裙、裙摆），从原图按「轮廓内/外」分离背景白，头发丝之间的白色空腔用邻域颜色填掉，左缘和右侧裁切边做了渐隐
  - `art/21_tail.png`：只取鲸尾原来缺失的部分，压在 `hair.back` 后面，所以发丝仍然压在尾巴上
  - 代价：下半身不参与头/发的骨骼动画（它本来就是静态的），头部位移在 y=462 的接缝处几乎为 0，看不出来
- **工程文件已回写**：`_sources/feiyu-04/layers/` 现在是改后的 21 张（替换了 `02_back_hair` / `07_headwear` / `08_face`，补了 `20_body` / `21_tail`，`01_reference` 重做成最终合成）；`live2dlayer_project.psd` 也按同样层序重建（21 层，`reference` 仍隐藏），直接拿出去做 Live2D 就是改后的样子
  - 重建后的 PSD 经逐层合成与运行时包比对，像素差 0
  - 原始导出没丢：`layers.original/`（19 张原始 PNG）+ `live2dlayer_project.original.psd`
  - 注意：PSD 是用 psd-tools 脚本重建的，只保证像素/层名/层序/显隐一致；live2dlayer 插件的附加元数据和 `depth_maps/` 没有跟着更新，新加的两层没有深度图
  - 重新用 `tools/import-skin.mjs` 导入这个工程时，`tail` 层会被识别成可摆动的 `tail` role（会跟发丝裂开），记得手改成 `tail.static`
- **导出裁切**：原图本身就是贴着画布左边裁过的，`02_back_hair` 左边是一条直边。已把左缘 110px 做成渐变透明，避免桌面深色时出现一堵墙
- **碎片清理**：`07_headwear` 和 `08_face` 里各有一块飘在画面左边的碎片（约 x150–300, y280–450），已清掉
- 尾巴是静态的，所以菜单里不出现「摇尾巴 / 摸摸尾巴」

### feiyu-05 · 今天硬气一点
- Q 版全身：汤盆扣在头上当头盔，鲸鱼围裙、独立腿 + 鞋，站姿
- 这套导出是**完整**的（有 `legwear`、左右手分开），没有补图，锚点按脸重新量的
- **层序修正**：原工程里 `09_headwear`（汤盆）和 `20_objects`（同一个盆的重复层）都排在头发下面，直接合成会把头发画到盆上。现在把盆移到头部组最末（压在头发上），并删掉重复的 `20_objects`（原始文件备份在 `_sources/feiyu-05/layers.original/`）
- 眼睛很大（82px），眨眼靠睫毛下移 55px 闭合；**嘴部**：原图是闭合的小点，`mouth.rest = closed`，
  说话时淡入 `art/12_mouth_open.png` 的小圆嘴，`12_mouth_open_cover.png` 负责擦掉原点（由 `tools/mouth-art.py` 生成）
- `jelly: true`：点击 / 拖拽会压扁回弹，和 Q 弹那套一样
- **钢盆音效**：头上那圈单独划成 `pot` 区域（`interaction.regions` 第一项），点一下就响 `sfx/pot-hit.wav`（合成的铁盆「邦」，0.62s），和摸摸动画同时；蹭摸不响，只有点一下才响

## 嘴部动作对照

| id | `mouth.rest` | 张嘴方式 | 额外素材 |
|---|---|---|---|
| feiyu-01 | closed | 收起 w 闭合线（`hideBase`），只留张开的嘴 | `art/11_mouth_open.png` |
| feiyu-02 | open | 嘴层纵向压扁来闭嘴 | — |
| feiyu-03 | closed | 淡入嘴洞 + 擦线补丁 | `art/10_mouth_open.png`、`10_mouth_open_cover.png` |
| feiyu-04 | open | 嘴层纵向压扁来闭嘴 | — |
| feiyu-05 | closed | 淡入小圆嘴 + 擦点补丁 | `art/12_mouth_open.png`、`12_mouth_open_cover.png` |

右键「张个嘴」会完整演一遍开→合；说话（`talk`）/ 语音回复时按音节自动开合。
`tools/mouth-art.py` 负责重新生成 01/03/05 的嘴部叠加图（需要 Pillow + numpy）。

## 文件约定

```
skins/<id>/
  manifest.json      层定义、锚点、缩放、命中区、区域手势、右键菜单
  art/*.png          所有层：同画布、同位置、透明底
  preview.png        128px 缩略图（供以后皮肤选择器用）
  persona.md         人设
```

命中遮罩不需要单独文件：运行时由所有层的 alpha 合成 256×256 交给主进程。

## 原始工程来源

| id | 源目录（工作区内） | 状态 |
|---|---|---|
| feiyu-01 | `skins/_sources/feiyu-01/` | 原始导出 |
| feiyu-02 | `skins/_sources/feiyu-02/` | 原始导出 |
| feiyu-03 | `skins/_sources/feiyu-03/` | 原始导出（补的发卡只在皮肤包里） |
| feiyu-04 | `skins/_sources/feiyu-04/` | **已回写**：层与 PSD 都是改后版本，原始版在 `layers.original/` 和 `*.original.psd` |
| feiyu-05 | `skins/_sources/feiyu-05/` | **已回写**：盆的层序已修正、重复层已删并重建 PSD |

重新导入用：

```bash
node tools/import-skin.mjs "<工程目录>" <id> "<显示名>" --rig-from skins/feiyu-01/manifest.json
```
