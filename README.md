# arx-assert

Маленька бібліотека тверджень (assertions) для тестів на
[ArxLang](https://github.com/Faneraiy14/ArxLang).

## Навіщо

Тести самої мови (`tests/*.arx` в репозиторії ArxLang) досі пишуться
вручну — в кожному файлі свій `if x == y { print("ок") } else { print("ПРОВАЛ") }`.
`arx-assert` виносить цей патерн у функції, які самі рахують
пройдені/провалені перевірки і друкують зрозумілий підсумок.

## Встановлення

```bash
arx install Faneraiy14/arx-assert
```

Тягне репозиторій у `arx_modules/arx-assert/` (потрібен `arx` —
[ArxNode](https://github.com/Faneraiy14/ArxNode)).

## Використання

```arx
import "arx-assert"

func main() {
    var t = newTest("моя перевірка")

    t.assertEqual(2 + 2, 4, "додавання")
    t.assertNotEqual(5, 6, "різні значення")
    t.assertTrue(len([1, 2, 3]) == 3, "довжина масиву")
    t.assertFalse(1 > 3, "хибна умова")
    t.assertNull(mapGet(newMap(), "нема"), "відсутній ключ мапи")
    t.assertArrayEqual([1, 2, 3], [1, 2, 3], "однакові масиви")

    t.summary()
}

main()
```

Вивід:

```
=== моя перевірка ===
  ✓ додавання
  ✓ різні значення
  ✓ довжина масиву
  ✓ хибна умова
  ✓ відсутній ключ мапи
  ✓ однакові масиви

моя перевірка: 6/6 пройдено
Усі перевірки пройдено.
```

Провалена перевірка показує очікуване й реальне значення, а не просто
"false":

```
  ✗ додавання (очікувалось 5, отримано 4)
```

## Функції

| Функція | Що перевіряє |
|---|---|
| `newTest(name)` | створює стан лічильника, друкує заголовок |
| `t.assertEqual(actual, expected, label)` | `actual == expected` |
| `t.assertNotEqual(actual, unexpected, label)` | `actual != unexpected` |
| `t.assertTrue(condition, label)` | `condition` — істина |
| `t.assertFalse(condition, label)` | `condition` — хиба |
| `t.assertNull(value, label)` | `value` — `null` |
| `t.assertArrayEqual(actual, expected, label)` | масиви однакової довжини й елементів (не порівняння посилань) |
| `t.summary()` | друкує підсумок `X/Y пройдено` |

## Відоме обмеження

ArxLang не дає скрипту напряму встановити конкретний код виходу (немає
`exit(code)` як функції стандартної бібліотеки) — тому `summary()` лише
друкує результат. Необроблений `throw` завершує процес з кодом 1, тож
для CI досить прокинути помилку самому, якщо є провалені перевірки:

```arx
t.summary()
if t.failed > 0 {
    throw "Тести провалено: " + toString(t.failed)
}
```

## Тести

```bash
arx tests/self_test.arx
```

## Ліцензія

MIT — див. [LICENSE](LICENSE). Автор — Faneraiy14.
