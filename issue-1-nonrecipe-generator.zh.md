# 非配方发电机卡片发送 `power.euPerTick: undefined`，被 live 服务拒绝

## 问题描述
通过拾取器的**"使用非配方"**选项把发电机（电源）加入已同步的计划板时，客户端发出的编辑操作中 `power.euPerTick` 字段值为 `undefined`。live 服务校验该字段必须是数字，整条编辑被以 `bad_op` 拒绝——卡片无法创建，界面弹出提示 "An edit could not be saved"。

## 环境
- GTNH Planner：0.2.0-beta.2
- GTNH 整合包：2.9.0-RC-2
- Java 25
- 已登录云账户，计划板已同步

## 复现步骤
1. 打开一个已同步到账户的计划板
2. 打开电力拾取器，选择任意发电机（如 Large Naquadah Reactor）
3. 选择**"使用非配方"**（而非常规的"加入计划"）
4. 观察：弹出 "An edit could not be saved"，板上不出现任何卡片

## 期望行为
成功创建非配方发电机卡片。由于没有配方可推算功率，客户端应发送合理默认值（`0`，或数据集中的标称值），或干脆省略该字段让服务端补默认值。

## 实际行为
操作包含 `power.euPerTick: undefined`，服务端拒绝：

```
[21:28:37] [Client thread/WARN] [gtnhplanner/]: The live service refused an edit to
110e22c9-b9e8-41da-bd44-aaecf5db61b0:8f741bdd-3491-429b-bb6b-828759c22ba2
(bad_op: recipes gtnhplanner:b7b6e5b9-09b2-4a36-827b-f5ab73c4129b:
power.euPerTick: Invalid input: expected number, received undefined)
```

## 根因分析
常规的"加入计划"流程会从电力数据集填充 `power.euPerTick`，而非配方路径在构建卡片时没有初始化 power 对象，该字段被序列化为 `undefined`。

## 临时绕过
改用电力拾取器常规的 **"加入计划"** 流程添加发电机，该路径正常。

## 修复建议
构建非配方发电机卡片时初始化 `power.euPerTick`（比如置 `0`），或在无值时跳过该字段而不是发送 `undefined`。
