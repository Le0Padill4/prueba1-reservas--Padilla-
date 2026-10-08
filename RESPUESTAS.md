## Escenarios que elegí y por qué

Elegí el cruce parcial porque `specs/001-reservas-sala/spec.md:39` y `specs/001-reservas-sala/spec.md:40` muestran una segunda reserva de la misma sala que empieza antes de terminar la primera y debe rechazarse. Elegí horarios contiguos porque `specs/001-reservas-sala/spec.md:42` y `specs/001-reservas-sala/spec.md:43` muestran que el límite de las 10:00 queda disponible. Elegí otra sala porque `specs/001-reservas-sala/spec.md:44` y `specs/001-reservas-sala/spec.md:45` muestran que compartir horario no impide reservar una sala distinta.

## Riesgo más grave del repositorio

El riesgo más grave es que la pantalla evita el caso de uso: `lib/presentation/reserva_page.dart:103` muestra que el botón llama a `_reservar`, y `lib/presentation/reserva_page.dart:60` muestra que ese método inserta directamente en Supabase. Aunque `lib/main.dart:26` entrega `CrearReserva` a la página, la inserción no pasa por su comprobación de solapamiento, ubicada en `lib/domain/crear_reserva.dart:17`.

## ¿La regla protege la app real?

No protege el flujo actual de la pantalla: `lib/domain/crear_reserva.dart:18` comprueba el cruce solo cuando se usa el caso de uso, mientras `lib/presentation/reserva_page.dart:60` guarda la reserva directamente. Además, `supabase/migracion.sql:11` muestra una validación de orden entre inicio y fin, sin una restricción de solapamiento en esa definición de tabla. Por ello, la pantalla puede registrar reservas superpuestas.
