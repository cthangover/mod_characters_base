# Characters Engine — personajes, sus acciones y el grupo

Characters Engine define quién participa en los combates: el héroe, los compañeros y los enemigos, con su salud, ataque, defensa, puntos de acción y las cartas de acción que pueden usar. También muestra el panel **Acciones de personaje**, donde ves tu grupo y decides qué cartas lleva cada personaje al combate, y proporciona los retratos que aparecen junto a las líneas de diálogo.

Forma parte de Cthangover y RPG Plus, y sobre él se construyen los mods de combate, recruiting y los editores.

## El panel Acciones de personaje

Se abre con el icono de cartas en la barra de herramientas.

![El panel Acciones de personaje en Cthangover: Marao y Murakami con HP, AP, ATK y DEF, sus cartas de acción (Ataque básico, Defensa, Aturdir) con objetivos, costes y valores, un tercer hueco vacío en cada uno, y la reserva de acciones vacía a la derecha.](assets/character-panel.jpg)

- **Grupo** — todos los personajes de tu grupo con su retrato, salud (HP), puntos de acción por turno (AP), ataque (ATK) y defensa (DEF).
- **Huecos** — cada personaje tiene tres huecos para cartas de acción. Una carta muestra su objetivo (enemigo, aliado, uno mismo), su coste en AP y sus valores.
- **Reserva de acciones** — cartas que posees pero no has asignado. Arrastra una carta a un hueco, o selecciona una carta y luego un hueco; arrastrar entre huecos los intercambia. La búsqueda y los filtros reducen la reserva.
- **Restablecer** devuelve a los huecos las tres primeras acciones de la definición de cada personaje; sus demás acciones van a la reserva.

## Tres imágenes distintas

Un personaje puede aparecer de tres maneras, y cada una viene de un sitio diferente:

![Izquierda: el retrato de diálogo de Marao (avatar «marao/sad»). Centro: la imagen de personaje de Marao en su carta. Derecha: un sprite de mapa (charset) del héroe de RPG Plus, que pertenece a otro sistema.](assets/three-pictures.jpg)

1. **Retrato de diálogo (avatar)** — una imagen en la carpeta `avatars/` de un mod, elegida línea a línea en el escenario: `text "…" first=marao/sad` ocupa el primer puesto de retrato y `second=…` el segundo (para un diálogo entre dos personas). No está ligado a la definición del personaje.
2. **Imagen de personaje** — el `Image` de la definición, mostrado en las cartas de este panel y en los combates.
3. **Sprite de mapa (charset)** — el sprite que camina por los mapas de tiles; lo elige el mod RPG (el ajuste **Appearance** del juego o el `Sprite` de un evento), no este mod.

## Una definición de personaje

Los personajes son archivos JSON de tipo `character` en la carpeta `characters/` de un mod. Este es Marao, de Cthangover; compáralo con su carta en el panel de arriba:

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

`Health 16` es su **HP 16/16**, `Point 1` su **AP 1** y `Attack 5` su **ATK 5**; las dos `Actions` son las cartas **Ataque básico** y **Defensa** de sus huecos.

Otros campos: `Behaviour` (la IA de combate del enemigo, por ejemplo `base`), `Exp` (la experiencia que da al caer), `RecruitmentChance` y `Loot` — una lista de `ItemId`, `MinCount`, `MaxCount` y `Probability` que suelta el personaje al ser derrotado.

## Cartas de acción

Las acciones son archivos JSON de tipo `action`. Este mod incluye las físicas básicas — **Ataque básico**, **Defensa** y **Aturdir**:

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

`Type` fija el objetivo (enemigo, aliado, uno mismo) o lo vuelve pasiva o activa; `RequiredPoint` es el coste en AP. Lo que hacen los demás valores en un combate lo decide el mod de combate en uso.

## Añadir un personaje al grupo desde un escenario

```scenario
action party.add id=rpg_hero
```

El personaje se une al grupo y al equipo de combate. Quien ya estaba en la plantilla conserva su nivel y su salud. RPG Demo usa esta línea para darte el héroe al entrar por primera vez en el pueblo.

## Si algo falla

1. **Un personaje muestra su id en vez de un nombre.** El `Name` es una clave de traducción; añádela a los archivos `locale` del mod.
2. **No hay retrato junto a una línea.** El nombre del avatar tras `first=` / `second=` debe coincidir con una imagen en la carpeta `avatars/` de algún mod, por ejemplo `avatars/marao/sad.png` para `marao/sad`.
3. **Una carta no se puede usar en combate.** El personaje tiene menos AP que el `RequiredPoint` de la carta.
