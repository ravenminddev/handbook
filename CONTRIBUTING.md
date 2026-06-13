# Contributing

Toda propuesta de cambio a este handbook se hace mediante **Pull Request** desde un fork o una rama.

## Flujo

1. Crear una rama con nombre descriptivo: `docs/<seccion>-<cambio-corto>`.
   - Ejemplos: `docs/normativas-agregar-vacaciones`, `docs/onboarding-corregir-telefono`.
2. Realizar los cambios respetando la estructura y el formato de cada documento.
3. Abrir un Pull Request usando la [plantilla](.github/PULL_REQUEST_TEMPLATE.md).
4. Esperar revisión de al menos un cofundador.
5. Una vez aprobado, hacer merge a `main`.

## Convenciones de formato

- Markdown estándar (CommonMark + GFM).
- Encabezados en español, sin punto final.
- Listas ordenadas con `1.`, `2.`, `3.`.
- Usar enlaces relativos a otros archivos del repo: `[README](README.md)`.
- Indentación: 2 espacios (ver `.editorconfig`).

## Reglas mínimas

- Un PR = un cambio temático. No mezclar cambios no relacionados.
- Si vas a eliminar o reemplazar una sección, documenta el motivo en la descripción del PR.
- Las nuevas secciones deben enlazarse desde el README raíz en el índice correspondiente.
