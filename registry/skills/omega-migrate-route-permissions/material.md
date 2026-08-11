---
id: omega.migrate-route-permissions
kind: skill
title: Omega Migrate Route Permissions
summary: Миграция одного модуля Omega со старой архитектуры ROUTES_PERMISSIONS_CONFIG и configs/menu на SecureRouteMetadata и FeatureMenuConfig. Использовать, когда пользователь просит мигрировать права и меню модуля.
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

# Omega Migrate Route Permissions

## Назначение

Мигрировать один модуль со старой архитектуры прав и меню на
`SecureRouteMetadata` и `FeatureMenuConfig`.

## Главный Принцип

Кастомные доменные гварды не заменяй на `permissionGuard`: они делают больше,
чем проверка прав. Если в структуре или правах есть неоднозначность, не гадай —
уточни у пользователя до изменения кода.

## Workflow

1. Выполни обязательную Angular-подготовку и прочитай архитектурный документ.
2. Изучи роуты, старое меню и `ROUTES_PERMISSIONS_CONFIG`; выпиши состояние
   модуля и уточни неоднозначности.
3. Раздели generic `PermissionGuard`, кастомные доменные гварды и роуты без
   гварда.
4. Определи структуру меню и объясни пользователю план миграции.
5. Создай `FeatureMenuConfig`, обнови роуты, зарегистрируй конфиги, оставь
   deprecated якорный конфиг и удали старые записи прав.
6. Проверь результат TypeScript-проверкой и контрольным списком.

## References

- Подробный порядок миграции, файлы и ограничения:
  [references/STRUCTURE.md](references/STRUCTURE.md).
- Образцы конфигов и безопасной замены guard:
  [references/EXAMPLES.md](references/EXAMPLES.md).

## Чек-Лист

- Кастомные доменные гварды сохранены.
- Все новые меню-конфиги зарегистрированы.
- Старое меню оставлено только якорным пунктом с `children: []`.
- Записи модуля удалены из `ROUTES_PERMISSIONS_CONFIG`.
- TypeScript-проверка прошла.
