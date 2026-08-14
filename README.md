# ASF-RandomAvatarFrame

Плагин для **[ArchiSteamFarm](https://github.com/JustArchiNET/ArchiSteamFarm)**, который через случайные интервалы экипирует боту случайную рамку аватара (avatar frame) из тех, что он **реально получил сам** (крафтом бейджей Steam Trading Cards) — никакого стороннего контента, ничего не выдумывается и не скрапится.

Использует современную (пост-2020) систему "profile customization" Steam: `IPlayerService/GetProfileItemsOwned` для списка владения и `IPlayerService/SetAvatarFrame` для экипировки. Рамка аватара — отдельный, независимый от самой картинки аватара визуальный слот (в отличие от анимированного аватара, который заменяет саму картинку) — этот плагин не конфликтует с плагинами, которые меняют статичный аватар (например [ASF-RandomProfileAvatar](https://github.com/buddymurdock/ASF-RandomProfileAvatar)).

Пауза между сменами рамки задаётся диапазоном `[MinDelayDays; MaxDelayDays]` дней, но это **не жёсткие границы** — задержка берётся из клэмпированного лог-нормального распределения (min/max ≈ 5-й/95-й перцентиль, медиана `sqrt(min*max)`), а не uniform — так пауза между действиями выглядит более по-человечески рваной, а не идеально равномерной.

## Установка

1. Скачайте архив плагина из [Releases](../../releases) и распакуйте в папку `plugins` рядом с ASF (создайте подпапку с именем плагина).
2. Перезапустите ASF.

## Конфигурация

Настройки задаются **глобально**, в `ASF.json`, как дополнительные (нераспознанные ASF) свойства верхнего уровня:

```json
{
	"RandomAvatarFrameEnabled": true,
	"RandomAvatarFrameMinDelayDays": 14,
	"RandomAvatarFrameMaxDelayDays": 60
}
```

| Свойство | Тип | По умолчанию | Описание |
| --- | --- | --- | --- |
| `RandomAvatarFrameEnabled` | `bool` | `false` | Включает/выключает плагин. |
| `RandomAvatarFrameMinDelayDays` | `ushort` | `14` | Нижняя граница (≈5-й перцентиль) случайной паузы между сменами рамки, в днях. |
| `RandomAvatarFrameMaxDelayDays` | `ushort` | `60` | Верхняя граница (≈95-й перцентиль) случайной паузы. |

Если у бота нет накрафченных рамок аватара — плагин один раз пишет предупреждение в лог и ничего не делает, пока хотя бы одна не появится. Если `MinDelayDays` больше `MaxDelayDays`, значения меняются местами автоматически.

## Сборка

Проект использует **[ASF-PluginTemplate](https://github.com/JustArchiNET/ASF-PluginTemplate)** и собирается вместе с исходниками ASF, подключёнными как git submodule:

```sh
git clone --recurse-submodules https://github.com/buddymurdock/ASF-RandomAvatarFrame.git
cd ASF-RandomAvatarFrame
dotnet build -c Release
```

Если репозиторий уже склонирован без `--recurse-submodules`, подтяните submodule отдельно:

```sh
git submodule update --init --recursive
```

## Лицензия

Apache-2.0, см. [LICENSE.txt](LICENSE.txt).
