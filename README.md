# 🏆 Sistema de Gestión - Torneo Interfacultades de Fútbol 5 (UCU)
**Trabajo Obligatorio – Base de Datos 1 | Universidad Católica del Uruguay (Segundo Semestre 2026)**

---

## 1. Descripción del Proyecto
Sistema de información unificado desarrollado para la gestión integral del **Torneo Interfacultades de Fútbol 5 de la UCU**. Su objetivo es automatizar y reemplazar el uso de planillas de cálculo y grupos de mensajes, garantizando la trazabilidad de inscripciones, control estricto del fixture, gestión de planillas de partido, cálculo automático de sanciones y la generación de métricas analíticas avanzadas.

---

## 2. Reglas de Negocio y Funcionalidades
- **Etapas del Torneo:** Inscripción, En curso y Finalizado.
- **Formato:** Liga a una sola rueda por fechas (todos contra todos una única vez).
- **Horarios y Complejos:** Partidos organizados en turnos de 1 hora entre las 18:00 y las 23:00.
- **Equipos y Planteles:**
  - Planteles de **5 a 10 jugadores** pertenecientes a la misma facultad.
  - Máximo **2 docentes/funcionarios** por plantel (el resto estudiantes de grado o posgrado).
  - Un jugador solo puede integrar un equipo por edición. Capitán obligatorio.
- **Validaciones del Fixture y Partidos:**
  - Control de planteles mínimos (5 jugadores) y misma edición.
  - Sin cruces repetidos ni superposición de equipos en la misma fecha.
  - Restricción de canchas (sin partidos superpuestos en el mismo día y turno).
  - Modificación de resultados permitida únicamente dentro de las **48 horas** de disputado el encuentro.
- **Sistema de Puntuación y Posiciones:** 
  - 3 puntos por ganado, 1 por empate, 0 por derrota.
  - Desempates: Puntos -> Diferencia de goles -> Goles a favor.
- **Sanciones Automáticas y Disciplinarias:**
  - Tarjeta roja directa o acumulación de **3 tarjetas amarillas** genera automáticamente 1 fecha de suspensión (cumple en la fecha siguiente).
  - Soporte para sanciones disciplinarias adicionales impuestas por la organización.

---

## 3. Roles de Usuario y Seguridad
- **Administrador:** Gestión total (ABM de jugadores, equipos, complejos, canchas, partidos, sanciones y consultas).
- **Planillero:** Registro exclusivo de planillas de juego, goles y tarjetas.
*(Implementado mediante permisos y seguridad a nivel de base de datos MySQL).*

---

## 4. Modelo de Datos (Esquema Relacional)
El sistema se estructura sobre una base de datos relacional MySQL compuesta por las siguientes tablas principales:
- `login` (correo, contraseña, rol)
- `facultad` (id_facultad, nombre)
- `jugador` (ci, nombre, apellido, email, fecha_nacimiento, id_facultad, tipo)
- `torneo` (id_torneo, nombre, anio, semestre, fecha_inicio, fecha_fin, estado)
- `equipo` (id_equipo, nombre, id_torneo, id_facultad, ci_capitan)
- `equipo_jugador_torneo` (id_equipo, ci_jugador, id_torneo, numero_camiseta)
- `complejo` (id_complejo, nombre, direccion, departamento)
- `cancha` (nombre_cancha, id_complejo, superficie, techada)
- `turno` (id_turno, hora_inicio, hora_fin)
- `partido` (id_partido, id_torneo, numero_fecha, id_equipo_local, id_equipo_visitante, dia, id_turno, nombre_cancha, id_complejo, estado)
- `planilla_partido` (id_partido, ci_jugador, id_equipo)
- `gol` (id_gol, id_partido, ci_jugador, minuto, autogol)
- `tarjeta` (id_tarjeta, id_partido, ci_jugador, minuto, color)
- `sancion_jugador` (id_sancion, ci_jugador, id_torneo, motivo, numero_fecha_inicio, cantidad_fechas)

---

## 5. Reportes Analíticos Disponibles
1. **Tabla de posiciones** (PJ, PG, PE, PP, GF, GC, DG, Pts).
2. **Tabla de goleadores** (Top 10).
3. **Tabla de fair play** por equipo (1 pt por amarilla, 3 pts por roja).
4. **Promedio de goles** por partido (por fecha y total de la edición).
5. **Canchas y turnos** más utilizados.
6. **Demografía de jugadores** por facultad y por tipo (grado, posgrado, docente, funcionario).
7. **Inscriptos fantasma** (jugadores en plantel que nunca jugaron).
8. **Sanciones** agrupadas por equipo y motivo.
9. **Estadísticas de partidos** (Porcentaje jugados vs. postergados/cancelados).
10. Consultas analíticas adicionales propuestas por el equipo.

---

## 6. Requerimientos Técnicos
- **Backend:** Python (sin uso de ORM).
- **Base de Datos:** MySQL (con control de accesos por roles).
