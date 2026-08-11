# Примеры Добавления Раздела

## Роут

```ts
{
  path: 'NEW_PATH',
  title: 'Название раздела',
  data: {
    permissionsView: [EPermissions.PERMISSION_KEY],
    // useAppPermissions: true — только если право типа "приложение"
  } satisfies SecureRouteMetadata,
  loadComponent: () => import('./pages/new-section/new-section.component'),
  canActivate: [permissionGuard],
}
```

## Пункт Меню

```ts
{
  path: `${MODULE_PATH}/NEW_PATH`,
  title: 'Название раздела',
  permissionsView: [EPermissions.PERMISSION_KEY],
  // useAppPermissions: true — если нужно
}
```
