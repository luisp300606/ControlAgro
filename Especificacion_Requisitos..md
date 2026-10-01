# Especificación de requisitos

**Sistema:** Sistema de control de agroquímicos
**Autor:** Luis
**Versión:** 1.1
**Fecha de la última actualización:** 29 de septiembre de 2026

---

## 1. Propósito y alcance

**Propósito del documento:** Definir de forma detallada y verificable los requisitos funcionales y no funcionales del sistema de control de agroquímicos, para que sirvan de base al diseño, al prototipo y a las pruebas de aceptación con Agrícola Barceló.

**Alcance del sistema:**
- Registro de aplicaciones de agroquímicos: producto, cantidad, fecha, parcela, cultivo y responsable.
- Registro de lotes de producto comprados (cantidad disponible), a nivel finca.
- Corrección de un registro de aplicación, con autorización del encargado y rastro de qué cambió.
- Registro de una aplicación con fecha retroactiva (cuando no hubo forma de capturarla en el momento).
- Catálogo de parcelas y su cultivo asignado por temporada (una parcela = un cultivo por temporada, o sin cultivo si está en descanso).
- Catálogo de productos, donde cada nombre comercial o presentación se trata como un producto independiente, sin alias ni combinación automática.
- Cálculo del total de agroquímico aplicado por parcela, por cultivo y por temporada.
- Cálculo y visualización del sobrante de cada producto a nivel finca.
- Reporte de consumo por producto y por cultivo.
- Control de acceso: el trabajador de campo ve solo lo necesario para registrar su aplicación; el encargado ve los totales.

**Fuera del alcance:**
- Manejo de múltiples fincas o clientes (el sistema es para Agrícola Barceló).
- Cultivos mixtos dentro de la misma parcela en la misma temporada.
- Recomendaciones automáticas de dosis o mezcla de agroquímicos.
- Integración con sensores, drones o dispositivos IoT.
- Gestión de compras a proveedores (cotizaciones, pagos, facturación) — solo se registra el lote recibido, no cómo se adquirió.
- Cumplimiento normativo o reportes para autoridades ambientales.
- Alertas o notificaciones automáticas de stock bajo (el sistema muestra el número, no avisa activamente).

---

## 2. Usuarios y su contexto

| Usuario | Qué hace hoy sin el sistema | Qué espera del sistema |
|---------|-----------------------------|------------------------|
| Trabajador de campo | Aplica el producto por indicación del encargado; no deja ningún registro formal, a veces mide por estimado cuando queda poco producto | Registrar su aplicación rápido, con los datos mínimos (fecha, producto, cantidad, parcela, cultivo) |
| Encargado (Agrícola Barceló, 200 hectáreas, papa y cártamo) | Compra por lote y lo va usando sin medir; revisa la bodega a ojo; solo se entera de que "se acabó" cuando ya no queda nada; vuelve a comprar "más o menos lo mismo" | Saber cuánto se aplicó por parcela y cultivo, cuánto queda disponible de cada producto, y cuándo hace falta comprar de nuevo. Disponible para revisar avances una vez por semana |

**Conflictos identificados entre usuarios:** El trabajador de campo prioriza rapidez al registrar; el encargado necesita el dato exacto para que el reporte sirva. Además, el encargado necesita poder **corregir** registros con errores (confirmado en la entrevista, pregunta 12), pero esa corrección debe quedar controlada — si cualquiera pudiera editar libremente, se perdería la confianza en los totales que el propio sistema existe para dar.

---

## 3. Requisitos funcionales

### 3.1 Resumen

| ID | Nombre | Prioridad | Origen |
|----|--------|-----------|--------|
| RF-001 | Registrar aplicación de agroquímico | Imprescindible | Confirmado en entrevista, pregunta 9 |
| RF-002 | Impedir doble cultivo por parcela y temporada | Imprescindible | Confirmado en entrevista, pregunta 27 |
| RF-003 | Calcular totales por parcela, cultivo y temporada | Imprescindible | Confirmado en entrevista, pregunta 31 |
| RF-004 | Calcular y actualizar sobrante por producto | Imprescindible | Confirmado en entrevista, preguntas 16-19, 28, 31 |
| RF-005 | Generar reporte de consumo por producto y cultivo | Deseable | Confirmado en entrevista, pregunta 31 |
| RF-006 | Registrar lote de producto comprado | Imprescindible | Confirmado en entrevista, preguntas 16-17, 22 |
| RF-007 | Corregir un registro de aplicación | Deseable | Confirmado en entrevista, pregunta 12 |
| RF-008 | Registrar aplicación con fecha retroactiva | Deseable | Confirmado en entrevista, pregunta 26 |
| RF-009 | Marcar parcela sin cultivo en una temporada | Deseable | Confirmado en entrevista, pregunta 14 |

### 3.2 Fichas

#### RF-001 · Registrar aplicación de agroquímico

| Campo | Contenido |
|-------|-----------|
| Descripción | El sistema registra una aplicación de agroquímico con fecha, producto, cantidad, parcela, cultivo y nombre de quien la aplicó. |
| Origen | Confirmado en entrevista, pregunta 9: "Fecha, producto, cantidad exacta, parcela, cultivo y nombre de quien lo aplicó." |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al guardar una aplicación con los seis datos, esta aparece asociada a la parcela y cultivo correspondientes. Si falta alguno, el sistema no guarda y señala cuál falta. El sistema permite registrar más de una aplicación en la misma parcela el mismo día, cada una con su propio responsable. |
| Relacionado con | RF-003, RF-004, RF-007, RF-008, RNF-USA-001 |

#### RF-002 · Impedir doble cultivo por parcela y temporada

| Campo | Contenido |
|-------|-----------|
| Descripción | El sistema impide asignar un segundo cultivo a una parcela mientras ya tenga uno activo en la misma temporada. |
| Origen | Confirmado en entrevista, pregunta 27: "No. Cada parcela tiene un solo cultivo por temporada." |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al intentar asignar un segundo cultivo a una parcela con uno ya activo en esa temporada, el sistema rechaza la operación. |
| Relacionado con | RF-001, RF-003, RF-009 |

#### RF-003 · Calcular totales por parcela, cultivo y temporada

| Campo | Contenido |
|-------|-----------|
| Descripción | El sistema calcula el total de agroquímico aplicado por parcela, por cultivo y por temporada. |
| Origen | Confirmado en entrevista, pregunta 31: "Es importante que el sistema permita saber cuánto producto se aplicó por parcela y cultivo." |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al registrar tres aplicaciones conocidas en una misma parcela, el total mostrado coincide con la suma manual de esas tres aplicaciones. |
| Relacionado con | RF-001, RF-005 |

#### RF-004 · Calcular y actualizar sobrante por producto

| Campo | Contenido |
|-------|-----------|
| Descripción | El sistema actualiza el sobrante de cada producto después de cada aplicación registrada, restando la cantidad aplicada de la cantidad disponible a nivel finca. |
| Origen | Confirmado en entrevista, preguntas 16-19 y 28: el sobrante se controla por finca en general, porque un mismo lote se usa en varias parcelas. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Con un lote de 200 litros y una aplicación de 30 litros, el sobrante mostrado es 170. El sistema no permite guardar una aplicación que deje el sobrante en negativo. |
| Relacionado con | RF-001, RF-006, RNF-INT-001 |

#### RF-005 · Generar reporte de consumo por producto y cultivo

| Campo | Contenido |
|-------|-----------|
| Descripción | El sistema genera, para una temporada seleccionada, un reporte con el total de cada producto aplicado agrupado por cultivo, con cantidad y unidad de medida. |
| Origen | Confirmado en entrevista, pregunta 31. |
| Prioridad | Deseable |
| Criterio de aceptación | Al generar el reporte de una temporada con datos conocidos, las cifras agrupadas por cultivo (papa, cártamo) coinciden con lo esperado. |
| Relacionado con | RF-003, RNF-SEG-002 |

#### RF-006 · Registrar lote de producto comprado

| Campo | Contenido |
|-------|-----------|
| Descripción | El sistema registra la entrada de un lote de producto comprado (producto, cantidad y fecha de recepción), sumándolo a la cantidad disponible de ese producto específico a nivel finca. Cada nombre comercial se trata como un producto independiente, sin alias ni combinación automática entre presentaciones distintas del mismo químico. |
| Origen | Confirmado en entrevista, preguntas 16-17 y 22: el producto se compra por lote; distintas marcas o presentaciones se identifican por su propio nombre comercial, sin unificarlas. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al registrar un lote de 200 litros de un producto con nombre comercial "X", la cantidad disponible de "X" aumenta en 200 litros, sin afectar el sobrante de ningún otro producto con nombre distinto. |
| Relacionado con | RF-004 |

#### RF-007 · Corregir un registro de aplicación

| Campo | Contenido |
|-------|-----------|
| Descripción | El sistema permite corregir un campo de un registro de aplicación ya guardado, previa autorización del encargado. |
| Origen | Confirmado en entrevista, pregunta 12: "Debería poder corregirse, dejando claro qué se cambió y quién lo corrigió. Yo, como encargado, debería autorizarlo." |
| Prioridad | Deseable |
| Criterio de aceptación | Un trabajador de campo no puede corregir un registro sin autorización del encargado. Al intentarlo sin autorización, el sistema rechaza la corrección. |
| Relacionado con | RF-001, RNF-TRZ-001, RNF-TRZ-002, RNF-SEG-002 |

#### RF-008 · Registrar aplicación con fecha retroactiva

| Campo | Contenido |
|-------|-----------|
| Descripción | El sistema permite registrar una aplicación con una fecha anterior a la fecha de captura, para los casos en que se anotó en papel por falta de conexión o dispositivo. |
| Origen | Confirmado en entrevista, pregunta 26: "Debería anotarla en papel y registrarla en el sistema después." |
| Prioridad | Deseable |
| Criterio de aceptación | Al registrar una aplicación indicando una fecha distinta (anterior) a la fecha actual, el sistema la guarda y la refleja correctamente en los totales de la temporada correspondiente a esa fecha. |
| Relacionado con | RF-001 |

#### RF-009 · Marcar parcela sin cultivo en una temporada

| Campo | Contenido |
|-------|-----------|
| Descripción | El sistema permite marcar una parcela como sin cultivo asignado en una temporada. |
| Origen | Confirmado en entrevista, pregunta 14: "Sí podría quedar sin sembrar. Su historial debería indicar que estuvo sin cultivo esa temporada." |
| Prioridad | Deseable |
| Criterio de aceptación | Al no asignar cultivo a una parcela en una temporada, el sistema la muestra como "sin cultivo" en esa temporada, sin afectar el historial de temporadas previas. |
| Relacionado con | RF-002 |

---

## 4. Requisitos no funcionales

### 4.1 Resumen

| ID | Atributo | Nombre | Prioridad | Origen |
|----|----------|--------|-----------|--------|
| RNF-USA-001 | Usabilidad | Registro rápido en campo | Imprescindible | Derivado del tipo de sistema y del perfil de uso del trabajador de campo |
| RNF-INT-001 | Integridad | Sobrante nunca negativo | Imprescindible | Confirmado en entrevista, pregunta 25 |
| RNF-TRZ-001 | Trazabilidad | Registro no editable sin autorización | Imprescindible | Confirmado en entrevista, pregunta 12 |
| RNF-TRZ-002 | Trazabilidad | Historial de cambios visible | Deseable | Confirmado en entrevista, pregunta 12 |
| RNF-SEG-002 | Seguridad | Control de acceso por rol | Deseable | Confirmado en entrevista, pregunta 23 |

### 4.2 Fichas

#### RNF-USA-001 · Registro rápido en campo

| Campo | Contenido |
|-------|-----------|
| Atributo de calidad | Usabilidad |
| Descripción | Un usuario completa el formulario de registro de aplicación en dos minutos. |
| Métrica | Tiempo entre abrir el formulario y guardar el registro, medido con un trabajador de campo real en condiciones normales. |
| Origen | Derivado del tipo de sistema (de información) y del perfil de uso del trabajador de campo, que registra con las manos ocupadas en pleno trabajo de campo. |
| Prioridad | Imprescindible |
| Por qué importa | Si el registro es lento, el trabajador deja de usarlo y el sistema pierde su fuente principal de datos. |
| Afecta a | RF-001 |

#### RNF-INT-001 · Sobrante nunca negativo

| Campo | Contenido |
|-------|-----------|
| Atributo de calidad | Integridad de datos |
| Descripción | El sistema impide guardar una aplicación cuyo resultado dejaría el sobrante de un producto en negativo. |
| Métrica | Al intentar registrar una cantidad mayor al sobrante disponible, el sistema rechaza la operación el 100% de las veces. |
| Origen | Confirmado en entrevista, pregunta 25: si no queda producto, se detiene la aplicación y se compra de emergencia. |
| Prioridad | Imprescindible |
| Por qué importa | Un sobrante negativo o incorrecto invalida el propósito principal del sistema: evitar el desperdicio de agroquímico. |
| Afecta a | RF-004, RF-006 |

#### RNF-TRZ-001 · Registro no editable sin autorización

| Campo | Contenido |
|-------|-----------|
| Atributo de calidad | Trazabilidad |
| Descripción | El sistema impide modificar un registro de aplicación sin una corrección autorizada por el encargado. |
| Métrica | El 100% de los intentos de modificación sin autorización son rechazados por el sistema. |
| Origen | Confirmado en entrevista, pregunta 12. |
| Prioridad | Imprescindible |
| Por qué importa | Sin esto, un dato podría manipularse sin control, y se perdería la confianza en el sobrante calculado. |
| Afecta a | RF-001, RF-007 |

#### RNF-TRZ-002 · Historial de cambios visible

| Campo | Contenido |
|-------|-----------|
| Atributo de calidad | Trazabilidad |
| Descripción | El sistema conserva el valor anterior de un campo corregido, visible en el historial del registro. |
| Métrica | El 100% de las correcciones quedan asociadas a quién las autorizó y al valor anterior del campo modificado. |
| Origen | Confirmado en entrevista, pregunta 12. |
| Prioridad | Deseable |
| Por qué importa | Sin esto, un error corregido no se puede rastrear, y no queda forma de auditar qué cambió en un registro. |
| Afecta a | RF-007 |

#### RNF-SEG-002 · Control de acceso por rol

| Campo | Contenido |
|-------|-----------|
| Atributo de calidad | Seguridad |
| Descripción | El sistema restringe la vista de totales de consumo y sobrante al rol de encargado; el trabajador de campo solo ve lo necesario para registrar su propia aplicación. |
| Métrica | Un usuario con rol "trabajador de campo" no puede acceder a las pantallas de totales ni de reportes, verificado con intento directo de acceso. |
| Origen | Confirmado en entrevista, pregunta 23: "Los totales los debería ver principalmente el encargado. El trabajador de campo debería ver solo lo necesario para registrar su aplicación." |
| Prioridad | Deseable |
| Por qué importa | Mantiene separada la información operativa de campo de la información estratégica de consumo y compra que solo le corresponde decidir al encargado. |
| Afecta a | RF-003, RF-004, RF-005, RF-007 |

---

## 5. Casos de uso

**Actores:**
- **Trabajador de campo** — aplica el producto y registra su aplicación.
- **Encargado** — decide qué se siembra y qué se aplica, autoriza correcciones, registra lotes y consulta totales y sobrante.

---

### CU-01 · Registrar aplicación de agroquímico

**Actor principal**
Trabajador de campo

**Objetivo**
Dejar constancia de que se aplicó un producto en una parcela, con la cantidad y fecha exactas.

**Precondición**
La parcela ya tiene un cultivo asignado en la temporada activa, y el producto ya existe en el catálogo con cantidad disponible mayor a cero.

**Escenario principal**
1. El trabajador de campo abre el formulario de registro.
2. El sistema muestra la lista de parcelas y productos disponibles.
3. El trabajador elige parcela, producto, cantidad aplicada y fecha.
4. El sistema verifica que la cantidad no deje el sobrante en negativo.
5. El sistema guarda la aplicación asociada a la parcela, el cultivo activo y el responsable.

**Flujos alternos**
4a. La cantidad excede el sobrante disponible: el sistema rechaza el registro y muestra el sobrante real. El trabajador detiene la aplicación.
3a. El trabajador no tuvo acceso al sistema en el momento de aplicar: al capturar después, indica la fecha real de la aplicación en vez de la fecha actual.

**Postcondición**
La aplicación queda registrada, visible en los totales de su parcela y cultivo, y el sobrante del producto queda actualizado.

**Requisitos que realiza**
RF-001, RF-003, RF-004, RF-008, RNF-USA-001, RNF-INT-001

---

### CU-02 · Corregir un registro de aplicación

**Actor principal**
Encargado

**Objetivo**
Corregir un dato erróneo en una aplicación ya guardada, sin perder el rastro del valor original.

**Precondición**
Existe al menos un registro de aplicación ya guardado.

**Escenario principal**
1. El encargado busca la aplicación que necesita corregir.
2. El sistema muestra los datos actuales del registro.
3. El encargado indica el campo a corregir y el nuevo valor.
4. El sistema guarda el valor anterior junto con el nuevo, y registra que el encargado autorizó el cambio.
5. El sistema actualiza los totales y el sobrante afectados por la corrección.

**Flujos alternos**
3a. El campo corregido es la cantidad aplicada: el sistema recalcula el sobrante del producto con el nuevo valor antes de guardar.
4a. El nuevo valor dejaría el sobrante en negativo: el sistema rechaza la corrección y muestra el sobrante real.

**Postcondición**
El registro queda corregido, con el valor anterior visible en su historial de cambios y el nombre de quien autorizó la corrección.

**Requisitos que realiza**
RF-007, RNF-TRZ-001, RNF-TRZ-002, RNF-INT-001

---

### CU-03 · Registrar lote de producto comprado

**Actor principal**
Encargado

**Objetivo**
Sumar al sistema la cantidad disponible de un producto recién comprado, para que quede reflejada en el sobrante de la finca.

**Precondición**
Ninguna — el producto puede ya existir en el catálogo o darse de alta en este mismo caso de uso.

**Escenario principal**
1. El encargado abre el formulario de entrada de lote.
2. El sistema muestra el catálogo de productos existentes.
3. El encargado elige el producto (o lo da de alta si es nuevo), indica la cantidad del lote y la fecha de recepción.
4. El sistema suma la cantidad del lote a la disponible de ese producto.
5. El sistema muestra el nuevo sobrante actualizado del producto.

**Flujos alternos**
3a. El producto no existe en el catálogo: el encargado lo da de alta con su unidad de medida antes de continuar.
3b. El producto tiene una presentación o marca comercial distinta a otra ya registrada: el encargado lo da de alta como un producto independiente en el catálogo, con su propio nombre comercial y su propio sobrante — el sistema no combina cantidades entre nombres distintos.

**Postcondición**
El lote queda registrado y la cantidad disponible del producto refleja la suma del lote recién ingresado.

**Requisitos que realiza**
RF-006, RNF-INT-001

---

### CU-04 · Consultar sobrante de un producto

**Actor principal**
Encargado

**Objetivo**
Saber cuánto queda disponible de un producto en un momento dado, sin necesidad de revisar la bodega físicamente.

**Precondición**
El producto ya tiene al menos un lote registrado.

**Escenario principal**
1. El encargado abre la vista de productos.
2. El sistema muestra la lista de productos con su cantidad disponible y su sobrante actual.
3. El encargado selecciona un producto para ver el detalle.
4. El sistema muestra el historial de lotes recibidos y aplicaciones realizadas de ese producto.

**Flujos alternos**
2a. El sobrante mostrado no coincide con lo que hay físicamente en la bodega: el encargado revisa el historial de lotes y aplicaciones para localizar la diferencia, y corrige el registro correspondiente (ver CU-02).

**Postcondición**
El encargado conoce el sobrante actual del producto y, si lo necesita, el detalle de cómo se llegó a ese número.

**Requisitos que realiza**
RF-004, RNF-SEG-002

---

### CU-05 · Generar reporte de consumo por cultivo

**Actor principal**
Encargado

**Objetivo**
Ver cuánto agroquímico se aplicó en total, agrupado por cultivo, durante una temporada determinada.

**Precondición**
Existen aplicaciones registradas en la temporada seleccionada.

**Escenario principal**
1. El encargado abre la vista de reportes.
2. El sistema solicita seleccionar una temporada.
3. El encargado elige la temporada (papa o cártamo, según el cultivo).
4. El sistema agrupa y suma el total aplicado de cada producto por cultivo.
5. El sistema muestra el reporte con cantidad total y unidad de medida por producto y cultivo.

**Flujos alternos**
3a. La temporada seleccionada no tiene aplicaciones registradas: el sistema indica que no hay datos disponibles para esa temporada.

**Postcondición**
El encargado tiene a la vista el total de consumo por cultivo, con base en el cual puede decidir si la próxima compra debe ajustarse.

**Requisitos que realiza**
RF-005, RF-003, RNF-SEG-002

---

### CU-06 · Asignar cultivo a una parcela

**Actor principal**
Encargado

**Objetivo**
Definir qué cultivo se sembrará en una parcela durante una temporada, o dejarla marcada sin cultivo si no se sembrará.

**Precondición**
La parcela ya existe en el catálogo de parcelas de la finca.

**Escenario principal**
1. El encargado selecciona una parcela y una temporada.
2. El sistema muestra si la parcela ya tiene un cultivo asignado en esa temporada.
3. El encargado asigna el cultivo correspondiente (papa o cártamo).
4. El sistema guarda la asignación y la asocia a esa parcela y temporada.

**Flujos alternos**
2a. La parcela ya tiene un cultivo asignado en esa temporada: el sistema rechaza la nueva asignación y muestra el cultivo ya registrado.
3a. La parcela no se sembrará esa temporada: el encargado la marca como "sin cultivo" en vez de asignar uno, y el sistema conserva su historial de temporadas anteriores.

**Postcondición**
La parcela queda asociada a un cultivo (o marcada sin cultivo) para esa temporada, y toda aplicación futura registrada en ella quedará vinculada a ese cultivo.

**Requisitos que realiza**
RF-002, RF-009

---

## 6. Trazabilidad

| Requisito | Origen | Caso de uso | Elemento del prototipo |
|-----------|--------|--------------|--------------------------|
| RF-001 | Entrevista, pregunta 9 | CU-01 Registrar aplicación de agroquímico | Formulario de registro de aplicación |
| RF-002 | Entrevista, pregunta 27 | CU-06 Asignar cultivo a una parcela | Validación al asignar cultivo |
| RF-003 | Entrevista, pregunta 31 | CU-01, CU-05 | Vista de totales por parcela/cultivo |
| RF-004 | Entrevista, preguntas 16-19, 28 | CU-01, CU-04 Consultar sobrante de un producto | Pantalla de sobrante por producto |
| RF-005 | Entrevista, pregunta 31 | CU-05 Generar reporte de consumo por cultivo | Reporte de consumo |
| RF-006 | Entrevista, preguntas 16-17, 22 | CU-03 Registrar lote de producto comprado | Formulario de entrada de lote |
| RF-007 | Entrevista, pregunta 12 | CU-02 Corregir un registro de aplicación | Pantalla de corrección con historial de cambios |
| RF-008 | Entrevista, pregunta 26 | CU-01 (flujo alterno) | Campo de fecha editable en el formulario |
| RF-009 | Entrevista, pregunta 14 | CU-06 Asignar cultivo a una parcela | Estado "sin cultivo" en el catálogo de parcelas |

---

## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
|-------|-----------|------------|---------|
| 29/09/2026 | RF-001 a RF-005 | Se actualizó el campo Origen de "supuesto de diseño" a "confirmado en entrevista", con referencia a la pregunta específica | Se realizó la entrevista de elicitación con la ficha de Agrícola Barceló |
| 29/09/2026 | RF-006, RF-007, RF-008, RF-009, RNF-SEG-002 | Se agregaron como requisitos nuevos | Aparecieron en la entrevista y no estaban contemplados en la versión anterior del documento |
| 29/09/2026 | Sección 2 | Se actualizó la tabla de usuarios con datos reales de Agrícola Barceló (200 hectáreas, papa y cártamo, disponibilidad semanal) | Confirmado en la ficha de dominio y la entrevista |
| 29/09/2026 | RF-006, CU-03 | Se definió que no hay alias entre presentaciones/marcas: cada nombre comercial es un producto independiente en el catálogo | Confirmado por el cliente: el sobrante y el consumo se controlan por nombre comercial, sin unificación automática |
| 29/09/2026 | Sección 5 | Se completaron los seis casos de uso (CU-01 a CU-06) con escenario principal, flujos alternos, pre/postcondición y requisitos relacionados | Cierre de la sección de casos de uso tras la entrevista |
| 29/09/2026 | RF-004, RF-007, RF-009, RNF-TRZ-001, RNF-TRZ-002 (nuevo) | Se separó cada requisito para que exprese una sola condición: RF-004 quedó en un solo verbo, RF-007 se redujo a la autorización previa, y RNF-TRZ-001 se dividió en dos (restricción de edición e historial de cambios visible) | Ningún requisito debe unir dos condiciones distintas con "y" |
| 29/09/2026 | RNF-USA-001 | Se fijó el tiempo en menos de dos minutos, sin marcarlo como supuesto pendiente | Definición firme del requisito, sin dejarlo como condicional |
