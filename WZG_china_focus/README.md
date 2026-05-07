# WZG-中共国策修改

## MOD 目标

本 MOD 只修改新版中共国策树：`china_communist_focus_sea`。

当前修改内容：

1. 只覆盖游戏本体文件：
   - `common/national_focus/china_communist_sea.txt`
2. 将该文件中所有未注释的 `cost = 10` 改为 `cost = 7`。
   - HOI4 中 `cost = 10` 通常等于 70 天国策。
   - `cost = 7` 通常等于 49 天国策。
3. 将一个减少战争目标正当化时间的中央委员会修正提前到顶层国策：
   - 顶层国策：`PRC_put_an_end_to_the_sufan`
   - 添加效果：`add_to_variable = { PRC_central_committee_justify_war_goal_time = -0.5 tooltip = justify_war_goal_time_tt }`
4. 不修改旧版中共国策树。
5. 不修改军阀中共国策树。
6. 不修改中国共享国策文件。

## 当前 MOD 文件结构

```text
WZG-中共国策修改/
├── descriptor.mod
├── README.md
└── common/
    └── national_focus/
        └── china_communist_sea.txt
```

根目录旁边还有启动器用文件：

```text
F:/HeartsOfIron4/mod/WZG-中共国策修改.mod
```

## 游戏本体源文件

升级时，从游戏本体复制新版中共国策文件：

```text
F:/HeartsOfIron4/Hearts of Iron IV/common/national_focus/china_communist_sea.txt
```

复制到 MOD：

```text
F:/HeartsOfIron4/mod/WZG-中共国策修改/common/national_focus/china_communist_sea.txt
```

不要复制以下文件，除非用户明确要求扩大修改范围：

```text
china_communist.txt
china_communist_warlord_TSR.txt
china_shared.txt
china_shared_TSR.txt
```

## 给 AI 的升级步骤

当游戏本体升级后，请按以下步骤升级本 MOD。

### 1. 确认目标文件

只处理游戏本体：

```text
F:/HeartsOfIron4/Hearts of Iron IV/common/national_focus/china_communist_sea.txt
```

确认该文件内存在：

```hoi4
focus_tree = {
    id = china_communist_focus_sea
```

如果 `id` 不再是 `china_communist_focus_sea`，先停止并告知用户新版中共国策树文件可能变更。

### 2. 用新版本体文件覆盖 MOD 文件

将本体 `china_communist_sea.txt` 复制到：

```text
F:/HeartsOfIron4/mod/WZG-中共国策修改/common/national_focus/china_communist_sea.txt
```

这样可以继承新版游戏本体的国策树改动，避免旧文件覆盖新内容。

### 3. 修改国策耗时

在 MOD 的 `china_communist_sea.txt` 中，将所有未注释的：

```hoi4
cost = 10
```

改成：

```hoi4
cost = 7
```

注意：

- 只改未注释行。
- 不要改 `# cost = 10` 这种模板注释。
- 不要改其他数字，例如 `cost = 5`、`cost = 3`、`cost = 1`。

推荐匹配规则：

```regex
(?m)^(\s*)cost\s*=\s*10\b
```

替换为：

```text
$1cost = 7
```

### 4. 将正当化时间奖励放到顶层国策

目标顶层国策：

```hoi4
id = PRC_put_an_end_to_the_sufan
```

在该国策的 `completion_reward` 中找到：

```hoi4
custom_effect_tooltip = PRC_modify_central_committee_modifier
add_to_variable = { PRC_central_committee_political_power_factor = 0.05 tooltip = political_power_factor_tt }
```

在其后添加：

```hoi4
add_to_variable = { PRC_central_committee_justify_war_goal_time = -0.5 tooltip = justify_war_goal_time_tt }
```

最终应类似：

```hoi4
custom_effect_tooltip = PRC_modify_central_committee_modifier
add_to_variable = { PRC_central_committee_political_power_factor = 0.05 tooltip = political_power_factor_tt }
add_to_variable = { PRC_central_committee_justify_war_goal_time = -0.5 tooltip = justify_war_goal_time_tt }
```

### 5. 从原位置移除同一个 -0.5 正当化奖励

为了避免重复叠加，从国策：

```hoi4
id = PRC_the_peoples_republic
```

的 `completion_reward` 中删除这一行：

```hoi4
add_to_variable = { PRC_central_committee_justify_war_goal_time = -0.5 tooltip = justify_war_goal_time_tt }
```

注意：

- 只删除 `PRC_the_peoples_republic` 里的这一行。
- 不要删除其他国策里的正当化奖励，除非用户明确要求。
- 当前仍允许保留其他路线的正当化奖励，例如：
  - `PRC_adopt_sun_yat_sens_principles` 中的 `-0.5`
  - `PRC_avenge_the_century_of_humiliation` 中的 `-0.25`

### 6. 更新版本声明

根据当前游戏版本更新：

```text
F:/HeartsOfIron4/mod/WZG-中共国策修改/descriptor.mod
F:/HeartsOfIron4/mod/WZG-中共国策修改.mod
```

例如游戏版本为 1.18.x 时：

```hoi4
supported_version="1.18.*"
```

如果升级到 1.19.x，则改为：

```hoi4
supported_version="1.19.*"
```

### 7. 验证

验证目标：

1. MOD 目录中 `common/national_focus/` 下只保留 `china_communist_sea.txt`。
2. `focus_tree` ID 仍为 `china_communist_focus_sea`。
3. 大括号数量平衡。
4. 未注释的 `cost = 10` 数量为 0。
5. `cost = 7` 数量应等于新版本体中未注释 `cost = 10` 的数量。
6. `PRC_put_an_end_to_the_sufan` 中包含 `PRC_central_committee_justify_war_goal_time = -0.5`。
7. `PRC_the_peoples_republic` 中不再包含 `PRC_central_committee_justify_war_goal_time = -0.5`。
8. 不要修改游戏本体文件，只修改 MOD 文件。

可用 PowerShell 验证：

```powershell
$mod='F:\HeartsOfIron4\mod\WZG-中共国策修改\common\national_focus'
Get-ChildItem $mod | Select-Object Name,Length
$file=Join-Path $mod 'china_communist_sea.txt'
$t=Get-Content $file -Raw
$open=([regex]::Matches($t,'\{')).Count
$close=([regex]::Matches($t,'\}')).Count
$cost10=([regex]::Matches($t,'(?m)^\s*cost\s*=\s*10\b')).Count
$cost7=([regex]::Matches($t,'(?m)^\s*cost\s*=\s*7\b')).Count
$tree=([regex]::Match($t,'(?m)^\s*id\s*=\s*(\w+)')).Groups[1].Value
"focus_tree=$tree braces=$open/$close cost=7=$cost7 remaining cost=10=$cost10"
```

## 当前已知验证结果

在当前 1.18.1 本体版本下：

```text
focus_tree=china_communist_focus_sea
braces=3462/3462
cost=7=125
remaining cost=10=0
```

当前正当化时间相关位置：

```text
PRC_put_an_end_to_the_sufan: -0.5
PRC_adopt_sun_yat_sens_principles: -0.5
PRC_avenge_the_century_of_humiliation: -0.25
```

## 兼容性说明

本 MOD 是全文件覆盖型 MOD。它覆盖：

```text
common/national_focus/china_communist_sea.txt
```

因此：

- 与其他修改同一文件的 MOD 可能冲突。
- 游戏本体升级后，必须重新从本体复制新版文件再套用上述修改。
- 不建议长期使用旧版 `china_communist_sea.txt` 覆盖新版游戏本体。
