# Documentación de arquitectura: ControlAgro

Luis Pablo Apodaca

---

## 1. Nivel 1 · Diagrama de contexto

El sistema completo como una sola caja, quién lo usa y con quién se comunica. No incluye tecnología.

```mermaid
flowchart TD
    trab([Trabajador de campo])
    enc([Encargado])
    sis[ControlAgro<br/>Control de agroquímicos<br/>de Agrícola Barceló]

    trab -->|registra aplicaciones| sis
    enc -->|registra lotes, corrige registros<br/>y asigna cultivos a rangos| sis
    sis -->|muestra sobrante, totales<br/>y reporte por cultivo| enc

    style sis fill:#2f5233,color:#fff
```

**Sistemas externos: ninguno.** El alcance de la especificación excluye sensores, drones, proveedores, facturación y reportes para autoridades, así que ControlAgro no se comunica con ningún sistema ajeno. Si en el futuro se integrara con el sistema contable o de compras del cliente, aparecería aquí como una caja nueva.

---

## 2. Nivel 2 · Diagrama de contenedores

Las piezas que se despliegan y se ejecutan por separado, con su tecnología. El `subgraph` dibuja la frontera del sistema, que se instala en el servidor de la finca.

```mermaid
flowchart TD
    trab([Trabajador de campo])
    enc([Encargado])

    subgraph sistema[ControlAgro instalado en el servidor de la finca]
        web[App web<br/>HTML, Tailwind CSS y JavaScript]
        app[Aplicación ControlAgro<br/>monolito modular en Node.js]
        db[(Base de datos<br/>PostgreSQL)]
    end

    trab -->|usa desde el celular| web
    enc -->|usa desde celular o computadora| web
    web -->|HTTPS/JSON| app
    app -->|SQL| db

    style app fill:#2f5233,color:#fff
```

Las tecnologías son la propuesta de este documento. Si el equipo usa otras, solo cambian las etiquetas: la estructura de tres piezas se mantiene.

---

## 3. Nivel 3 · Diagrama de componentes

Lo que hay dentro del contenedor **Aplicación ControlAgro**, donde viven las reglas de negocio. Son los cuatro módulos de la Actividad 2. Solo Aplicaciones habla con otros módulos, y Acceso queda aislado.

```mermaid
flowchart TD
    web[App web]

    subgraph app[Aplicación ControlAgro · monolito modular]
        acceso[Acceso<br/>Usuario y roles]
        aplic[Aplicaciones<br/>registro, correcciones,<br/>totales y reporte por cultivo]
        inv[Inventario<br/>Producto, Lote y Sobrante]
        finca[Finca<br/>Rangos, Cultivos y Temporadas]
    end

    db[(Base de datos)]

    web -->|"qué puede ver y hacer el rol"| acceso
    web --> aplic
    web --> inv
    web --> finca
    aplic -->|"¿alcanza el sobrante?"| inv
    aplic -->|"rango y cultivo activo"| finca
    aplic -.->|"anuncia: aplicación registrada o corregida"| inv
    acceso --> db
    aplic --> db
    inv --> db
    finca --> db

    style aplic fill:#2f5233,color:#fff
```

