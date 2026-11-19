# ADR-003 · Observer para actualizar el sobrante, con consulta previa

## 1. Estado
Propuesta. 19 de noviembre de 2026. Pasa a Aceptada cuando se revise.

## 2. Contexto
Cada aplicación registrada o corregida debe cambiar el sobrante del producto (RF-004), y el sobrante nunca puede quedar negativo (RNF-INT-001). Más adelante pueden querer enterarse de una aplicación otros interesados (por ejemplo, un aviso de que queda poco producto; el cliente quiere ver "solo el número" por ahora). Un aviso, por naturaleza, no puede rechazar la operación que anuncia.

## 3. Decisión
Dos pasos, en este orden:
1. **Antes de guardar**, `RegistroDeAplicaciones` pregunta `Producto.alcanza(cantidad)`, que delega en `Sobrante.alcanza(cantidad)`. Si no alcanza, rechaza.
2. **Después de guardar**, `RegistroDeAplicaciones` avisa a sus `ObservadorDeAplicacion` (`alRegistrar`, `alCorregir`). `Sobrante` es observador y llama a `descontar`, que vuelve a validar que no quede negativo.

El patrón Adapter se evaluó y no aplica: no hay sistemas externos (C4 nivel 1).

## 4. Alternativas consideradas
- **Aplicaciones descuenta el sobrante directamente.** Descartada: la regla del sobrante le toca a Inventario, y Aplicaciones tendría que conocer a cada módulo interesado.
- **Solo Observer, sin consulta previa.** Descartada: el aviso llega cuando la aplicación ya está guardada y no puede rechazarla.

## 5. Consecuencias
**Positivas:** agregar un interesado nuevo es suscribirlo, sin tocar el registro. La regla del sobrante vive en `Sobrante`.
**Negativas:** hay dos comprobaciones de la misma regla (`alcanza` y la de `descontar`); es a propósito, por la captura simultánea.
**Revisar si:** los avisos empiezan a necesitar rechazar la operación; entonces ya no son observadores.

## 6. Trazabilidad
RF-004, RF-001, RNF-INT-001 · CU-01, CU-02 · `RegistroDeAplicaciones`, `ObservadorDeAplicacion`, `Sobrante`, `Producto` · componentes Aplicaciones e Inventario.
