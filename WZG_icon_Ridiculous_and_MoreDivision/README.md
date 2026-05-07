# WZG-图标-Ridiculous_MoreDivision

## MOD 目标

为师团编制/师团模板界面增加更多可选图标。

这是纯图形/UI 资源 MOD，不修改国策、事件、科技、单位属性或数值平衡。

## 当前功能

该 MOD 通过 `.gfx` 文件注册新的师团模板图标 sprite，并提供对应 PNG 图片资源。

主要覆盖目录：

```text
interface/
gfx/interface/counters/division_templates_large/
gfx/interface/counters/division_templates_small/
```

## 当前文件结构

```text
WZG-图标-Ridiculous_MoreDivision/
├── descriptor.mod
├── README.md
├── thumbnail.png
├── interface/
│   ├── ridiculous_unit_icons.gfx
│   └── more_division_icons_merged.gfx
└── gfx/
    └── interface/
        └── counters/
            ├── division_templates_large/
            └── division_templates_small/
```

## 图标注册范围

当前统计：

```text
ridiculous_unit_icons.gfx: GFX_div_templ_121 到 GFX_div_templ_322
more_division_icons_merged.gfx: GFX_div_templ_323 到 GFX_div_templ_437
```

对应 sprite 命名格式：

```hoi4
GFX_div_templ_数字_large
GFX_div_templ_数字_small
```

对应贴图路径格式：

```hoi4
textureFile = "gfx/interface/counters/division_templates_large/数字.png"
textureFile = "gfx/interface/counters/division_templates_small/数字.png"
```

## 关键文件说明

### `interface/ridiculous_unit_icons.gfx`

注册早期图标编号，大致范围为 `121` 到 `322`。

### `interface/more_division_icons_merged.gfx`

注册后续图标编号，大致范围为 `323` 到 `437`。

### 图片目录

每个图标通常需要两张图片：

```text
gfx/interface/counters/division_templates_large/数字.png
gfx/interface/counters/division_templates_small/数字.png
```

## 给 AI 的升级步骤

当游戏本体升级后，按以下步骤检查/升级本 MOD。

### 1. 确认本体图标编号是否冲突

检查新版游戏本体或其他必须兼容的 MOD 中是否新增了相同 sprite 名称：

```text
GFX_div_templ_121_large
GFX_div_templ_121_small
...
GFX_div_templ_437_large
GFX_div_templ_437_small
```

如果本体已占用这些编号，可能出现图标覆盖或显示错误。需要改用更高编号，并同步重命名图片和 `.gfx` 中的 sprite 名称。

### 2. 检查 `.gfx` 语法

每个 sprite 应类似：

```hoi4
spriteType = { name = "GFX_div_templ_323_large" textureFile = "gfx/interface/counters/division_templates_large/323.png" }
spriteType = { name = "GFX_div_templ_323_small" textureFile = "gfx/interface/counters/division_templates_small/323.png" }
```

确保：

- `large` 指向 `division_templates_large`。
- `small` 指向 `division_templates_small`。
- 文件名数字与 sprite 数字一致。
- 大括号平衡。

### 3. 检查图片是否齐全

对每个编号，理论上需要：

```text
large/数字.png
small/数字.png
```

如果 `.gfx` 注册了某个编号但图片不存在，游戏内可能显示缺失贴图。

### 4. 更新版本声明

根据游戏版本更新：

```text
F:/HeartsOfIron4/mod/WZG-图标-Ridiculous_MoreDivision/descriptor.mod
F:/HeartsOfIron4/mod/WZG-图标-Ridiculous_MoreDivision.mod
```

例如 1.18.x：

```hoi4
supported_version="1.18.*"
```

### 5. 不要修改本体文件

该 MOD 只应修改自身目录，不应直接修改游戏本体的 `interface` 或 `gfx` 目录。

## 验证步骤

1. 检查 `interface/*.gfx` 大括号是否平衡。
2. 检查 `.gfx` 引用的 PNG 是否都存在。
3. 进入游戏，打开师团模板图标选择界面。
4. 确认新增图标可以显示，且 large/small 两种尺寸都正常。

## 兼容性说明

- 这是图标资源 MOD，通常不会影响存档逻辑。
- 与其他新增师团图标的 MOD 可能冲突，冲突点是相同 `GFX_div_templ_数字_large/small` 名称。
- 如果发生冲突，优先改本 MOD 的编号范围到未占用的更高数字。
