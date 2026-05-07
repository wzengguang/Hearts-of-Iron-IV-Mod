# WZG-女子军官学院将领包

## MOD 目标

给玩家国家添加一个决议，通过事件一次性生成一批带自定义头像的女性陆军将领/元帅。

该 MOD 主要新增：

- 决议分类
- 一次性决议
- 触发事件
- 40 张将领头像
- 中英文本地化

## 当前功能

决议分类：

```hoi4
frnco_category
```

决议：

```hoi4
girl_for_battle
```

事件：

```hoi4
Female_sergeant.2
```

当前事件生成：

```text
create_corps_commander: 38
create_field_marshal: 2
总计: 40 名将领/元帅
```

头像路径：

```text
gfx/leaders/000/001.dds
...
gfx/leaders/000/040.dds
```

## 当前文件结构

```text
WZG-女子军官学院将领包/
├── descriptor.mod
├── README.md
├── thumbnail.png
├── common/
│   └── decisions/
│       ├── girls.txt
│       └── categories/
│           └── new_army_school.txt
├── events/
│   └── girl.txt
├── gfx/
│   └── leaders/
│       └── 000/
│           ├── 001.dds
│           ├── ...
│           └── 040.dds
└── localisation/
    ├── english/
    │   └── frnco_l_english.yml
    └── simp_chinese/
        └── frnco_l_simp_chinese.yml
```

## 关键文件说明

### `common/decisions/categories/new_army_school.txt`

定义决议分类 `frnco_category`。

当前限制：

```hoi4
allowed = {
    has_dlc = "Waking the Tiger"
}
visible = {
    is_ai = no
}
```

也就是说：

- 需要 `Waking the Tiger` DLC。
- 只对玩家显示。

### `common/decisions/girls.txt`

定义决议 `girl_for_battle`。

核心效果：

```hoi4
country_event = { id = Female_sergeant.2 }
```

该决议：

- 只对玩家显示。
- `fire_only_once = YES`，只能使用一次。
- `cost = 5`，需要 5 政治点数。

### `events/girl.txt`

定义事件 `Female_sergeant.2`。

选择第一个选项时创建 40 名将领/元帅。

常用结构：

```hoi4
create_corps_commander = {
    name = "X1娜塔莎"
    portrait_path = "gfx/leaders/000/001.dds"
    traits = { career_officer winter_specialist }
    skill = 1
    attack_skill = 1
    defense_skill = 1
    planning_skill = 1
    logistics_skill = 1
}
```

## 给 AI 的升级步骤

当游戏本体升级后，按以下步骤检查/升级本 MOD。

### 1. 不需要复制本体文件

此 MOD 是新增决议和新增事件，不覆盖本体文件。升级时通常不需要从游戏本体复制任何文件。

### 2. 检查决议系统语法

确认以下结构仍被新版游戏支持：

```hoi4
common/decisions/categories/*.txt
common/decisions/*.txt
```

确认 `fire_only_once`、`cost`、`visible`、`complete_effect` 仍可用。

### 3. 检查事件系统语法

确认以下结构仍被新版游戏支持：

```hoi4
add_namespace = Female_sergeant
country_event = {
    id = Female_sergeant.2
    is_triggered_only = yes
}
```

### 4. 检查将领创建命令

确认新版游戏仍支持：

```hoi4
create_corps_commander = { ... }
create_field_marshal = { ... }
```

如果游戏日志提示某些 trait 无效，需要用新版本体 `common/unit_leader/` 中存在的 trait 替换。

当前常用 trait 包括：

```text
career_officer
winter_specialist
media_personality
jungle_rat
inflexible_strategist
armor_officer
war_hero
brilliant_strategist
harsh_leader
trickster
trait_reckless
politically_connected
trait_cautious
bold
cavalry_officer
organizer
aviation_enthusiast
gunnery_expert
old_guard
desert_fox
engineer_officer
infantry_officer
```

### 5. 检查头像文件

确保事件中的每个：

```hoi4
portrait_path = "gfx/leaders/000/数字.dds"
```

都有对应文件。

当前应有：

```text
001.dds 到 040.dds
```

### 6. 更新版本声明

根据当前游戏版本更新：

```text
F:/HeartsOfIron4/mod/WZG-女子军官学院将领包/descriptor.mod
F:/HeartsOfIron4/mod/WZG-女子军官学院将领包.mod
```

例如 1.18.x：

```hoi4
supported_version="1.18.*"
```

## 验证步骤

1. 检查 `events/girl.txt` 大括号是否平衡。
2. 检查 `create_corps_commander` 和 `create_field_marshal` 数量。
3. 检查 40 个头像文件是否存在。
4. 进入游戏，玩家应能看到“女子军官学院”决议分类。
5. 点击决议后应弹出事件。
6. 选择“重点培养”后应创建 40 名将领/元帅。
7. 决议只能使用一次。

## 兼容性说明

- 该 MOD 新增独立决议和事件，通常与大多数 MOD 兼容。
- 如果其他 MOD 修改了将领 trait 名称或禁用了某些 trait，可能导致日志报错。
- 如果没有 `Waking the Tiger` DLC，决议分类不会显示。
