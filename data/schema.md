# 开放数据层 · 数据字典

> 本目录把作品的**结构化内容**从代码中独立导出为 JSON，便于复核、二次利用与教学。
> 全部数据为参赛团队原创整理（术语库、时间线、航线、器物、介绍文字）；
> 选读的引文部分与研究方法的著录部分属第三方内容，见 §5 使用限制。

## 1. 通用约定

| 项 | 说明 |
|---|---|
| 顶层结构 | `{ "schema": "blueandwhite/1", "dataset": "...", "count": N, "records": [...] }` |
| 编码 | UTF-8，无 BOM |
| 双语字段 | 后缀 `z` = 中文，`e` = 英文（如 `tz`/`te`）；部分字段用 `zh`/`en` |
| 版本 | `schema` 字段为数据版本号，字段增删须升版本 |

## 2. 各数据集

| 文件 | 记录数 | 关键字段 | 说明 |
|---|---|---|---|
| `terminology.json` | **270** | `en` `zh` `cat` `key` `note` `ne` | 原创双语术语库；`note` 中文注记，`ne` 英文注记（55 条），`cat` 门类名，`key` 门类键 |
| `timeline.json` | **12** | `x` `era` `tz` `te` `pz` `pe` `img` | 历史节点；`x` 为时间轴相对位置，`img` 为缩略图路径 |
| `ports.json` | **19** | `id` `n` `e` `lon` `lat` `kind` `rz` `re` | 港口/地点；经纬度为示意坐标，`kind` ∈ site/port/city |
| `routes.json` | **19** | `from` `to` `kind` `label` | 航线；`from`/`to` 必须存在于 `ports.json` |
| `objects.json` | **10** | `id` `tz` `te` `img` `era` `group` | 器物画廊；`img` 为图像路径 |
| `readings.json` | **6** | `id` `tz` `te` `src` `quote` `comment` `en` `kt` | 选读；`quote` 为 ≤300 字有限引用，`comment` 为团队原创解读，`en` 为团队撰写的英文三段 |
| `notes_index.json` | **5** | `n` `src` `t` `ref` `note` `en` | 研究方法的研究方法与来源；`ref` 为引注著录，`note` 为团队处理说明 |
| `trans_chain.json` | **4** | `n` `place` `era` `tz` `te` `mat_z` `mat_e` `key_z` | 跨材质转译链路（四次转世） |
| `trans_network.json` | **12 节点 / 14 边** | `nodes[]` `edges[]` | 转译网络；`edges` 为 `[源, 目标, 机制键]`，机制键见 `MECH` |

## 3. 转译网络的机制键

| 键 | 中文 | English | 含义 |
|---|---|---|---|
| `pre` | 技术前置 | Technical precursor | 同一技术路径在其它窑口的先行实践 |
| `mat` | 原料输入 | Material supply | 原料（如钴料）的跨区供应 |
| `imi` | 直接模仿 | Direct imitation | 直接仿制器形与纹样 |
| `sub` | 材质替代 | Material substitution | 以其它材质替代瓷（锡釉陶、炻器） |
| `rep` | 图像记录 | Representation | 以绘画等媒介记录器物 |
| `dem` | 需求反塑 | Demand shaping | 订单与消费偏好反过来塑造生产 |

## 4. 校验

全部数据由 `07_技术文档与运行说明/测试脚本/validate_data.py` 校验：
字段完整性、ID 唯一性、引用的图像文件是否存在、航线端点是否存在、经纬度范围、
双语字段齐备、无内部工作标记。可复跑：

```bash
py -X utf8 submission/07_技术文档与运行说明/测试脚本/validate_data.py
```

## 5. 使用限制

- **团队原创部分**（术语库、时间线、航线、器物、介绍文字、转译网络、解读文字）：
  可在注明出处的前提下用于教学与非商业研究。
- **第三方部分**（`readings.json` 的 `quote` 字段、`notes_index.json` 的 `ref`/`en` 字段）：
  权利属原作者与原出版方，仅为评论与研究目的的有限引用；**不得单独再分发或用于商业用途**。
- 数据导出的目的之一是**可核验**：任何人可用上述脚本复算本文档中的每一个数字。
