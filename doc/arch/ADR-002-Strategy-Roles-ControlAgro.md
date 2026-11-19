# ADR-002 · Strategy para los permisos por rol

## 1. Estado
Propuesta. 19 de noviembre de 2026. Pasa a Aceptada cuando se revise.

## 2. Contexto
Hay dos roles: trabajador de campo y encargado. Lo que cada uno puede hacer cambia pantalla por pantalla (RNF-SEG-002): el trabajador registra aplicaciones; el encargado además registra lotes, corrige registros, asigna cultivos y ve totales y reportes (entrevista, pregunta 23). El cliente puede cambiar los permisos con el tiempo y el proyecto es Ágil.

## 3. Decisión
`Usuario` se rige por una `PoliticaDeAcceso` (interfaz con `puedeRealizar(accion)`), con dos realizaciones: `PoliticaTrabajador` y `PoliticaEncargado`. Las acciones son valores de la enumeración `Accion`. El módulo Acceso es el único que decide; la interfaz solo pregunta `Usuario.puedeRealizar(accion)`.

## 4. Alternativas consideradas
- **`if rol == "encargado"` repartido por el código.** Descartada: cada permiso nuevo obliga a buscar y modificar condiciones en varios módulos.
- **Tabla de permisos en base de datos.** Descartada por ahora: con dos roles fijos es más maquinaria de la que se necesita. Se reconsidera si el cliente pide roles que él mismo configure.

## 5. Consecuencias
**Positivas:** un tercer rol es una realización nueva, sin tocar las existentes. Los permisos se prueban en un solo lugar.
**Negativas:** más clases (cuatro) para dos roles. Si los roles nunca crecen, el patrón es más de lo necesario; la alternativa simple es dejar `rol` como atributo de `Usuario`.
**Revisar si:** el cliente pide un tercer rol o permisos configurables por él.

## 6. Trazabilidad
RNF-SEG-002 · CU-02 (solo el encargado autoriza) · `Usuario`, `PoliticaDeAcceso`, `Accion` · componente Acceso.
