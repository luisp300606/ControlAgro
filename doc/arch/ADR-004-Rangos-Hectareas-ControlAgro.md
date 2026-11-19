# ADR-004 · Rangos de hectáreas y AsignacionDeCultivo en lugar de parcelas

## 1. Estado
Propuesta. 19 de noviembre de 2026. Pasa a Aceptada cuando el cliente confirme el cambio de parcela a rango.

## 2. Contexto
La especificación v1.1 habla de "parcelas" (42 menciones) y el cliente respondió en la entrevista en términos de parcela. Después, al diseñar el prototipo, se decidió que la finca se divide en **rangos de hectáreas** editables (por ejemplo, 1–120 con un cultivo y el resto con otro; se pueden agregar o quitar). El prototipo, el diagrama de clases y los diagramas C4 ya usan rangos; la especificación no.

## 3. Decisión
La finca se modela con `RangoDeHectareas` (hectárea inicial y final, sin traslape). El cultivo de un rango depende de la temporada, así que se guarda en `AsignacionDeCultivo` (rango + temporada + cultivo opcional). `Aplicacion` se liga a la asignación, no al rango.

## 4. Alternativas consideradas
- **Mantener parcelas fijas.** Descartada por el cliente al pedir hectáreas editables.
- **Cultivo como atributo del rango.** Descartada: no puede representar un cultivo distinto por temporada ni el historial (RF-009).

## 5. Consecuencias
**Positivas:** el historial por temporada sale solo; la restricción de un cultivo por rango y temporada se expresa con una asignación única (RF-002).
**Negativas:** una clase más; hay que actualizar la especificación y los casos de uso (ver `doc/cambios/ai-001-parcela-a-rango.md`). Mientras tanto hay una inconsistencia entre documentos.
**Revisar si:** el cliente necesita subdivisiones que no sean rangos contiguos (por ejemplo, lotes no contiguos).

## 6. Trazabilidad
RF-002, RF-003, RF-009 · CU-06 · `RangoDeHectareas`, `AsignacionDeCultivo`, `Temporada`, `Cultivo` · componente Finca.
