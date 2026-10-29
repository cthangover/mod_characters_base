# Characters Engine — nhân vật, hành động của họ và đội

Characters Engine quy định ai tham chiến: anh hùng, đồng đội và kẻ địch, cùng máu, tấn công, phòng thủ, điểm hành động và các thẻ hành động họ có thể dùng. Mod còn mở bảng **Hành động nhân vật**, nơi bạn xem đội của mình và quyết định mỗi nhân vật mang những thẻ nào vào trận, đồng thời cung cấp các chân dung xuất hiện bên cạnh câu thoại.

Mod là một phần của Cthangover và RPG Plus; các mod chiến đấu, recruiting và các trình soạn thảo đều xây trên nó.

## Bảng Hành động nhân vật

Mở bằng biểu tượng thẻ bài trên thanh công cụ.

![Bảng Hành động nhân vật trong Cthangover: Marao và Murakami với HP, AP, ATK, DEF, các thẻ hành động (Tấn công cơ bản, Phòng thủ, Choáng) kèm mục tiêu, giá và giá trị, mỗi người một ô trống thứ ba, bên phải là kho hành động trống.](assets/character-panel.jpg)

- **Đội** — mọi nhân vật trong đội của bạn cùng chân dung, máu (HP), điểm hành động mỗi lượt (AP), tấn công (ATK) và phòng thủ (DEF).
- **Ô chứa** — mỗi nhân vật có ba ô cho thẻ hành động. Một thẻ hiển thị mục tiêu (địch, đồng đội, bản thân), giá AP và các giá trị của nó.
- **Kho hành động** — các thẻ bạn sở hữu nhưng chưa phân chỗ. Kéo thẻ vào một ô, hoặc chọn thẻ rồi chọn ô; kéo giữa hai ô là đổi chỗ. Ô tìm kiếm và bộ lọc thu hẹp kho.
- **Đặt lại** đưa ba hành động đầu trong định nghĩa của từng nhân vật về các ô; các hành động còn lại chuyển vào kho.

## Ba kiểu hình khác nhau

Một nhân vật có thể xuất hiện theo ba cách, và mỗi cách đến từ một nơi khác:

![Trái: chân dung hội thoại của Marao (avatar "marao/sad"). Giữa: ảnh nhân vật của Marao trên thẻ của anh ta. Phải: sprite bản đồ (charset) của nhân vật chính RPG Plus, thuộc một hệ thống khác.](assets/three-pictures.jpg)

1. **Chân dung hội thoại (avatar)** — một ảnh trong thư mục `avatars/` của mod, chọn theo từng câu trong kịch bản: `text "…" first=marao/sad` chiếm chỗ chân dung thứ nhất, `second=…` chỗ thứ hai (hội thoại hai người). Nó không dính vào định nghĩa nhân vật.
2. **Ảnh nhân vật** — trường `Image` của định nghĩa, hiện trên các thẻ trong bảng này và trong trận chiến.
3. **Sprite bản đồ (charset)** — sprite biết đi trên bản đồ tile; do mod RPG chọn (thiết lập **Diện mạo** của game hoặc trường `Sprite` của sự kiện), không phải mod này.

## Định nghĩa nhân vật

Nhân vật là tệp JSON kind `character` trong thư mục `characters/` của mod. Đây là Marao của Cthangover — hãy so với thẻ của cậu ấy trong bảng phía trên:

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

`Health 16` chính là **HP 16/16** trên thẻ, `Point 1` là **AP 1**, `Attack 5` là **ATK 5**, còn hai `Actions` là thẻ **Tấn công cơ bản** và **Phòng thủ** trong các ô của cậu ấy.

Các trường khác: `Behaviour` (AI chiến đấu của kẻ địch, ví dụ `base`), `Exp` (kinh nghiệm nhận được khi hạ được), `RecruitmentChance`, và `Loot` — danh sách `ItemId`, `MinCount`, `MaxCount`, `Probability` rơi ra khi nhân vật bị hạ.

## Thẻ hành động

Hành động là tệp JSON kind `action`. Mod này có sẵn các hành động vật lý cơ bản — **Tấn công cơ bản**, **Phòng thủ** và **Choáng**:

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

`Type` đặt mục tiêu (địch, đồng đội, bản thân) hoặc biến thẻ thành bị động hay chủ động; `RequiredPoint` là giá AP. Các giá trị còn lại làm gì trong trận chiến do mod chiến đấu đang dùng quyết định.

## Thêm nhân vật vào đội từ kịch bản

```scenario
action party.add id=rpg_hero
```

Nhân vật gia nhập đội và danh sách ra trận. Ai đã có trong danh sách thì giữ nguyên cấp và máu. RPG Demo dùng dòng này để trao nhân vật chính cho người chơi khi lần đầu bước vào ngôi làng.

## Khi có trục trặc

1. **Nhân vật hiện id thay vì tên.** `Name` là khóa dịch; hãy thêm nó vào các tệp `locale` của mod.
2. **Không có chân dung bên cạnh câu thoại.** Tên avatar sau `first=` / `second=` phải khớp với một ảnh trong thư mục `avatars/` của mod nào đó, ví dụ `avatars/marao/sad.png` cho `marao/sad`.
3. **Không dùng được thẻ trong trận.** Nhân vật có ít AP hơn `RequiredPoint` của thẻ.
