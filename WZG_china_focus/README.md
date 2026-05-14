# WZG-中共国策修改

## MOD 目标

本 MOD 只修改新版中共国策树：`china_communist_focus_sea`。

当前修改内容：

1. 只覆盖游戏本体文件：
   - `common/national_focus/china_communist_sea.txt`
   - `common/on_actions/WZG_low_tension_wargoal_on_actions.txt`
   - `common/scripted_effects/WZG_low_tension_wargoal_effects.txt`
   - `common/wargoals/WZG_low_tension_wargoals.txt`
   - `localisation/simp_chinese/WZG_focus_l_simp_chinese.yml`
2. 将该文件中所有未注释的 `cost = 10` 改为 `cost = 7`。
   - HOI4 中 `cost = 10` 通常等于 70 天国策。
   - `cost = 7` 通常等于 49 天国策。
3. 将一个减少战争目标正当化时间的中央委员会修正提前到顶层国策：
   - 顶层国策：`PRC_put_an_end_to_the_sufan`
   - 添加效果：`add_to_variable = { PRC_central_committee_justify_war_goal_time = -0.5 tooltip = justify_war_goal_time_tt }`
4. 完成国策 `PRC_bring_war_to_neighboring_warlords` 后，设置永久旗标并降低后续国策树吞并战争目标的宣战紧张度：
   - 添加效果：`set_country_flag = PRC_low_tension_annex_wargoals`
   - 新增战争目标：`PRC_annex_everything_low_tension`，`threat = 0.05`
   - 原版 `annex_everything` 的 `threat = 0.5`，因此新战争目标的全球紧张度为原来的 1/10。
   - 由于 HOI4 宣战按钮的“提交增加全球紧张度”不读取 `wargoal_types.threat`，另通过 `on_declare_war` 对低紧张度吞并目标添加 `-3.6` 全球紧张度抵消，使当前测试中的 `+4%` 净增约为 `+0.4%`。
   - 只替换本国策树通过脚本效果发出的吞并战争目标，不全局修改 `annex_everything`。
5. 不修改旧版中共国策树。
6. 不修改军阀中共国策树。
7. 不修改中国共享国策文件。

## 当前 MOD 文件结构

```text
WZG-中共国策修改/
├── descriptor.mod
├── README.md
├── common/
│   ├── national_focus/
│   │   └── china_communist_sea.txt
│   ├── on_actions/
│   │   └── WZG_low_tension_wargoal_on_actions.txt
│   ├── scripted_effects/
│   │   └── WZG_low_tension_wargoal_effects.txt
│   └── wargoals/
│       └── WZG_low_tension_wargoals.txt
└── localisation/
   └── simp_chinese/
      └── WZG_focus_l_simp_chinese.yml
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

### 6. 添加周边军阀战争后的低紧张度吞并战争目标

目标国策：

```hoi4
id = PRC_bring_war_to_neighboring_warlords
```

在该国策的 `completion_reward` 中找到：

```hoi4
custom_effect_tooltip = PRC_bring_war_to_neighboring_warlords_tt
```

在其后添加：

```hoi4
custom_effect_tooltip = PRC_low_tension_annex_wargoals_tt
set_country_flag = PRC_low_tension_annex_wargoals
```

最终应类似：

```hoi4
custom_effect_tooltip = PRC_bring_war_to_neighboring_warlords_tt
custom_effect_tooltip = PRC_low_tension_annex_wargoals_tt
set_country_flag = PRC_low_tension_annex_wargoals
```

同时确保 MOD 中存在低紧张度战争目标文件：

```text
common/wargoals/WZG_low_tension_wargoals.txt
```

内容应包含：

```hoi4
wargoal_types = {
   PRC_annex_everything_low_tension = {
      war_name = WAR_NAME
      allowed = { always = no }
      available = {
      }
      take_states = {
      }
      threat = 0.05
   }
}
```

同时确保 MOD 中存在脚本效果文件：

```text
common/scripted_effects/WZG_low_tension_wargoal_effects.txt
```

该文件中的脚本效果应在国家拥有 `PRC_low_tension_annex_wargoals` 旗标时创建 `PRC_annex_everything_low_tension`，否则继续创建原版 `annex_everything`。
创建 `PRC_annex_everything_low_tension` 时，还应给目标国设置：

```hoi4
set_country_flag = PRC_low_tension_annex_wargoal_target
```

同时确保 MOD 中存在宣战抵消文件：

```text
common/on_actions/WZG_low_tension_wargoal_on_actions.txt
```

该文件应在 `on_declare_war` 中判断：宣战国拥有 `PRC_low_tension_annex_wargoals`，且被宣战国拥有 `PRC_low_tension_annex_wargoal_target` 时，添加负全球紧张度：

```hoi4
add_named_threat = {
   threat = -3.6
   name = PRC_low_tension_annex_wargoals_threat_offset
}
```

说明：HOI4 宣战按钮显示的“提交增加全球紧张度”来自 `TENSION_CB_WAR` 等宣战公式，不读取 `wargoal_types.threat`。因此 `PRC_annex_everything_low_tension` 能改变战争目标名称，但要降低实际宣战后的全球紧张度，需要用 `add_named_threat` 在宣战后抵消。

在 `china_communist_sea.txt` 中，本国策树里通过国策发出的 `create_wargoal = { type = annex_everything ... }` 应改为调用对应脚本效果，例如：

```hoi4
WZG_create_permanent_annex_wargoal_against_prev = yes
```

该方案只影响本国策树中被替换为脚本效果的吞并战争目标，不修改全局 `annex_everything` 战争目标。

同时确保 MOD 中存在简中本地化文件：

```text
localisation/simp_chinese/WZG_focus_l_simp_chinese.yml
```

该文件必须保存为 UTF-8 with BOM，否则游戏可能不读取本地化并直接显示 key。

内容应包含：

```yml
l_simp_chinese:
 PRC_low_tension_annex_wargoals_tt:0 "后续通过该国策树获得的§Y吞并§!战争目标产生的全球紧张度降至原来的§Y10%§!。"
 PRC_annex_everything_low_tension:0 "低紧张度吞并"
 PRC_low_tension_annex_wargoals_threat_offset:0 "低紧张度吞并战争目标"
```

### 7. 更新版本声明

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

### 8. 验证

验证目标：

1. MOD 目录中 `common/national_focus/` 下只保留 `china_communist_sea.txt`。
2. `focus_tree` ID 仍为 `china_communist_focus_sea`。
3. 大括号数量平衡。
4. 未注释的 `cost = 10` 数量为 0。
5. `cost = 7` 数量应等于新版本体中未注释 `cost = 10` 的数量。
6. `PRC_put_an_end_to_the_sufan` 中包含 `PRC_central_committee_justify_war_goal_time = -0.5`。
7. `PRC_the_peoples_republic` 中不再包含 `PRC_central_committee_justify_war_goal_time = -0.5`。
8. `PRC_bring_war_to_neighboring_warlords` 中包含 `set_country_flag = PRC_low_tension_annex_wargoals`。
9. `common/wargoals/WZG_low_tension_wargoals.txt` 中包含 `PRC_annex_everything_low_tension`，且 `threat = 0.05`。
10. `common/scripted_effects/WZG_low_tension_wargoal_effects.txt` 中包含 `PRC_low_tension_annex_wargoals` 条件判断。
11. `common/scripted_effects/WZG_low_tension_wargoal_effects.txt` 会给低紧张度战争目标的目标国设置 `PRC_low_tension_annex_wargoal_target`。
12. `common/on_actions/WZG_low_tension_wargoal_on_actions.txt` 中包含 `on_declare_war` 和 `add_named_threat = { threat = -3.6 ... }`。
13. `localisation/simp_chinese/WZG_focus_l_simp_chinese.yml` 中包含 `PRC_low_tension_annex_wargoals_tt`、`PRC_annex_everything_low_tension` 和 `PRC_low_tension_annex_wargoals_threat_offset`。
14. 不要修改游戏本体文件，只修改 MOD 文件。

测试低紧张度吞并战争目标时，需要使用完成 `PRC_bring_war_to_neighboring_warlords` 之前的存档，或开新档重新完成该国策。已经生成在存档里的旧 `annex_everything` 战争目标不会因为脚本更新自动变成 `PRC_annex_everything_low_tension`。

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

当前低紧张度吞并战争目标相关位置：

```text
PRC_bring_war_to_neighboring_warlords: set_country_flag = PRC_low_tension_annex_wargoals
WZG_low_tension_wargoals.txt: PRC_annex_everything_low_tension threat = 0.05
WZG_low_tension_wargoal_effects.txt: 有旗标时创建 PRC_annex_everything_low_tension，否则创建 annex_everything，并给目标国设置 PRC_low_tension_annex_wargoal_target
WZG_low_tension_wargoal_on_actions.txt: 对低紧张度吞并目标宣战时添加 -3.6 全球紧张度抵消
WZG_focus_l_simp_chinese.yml: PRC_low_tension_annex_wargoals_tt / PRC_annex_everything_low_tension / PRC_low_tension_annex_wargoals_threat_offset
```

## 兼容性说明

本 MOD 是全文件覆盖型 MOD。它覆盖：

```text
common/national_focus/china_communist_sea.txt
common/on_actions/WZG_low_tension_wargoal_on_actions.txt
common/scripted_effects/WZG_low_tension_wargoal_effects.txt
common/wargoals/WZG_low_tension_wargoals.txt
localisation/simp_chinese/WZG_focus_l_simp_chinese.yml
```

因此：

- 与其他修改同一文件的 MOD 可能冲突。
- 游戏本体升级后，必须重新从本体复制新版文件再套用上述修改。
- 不建议长期使用旧版 `china_communist_sea.txt` 覆盖新版游戏本体。
