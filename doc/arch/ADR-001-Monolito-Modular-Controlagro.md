# ADR-001 · Monolito modular para ControlAgro

## 1. Estado

Aceptada. 29 de octubre de 2026.

## 2. Contexto

ControlAgro es un sistema de información para un solo cliente, Agrícola Barceló. Lo construye una persona, con revisión de una dupla. El alcance excluye varias fincas y la integración con sistemas ajenos, así que es software instalado en la finca y no un servicio.

El sistema tiene cuatro módulos con fronteras claras. Solo Aplicaciones habla con otros dos módulos, porque es el único punto de entrada de datos. Las reglas que más pesan están repartidas entre ellos: el sobrante nunca puede quedar en negativo, un rango tiene un solo cultivo por temporada, las correcciones requieren autorización y dejan historial, y cada rol ve pantallas distintas.

Una caída momentánea no es grave, así que la disponibilidad no es un atributo crítico. El cliente puede revisar avances una vez por semana, y el proyecto sigue un ciclo de vida Ágil, con cambios frecuentes en las reglas.

## 3. Decisión

Se construye un **monolito modular**: una sola aplicación que se despliega como una unidad, dividida internamente en los cuatro módulos. Cada módulo expone una interfaz pública y ningún otro módulo lee sus tablas directamente. Esto se refleja en el diagrama de componentes: Aplicaciones le pregunta a Inventario si alcanza el sobrante, en lugar de consultar su información por su cuenta. Los módulos se comunican dentro del mismo proceso, y el aviso de que hubo una aplicación (Observer) también ocurre en memoria.

Los módulos se mantienen separados con una condición explícita para sacar uno después: se extraería **Aplicaciones** si guardar un registro tarda más de tres segundos cuando varios trabajadores capturan a la vez mientras las consultas siguen rápidas, o si cada cambio pequeño en ese módulo obligara a volver a desplegar todo el sistema.

## 4. Alternativas consideradas

- **Monolito sin módulos.** Descartada: al ser un solo bloque, las reglas de sobrante, rangos y autorización quedarían mezcladas y un cambio en una podría romper otra. El sistema ya tiene fronteras claras que vale la pena respetar.
- **Microservicios.** Descartada: exigen desplegar, monitorear y versionar cada servicio por separado, y comunicarlos por red. Con una sola persona construyendo y un solo cliente, ese costo no se paga con ningún beneficio. Además, según la ley de Conway, un equipo de una persona produce un solo bloque de software.

## 5. Consecuencias

**Positivas**

- Un solo despliegue en el servidor de la finca, fácil de instalar y de respaldar.
- Las reglas viven en el módulo que guarda el dato, y las pruebas de integridad del sobrante y de los rangos se hacen en un solo lugar.
- Los límites entre módulos dejan abierta la puerta para separarlos más adelante sin rehacer el diseño.

**Negativas**

- Todos los módulos comparten proceso y base de datos: un error grave en uno puede tumbar al resto.
- Mantener las fronteras depende de disciplina. Ninguna herramienta impide que un módulo lea las tablas de otro, y habrá que cuidarlo en las revisiones.
- Si el cliente pidiera convertirlo en servicio para varias fincas, habría que añadir el cliente a cada registro, sacar el nombre y las hectáreas de la finca del código y operar el servidor. Los módulos y las reglas se reutilizarían, pero esa parte se rehace.

**Cuándo revisar esta decisión:** si aparece la señal de carga o de despliegue descrita arriba, o si el proyecto pasa de una persona a varios equipos que trabajan por separado en cada módulo.
