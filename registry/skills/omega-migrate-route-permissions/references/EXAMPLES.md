# Примеры Миграции Прав Роутов

## FeatureMenuConfig

Проверь точное имя enum: в проекте встречаются оба стиля —
`AdministrationRoutes` и `EAdministrationRoutes`.

```ts
import { AppRoutes } from '../../enums/routes/app-routes.enum';
import { EModuleRoutes } from './routes/module-routes.enum';
import { EPermissions } from '../../enums/permissions.enum';
import { FeatureMenuConfig } from '../../features/layout/interfaces/feature-menu-config.interface';

const MODULE_PATH = AppRoutes.MODULE;

export const MODULE_MENU_CONFIG: FeatureMenuConfig = {
  parentId: MODULE_PATH,
  items: [
    {
      path: `${MODULE_PATH}/ROUTE_PATH`,
      title: 'Название из старого конфига',
      permissionsView: [EPermissions.PERMISSION_KEY],
    },
  ],
};
```

## Generic PermissionGuard

```ts
// Было:
{
  path: EModuleRoutes.SECTION,
  loadComponent: () => import('...'),
  canActivate: [PermissionGuard],
}

// Стало:
{
  path: EModuleRoutes.SECTION,
  title: 'Название раздела',
  data: {
    permissionsView: [EPermissions.PERMISSION_KEY],
  } satisfies SecureRouteMetadata,
  loadComponent: () => import('...'),
  canActivate: [permissionGuard],
}
```

## Кастомный Доменный Гвард

```ts
{
  path: EModuleRoutes.SECTION,
  title: 'Название раздела', // добавляем
  loadComponent: () => import('...'),
  canActivate: [CustomDomainGuard], // не трогаем
}
```

## Deprecated Якорный Конфиг

```ts
/**
 * @deprecated Дочерние пункты меню перенесены в products/MODULE/MODULE.menu.ts
 * и зарегистрированы через MenuRegistryService (APP_MENU_REGISTRY).
 * Будет удалён в финальной задаче ЭПИКА (Cleanup).
 */
export const MODULE_MENU: ISideMenuItem[] = [
  {
    label: 'Название модуля',
    icon: 'icon-name',
    path: AppRoutes.MODULE,
    children: [],
  },
];
```
