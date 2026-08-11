# Миграция Прав Роутов: Подробный Процесс

## Обязательная Подготовка

Перед любой работой с Angular-кодом вызови по порядку:

1. `mcp__angular-cli__list_projects` — структура workspace.
2. `mcp__angular-cli__get_best_practices` — стандарты Angular.

Прочти `apps/front/docs/menu-permissions-architecture.md`: полное описание
новой архитектуры, интерфейсов и примеров.

## Изучи Текущее Состояние Модуля

Прочти следующие файлы, заменив `MODULE` на имя модуля:

1. `apps/front/src/app/products/MODULE/routes.ts`.
2. Проверь суброуты в `apps/front/src/app/products/MODULE/routes/` и прочти их, если они есть.
3. `apps/front/src/app/configs/menu/MODULE.menu.ts`.
4. `apps/front/src/app/configs/routes-permissions.config.ts` — найди все записи для этого модуля.

Выпиши:

- список всех роутов с путями, включая суброуты;
- файл, в котором стоит `[PermissionGuard]`;
- используемые гварды: generic `[PermissionGuard]` или кастомные доменные;
- права из `ROUTES_PERMISSIONS_CONFIG` для каждого роута;
- названия пунктов меню из старого конфига;
- наличие вложенных групп в меню.

Спроси пользователя, если что-то неоднозначно — не угадывай.

## Разбери Гварды

Кастомные доменные гварды не заменяются на `permissionGuard`: они делают больше
чем просто проверка прав — в том числе валидацию типов, редиректы или загрузку
данных. К ним только добавляется `title`.

Generic `[PermissionGuard]` (класс) заменяется на `[permissionGuard]` (функция)
с `data: SecureRouteMetadata`.

Для каждого роута:

- `[PermissionGuard]` — заменяем, добавляем `title` и `data`;
- кастомный гвард — оставляем, только добавляем `title`;
- нет гварда (детальные/дочерние страницы) — добавляем `title`, если нужны хлебные крошки.

## Определи Структуру Меню

Выясни:

- есть ли подгруппы с вложенными `children`; для каждой группы нужен отдельный `FeatureMenuConfig`;
- есть ли нестандартные флаги `isKiosk`, `isBrandApp`;
- есть ли права типа «приложение» (`useAppPermissions: true`);
- есть ли динамические роуты `/:id`; для них `title` может быть `ResolveFn<string>`, если нужны динамические хлебные крошки.

Объясни пользователю, что собираешься сделать, до начала изменений.

## Выполни Миграцию

1. Создай `apps/front/src/app/products/MODULE/MODULE.menu.ts`. Если есть
   вложенные группы, создай отдельный `FeatureMenuConfig` для каждой группы в
   `apps/front/src/app/products/MODULE/routes/GROUP/group.menu.ts`.
2. В каждом файле с `[PermissionGuard]` замени его на `[permissionGuard]`,
   добавь `title` и `data: SecureRouteMetadata`. Путь импорта зависит от
   уровня вложенности.
3. Для роутов с кастомными гвардами добавь только `title`; гвард не трогай.
4. В `apps/front/src/app/configs/menu/app-menu-registry.config.ts` добавь
   импорт и новый конфиг в `APP_MENU_REGISTRY`.
5. В старом `apps/front/src/app/configs/menu/MODULE.menu.ts` оставь только
   deprecated якорный пункт верхнего уровня с пустым `children: []`.
6. Удали из `apps/front/src/app/configs/routes-permissions.config.ts` все
   записи этого модуля.

Полные образцы см. в [EXAMPLES.md](EXAMPLES.md).

## Что Не Делать

- Не заменять кастомные доменные гварды на `permissionGuard`.
- Не удалять якорный пункт из `configs/menu/MODULE.menu.ts` — только `children: []`.
- Не трогать `APP_MENU` в `app-menu.config.ts`.
- Не переименовывать enum-файлы без необходимости.
- Не придумывать нового, если не знаешь.

## Проверка После Изменений

1. Новый конфиг добавлен в `app-menu-registry.config.ts`.
2. `products/MODULE/MODULE.menu.ts` создан, данные верны.
3. В каждом файле роутов generic `[PermissionGuard]` заменён, у кастомных
   гвардов добавлен только `title`.
4. Записи модуля удалены из `routes-permissions.config.ts`.
5. В старом `configs/menu/MODULE.menu.ts` установлено `children: []` и есть
   `@deprecated`.
6. Запусти `npx tsc --noEmit` в `apps/front`.

## Эталон Для Сравнения

Полностью мигрированный модуль Communicator:

- `apps/front/src/app/products/cdp/routes/communicator/communicator.menu.ts`;
- `apps/front/src/app/products/cdp/routes/communicator/communicator.routes.ts`;
- `apps/front/src/app/products/cdp/cdp.menu.ts`.
