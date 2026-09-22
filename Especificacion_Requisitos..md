# Guía de redacción de requisitos

> Material de consulta · Ingeniería de Software I · SIS3407  
> Este documento no se entrega. Es la referencia para escribir los requisitos de tu proyecto y para revisar los de tu dupla. La plantilla que sí se llena y se entrega es `especificacion-requisitos.md`.

## 1. Cómo se nombra un requisito

Cada requisito tiene un identificador que nunca cambia y nunca se reutiliza. Si un requisito se elimina, su identificador queda muerto: no se le asigna a otro.

Esa estabilidad no es burocracia. Es lo que permite escribir “el cambio afecta a RF-007 y RNF-SEG-002” sin repetir el texto completo, y es lo que va a hacer posible el análisis de impacto de la semana 15.

### Requisitos funcionales

`RF-###`

Ejemplos: `RF-001`, `RF-002`, `RF-003`… Numeración consecutiva, sin agrupar por módulo.

### Requisitos no funcionales

`RNF-<ATRIBUTO>-###`

| Atributo de calidad | Clave | Ejemplo |
|---|---|---|
| Rendimiento | REN | RNF-REN-001 |
| Seguridad | SEG | RNF-SEG-001 |
| Usabilidad | USA | RNF-USA-001 |
| Confiabilidad | CON | RNF-CON-001 |
| Mantenibilidad | MAN | RNF-MAN-001 |
| Escalabilidad | ESC | RNF-ESC-001 |

## 2. Cómo se redacta un requisito funcional

```text
El sistema <verbo firme> <objeto> <condición o restricción>
```

**Verbos firmes:** registra, calcula, notifica, impide, genera, valida, muestra, envía, asigna.

**Evitar:** debería, podría, de preferencia, permite que el usuario pueda, tratará de.

✅ El sistema impide agendar dos citas en el mismo horario con el mismo veterinario.

❌ El sistema debería tratar de evitar empalmes en la agenda.

## 3. Cómo se redacta un requisito no funcional

```text
<Sujeto> <comportamiento esperado> <métrica o condición medible>
```

Todo requisito no funcional necesita un número, un límite o una condición comprobable. Si no lo tiene, todavía no es un requisito: es una aspiración.

✅ El historial completo de un paciente se despliega en menos de tres segundos.

❌ El sistema debe ser rápido al consultar el historial.

## 4. Reglas que aplican a todos

- Una sola idea por requisito. Si aparece una “y” que une dos comportamientos distintos, son dos requisitos.
- Describe qué debe pasar, no cómo implementarlo. La solución técnica se decide en la Unidad 3, no aquí.
- Debe poder convertirse en una prueba. Si no hay forma de comprobar si se cumplió, reescríbelo.
- Debe caber en el alcance definido en la Visión del producto. Si no cabe, va a una lista de ideas futuras, no al documento.
- Dos personas distintas deben entenderlo igual. Si tu dupla lo interpretó de otra forma, el requisito está mal escrito. No es cuestión de quién tiene razón.

## 5. Los campos de cada ficha

### Requisito funcional

| Campo | Qué va ahí |
|---|---|
| Descripción | El requisito redactado con la fórmula del punto 2. |
| Origen | De dónde salió: entrevista y fecha, documento revisado, observación, o supuesto propio. |
| Prioridad | Imprescindible, importante o deseable. |
| Criterio de aceptación | Cómo se comprueba que se cumplió. Redáctalo como si fuera una prueba. |
| Relacionado con | Otros requisitos con los que se conecta, depende o entra en conflicto. |

### Requisito no funcional

| Campo | Qué va ahí |
|---|---|
| Atributo de calidad | Cuál de los seis atributos representa. |
| Descripción | El requisito redactado con la fórmula del punto 3. |
| Métrica | El valor o condición que se mide, y bajo qué circunstancias. |
| Origen | De dónde salió, incluyendo si se derivó del tipo de sistema. |
| Prioridad | Imprescindible, importante o deseable. |
| Por qué importa | Qué pasa si no se cumple. Es lo que justifica el límite elegido. |
| Afecta a | Qué requisitos funcionales quedan condicionados por este. |

## 6. Sobre el campo Origen

Es el campo más importante del curso y el que más se descuida.

Sirve para distinguir tres cosas que se ven iguales en el documento pero no lo son:

- Lo que el cliente confirmó explícitamente.
- Lo que dedujimos de un documento o de observar el proceso.
- Lo que estamos suponiendo porque nos pareció obvio.

Un requisito con origen “supuesto propio” no está mal por serlo. Está mal cuando nadie sabe que lo es, porque entonces se trata como verdad confirmada y nunca se valida.

Después de la entrevista de elicitación, revisa cuántos de tus supuestos sobrevivieron.

## 7. Los seis defectos más comunes

| Defecto | Cómo se ve | Cómo se arregla |
|---|---|---|
| Adjetivo sin medida | “Debe ser rápido” | Sustituir por un número. |
| Dos en uno | “Seguro y fácil de usar” | Separar en dos requisitos. |
| Solución disfrazada | “Debe usar una base de datos en la nube” | Escribir la necesidad, no la técnica. |
| Condicional vago | “Debería”, “de preferencia” | Usar formulación firme. |
| Sin criterio | No se sabe cómo comprobarlo | Agregar criterio de aceptación. |
| Ambiguo | Dos lecturas posibles | Reescribir hasta que solo haya una. |

## 8. Lista de verificación para revisar

Úsala para revisar tu propio documento y el de tu dupla.

- [ ] Todos los requisitos tienen identificador único y ninguno está repetido.
- [ ] Cada requisito expresa una sola idea.
- [ ] Cada requisito funcional tiene criterio de aceptación comprobable.
- [ ] Cada requisito no funcional tiene una métrica, no solo un adjetivo.
- [ ] El campo Origen distingue lo confirmado de lo supuesto.
- [ ] Hay al menos un requisito no funcional por cada atributo de calidad que impone el tipo de sistema.
- [ ] Ningún requisito impone una solución técnica.
- [ ] Todos los requisitos caben dentro del alcance declarado.
- [ ] No hay dos requisitos que se contradigan entre sí.
- [ ] Ningún requisito se puede interpretar de dos maneras distintas.
