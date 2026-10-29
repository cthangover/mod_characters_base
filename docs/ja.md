# Characters Engine — キャラクター、そのアクション、パーティ

Characters Engine は、誰が戦闘に参加するかを定義します。主人公・仲間・敵に、HP・攻撃・防御・アクションポイント、そして使えるアクションカードを持たせるものです。さらに**キャラクターアクション**パネルを表示します。ここでパーティを見ながら、どのカードを各キャラクターが戦闘に持っていくか決めます。会話の台詞のそばに出るポートレートも、この mod が供給します。

Cthangover と RPG Plus の一部であり、戦闘 mod、recruiting、各エディターがこの上に作られています。

## キャラクターアクションパネル

ツールバーのカード・アイコンで開きます。

![Cthangover のキャラクターアクションパネル: HP・AP・ATK・DEF を持つ Marao と Murakami、目標・コスト・値つきのアクションカード（基本攻撃、防御、スタン）、それぞれの空いた三つ目のスロット、右側の空のアクションプール。](assets/character-panel.jpg)

- **パーティ** — パーティの全キャラクターを、ポートレート・HP（体力）・ターンごとの AP（アクションポイント）・ATK（攻撃）・DEF（防御）つきで一覧にします。
- **スロット** — 各キャラクターにはアクションカード用のスロットが三つあります。カードには目標（敵・味方・自分）、AP コスト、値が表示されます。
- **アクションプール** — 持っているけれどまだ割り当てていないカード。カードをスロットへドラッグするか、カードを選んでからスロットを選びます。スロット間のドラッグは入れ替えになります。検索とフィルターでプールを絞り込めます。
- **リセット** は、各キャラクターの定義の先頭三つのアクションをスロットへ戻します。残りはプールへ移ります。

## 三つの異なる絵

キャラクターは三つの姿で現れ、それぞれ別の場所から来ます。

![左: Marao の会話ポートレート（アバター「marao/sad」）。中央: カードに映る Marao のキャラクター画像。右: RPG Plus の主人公のマップスプライト（charset）で、これは別のシステムのもの。](assets/three-pictures.jpg)

1. **会話ポートレート（アバター）** — mod の `avatars/` フォルダーに入る画像で、シナリオの行ごとに選びます。`text "…" first=marao/sad` が一つ目のポートレート位置を、`second=…` が二つ目（二人の会話）を埋めます。キャラクターの定義とは結びついていません。
2. **キャラクター画像** — 定義の `Image`。このパネルのカードと戦闘で表示されます。
3. **マップスプライト（charset）** — タイルマップを歩くスプライト。選ぶのは RPG mod（ゲームの**外見**設定かイベントの `Sprite`）で、この mod ではありません。

## キャラクターの定義

キャラクターは、mod の `characters/` フォルダーに入る kind `character` の JSON ファイルです。こちらは Cthangover の Marao。上のパネルのカードと見比べてください。

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

`Health 16` がカードの **HP 16/16**、`Point 1` が **AP 1**、`Attack 5` が **ATK 5** で、二つの `Actions` がスロットの**基本攻撃**と**防御**のカードです。

その他のフィールド: `Behaviour`（敵の戦闘 AI。例は `base`）、`Exp`（倒したときの経験値）、`RecruitmentChance`、そして `Loot` — キャラクターを倒したときに落ちる `ItemId`・`MinCount`・`MaxCount`・`Probability` の一覧です。

## アクションカード

アクションは kind `action` の JSON ファイルです。この mod は基本的な物理アクション — **基本攻撃**・**防御**・**スタン** — を同梱しています。

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

`Type` が目標（敵・味方・自分）を決め、パッシブかアクティブかも定めます。`RequiredPoint` は AP コストです。ほかの値が戦闘で何をするかは、使われている戦闘 mod が決めます。

## シナリオからパーティにキャラクターを加える

```scenario
action party.add id=rpg_hero
```

キャラクターはパーティと戦闘メンバーに入ります。すでに名簿にいる者はレベルと HP を保ちます。RPG Demo はこの行で、村に初めて入ったときに主人公をプレイヤーに渡します。

## うまく動かないとき

1. **キャラクターが名前ではなく id を表示する。** `Name` は翻訳キーです。mod の `locale` ファイルに追加してください。
2. **台詞のそばにポートレートがない。** `first=` / `second=` の後のアバター名が、いずれかの mod の `avatars/` フォルダーにある画像と一致する必要があります。例は `marao/sad` に対する `avatars/marao/sad.png`。
3. **カードが戦闘で使えない。** キャラクターの AP がカードの `RequiredPoint` に足りていません。
