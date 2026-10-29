# Characters Engine — 캐릭터, 액션, 파티

Characters Engine은 누가 전투에 참가하는지를 정의합니다. 주인공, 동료, 적에게 체력, 공격, 방어, 액션 포인트, 그리고 사용할 수 있는 액션 카드를 지니게 하는 모드입니다. 또한 **캐릭터 액션** 패널을 보여 주어 파티를 확인하고 각 캐릭터가 전투에 어떤 카드를 가져갈지 정하게 하며, 대화 대사 옆에 나타나는 초상화도 공급합니다.

Cthangover와 RPG Plus의 일부이며, 전투 mod, recruiting, 편집기들이 이 위에 세워집니다.

## 캐릭터 액션 패널

툴바의 카드 아이콘으로 엽니다.

![Cthangover의 캐릭터 액션 패널: HP, AP, ATK, DEF를 지닌 Marao와 Murakami, 대상·비용·수치가 표시된 액션 카드(기본 공격, 방어, 기절), 각자의 빈 세 번째 슬롯, 오른쪽의 빈 액션 풀.](assets/character-panel.jpg)

- **파티** — 파티의 모든 캐릭터를 초상화, 체력(HP), 턴당 액션 포인트(AP), 공격(ATK), 방어(DEF)와 함께 보여 줍니다.
- **슬롯** — 캐릭터마다 액션 카드용 슬롯이 세 개 있습니다. 카드는 대상(적, 아군, 자신), AP 비용, 수치를 표시합니다.
- **액션 풀** — 가지고 있지만 아직 배정하지 않은 카드. 카드를 슬롯으로 끌어 놓거나, 카드를 고른 뒤 슬롯을 고릅니다. 슬롯 사이의 드래그는 자리를 바꿉니다. 검색과 필터로 풀을 좁힐 수 있습니다.
- **초기화**는 각 캐릭터 정의의 처음 세 액션을 슬롯으로 되돌립니다. 나머지 액션은 풀로 갑니다.

## 서로 다른 세 가지 그림

캐릭터는 세 가지 모습으로 나타나며, 각각 다른 곳에서 옵니다.

![왼쪽: Marao의 대화 초상화(아바타 "marao/sad"). 가운데: 카드에 나타난 Marao의 캐릭터 이미지. 오른쪽: RPG Plus 주인공의 맵 스프라이트(charset)로, 다른 시스템에 속합니다.](assets/three-pictures.jpg)

1. **대화 초상화(아바타)** — mod의 `avatars/` 폴더에 있는 그림으로, 시나리오에서 줄마다 고릅니다. `text "…" first=marao/sad`가 첫 번째 초상화 자리를, `second=…`가 두 번째 자리(두 사람의 대화)를 채웁니다. 캐릭터 정의와 묶여 있지 않습니다.
2. **캐릭터 이미지** — 정의의 `Image`. 이 패널의 카드와 전투에서 표시됩니다.
3. **맵 스프라이트(charset)** — 타일 맵을 걷는 스프라이트. 고르는 것은 RPG mod(게임의 **외형** 설정이나 이벤트의 `Sprite`)이며 이 모드가 아닙니다.

## 캐릭터 정의

캐릭터는 mod의 `characters/` 폴더에 있는 kind `character` JSON 파일입니다. Cthangover의 Marao입니다. 위 패널의 카드와 비교해 보세요.

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

`Health 16`이 카드의 **HP 16/16**이고, `Point 1`은 **AP 1**, `Attack 5`는 **ATK 5**이며, 두 `Actions`는 슬롯의 **기본 공격**과 **방어** 카드입니다.

다른 필드: `Behaviour`(적의 전투 AI, 예: `base`), `Exp`(물리쳤을 때 주는 경험치), `RecruitmentChance`, 그리고 `Loot` — 캐릭터를 쓰러뜨렸을 때 떨어지는 `ItemId`, `MinCount`, `MaxCount`, `Probability` 목록입니다.

## 액션 카드

액션은 kind `action` JSON 파일입니다. 이 모드는 기본 물리 액션 — **기본 공격**, **방어**, **기절** — 을 함께 제공합니다.

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

`Type`은 대상(적, 아군, 자신)을 정하고 패시브나 액티브로 만들기도 합니다. `RequiredPoint`는 AP 비용입니다. 나머지 값이 전투에서 하는 일은 사용 중인 전투 mod가 결정합니다.

## 시나리오에서 파티에 캐릭터 추가하기

```scenario
action party.add id=rpg_hero
```

캐릭터는 파티와 전투 명단에 들어옵니다. 이미 명단에 있는 자는 레벨과 체력을 유지합니다. RPG Demo는 이 줄로 마을에 처음 들어섰을 때 주인공을 플레이어에게 줍니다.

## 문제가 생겼을 때

1. **캐릭터가 이름 대신 id를 보여 줍니다.** `Name`은 번역 키입니다. mod의 `locale` 파일에 추가하세요.
2. **대사 옆에 초상화가 없습니다.** `first=` / `second=` 뒤의 아바타 이름이 어느 mod의 `avatars/` 폴더에 있는 그림과 일치해야 합니다. 예: `marao/sad`에는 `avatars/marao/sad.png`.
3. **카드를 전투에서 쓸 수 없습니다.** 캐릭터의 AP가 카드의 `RequiredPoint`보다 적습니다.
