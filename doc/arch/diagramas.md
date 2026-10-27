# Diagramas de arquitectura y diseño: Sistema de la clínica veterinaria

Luis Pablo Apodaca

---

## 1. Diagrama de contexto

El sistema completo, quién lo usa y con quién se comunica. No incluye tecnología.

```mermaid
flowchart TD
    rec([Recepcionista])
    vet([Veterinario])
    dueno([Dueño de la mascota])
    sis[Sistema de la clínica]
    lab[Laboratorio externo]
    pago[Pasarela de pagos]

    rec -->|agenda y cobra| sis
    vet -->|consulta expedientes| sis
    sis -->|envía estudios| lab
    sis -->|cobra consultas| pago
    sis -->|envía recordatorios de cita| dueno

    style sis fill:#ff5d00,color:#fff
```

<!-- NUEVO: actor "Dueño de la mascota" y la flecha de recordatorios -->

---

## 2. Diagrama de contenedores

Las piezas que se despliegan y se ejecutan por separado. El `subgraph` dibuja la frontera del sistema.

```mermaid
flowchart TD
    rec([Recepcionista])

    subgraph sistema[Sistema de la clínica]
        web[App web<br/>React]
        api[API<br/>Node.js]
        db[(Base de datos<br/>PostgreSQL)]
    end

    lab[Laboratorio externo]
    correo[Servicio de correo]

    rec -->|usa| web
    web -->|HTTPS/JSON| api
    api -->|lee y escribe| db
    api -->|SFTP| lab
    api -->|SMTP| correo

    style api fill:#ff5d00,color:#fff
```

<!-- NUEVO: sistema externo "Servicio de correo" y la flecha SMTP -->

---

## 3. Diagrama de componentes

Lo que hay dentro del contenedor API. Cobros le pregunta a Agenda en lugar de leer sus tablas (regla del ADR), y el Notificador es una interfaz, no un servicio concreto (patrón Adapter).

```mermaid
flowchart TD
    web[App web]

    subgraph api[API del sistema]
        agenda[Agenda<br/>citas y horarios]
        cobros[Cobros<br/>cuentas y pagos]
        exp[Expedientes<br/>historial clínico]
        recor[Recordatorios<br/>avisos de citas próximas]
        notif[Notificador<br/>interfaz de envío]
    end

    db[(Base de datos)]
    correo[Servicio de correo]

    web --> agenda
    web --> cobros
    web --> exp
    cobros -->|pregunta qué se atendió| agenda
    cobros -->|enviarRecibo| notif
    recor -->|consulta citas próximas| agenda
    recor -->|enviarRecordatorio| notif
    notif --> correo
    agenda --> db
    cobros --> db
    exp --> db

    style cobros fill:#ff5d00,color:#fff
```

<!-- NUEVO: componente Recordatorios; usa a Agenda y al Notificador, nunca al correo directo -->

---

## 4. Diagrama de flujo con decisiones

El flujo de validaciones del abono. Los rombos son decisiones y las flechas se etiquetan con Sí y No.

```mermaid
flowchart TD
    ini([Inicio])
    val{Monto mayor a cero?}
    exc{Excede el saldo?}
    apl[Aplicar el abono]
    rcb[Enviar recibo por correo]
    err1[Monto inválido]
    err2[Excede el saldo]
    fin([Fin])

    ini --> val
    val -->|No| err1
    val -->|Sí| exc
    exc -->|Sí| err2
    exc -->|No| apl
    apl --> rcb
    err1 --> fin
    err2 --> fin
    rcb --> fin

    style apl fill:#ff5d00,color:#fff
```

<!-- NUEVO: paso "Enviar recibo por correo" después de aplicar el abono -->

---

## 5. Diagrama de secuencia

Cómo se pasan los mensajes entre las piezas, en orden y en el tiempo.

```mermaid
sequenceDiagram
    actor Rec as Recepcionista
    participant Web as App web
    participant Cob as Cobros
    participant Age as Agenda
    participant BD as Base de datos
    participant Not as Notificador

    Rec->>Web: Registra un abono de 500
    Web->>Cob: abonar(cuenta, 500)
    Cob->>Age: Qué servicio se atendió?
    Age-->>Cob: Consulta general
    Cob->>BD: Guarda el pago
    BD-->>Cob: Listo
    Cob->>Not: enviarRecibo(cuenta)
    Not-->>Cob: Recibo enviado
    Cob-->>Web: Pago registrado
    Web-->>Rec: Muestra el nuevo saldo
```

<!-- NUEVO: participante Notificador y los mensajes enviarRecibo / Recibo enviado -->

---

## 6. Diagrama de clases

Atributos, métodos, visibilidad y multiplicidad. `-` es privado y `+` es público.

```mermaid
classDiagram
    class CuentaDePaciente {
        -saldo: double
        -pagos: List~Pago~
        +abonar(monto) string
        +consultarSaldo() double
    }
    class Pago {
        -monto: double
        -fecha: Date
    }
    class Paciente {
        -nombre: string
        -especie: string
    }
    class Cita {
        -fecha: Date
        -motivo: string
    }
    class Recordatorio {
        -fechaEnvio: Date
        -enviado: boolean
        +enviar() boolean
    }

    Paciente "1" --> "1" CuentaDePaciente : tiene
    CuentaDePaciente "1" --> "*" Pago : registra
    Paciente "1" --> "*" Cita : agenda
    Cita "1" --> "0..1" Recordatorio : genera
```

<!-- NUEVO: clases Cita y Recordatorio, con sus relaciones -->

---

## 7. Diagrama de estados

Por qué estados pasa una cita y qué evento la mueve.

```mermaid
stateDiagram-v2
    [*] --> Agendada
    Agendada --> Recordada: se envía el recordatorio
    Agendada --> Confirmada: el dueño confirma
    Agendada --> Cancelada: se cancela a tiempo
    Recordada --> Confirmada: el dueño confirma
    Recordada --> Cancelada: se cancela a tiempo
    Confirmada --> Atendida: llega a consulta
    Confirmada --> NoAsistio: no se presenta
    Atendida --> [*]
    Cancelada --> [*]
    NoAsistio --> [*]
```

<!-- NUEVO: estado Recordada, entre Agendada y Confirmada -->

---

## 8. Diagrama entidad-relación

Las tablas de la base de datos y cómo se relacionan. `||` es uno y exactamente uno, `o{` es cero o muchos y `o|` es cero o uno.

```mermaid
erDiagram
    PACIENTE ||--o{ CITA : tiene
    PACIENTE ||--|| CUENTA : tiene
    CITA ||--o| EXPEDIENTE : genera
    CITA ||--o{ RECORDATORIO : programa
    CUENTA ||--o{ PAGO : registra

    PACIENTE {
        int id
        string nombre
        string especie
    }
    CITA {
        int id
        date fecha
        string motivo
    }
    PAGO {
        int id
        decimal monto
        date fecha
    }
    RECORDATORIO {
        int id
        date fechaEnvio
        boolean enviado
    }
```

<!-- NUEVO: tabla RECORDATORIO relacionada con CITA -->
