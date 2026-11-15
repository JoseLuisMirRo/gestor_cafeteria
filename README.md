# gestor_cafeteria

App Flutter de una cafetería (proyecto de la materia Desarrollo Móvil Integral, 10.º A). Por ahora tiene la capa de red: un cliente HTTP (`ApiClient`) que consume el servidor de pedidos y convierte cada error en un `Failure` tipado (`NotFoundFailure`, `ConflictFailure`, `ValidationFailure`, `ServerFailure`, `NetworkFailure` o `UnexpectedFailure`), con pruebas que no necesitan servidor.

## Requisitos

- Flutter estable (probado con 3.47.2) y Dart incluido.
- Node.js 22.13 o superior (`node --version`) para el servidor.
- Un emulador de Android, Chrome o un celular para ejecutar la app.

## Servidor (mini_backend)

El servidor es **otro repositorio**, no está dentro de este proyecto: <https://github.com/DMI-2026/mini_backend> (rama `master`). Se probó con el commit `94d2533` (8 oct 2026).

```bash
git clone https://github.com/DMI-2026/mini_backend.git
cd mini_backend
node server.js          # no hace falta npm install
```

Debe mostrar `Listening on http://0.0.0.0:8000`. Comprueba <http://localhost:8000/health>: responde `{"status":"ok"}`.

## Ejecutar la app

```bash
flutter pub get
flutter run -d chrome   # usa http://localhost:8000
```

- En el emulador de Android la app usa `http://10.0.2.2:8000` automáticamente.
- Para otra dirección (por ejemplo, un celular físico en la misma red) indica la URL con `API_URL`:

```bash
flutter run --dart-define=API_URL=http://<IP de tu computadora>:8000
```

## Pruebas

```bash
flutter analyze   # sin avisos
flutter test      # usa MockClient: no necesita el servidor
```

## Estructura

```
lib/
  core/failure.dart        jerarquía de fallos (clase sellada Failure)
  core/http_client.dart    ApiClient: GET, POST, PATCH, DELETE y mapeo de errores
  main.dart                pantalla de prueba con la lista de pedidos
test/core/
  failure_test.dart        pruebas de la jerarquía de fallos
  http_client_test.dart    pruebas de ApiClient con MockClient
```
