# Cobalt UI Navigation — документация C++-компонента

[English](README.md) | [Русский](README.ru.md)

**Опубликованная документация:** https://ekatmoz88-alt.github.io/cobalt-ui-navigation-docs/

Документация компонента навигации пользовательского интерфейса Cobalt, подготовленная на основе предоставленных фрагментов исходного кода C++ и конфигурации GYP.

## Обзор

Компонент предоставляет C++-обёртку над операциями нативной UI-навигации. В зависимости от версии Starboard API и доступности платформенного интерфейса используются указатели на функции Starboard либо локальные fallback-реализации.

## Разделы

- [Architecture](docs/architecture.md) — структура компонента и пути выполнения.
- [Native Interface](docs/native-interface.md) — нативные типы и интерфейс указателей на функции.
- [Navigation Item](docs/nav-item.md) — жизненный цикл NavItem и методы.
- [Focus Management](docs/focus-management.md) — управление фокусом и callbacks.
- [Containers and Geometry](docs/containers-and-geometry.md) — контейнеры, окна, размеры и преобразования.
- [Batch Updates](docs/batch-updates.md) — очередь операций и пакетная обработка.
- [Compatibility](docs/compatibility.md) — поведение по версиям и fallback.
- [Source Evidence](references/source-evidence.md) — связь документации с исходными фрагментами.

## Границы доказательности

Документация описывает поведение, видимое в исследованных исходниках. Она не претендует на описание полного Cobalt runtime или всех платформенных реализаций. Потокобезопасность, единицы координат и платформенные детали требуют дополнительных источников.

## Теги

`cpp` · `ui-navigation` · `component-architecture` · `api-reference` · `source-analysis`
