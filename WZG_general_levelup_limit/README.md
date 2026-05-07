# WZG-将领等级上限

## MOD 目标

提高将领、海军将领、元帅和特工的等级/技能上限。

当前版本已按 1.18.1 本体数据适配。

主要目标：

1. 将陆军将领、元帅、海军将领的等级/技能扩展到 20 级。
2. 将特工等级扩展到 5 级。
3. 保留 1.18.1 本体新增的海军 `building_module_modifier` 效果。

## 当前文件结构

```text
WZG-将领等级上限/
├── descriptor.mod
├── README.md
├── 等级上限.txt
├── thumbnail.png
├── thumbnail2.png
├── thumbnail3.png
└── common/
    └── unit_leader/
        ├── 99_attack_skills.txt
        ├── 99_coordination_skills.txt
        ├── 99_defense_skills.txt
        ├── 99_logistics_skills.txt
        ├── 99_maneuvering_skills.txt
        ├── 99_planning_skills.txt
        └── 99_skills.txt
```

## 修改的本体系统

该 MOD 修改：

```text
common/unit_leader/
```

本体对应文件通常是：

```text
00_skills.txt
00_attack_skills.txt
00_defense_skills.txt
00_planning_skills.txt
00_logistics_skills.txt
00_maneuvering_skills.txt
00_coordination_skills.txt
```

MOD 使用 `99_*.txt` 文件，让文件在本体 `00_*.txt` 之后加载。

## 各文件作用

```text
99_skills.txt              基础等级表；包含海军、陆军将领、元帅、特工
99_attack_skills.txt       攻击技能等级
99_defense_skills.txt      防御技能等级
99_planning_skills.txt     计划技能等级
99_logistics_skills.txt    后勤技能等级
99_maneuvering_skills.txt  海军机动技能等级
99_coordination_skills.txt 海军协调技能等级
```

## 当前已知适配状态

当前验证结果：

```text
99_attack_skills.txt         levels=60 building_module_modifier=20
99_defense_skills.txt        levels=60 building_module_modifier=20
99_planning_skills.txt       levels=60 building_module_modifier=0
99_logistics_skills.txt      levels=60 building_module_modifier=0
99_maneuvering_skills.txt    levels=20 building_module_modifier=20
99_coordination_skills.txt   levels=20 building_module_modifier=20
99_skills.txt                levels=65 building_module_modifier=0
```

解释：

- `levels=60` 通常表示海军 20 级 + 陆军将领 20 级 + 元帅 20 级。
- `levels=20` 通常表示只有海军 20 级。
- `levels=65` 表示基础等级表中包含海军、陆军将领、元帅、特工等多组等级。
- 1.18.1 本体在部分海军技能中加入了 `building_module_modifier`，本 MOD 已补齐到 20 级。

## 给 AI 的升级步骤

当游戏本体升级后，按以下步骤升级本 MOD。

### 1. 对照本体文件

游戏本体源目录：

```text
F:/HeartsOfIron4/Hearts of Iron IV/common/unit_leader/
```

MOD 目录：

```text
F:/HeartsOfIron4/mod/WZG-将领等级上限/common/unit_leader/
```

对应关系：

```text
00_skills.txt              -> 99_skills.txt
00_attack_skills.txt       -> 99_attack_skills.txt
00_defense_skills.txt      -> 99_defense_skills.txt
00_planning_skills.txt     -> 99_planning_skills.txt
00_logistics_skills.txt    -> 99_logistics_skills.txt
00_maneuvering_skills.txt  -> 99_maneuvering_skills.txt
00_coordination_skills.txt -> 99_coordination_skills.txt
```

### 2. 不要直接照搬旧 MOD 覆盖新版数据

升级时优先以新版游戏本体 `00_*.txt` 为基础，检查是否新增字段、修正、类型或子块。

特别注意新增字段：

```hoi4
building_module_modifier = { ... }
```

如果新版本体新增了其他子块，也要同步保留并扩展到 20 级。

### 3. 扩展等级

对于本体中 1 到 10 级的技能表：

- 保留本体原有字段。
- 将等级扩展到 20。
- 原 MOD 的经验消耗大致为：

```text
1=100
2=200
3=400
4=700
5=1200
6=2000
7=3000
8=4500
9=6500
10=9000
11=12000
12=16000
13=20000
14=25000
15=30000
16=40000
17=50000
18=60000
19=70000
20=80000
```

### 4. 保留当前 MOD 的数值风格

当前 MOD 比本体更强。升级时除非用户要求平衡化，否则保持原 MOD 的强度曲线。

例如陆军攻击技能：

```hoi4
offence = 0.05
...
offence = 1
```

表示 1 级 +5%，20 级 +100%。

### 5. 处理海军 `building_module_modifier`

当前 1.18.1 中应保留并扩展以下海军子效果：

攻击：

```text
spotting_chance
ships_at_battle_start
```

防御：

```text
naval_enemy_fleet_size_ratio_penalty_factor
screening_efficiency
```

机动：

```text
convoy_retreat_speed
naval_retreat_chance
```

协调：

```text
naval_coordination
navy_org_factor
```

如果新版本体调整这些字段，优先采用新版字段名，再按 20 级扩展。

### 6. 特工等级

`99_skills.txt` 中保留特工等级扩展：

```text
operative: 1 到 5 级
```

如果新版本体改变特工等级或特工 modifier 名称，需要同步更新。

### 7. 更新版本声明

根据当前游戏版本更新：

```text
F:/HeartsOfIron4/mod/WZG-将领等级上限/descriptor.mod
F:/HeartsOfIron4/mod/WZG-将领等级上限.mod
```

例如 1.18.x：

```hoi4
supported_version="1.18.*"
version="1.18.1"
```

如果升级到 1.19.x，则改为：

```hoi4
supported_version="1.19.*"
version="1.19.0"
```

## 验证步骤

### 1. 检查括号平衡

所有 `common/unit_leader/*.txt` 文件的大括号数量必须一致。

### 2. 检查根节点名称

每个 MOD 文件根节点必须与本体对应：

```text
leader_skills
leader_attack_skills
leader_defense_skills
leader_planning_skills
leader_logistics_skills
leader_maneuvering_skills
leader_coordination_skills
```

### 3. 检查等级数量

预期：

```text
99_attack_skills.txt: 60
99_defense_skills.txt: 60
99_planning_skills.txt: 60
99_logistics_skills.txt: 60
99_maneuvering_skills.txt: 20
99_coordination_skills.txt: 20
99_skills.txt: 65
```

如果新版本体新增类型，数量可能变化，需人工确认。

### 4. 检查海军新增字段

如果本体对应文件中存在 `building_module_modifier`，MOD 也必须保留。

当前预期：

```text
99_attack_skills.txt: 20 个 building_module_modifier
99_defense_skills.txt: 20 个 building_module_modifier
99_maneuvering_skills.txt: 20 个 building_module_modifier
99_coordination_skills.txt: 20 个 building_module_modifier
```

## 可用 PowerShell 验证

```powershell
$mod='F:\HeartsOfIron4\mod\WZG-将领等级上限\common\unit_leader'
Get-ChildItem "$mod\*.txt" | ForEach-Object {
    $t=Get-Content $_.FullName -Raw
    $open=([regex]::Matches($t,'\{')).Count
    $close=([regex]::Matches($t,'\}')).Count
    $bm=([regex]::Matches($t,'building_module_modifier')).Count
    $levels=([regex]::Matches($t,'(?m)^\s*\d+\s*=')).Count
    if($open -eq $close){
        "OK  $($_.Name) braces=$open levels=$levels building_module=$bm"
    } else {
        "BAD $($_.Name) open=$open close=$close levels=$levels building_module=$bm"
    }
}
```

## 兼容性说明

- 这是全文件覆盖/重定义型 MOD，可能与其他修改 `common/unit_leader/` 的 MOD 冲突。
- 与增加将领经验、增加特质槽位、增加部队经验等 MOD 通常可以搭配，但需要注意加载顺序。
- 游戏本体升级后，必须重新对照新版 `common/unit_leader/`，否则可能丢失新版字段或新机制。
