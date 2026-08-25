# Visión del producto

> **Plantilla del curso · Ingeniería de Software I · SIS3407**
> Este documento es el primer entregable del semestre y la base de todo lo que viene después.
> Se entrega completo en la **semana 4** y se presenta ante el grupo.
>
> **Cómo usarla:** copia este archivo a tu repositorio como `docs/vision-del-producto.md`, borra las instrucciones en gris de cada apartado y escribe tu contenido en su lugar. Conserva los títulos.

---

**Autor:**
**Fecha de la última versión:**
**Repositorio:**

---

## 1. Descripción del sistema

**Nombre del sistema:ControlAgro**

**Descripción:Es un sistema que ayuda a los productores agrícolas a llevar la cuenta de cuántos fertilizantes, pesticidas y otros productos para el campo se usan durante una temporada de siembra, todo esto con el fin de registrar lo comprado y utilizado para evitar gastos innecesarios.**

---

## 2. Problema y usuarios

**El problema: Los agricultores no cuentan con un registro formal de cuanta cantidad de agroquímocos utilizan por lote comprado, el sistema que planteamos hará registro de los productos utilizados para prevenir gastar de más o evitar usar distintos tipos de marcas con el fin de no dañar la siembra**

**Cómo se resuelve hoy sin el sistema: **

**Usuarios del sistema:**

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
|Trabajador de campo |Registrar rápido qué usó, en qué parcela y cuánto, sin perder tiempo de trabajo |Que llenar el registro no le quite tiempo ni sea complicado en pleno trabajo de campo |
|Agricultor dueño de la empresa |Ver el total gastado por temporada, por parcela y por tipo de producto, para planear la próxima compra |Que los datos sean exactos y confiables, no solo "aproximados" |

**El trabajador de campo quiere anotar lo mínimo posible porque está apurado y con las manos ocupadas. El encargado, en cambio, necesita el dato exacto: cuánto producto, en qué momento, y en qué parcela específica, porque de eso depende que el reporte final sirva para algo. Si el sistema le pide al trabajador demasiados datos, este dejará de usarlo; si le pide muy pocos, el reporte no le sirve al encargado.**

---

## 3. Alcance

*Instrucción: lo que escribes en "fuera del alcance" es lo que después evita que el proyecto crezca sin control. Sé específico: "reportes" no dice nada, "reportes de ventas mensuales exportables a PDF" sí.*

### Dentro del alcance

-Registro de aplicaciones de agroquímicos: producto usado, cantidad aplicada, fecha, quién la aplicó, y a qué parcela(cultivo) corresponde.

-Catálogo de productos agroquímicos (fertilizantes, pesticidas, herbicidas, etc.) con su unidad de medida (litros, kilos, etc.).

-Registro de la cantidad disponible de cada producto al inicio de la temporada.

-Cálculo automático del total de producto usado por parcela, por cultivo y por temporada completa.

-Cálculo y visualización del producto sobrante después de cada aplicación (cantidad disponible − cantidad aplicada = sobrante), mostrado como un número.

-Reporte de consumo por tipo de producto y por tipo de cultivo, visible para el encargado o dueño.

### Explícitamente fuera del alcance

-Cultivos mixtos o rotación de cultivos dentro de la misma parcela en una misma temporada.

-Recomendaciones automáticas de dosis o mezcla de agroquímicos según tipo de cultivo o plaga.

-Alertas o notificaciones automáticas de stock bajo — el sistema muestra el sobrante, pero no avisa activamente; si el usuario quiere saber, tiene que consultarlo.

**Por qué queda fuera:**

-Gestión de compras a proveedores: el problema central es saber cuánto producto se aplica y cuánto sobra, no cómo se compra. Meter proveedores, precios y facturación implica otro flujo completo que no ayuda a resolver el desperdicio — solo alargaría el desarrollo sin acercarnos a la meta.

-El sistema se centra en registrar y contabilizar, proyectando la información obtenida sin dar recomendaciones ni alertas molestas. 

---

## 4. Tipo de sistema y restricciones

*Instrucción: identifica de qué tipo es tu sistema y qué te obliga a garantizar ese tipo. Un sistema de información y un sistema crítico no se diseñan igual.*

**Tipo de sistema:**

*(De información · Embebido · Crítico · Web y SaaS · De datos y análisis)*

**Por qué es de ese tipo:**

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
| | | |
| | | |
| | | |

**Reglas de negocio que ya identifiqué:**

*Instrucción: reglas que no son obvias desde fuera y que alguien que conoce el dominio tendría que explicarte. Si no encuentras ninguna, tu caso puede ser demasiado simple.*

1.
2.
3.

---

## 5. Ciclo de vida elegido

*Instrucción: este apartado se trabaja en la semana 3, después de ver los modelos de desarrollo. La justificación pesa más que la elección: no hay un modelo correcto, hay uno defendible para tu caso.*

**Modelo elegido:**

**Por qué le conviene a este proyecto:**

*Instrucción: argumenta con las características reales de tu caso. Estabilidad de los requisitos, disponibilidad del cliente, nivel de riesgo, tamaño del equipo, frecuencia de entregas esperada.*

### Alternativas descartadas

**Alternativa 1:**

*Por qué la descarté:*

**Alternativa 2:**

*Por qué la descarté:*

---

## Antes de entregar

Reviso que el documento cumpla lo siguiente:

- [ ] La descripción del apartado 1 se entiende sin ser del área
- [ ] Hay al menos dos tipos de usuario con necesidades distintas
- [ ] Identifiqué un conflicto real entre usuarios
- [ ] El alcance dice qué queda fuera, no solo qué queda dentro
- [ ] Las exclusiones son específicas, no genéricas
- [ ] Identifiqué el tipo de sistema y al menos dos atributos de calidad
- [ ] Anoté al menos tres reglas de negocio no obvias
- [ ] Justifiqué el ciclo de vida contra dos alternativas descartadas
- [ ] El documento está en mi repositorio y se puede leer desde el navegador
- [ ] Borré todas las instrucciones en cursiva de la plantilla
