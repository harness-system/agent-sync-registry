---
id: omega.add-route-section
kind: skill
title: Omega Add Route Section
summary: Добавление нового раздела (роут + пункт меню) в уже мигрированный модуль Omega по новой архитектуре. Использовать, когда пользователь просит добавить раздел в меню модуля.
scope: project
loadMode: onDemand
status: active
category: frontend
severity: required
version: 1
owner: frontend
updatedAt: 2026-08-11
appliesTo:
  projects: ["omega"]
  agents: ["codex", "claude-code", "cursor", "github-copilot"]
  technologies: ["typescript", "angular"]
---

# Omega Add Route Section

## Назначение

Добавлять новый раздел — роут и пункт меню — в модуль, уже мигрированный на
новую архитектуру меню и прав доступа.

## Главный Принцип

Этот skill предназначен только для модулей, у которых есть `MODULE.menu.ts` в
`products/`. Старые конфиги меню и `ROUTES_PERMISSIONS_CONFIG` не изменяй.

## Workflow

1. Выполни обязательную подготовку и прочитай архитектурный документ.
2. Если аргументы не дают полного ответа, уточни модуль, путь роута, название,
   право доступа, тип права и компонент.
3. Найди подходящий файл роутов, добавь роут с `SecureRouteMetadata` и
   `permissionGuard`.
4. Добавь пункт в `MODULE.menu.ts`, уточнив его место в меню, если порядок
   неочевиден.
5. Выполни проверку TypeScript. Хлебные крошки подтянутся из `title` роута.

## References

- Подробный процесс, ограничения и проверка:
  [references/STRUCTURE.md](references/STRUCTURE.md).
- Образцы роута и пункта меню:
  [references/EXAMPLES.md](references/EXAMPLES.md).

## Чек-Лист

- Модуль уже мигрирован и содержит `MODULE.menu.ts`.
- Для роута задан `title`, metadata прав и нужный guard.
- Пункт добавлен в новый конфиг меню в правильном порядке.
- Старые конфиги и `ROUTES_PERMISSIONS_CONFIG` не изменены.
- TypeScript-проверка выполнена.
