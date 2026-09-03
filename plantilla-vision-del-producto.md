# Visión del producto

---

**Autor: Luis Pablo Apodaca Bojorquez**

**Fecha de la última versión:**

**Repositorio:**

---

## 1. Descripción del sistema

**Nombre del sistema:**

**ControlAgro**

Es un sistema que ayuda a los productores agrícolas a llevar la cuenta de cuántos fertilizantes, pesticidas y otros productos para el campo se usan durante una temporada de siembra, todo esto con el fin de registrar lo comprado y utilizado para evitar gastos innecesarios.

---

## 2. Problema y usuarios

Los agricultores no cuentan con un registro formal de cuanta cantidad de agroquímocos utilizan por lote comprado, el sistema que planteamos hará registro de los productos utilizados para prevenir gastar de más o evitar usar distintos tipos de marcas con el fin de no dañar la siembra


**Usuarios del sistema:**

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
|Trabajador de campo |Registrar rápido qué usó, en qué parcela y cuánto, sin perder tiempo de trabajo |Que llenar el registro no le quite tiempo ni sea complicado en pleno trabajo de campo |
|Agricultor dueño de la empresa |Ver el total gastado por temporada, por parcela y por tipo de producto, para planear la próxima compra |Que los datos sean exactos y confiables, no solo "aproximados" |

El trabajador de campo quiere anotar lo mínimo posible porque está apurado y con las manos ocupadas. El encargado, en cambio, necesita el dato exacto: cuánto producto, en qué momento, y en qué parcela específica, porque de eso depende que el reporte final sirva para algo. Si el sistema le pide al trabajador demasiados datos, este dejará de usarlo; si le pide muy pocos, el reporte no le sirve al encargado.

---

## 3. Alcance

### Dentro del alcance

* Registro de aplicaciones de agroquímicos: producto usado, cantidad aplicada, fecha, quién la aplicó, y a qué parcela(cultivo) corresponde.

* Catálogo de productos agroquímicos (fertilizantes, pesticidas, herbicidas, etc.) con su unidad de medida (litros, kilos, etc.).

* Registro de la cantidad disponible de cada producto al inicio de la temporada.

* Cálculo automático del total de producto usado por parcela, por cultivo y por temporada completa.

* Cálculo y visualización del producto sobrante después de cada aplicación (cantidad disponible − cantidad aplicada = sobrante), mostrado como un número.

* Reporte de consumo por tipo de producto y por tipo de cultivo, visible para el encargado o dueño.

### Explícitamente fuera del alcance

* Cultivos mixtos o rotación de cultivos dentro de la misma parcela en una misma temporada.

* Recomendaciones automáticas de dosis o mezcla de agroquímicos según tipo de cultivo o plaga.

* Alertas o notificaciones automáticas de stock bajo — el sistema muestra el sobrante, pero no avisa activamente; si el usuario quiere saber, tiene que consultarlo.

**Por qué queda fuera:**

* Gestión de compras a proveedores: el problema central es saber cuánto producto se aplica y cuánto sobra, no cómo se compra. Meter proveedores, precios y facturación implica otro flujo completo que no ayuda a resolver el desperdicio — solo alargaría el desarrollo sin acercarnos a la meta.

* El sistema se centra en registrar y contabilizar, proyectando la información obtenida sin dar recomendaciones ni alertas molestas. 

---

## 4. Tipo de sistema y restricciones

**Tipo de sistema:**

*De información*

-El sistema no controla ni automatiza nada físico, y su falla temporal no pone en riesgo vidas ni operaciones críticas. Su función esencial es capturar, organizar y mostrar información para que las personas tomen mejores decisiones.

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
|Usabilidad |El trabajador de campo lo usa con las manos ocupadas, con prisa, y no necesariamente es una persona técnica |Si es lento o complicado, deja de registrar y vuelve la libreta|
|Exactitud de los datos |Los totales de consumo y sobrante dependen 100% de que cada registro esté bien capturado |Datos mal capturados llevan a decisiones de compra o uso equivocadas |
|Disponibilidad razonable |Debe funcionar cuando se necesita, pero no requiere estar activo 24/7 sin interrupción |Una caída momentánea no es grave: se anota en papel y se captura después |

**Reglas de negocio que ya identifiqué:**

* Una parcela tiene un único cultivo asignado por temporada

* La cantidad aplicada de un producto nunca puede dejar el sobrante en negativo , si se intenta registrar más de lo disponible, el sistema debe señalarlo como inconsistencia, no simplemente restar.

* Cada aplicación debe quedar ligada a una parcela con cultivo ya definido, no se puede registrar una aplicación sin saber a qué cultivo pertenece.

* El stock/sobrante de cada producto es único para toda la finca, no por parcela.

---

## 5. Ciclo de vida elegido

**Modelo elegido:**

*Prototipado Rápido*

**Por qué le conviene a este proyecto:**

* Estabilidad de los requisitos: baja al inicio. Quedó demostrado en esta misma conversación: el cliente no tenía claro si habría control de inventario, ni cómo debía vincularse el registro con el cultivo, hasta que se le preguntó directamente y vio ejemplos concretos.

* Disponibilidad del cliente: el cliente puede validar entregas concretas mejor que responder preguntas abstractas sobre "requisitos". Ya se vio que las respuestas más útiles salieron de ejemplos concretos, no de preguntas teóricas.

Nivel de riesgo: bajo. No hay riesgo técnico alto ni riesgo de negocio grave si el primer prototipo no es perfecto se puede desechar o ajustar sin gran costo.


### Alternativas descartadas

**Alternativa 1: Cascada**

Cascada exige requisitos completos y estables desde el inicio, congelados antes de construir. Aquí el alcance se fue descubriendo activamente durante las primeras conversaciones con el cliente.

**Alternativa 2: Espiral**

Espiral está diseñado para proyectos de alto riesgo, donde cada ciclo requiere un análisis de riesgo formal antes de avanzar. Este sistema no maneja dinero ni operaciones críticas, y no usa tecnología experimental

---

