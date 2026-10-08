# Feature Specification: Reservas de sala

**Feature Branch**: `001-reservas-sala`
**Created**: 2026-10-01
**Status**: Draft

## User Scenarios & Testing

### User Story 1 — Reservar una sala (Priority: P1)

Como estudiante autenticado, quiero reservar una sala de estudio por un intervalo de tiempo
para tener dónde trabajar con mi grupo.

**Why this priority**: sin reservas, la app no tiene propósito.

**Independent Test**: se puede probar creando una reserva y comprobando que queda registrada.

**Acceptance Scenarios**:

1. **Dado** que la sala A está libre, **Cuando** la reservo de 09:00 a 10:00, **Entonces** la
   reserva queda registrada a mi nombre.
2. **Dado** que elijo como inicio las 10:00 y como fin las 09:00, **Cuando** intento reservar,
   **Entonces** la reserva se rechaza con el mensaje "La hora de fin debe ser posterior a la de inicio".

### User Story 2 — Evitar solapamientos (Priority: P1)

Como estudiante autenticado, quiero que una sala no se reserve por dos personas durante el mismo
intervalo para poder contar con el espacio que reservé.

**Why this priority**: una sala no puede atender dos reservas simultáneas.

**Independent Test**: se puede probar intentando crear reservas con distintos intervalos y salas.

Cada reserva tiene una fecha y hora de inicio y una fecha y hora de fin. En los escenarios
siguientes, todas las horas corresponden al mismo día.

**Acceptance Scenarios**:

1. **Dado** que la Sala A está reservada de 09:00 a 10:00, **Cuando** intento reservar la Sala A
   de 09:30 a 10:30, **Entonces** la solicitud se rechaza con el mensaje exacto
   "La sala ya está reservada en ese horario".
2. **Dado** que la Sala A está reservada de 09:00 a 10:00, **Cuando** intento reservar la Sala A
   de 10:00 a 11:00, **Entonces** la reserva se acepta.
3. **Dado** que la Sala A está reservada de 09:00 a 10:00, **Cuando** intento reservar la Sala B
   de 09:00 a 10:00, **Entonces** la reserva se acepta.

### Edge Cases

- Dos reservas de la misma sala pueden ser contiguas si una termina justo cuando la otra empieza.

## Requirements

### Functional Requirements

- **FR-001**: El estudiante elige una sala, una hora de inicio y una hora de fin.
- **FR-002**: El estudiante puede enviar una solicitud de reserva con la sala y el intervalo
  elegidos.
- **FR-003**: El sistema registra las solicitudes de reserva aceptadas con su sala, estudiante,
  inicio y fin.
- **FR-004**: El instante de fin debe ser posterior al instante de inicio.
- **FR-005**: El sistema rechaza una solicitud para una sala si comienza antes de que termine
  una reserva existente de esa sala y termina después de que esta comienza. Si una reserva termina
  justo cuando la otra empieza, no se considera solapamiento.
- **FR-006**: Una reserva existente de otra sala no causa el rechazo de la solicitud, aunque sus
  horarios coincidan.

### Key Entities

- **Reserva**: sala, estudiante, fecha y hora de inicio y de fin.
- **Sala**: identificada por su nombre (Sala A, Sala B, Sala C).

## Success Criteria

- **SC-001**: Un estudiante completa una reserva en menos de 30 segundos.
