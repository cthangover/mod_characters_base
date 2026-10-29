# Characters Engine — 角色、角色行动与队伍

Characters Engine 决定谁参加战斗：主角、同伴和敌人，以及他们的生命值、攻击、防御、行动点数和可以使用的行动卡。它还提供**角色行动**面板，让你查看队伍并决定每个角色带上哪些卡参战，同时提供对话台词旁出现的肖像。

它是 Cthangover 和 RPG Plus 的一部分，战斗 mod、recruiting 和各个编辑器都构建在它之上。

## 角色行动面板

用工具栏上的卡片图标打开。

![Cthangover 中的角色行动面板：Marao 和 Murakami 的 HP、AP、ATK、DEF，他们的行动卡（基础攻击、防御、眩晕）带目标、消耗和数值，每人一个空的第三栏，右侧是空的行动卡池。](assets/character-panel.jpg)

- **队伍** — 队伍里的每个角色，带肖像、生命值（HP）、每回合行动点（AP）、攻击（ATK）和防御（DEF）。
- **栏位** — 每个角色有三个行动卡栏位。卡片显示目标（敌人、队友、自身）、AP 消耗和自身数值。
- **卡池** — 已拥有但尚未分配的卡片。把卡拖进栏位，或先选卡再选栏位；在栏位之间拖动可以交换位置。搜索和筛选可以缩小卡池范围。
- **重置** 把每个角色定义中的前三个行动放回栏位；其余行动进入卡池。

## 三种不同的图片

一个角色可以以三种面目出现，每种来自不同的地方：

![左：Marao 的对话肖像（头像 "marao/sad"）。中：Marao 在卡片上的角色图。右：RPG Plus 主角的地图精灵（charset），属于另一套系统。](assets/three-pictures.jpg)

1. **对话肖像（头像）** — 某个 mod 的 `avatars/` 文件夹里的图片，在剧本中按台词选用：`text "…" first=marao/sad` 填入第一个肖像位，`second=…` 填入第二个（两人对话时）。它与角色定义没有绑定。
2. **角色图** — 角色定义中的 `Image`，显示在本面板的卡片上和战斗中。
3. **地图精灵（charset）** — 在图块地图上行走的精灵，由 RPG mod 决定（游戏的**外观**设置或事件的 `Sprite` 字段），本 mod 不管这件事。

## 角色定义

角色是 mod 的 `characters/` 文件夹里类型为 `character` 的 JSON 文件。这是 Cthangover 的 Marao——可与上面面板中他的卡片对照：

```json
{
  "Kind": "character",
  "Items": [
    {
      "Id": "Marao",
      "Name": "characters/player/marao",
      "Image": "characters/player/marao",
      "Level": 1,
      "Point": 1,
      "Health": 16,
      "Defence": 0,
      "Attack": 5,
      "Actions": ["physics/attack", "physics/defence"]
    }
  ]
}
```

`Health 16` 就是他卡片上的 **HP 16/16**，`Point 1` 是 **AP 1**，`Attack 5` 是 **ATK 5**，而两个 `Actions` 就是栏位里的**基础攻击**和**防御**两张卡。

其他字段：`Behaviour`（敌人的战斗 AI，例如 `base`）、`Exp`（被打败时给予的经验）、`RecruitmentChance`，以及 `Loot`——角色被击败时掉落的 `ItemId`、`MinCount`、`MaxCount`、`Probability` 列表。

## 行动卡

行动是类型为 `action` 的 JSON 文件。本 mod 自带基础物理行动——**基础攻击**、**防御**和**眩晕**：

```json
{
  "Id": "physics/attack",
  "Name": "characters/action/physics_attack/name",
  "Description": "characters/action/physics_attack/description",
  "Image": "characters/actions/physics_attack",
  "Type": "ToEnemy",
  "Values": ["RequiredPoint=1", "Attack=1.0", "AttackType=Physics"]
}
```

`Type` 决定目标（敌人、队友、自身），或者把卡设为被动/主动；`RequiredPoint` 是 AP 消耗。其余数值在战斗中的效果由所用的战斗 mod 决定。

## 用剧本把角色加进队伍

```scenario
action party.add id=rpg_hero
```

角色加入队伍和出战名单。已在名单中的角色保留等级和生命值。RPG Demo 用这一行在玩家首次进入村庄时把主角交给玩家。

## 出问题时

1. **角色显示的是 id 而不是名字。** `Name` 是翻译键；把它加进 mod 的 `locale` 文件。
2. **台词旁没有肖像。** `first=` / `second=` 后面的头像名必须对应某个 mod 的 `avatars/` 文件夹里的图片，例如 `marao/sad` 对应 `avatars/marao/sad.png`。
3. **战斗中用不了某张卡。** 角色的 AP 少于卡片的 `RequiredPoint`。
