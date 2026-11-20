# Análisis de impacto de SC-001

**Autor:** Luis Pablo Apodaca · **Fecha:** 19 de noviembre de 2026

## Solicitud que se analiza

Cambio C-01, de parcela a rango de hectáreas. Lo origina una decisión de diseño propia, tomada al construir el prototipo; no hay solicitud externa.

Fecha de la decisión: durante el prototipo. **[PENDIENTE: confirmar si el cliente lo pidió o lo validó]**

## Traducción a términos del sistema

- **En palabras de quien lo pide:** "las hectáreas de la finca deben poder dividirse y cambiarse; por ejemplo, de la 1 a la 120 un cultivo y el resto otro, y poder agregar o quitar".
- **En términos del sistema:** la finca deja de ser un conjunto de parcelas fijas. Pasa a ser una lista de `RangoDeHectareas` (hectárea inicial y final, sin traslape). El cultivo de un rango depende de la temporada, así que se guarda en `AsignacionDeCultivo` (rango + temporada + cultivo opcional). `Aplicacion` se liga a la asignación, no a una parcela. En requisitos, donde dice "parcela" pasa a decir "rango de hectáreas".

## Impacto directo

| Artefacto | Unidad | Qué hay que cambiar |
|---|---|---|
| Requisito RF-001 | 2 | "parcela" → "rango de hectáreas" en descripción, origen y criterio |
| Requisito RF-002 | 2 | Criterio: un rango no admite un segundo cultivo en la misma temporada |
| Requisito RF-003 | 2 | Totales por rango, cultivo y temporada |
| Requisito RF-009 | 2 | "Marcar rango sin cultivo en una temporada" |
| Requisito nuevo RF-010 | 2 | **[PENDIENTE: ¿se crea?]** Gestionar rangos sin traslape. En la v1.2 se cubrió con el flujo alterno 1a de CU-06, pero ningún requisito lo respalda todavía |
| Sección 2 y trazabilidad de la especificación | 2 | Reemplazar "parcela" (42 menciones en total) |
| Caso de uso CU-01 | 2 | Precondición y escenario: "parcela" → "rango" |
| Caso de uso CU-06 | 2 | Renombrar a "Gestionar rangos de hectáreas y cultivo" y agregar el flujo alterno 1a (agregar, editar o quitar un rango sin traslape). Hecho en v1.2 |
| Diagrama de clases | 4 | Ya aplicado en v1.1: `RangoDeHectareas` y `AsignacionDeCultivo` |
| Diagrama de secuencia | 4 | Ya aplicado: usa `AsignacionDeCultivo.vigenteEn` |

## Impacto indirecto

| Artefacto | Unidad | Por qué se ve afectado |
|---|---|---|
| ADR-004 | 3 | Es la decisión que registra este cambio. Pasa de Propuesta a Aceptada cuando se apruebe |
| ADR-001 monolito modular | 3 | Sin impacto: el cambio vive dentro del módulo Finca |
| ADR-002 Strategy de roles | 3 | Sin impacto. **[PENDIENTE: confirmar quién puede gestionar rangos; hoy `Accion.GESTIONAR_RANGOS` lo cubre]** |
| ADR-003 Observer | 3 | Sin impacto |
| Diagrama de contexto C4 | 3 | Sin impacto: el cambio no añade actores ni sistemas |
| Diagrama de contenedores C4 | 3 | Sin impacto |
| Diagrama de componentes C4 | 3 | Sin impacto: Finca ya habla de rangos |
| Máquina de estados de `Aplicacion` | 4 | Sin impacto |
| Diagrama de actividad de CU-01 | 4 | Sin impacto, salvo la etiqueta "rango" que ya usa |
| Prototipo (`prototipo.html`) | 2 | Los textos del código conservan la palabra "parcelas" |
| Datos de prueba | 4 | Si hubiera datos cargados por parcela, habría que migrarlos. **[PENDIENTE: ¿hay datos cargados?]** |

## Lo que queda sin tocar

- Módulos Acceso, Inventario y Aplicaciones, y la regla del sobrante (RNF-INT-001).
- `Producto`, `Lote`, `Sobrante`, `Usuario`, `PoliticaDeAcceso`, `Correccion`.
- Las respuestas de la entrevista, que se conservan citadas tal cual (dicen "parcela").
- La tecnología propuesta (Node.js, PostgreSQL).

## Riesgos

| Riesgo | Probabilidad | Mitigación |
|---|---|---|
| Rangos que se traslapan | Media | `RangoDeHectareas.seTraslapaCon` al crear o editar |
| Rangos que suman más de 200 ha (en el ejemplo de Stitch aparece 121–235) | Alta, ya ocurrió en el ejemplo | Validar contra la superficie de la finca. **[PENDIENTE: confirmar superficie y rangos reales]** |
| El cliente validó "parcela" y no "rango" | Media | Mostrar el prototipo con rangos en la próxima revisión semanal |
| Dejar la especificación sin actualizar | Alta si no se hace | Subir a v1.2 con fila en el registro de cambios |
| No hacerlo: el diseño y la especificación se contradicen | Cierta | Es el motivo de este análisis |

## Alternativas consideradas

1. **Rangos de hectáreas con `AsignacionDeCultivo` (la adoptada).** Cuesta una clase más y actualizar la especificación. Da el historial por temporada y permite editar hectáreas.
2. **Mantener parcelas fijas.** No cuesta nada ahora, pero no permite agregar o quitar hectáreas, que es lo que se pidió. Descartada.
3. **Cultivo como atributo del rango, sin asignación.** Una clase menos, pero no puede guardar un cultivo distinto por temporada ni el historial (RF-009). Descartada.

Me inclino por la 1.

## Estimación de esfuerzo

| Concepto | Esfuerzo |
|---|---|
| Cambios de diseño y documentación | 3 cambios pequeños (S) y 1 mediano (M). |
| **Total** | **12 Horas** |

