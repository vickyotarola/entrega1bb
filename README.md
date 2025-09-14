# Informe 

## Definición de Entidades, entidades debiles y relaciones en el modelo E/R

### 1. Entidades Fuertes
-------------------------------------------------
**1.1 Persona**
Entidad fundamental del sistema que representa a todas las personas registradas, tiene un identificador unico natural y existe independiente de otras entidades

**Atributos** : 
- RUN (primary key), nombres, apellidos, telefono, correo electronico, direccion

**1.2 Institucion provisional de salud**
Representa las instituciones previsionales de salud ISAPRES y FONASA.
Tiene identificador único

**Atributos** : 
- Codigo ministerial (primary key), nombre, rut, enlace

**1.3 Maestro Farmacia**
Catalogo de productos farmaceuticos con identificador unico, existe independiente al sistema interno

**Atributos** : 
- codigo generico (primary key), nombre_producto_generico, descripcion_producto_generico, tipo_producto, codigo_onu, clasificacion_onu, clasificacion_interna, estado_codigo_generico, canasta_esencial, precio

**1.4 Arancel Fonasa**
Lista oficial de prestaciones médicas de FONASA con identificadores unicos. Es independiente al centro médico

**Atributos** : 
 - codigo (primary key), codigo_adicional (primary key), consultas_atencion_medica, valor_fonasa, grupo, tipo

**1.5 Arancel DCColita de Rana**
Arancel interno del centro médico con código propios únicos. Existe independientemente aunque se base en Fonasa

**Atributos** : 
- codigo interno (primary key), codigo_fonasa, consultas_atencion_medica, valor

**1.6 consulta médica**
Representa las consultas medicas realizadas. Tiene identificador surrogate unico y existe como evento médico independiente

**Atributos** : 
- id_consulta(surrogate key), fecha, diagnostico

**1.7 Receta**
Documento medico con identificador surrogate unico. Aunque se origina es consuultas, tiene existencia propia como un documento legal

**Atributos** :
- id_receta (surrogate_key), fecha

**1.8 Orden**
Docuemtno médico para solicitud de examenes/procedimientos. Tiene identificador surrogate unico y existencia independiente como documento 

**Atributos** : 
- id_orden (surrogate_key), fecha


## 2. Entidades débiles
--------------------------------------------------
**2.1 Plan Isapre**
> Es una entidad debil respecto a ISAPRES porque su existencia depende
> completamente de la existencia de una ISAPRE, su identificacion 
> requiere la clave de la ISAPRE propietaria, no tiene significado sin
> la isapre que lo contiene y porque si se elimina una ISAPRE, 
> automaticamente significa que se eliminan sus planes

**llave parcial** : grupo
**llave primaria completa:** codigo_institucion + grupo
**Entidad propietaria:** ISAPRES


## 3. Entidades especializadas (sublclases de jerarquias)
-----------------------------------------------------
## **3.1 Jerarquia Persona**
La jerarquia de Persona se realiza por la natiraleza multifuncional de las personas dentro del centro médico, donde una misma persona puede desempeñar diferentes roles simultanaeamente, pero cada rol implica ya sea atributos o comportamientos específicos

# **Nivel 1: Persona -> {Paciente, Trabajador}**

**Atributos Comunes**: 
Todas las personas registradas en el sistema, independiente de su rol, comparten los siguientes atributos básicos
- RUN (primary key)
- nombres
- apellidos
- telefono
- correo electronico
- direccion

**Atributos especificos**
- **Paciente**: Niguno adicional , ya que los pacientes utilzan los atributos communes de persona para: identificacion de consultad medicas, facturacion de servicios y comunicacion

- **Trabajdores**: Los trabajadores requieren informacion adicional relacionada con su vinculo laboral con el centro médico, diferente a la info basica

**Caracterisiticas de la jerarquia Nivel 1**

- **Solapada**: 
Justifiacion: Según el enunciado "Una persona puede ser paciente, trabajar en el centro medico o ambos" y "un staff medico puede ser paciente""
Esta característica es fundamental porque, un medico puede necesitar atencion medica en el mismo centro, un administratico puede ser paciente del centro y porque una persona puede cambiar de rol

- **Parcial**:
Justificacion: Pueden existir personas registradas en el sistema que temporalmente no tengan un rol especifico, como las personas que aun estan en proceso de contratacion, ex trabajadores, etc.

# **Nivel 2: Trabajador -> {Staff Medico -> [Medico], Administrativo}**

**Atributos especificos**

- **StaffMedico**: Los medicos requieren atributos especificos porque necesitan una identifiacion especializada
- profesion (obligatorio segun el enunciado)

**Caracterisiticas de la jerarquia**

- **Disjunta**:

Justificación: Segun el enunciado "los trabajdores se dividen en staff medico o administrativo", entonces un trabajador no puede ser simultaneamente medico y administratico, son funciones mutuamente excluyentes por la naturaleza del enunciado y requieren responsabilidades diferente

- **Total**:

Justifiacion: Todo trabajador del centro medico debe ser clasificado necesariamente como Staff médico o Administrativo. No existene trbajadores que no pertenezcan a una de estas categorias en el contexto del centro medico

**Reglas de negocio**

La jerarqui multinivel pemite implementar las siguientes reglas del enunciado:
1. Control de funciones médicas
"Solo los mediocs realizan consultas médicas, emiten recetas y órdenes"
La relacion realiza solo conecta con la subclase medico y las relaciones emite y solicita se originan desde consultas realizadas por medicos

2. Restricciones de afiliacion previsional
"Un staff medico tambien peude ser paciente, pero en este caso solo puede ser titular"
La jerarquia solapada permite que una persona se meedico y paciente y esta restriccion se impementa a nvel de reglas de negocion adicionales

**Restricción adicional** : según el enunciado "un staff medico puede ser paciente , per en este caso solo puede ser titular", entonces si una persona es medico y paciente (por la jerarquia solapada) y ademas esa persona está afiliada a una institucion previsional de salud entonces en la relación afiliado debe tener "tipo_beneficiario" = titular.


## **3.2 Jerarquia Institucion previsional de Salud**
La jerarquía de institucion previsinal se realiza por la naturaleza del sistema previsional, donde el enunciado nos dice que existen dos tipos fundamentalmente diferentes de instituciones que brindan cobertura de salud en el centro medico, pero que a su vez comparten caracterisiticas comunes

**Atributos comunes**: (superclase institucion previsional)
Todas las instituciones de salud, independiente de su tipo, comparten los siguientes atributos:
- **codigo ministerial (Primary Key)**: código unico que le entrega el ministerio de salud
- **nombre**
- **rut**
- **enlace**
- **tipo**
Estos atributos son comunes porque todas las instituciones previsionales están sujetas a una misma normativa regulatoria básica en el sistema de salud

**Atributos especificos**: (subclases)
- **FONASA**: (ninguno adicional)
- **ISAPRES**: (ninguno adicional)

**Carcterisiticas de la jerarquía**

- **Disjunta**

Justificacion: Una institucion de salud no puede ser simultaneamente FONASA e ISAPRE. Son dos categorias mutuamente excluyentes definidas separadamente, ya que una es el sistema público y la otra es la representacion del sistema privado

- **Total** (completa)

Justificacion: En el sistema si hay un sistema provisional, debe ser necesariamente FONASA o ISAPRE. No existen instituciones de salud previsional que no pertenezcan a una de estas dos categorias, ya que el enunciado define el universo de instituciones disponibles 

**Reglas del negocio**
La jerarquía permite implementar las siguientes reglas del enunciado:

1. "Una persona puede estar afiliada a una institucion de salud previsional": La superclase permite la afiliacion sin distinguir el tipo específico

2. "En caso de que el beneficiario este afiliado a FONASA, el valor a cobrar es el establecido en el Arancel FONASA": La especializacion FONASA permite lógica específica de facturación

3. "Cada ISAPRE tiene un solo plan de salud": La especializacion ISAPRES permite la relacion específica con plan_isapre

## **3.3 Jerarquía RECETA**

La jerarquía de RECETA se realiza por la naturaleza regulatoria del sistema farmacéutico, donde el enunciado nos dice que existen dos tipos fundamentalmente diferentes de recetas médicas que manejan categorías distintas de medicamentos, pero que a su vez comparten características comunes como documentos médicos legales.

**Atributos comunes (superclase RECETA):**
Todas las recetas médicas, independiente de su tipo, comparten los siguientes atributos:
- **id_receta (Primary Key)**: identificador único surrogate del documento médico
- **fecha**: momento de emisión de la receta

Estos atributos son comunes porque todas las recetas están sujetas a una misma normativa regulatoria básica como documentos médicos válidos y requieren trazabilidad temporal.

**Atributos específicos (subclases):**
- **RECETA_NORMAL**: (ninguno adicional)
- **RECETA_PSICOTROPICOS**: 
  - **codigo_autorizacion**: código único obligatorio para medicamentos controlados

**Características de la jerarquía:**

- **Disjunta**

Justificación: Una receta no puede ser simultáneamente normal y psicotrópica. Son dos categorías mutuamente excluyentes definidas por la regulación farmacéutica, ya que el enunciado establece que "los medicamentos de tipo=psicotrópicos deben ir en una receta independiente aislada", lo que implica separación total entre ambos tipos.

- **Total (completa)**

Justificación: En el sistema, si hay una receta médica, debe ser necesariamente normal o psicotrópica. No existen recetas que no pertenezcan a una de estas dos categorías, ya que el enunciado define exhaustivamente los tipos de medicamentos disponibles y sus formas de prescripción.

**Reglas del negocio:**
La jerarquía permite implementar las siguientes reglas del enunciado:

1. "Una receta especifica un conjunto de medicamentos del archivo Maestro farmacia con tipo=Fármacos, refrigerados, sueros": La especialización RECETA_NORMAL permite la relación específica con medicamentos estándar a través de CONTIENE_NORMAL.

2. "Los medicamentos de tipo=psicotrópicos deben ir en una receta independiente aislada con un código de autorización único": La especialización RECETA_PSICOTROPICOS permite el manejo exclusivo de medicamentos controlados con su código de autorización obligatorio a través de CONTIENE_PSICO.

3. "Toda atención comienza con una consulta médica y en esta se pueden generar una o más recetas médicas": La superclase RECETA permite la relación EMITE desde CONSULTA_MEDICA sin distinguir el tipo específico de receta que se generará.

## Definición de Relaciones y justificacion de la cardinalidad elegida

## 1. Relaciones de Afiliación y Previsión

### 1.1 PERSONA - AFILIADO_A - INSTITUCION_SALUD
**Cardinalidad:** (0,1) - (0,N)

**Justificación lado PERSONA (0,1):**
- **0:** Una persona puede no tener previsión de salud (ser particular), según el enunciado que indica "también existen las personas sin previsión de salud"
- **1:** Una persona puede estar afiliada a máximo una institución, ya que el enunciado establece que "Un beneficiario solo puede estar registrado en una institución"

**Justificación lado INSTITUCION_SALUD (0,N):**
- **0:** Una institución puede existir sin tener afiliados registrados en el sistema del centro médico
- **N:** Una institución puede tener múltiples personas afiliadas, ya que es la naturaleza del sistema previsional

### 1.2 ISAPRES - TIENE - PLAN_ISAPRE
**Cardinalidad:** (1,1) - (1,N)

**Justificación lado ISAPRES (1,1):**
- **1:** Cada ISAPRE tiene exactamente un plan de salud, según el enunciado "Cada ISAPRE tiene un solo plan de salud"

**Justificación lado PLAN_ISAPRE (1,N):**
- **1:** Cada plan pertenece a exactamente una ISAPRE (dependencia existencial como entidad débil)
- **N:** El plan se desglosa por grupos del Arancel FONASA, generando múltiples registros por ISAPRE

## 2. Relaciones de Consulta Médica

### 2.1 MEDICO - REALIZA - CONSULTA_MEDICA
**Cardinalidad:** (0,N) - (1,1)

**Justificación lado MEDICO (0,N):**
- **0:** Un médico puede no haber realizado consultas aún (médico recién contratado)
- **N:** Un médico puede realizar múltiples consultas médicas

**Justificación lado CONSULTA_MEDICA (1,1):**
- **1:** Cada consulta debe ser realizada por exactamente un médico, según la regla "Solo los médicos realizan consultas médicas"

### 2.2 PACIENTE - RECIBE - CONSULTA_MEDICA
**Cardinalidad:** (0,N) - (1,1)

**Justificación lado PACIENTE (0,N):**
- **0:** Un paciente puede estar registrado pero no haber recibido consultas aún
- **N:** Un paciente puede recibir múltiples consultas médicas a lo largo del tiempo

**Justificación lado CONSULTA_MEDICA (1,1):**
- **1:** Cada consulta es recibida por exactamente un paciente (no puede haber consultas sin paciente o consultas compartidas)

## 3. Relaciones de Generación de Documentos

### 3.1 CONSULTA_MEDICA - EMITE - RECETA
**Cardinalidad:** (1,1) - (0,N)

**Justificación lado CONSULTA_MEDICA (1,1):**
- **1:** Una consulta específica genera documentos desde ella misma

**Justificación lado RECETA (0,N):**
- **0:** Una consulta puede no generar recetas (solo observación médica)
- **N:** Una consulta puede generar múltiples recetas según el enunciado "se pueden generar una o más recetas médicas"

### 3.2 CONSULTA_MEDICA - SOLICITA - ORDEN
**Cardinalidad:** (1,1) - (0,N)

**Justificación lado CONSULTA_MEDICA (1,1):**
- **1:** Una consulta específica genera documentos desde ella misma

**Justificación lado ORDEN (0,N):**
- **0:** Una consulta puede no generar órdenes de exámenes
- **N:** Una consulta puede generar múltiples órdenes según el enunciado "se pueden generar una o más órdenes de procedimientos o exámenes"

## 4. Relaciones de Contenido de Medicamentos

### 4.1 RECETA_NORMAL - CONTIENE_NORMAL - MAESTRO_FARMACIA
**Cardinalidad:** (1,N) - (0,N)

**Justificación lado RECETA_NORMAL (1,N):**
- **1:** Una receta debe contener al menos un medicamento para ser válida, según el enunciado "Una receta especifica un conjunto de medicamentos"
- **N:** Una receta puede contener múltiples medicamentos diferentes

**Justificación lado MAESTRO_FARMACIA (0,N):**
- **0:** Un medicamento puede no haber sido prescrito nunca
- **N:** Un medicamento puede ser prescrito en múltiples recetas diferentes

### 4.2 RECETA_PSICOTROPICOS - CONTIENE_PSICO - MAESTRO_FARMACIA
**Cardinalidad:** (1,N) - (0,N)

**Justificación lado RECETA_PSICOTROPICOS (1,N):**
- **1:** Una receta psicotrópica debe contener al menos un medicamento psicotrópico
- **N:** Una receta puede contener múltiples medicamentos psicotrópicos

**Justificación lado MAESTRO_FARMACIA (0,N):**
- **0:** Un medicamento psicotrópico puede no haber sido prescrito
- **N:** Un medicamento psicotrópico puede ser prescrito en múltiples recetas

## 5. Relaciones de Facturación

### 5.1 CONSULTA_MEDICA - FACTURA_CONSULTA - ARANCEL_DCCOLITA
**Cardinalidad:** (1,1) - (0,N)

**Justificación lado CONSULTA_MEDICA (1,1):**
- **1:** Cada consulta debe ser facturada según exactamente un código del arancel interno, ya que todo servicio médico tiene un costo asociado

**Justificación lado ARANCEL_DCCOLITA (0,N):**
- **0:** Un código de arancel puede no haber sido utilizado para facturar consultas aún
- **N:** Un código puede ser utilizado para facturar múltiples consultas (ej: "Consulta medicina general")

### 5.2 ORDEN - FACTURA_PROCEDIMIENTO - ARANCEL_DCCOLITA
**Cardinalidad:** (1,N) - (0,N)

**Justificación lado ORDEN (1,N):**
- **1:** Una orden debe incluir al menos un procedimiento/examen para ser válida
- **N:** Una orden puede incluir múltiples procedimientos diferentes (ej: hemograma + radiografía)

**Justificación lado ARANCEL_DCCOLITA (0,N):**
- **0:** Un código de procedimiento puede no haber sido utilizado
- **N:** Un código puede ser utilizado en múltiples órdenes

## 6. Relaciones de Procedimientos con FONASA

### 6.1 ORDEN - INCLUYE - ARANCEL_FONASA
**Cardinalidad:** (1,N) - (0,N)

**Justificación lado ORDEN (1,N):**
- **1:** Una orden debe incluir al menos un examen/procedimiento registrado en FONASA
- **N:** Una orden puede incluir múltiples exámenes diferentes del arancel FONASA

**Justificación lado ARANCEL_FONASA (0,N):**
- **0:** Un procedimiento FONASA puede no ser ofrecido por el centro médico
- **N:** Un procedimiento puede ser solicitado en múltiples órdenes

## 7. Relaciones de Referencias entre Aranceles

### 7.1 ARANCEL_DCCOLITA - BASADO_EN - ARANCEL_FONASA
**Cardinalidad:** (0,1) - (0,N)

**Justificación lado ARANCEL_DCCOLITA (0,1):**
- **0:** El centro puede tener servicios únicos sin equivalente en el arancel FONASA
- **1:** Cuando existe equivalencia, es con máximo un código FONASA específico

**Justificación lado ARANCEL_FONASA (0,N):**
- **0:** Códigos FONASA que el centro médico no ofrece
- **N:** Un código FONASA puede tener múltiples variaciones en el arancel interno del centro

## Resumen de Principios Aplicados

### Principio de Obligatoriedad
Las cardinalidades mínimas (1) se aplican cuando:
- El enunciado indica obligatoriedad explícita
- La lógica de negocio requiere la existencia del vínculo
- La integridad referencial es fundamental

### Principio de Opcionalidad
Las cardinalidades mínimas (0) se aplican cuando:
- El enunciado permite la ausencia de la relación
- Existen casos de uso válidos sin el vínculo
- Se requiere flexibilidad operacional

### Principio de Multiplicidad
Las cardinalidades máximas (N) reflejan: la naturaleza reutilizable de las entidades, laa posibilidad de relaciones múltiples en el tiempo y muestra la escalabilidad del sistema

Todas las cardinalidades establecidas se fundamentan en las reglas de negocio explícitas del enunciado y en la lógica operacional del centro médico DCColita de rana.


# Esquema Relacional Normalizado en BCNF

## Proceso de Transformación del Modelo E/R

### 1. Transformación de Entidades 

#### 1.1 PERSONA
```sql
PERSONA(
    run VARCHAR(12) PRIMARY KEY,
    nombres VARCHAR(100) NOT NULL,
    apellidos VARCHAR(100) NOT NULL,
    telefono VARCHAR(15),
    correo_electronico VARCHAR(100),
    direccion VARCHAR(200),
    profesion VARCHAR(100)
)
```
**Dominios:**
- `run`
- `nombres, apellidos`
- `telefono`
- `correo_electronico`
- `direccion`


#### 1.2 INSTITUCION_PREVISIONAL
```sql
INSTITUCION_SALUD(
    codigo_ministerial VARCHAR(20) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    rut VARCHAR(12) NOT NULL,
    enlace VARCHAR(200),
    tipo VARCHAR(10) CHECK (tipo IN ('abierta', 'cerrada'))
)
```
**Dominios:**
- `codigo_ministerial`
- `nombre`
- `rut`
- `enlace`
- `tipo`: ('abierta' o 'cerrada')

#### 1.3 MAESTRO_FARMACIA
```sql
MAESTRO_FARMACIA(
    codigo_generico VARCHAR(20) PRIMARY KEY,
    nombre_producto_generico VARCHAR(150) NOT NULL,
    descripcion_producto_generico TEXT,
    tipo_producto VARCHAR(20) NOT NULL CHECK (tipo_producto IN ('Fármacos', 'refrigerados', 'sueros', 'psicotrópicos')),
    codigo_onu VARCHAR(15),
    clasificacion_onu VARCHAR(50),
    clasificacion_interna VARCHAR(50),
    estado_codigo_generico VARCHAR(20),
    canasta_esencial BOOLEAN,
    precio DECIMAL(10,2)
)
```
**Dominios:**
- `codigo_generico`
- `tipo_producto`
- `precio`
- `canasta_esencial`

#### 1.4 ARANCEL_FONASA
```sql
ARANCEL_FONASA(
    codigo VARCHAR(10),
    codigo_adicional VARCHAR(10),
    consultas_atencion_medica VARCHAR(200) NOT NULL,
    valor_fonasa DECIMAL(10,2) NOT NULL,
    grupo VARCHAR(5) NOT NULL,
    tipo VARCHAR(50) NOT NULL,
    PRIMARY KEY (codigo, codigo_adicional)
)
```
**Dominios:**
- `codigo, codigo_adicional`
- `valor_fonasa`
- `grupo`
- `tipo`

#### 1.5 ARANCEL_DCCOLTA_DE_RANA
```sql
ARANCEL_DCCOLITA(
    codigo_interno VARCHAR(20) PRIMARY KEY,
    codigo_fonasa VARCHAR(10),
    codigo_adicional_fonasa VARCHAR(10),
    consultas_atencion_medica VARCHAR(200) NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (codigo_fonasa, codigo_adicional_fonasa) REFERENCES ARANCEL_FONASA(codigo, codigo_adicional)
)
```

#### 1.6 CONSULTA_MEDICA
```sql
CONSULTA_MEDICA(
    id_consulta SERIAL PRIMARY KEY,
    fecha TIMESTAMP NOT NULL,
    diagnostico TEXT NOT NULL
)
```

#### 1.7 RECETA
```sql
RECETA(
    id_receta SERIAL PRIMARY KEY,
    fecha TIMESTAMP NOT NULL,
    tipo_receta VARCHAR(20) NOT NULL CHECK (tipo_receta IN ('normal', 'psicotropicos')),
    codigo_autorizacion VARCHAR(50) UNIQUE -- Solo para psicotrópicos
)
```

#### 1.8 ORDEN
```sql
ORDEN(
    id_orden SERIAL PRIMARY KEY,
    fecha TIMESTAMP NOT NULL
)
```

### 2. Transformación de Entidades Débiles

#### 2.1 PLAN_ISAPRE (Entidad débil de ISAPRES)
```sql
PLAN_ISAPRE(
    codigo_institucion VARCHAR(20),
    grupo VARCHAR(5),
    bonificacion DECIMAL(5,2) NOT NULL CHECK (bonificacion >= 0 AND bonificacion <= 100),
    PRIMARY KEY (codigo_institucion, grupo),
    FOREIGN KEY (codigo_institucion) REFERENCES INSTITUCION_SALUD(codigo_ministerial)
)
```

### 3. Transformación de Especializaciones (Jerarquías)

#### 3.1 Jerarquía PERSONA (Solapada y Parcial)
```sql
PACIENTE(
    run VARCHAR(12) PRIMARY KEY,
    FOREIGN KEY (run) REFERENCES PERSONA(run) ON DELETE CASCADE
)

TRABAJADOR(
    run VARCHAR(12) PRIMARY KEY,
    FOREIGN KEY (run) REFERENCES PERSONA(run) ON DELETE CASCADE
)

STAFF_MEDICO(
    run VARCHAR(12) PRIMARY KEY,
    FOREIGN KEY (run) REFERENCES TRABAJADOR(run) ON DELETE CASCADE
)

ADMINISTRATIVO(
    run VARCHAR(12) PRIMARY KEY,
    FOREIGN KEY (run) REFERENCES TRABAJADOR(run) ON DELETE CASCADE
)

MEDICO(
    run VARCHAR(12) PRIMARY KEY,
    profesion VARCHAR(100)
    FOREIGN KEY (run) REFERENCES TRABAJADOR(run) ON DELETE CASCADE
)
```


#### 3.2 Jerarquía INSTITUCION_SALUD (Disjunta y Total)
**Implementación:** Se mantiene una sola tabla ya que las subclases no tienen atributos específicos diferenciados significativos


### 4. Relaciones

#### 4.1 Relaciones Binarias con Atributos

##### AFILIADO (PERSONA - INSTITUCION_SALUD)
```sql
AFILIADO(
    run VARCHAR(12) PRIMARY KEY,
    codigo_institucion VARCHAR(20) NOT NULL,
    tipo_beneficiario VARCHAR(20) NOT NULL CHECK (tipo_beneficiario IN ('titular', 'dependiente')),
    run_titular VARCHAR(12),
    FOREIGN KEY (run) REFERENCES PERSONA(run),
    FOREIGN KEY (codigo_institucion) REFERENCES INSTITUCION_SALUD(codigo_ministerial),
    FOREIGN KEY (run_titular) REFERENCES PERSONA(run),
    CHECK (
        (tipo_beneficiario = 'titular' AND run_titular IS NULL) OR
        (tipo_beneficiario = 'dependiente' AND run_titular IS NOT NULL AND run_titular != run)
    )
)
```
Se transforma en tabla ya que la realcion tienen atributos propios que justifican una tabla separada 

##### REALIZA (MEDICO - CONSULTA_MEDICA)
```sql
REALIZA(
    run_medico VARCHAR(12),
    id_consulta INTEGER,
    PRIMARY KEY (run_medico, id_consulta),
    FOREIGN KEY (run_medico) REFERENCES MEDICO(run),
    FOREIGN KEY (id_consulta) REFERENCES CONSULTA_MEDICA(id_consulta)
)
```
No se transformaria en tabla ya que se implementa mediante llave foranea en consulta_medica

##### RECIBE (PACIENTE - CONSULTA_MEDICA)
```sql
RECIBE(
    run_paciente VARCHAR(12),
    id_consulta INTEGER,
    PRIMARY KEY (run_paciente, id_consulta),
    FOREIGN KEY (run_paciente) REFERENCES PACIENTE(run),
    FOREIGN KEY (id_consulta) REFERENCES CONSULTA_MEDICA(id_consulta)
)
```
No se transforma en tabla porque se implementa mediante llave foranea en consulta_medica

##### EMITE (CONSULTA_MEDICA - RECETA)
```sql
EMITE(
    id_consulta INTEGER,
    id_receta INTEGER,
    PRIMARY KEY (id_consulta, id_receta),
    FOREIGN KEY (id_consulta) REFERENCES CONSULTA_MEDICA(id_consulta),
    FOREIGN KEY (id_receta) REFERENCES RECETA(id_receta)
)
```
No se transforma en tabla porque se implementa mediante llave foranea en receta

##### SOLICITA (CONSULTA_MEDICA - ORDEN)
```sql
SOLICITA(
    id_consulta INTEGER,
    id_orden INTEGER,
    PRIMARY KEY (id_consulta, id_orden),
    FOREIGN KEY (id_consulta) REFERENCES CONSULTA_MEDICA(id_consulta),
    FOREIGN KEY (id_orden) REFERENCES ORDEN(id_orden)
)
```
No se transorma en tabla porque se implementa como llave foranea en orden

##### CONTIENE_NORMAL (RECETA_NORMAL - MAESTRO_FARMACIA)
```sql
CONTIENE_NORMAL(
    id_receta INTEGER,
    codigo_generico VARCHAR(20),
    PRIMARY KEY (id_receta, codigo_generico),
    FOREIGN KEY (id_receta) REFERENCES RECETA(id_receta),
    FOREIGN KEY (codigo_generico) REFERENCES MAESTRO_FARMACIA(codigo_generico),
    CHECK (
        (SELECT tipo_receta FROM RECETA WHERE id_receta = CONTIENE_NORMAL.id_receta) = 'normal'
        AND (SELECT tipo_producto FROM MAESTRO_FARMACIA WHERE codigo_generico = CONTIENE_NORMAL.codigo_generico) 
            IN ('Fármacos', 'refrigerados', 'sueros')
    )
)
```
Si se convierte en tabla ya que una receta puede contener multiples medicamentos, y un medicamento puede estar en multiples recetas y no se puede implementar con Foreign Key

##### CONTIENE_PSICO (RECETA_PSICOTROPICOS - MAESTRO_FARMACIA)
```sql
CONTIENE_PSICO(
    id_receta INTEGER,
    codigo_generico VARCHAR(20),
    PRIMARY KEY (id_receta, codigo_generico),
    FOREIGN KEY (id_receta) REFERENCES RECETA(id_receta),
    FOREIGN KEY (codigo_generico) REFERENCES MAESTRO_FARMACIA(codigo_generico),
    CHECK (
        (SELECT tipo_receta FROM RECETA WHERE id_receta = CONTIENE_PSICO.id_receta) = 'psicotropicos'
        AND (SELECT tipo_producto FROM MAESTRO_FARMACIA WHERE codigo_generico = CONTIENE_PSICO.codigo_generico) 
            = 'psicotrópicos'
    )
)
```
Si se transforma a tabla ya que tiene restricciones especificas para medicamentos psicotropicos

##### FACTURA_CONSULTA (CONSULTA_MEDICA - ARANCEL_DCCOLITA)
```sql
FACTURA_CONSULTA(
    id_consulta INTEGER PRIMARY KEY,
    codigo_interno VARCHAR(20) NOT NULL,
    FOREIGN KEY (id_consulta) REFERENCES CONSULTA_MEDICA(id_consulta),
    FOREIGN KEY (codigo_interno) REFERENCES ARANCEL_DCCOLITA(codigo_interno)
)
```
No se transforma en table ya que se implemneta mediante llave foranea en cosnulta medica

##### FACTURA_PROCEDIMIENTO (ORDEN - ARANCEL_DCCOLITA)
```sql
FACTURA_PROCEDIMIENTO(
    id_orden INTEGER,
    codigo_interno VARCHAR(20),
    PRIMARY KEY (id_orden, codigo_interno),
    FOREIGN KEY (id_orden) REFERENCES ORDEN(id_orden),
    FOREIGN KEY (codigo_interno) REFERENCES ARANCEL_DCCOLITA(codigo_interno)
)
```
Si se transforma en tabla ya que una orden puede incluir multiples procedimientos del arancel

##### INCLUYE (ORDEN - ARANCEL_FONASA)
```sql
INCLUYE(
    id_orden INTEGER,
    codigo VARCHAR(10),
    codigo_adicional VARCHAR(10),
    PRIMARY KEY (id_orden, codigo, codigo_adicional),
    FOREIGN KEY (id_orden) REFERENCES ORDEN(id_orden),
    FOREIGN KEY (codigo, codigo_adicional) REFERENCES ARANCEL_FONASA(codigo, codigo_adicional)
)
```
Si se trabsforma a tabla porque es una relacion n a n y no puede ser implementada con llave foranes, una orden puede incluir multiples procedimeintos y un procedimiento puede estar en multiples ordenes diferente y al intentar usar llaves foraneas enfrentariamos problemas como el de incluir multiples procedimietnos en una sola orden o como usar un mismo procedimiento en las multiples ordenes

## Verificación de BCNF

### Análisis de Dependencias Funcionales

Para cada tabla, las dependencias funcionales identificadas son:

#### PERSONA
- `run → nombres, apellidos, telefono, correo_electronico, direccion`
- **BCNF:**  Solo tiene una clave candidata (run)

#### ARANCEL_FONASA
- `(codigo, codigo_adicional) → consultas_atencion_medica, valor_fonasa, grupo, tipo`
- **BCNF:**  Solo tiene una clave candidata

#### PLAN_ISAPRE
- `(codigo_institucion, grupo) → bonificacion`
- **BCNF:**  Solo tiene una clave candidata

#### Tablas de Relaciones M:N
- **CONTIENE_NORMAL:** `(id_receta, codigo_generico) → cantidad, dosis, duracion, instrucciones`
- **FACTURA_PROCEDIMIENTO:** `(id_orden, codigo_interno) → cantidad, observaciones`
- **BCNF:**  Todas las dependencias tienen como determinante la clave primaria completa

### Justificación de BCNF

**Todas las tablas están en BCNF porque:**

1. **No existen dependencias funcionales parciales** (están en 2NF)
2. **No existen dependencias funcionales transitivas** (están en 3NF)  
3. **Todos los determinantes son claves candidatas** (están en BCNF)

### Restricciones de Integridad Adicionales

```sql
-- Restricción: Staff médico como paciente debe ser titular
ALTER TABLE AFILIADO_A ADD CONSTRAINT staff_medico_titular 
CHECK (
    NOT EXISTS (
        SELECT 1 FROM MEDICO M 
        WHERE M.run = AFILIADO_A.run 
        AND AFILIADO_A.tipo_beneficiario = 'dependiente'
    )
);

-- Restricción: Código de autorización obligatorio para recetas psicotrópicas
ALTER TABLE RECETA ADD CONSTRAINT codigo_auth_psicotropicos
CHECK (
    (tipo_receta = 'psicotropicos' AND codigo_autorizacion IS NOT NULL) OR
    (tipo_receta = 'normal' AND codigo_autorizacion IS NULL)
);
```

# Justificación de Llaves Foráneas y Surrogate

## Llaves Surrogate Utilizadas

### 1. CONSULTA_MEDICA.id_consulta
**Tipo:** (Clave Surrogate)

**Justificación:**
No existe un identificador natural obvio para las consultas médicas. Una combinación de run_paciente + run_medico + fecha podría no ser única ya que el mismo médico puede atender al mismo paciente múltiples veces en el mismo día. Además, esta combinación sería muy larga y compleja para referenciar desde las tablas RECETA y ORDEN

**Restricción de Integridad:** Garantiza unicidad absoluta y facilita las referencias desde documentos derivados de la consulta.

### 2. RECETA.id_receta
**Tipo:** (Clave Surrogate)

**Justificación:**
Las recetas médicas no tienen un identificador natural único en el sistema. Aunque podrían identificarse por la consulta que las origina, una consulta puede generar múltiples recetas (especialmente recetas normales y psicotrópicas separadas según las regulaciones farmacéuticas).

**Restricción de Integridad:** Permite trazabilidad única de cada documento y facilita las referencias desde las tablas de contenido de medicamentos.

### 3. ORDEN.id_orden
**Tipo:** (Clave Surrogate)

**Justificación:**
Similar a las recetas, las órdenes médicas carecen de identificador natural. Una consulta puede generar múltiples órdenes para diferentes tipos de exámenes o procedimientos, y necesitan identificación independiente para su trazabilidad y facturación.

**Restricción de Integridad:** Garantiza identificación única de cada documento de solicitud de procedimientos y facilita las relaciones con aranceles.

## Llaves Foráneas y sus Restricciones de Integridad

### 1. Integridad Referencial de Jerarquías

#### TRABAJADOR.run → PERSONA.run
```sql
FOREIGN KEY (run) REFERENCES PERSONA(run) ON DELETE CASCADE
```

**Justificación:**
Garantiza que todo trabajador debe ser primero una persona válida en el sistema. La eliminación en cascada asegura que si se elimina una persona, automáticamente se elimine su información como trabajador, manteniendo la coherencia de la jerarquía.

**Restricción de Integridad:** Integridad referencial de especialización, no pueden existir trabajadores sin su entidad padre correspondiente.

#### MEDICO.run → STAFF_MEDICO.run
```sql
FOREIGN KEY (run) REFERENCES STAFF_MEDICO(run) ON DELETE CASCADE
```

**Justificación:**
Asegura que todo médico debe ser primero staff médico. Esta restricción es fundamental porque solo el staff médico puede realizar consultas médicas según las reglas del negocio, y dentro del staff médico, solo los médicos tienen la competencia legal para hacerlo.

**Restricción de Integridad:** Integridad de especialización multinivel, mantiene la coherencia de la jerarquía PERSONA → TRABAJADOR → STAFF_MEDICO → MEDICO.

#### PACIENTE.run → PERSONA.run
```sql
FOREIGN KEY (run) REFERENCES PERSONA(run) ON DELETE CASCADE
```

**Justificación:**
Garantiza que todo paciente debe existir como persona en el sistema. La cascada es necesaria para mantener consistencia cuando una persona es eliminada del sistema.

**Restricción de Integridad:** Integridad referencial de especialización solapada, permite que una persona sea paciente y trabajador simultáneamente.

### 2. Integridad Referencial de Afiliaciones

#### AFILIADO_A.run → PERSONA.run
```sql
FOREIGN KEY (run) REFERENCES PERSONA(run) ON DELETE CASCADE
```

**Justificación:**
Asegura que solo personas válidas pueden tener afiliación previsional. La eliminación en cascada mantiene la coherencia cuando una persona es eliminada del sistema.

**Restricción de Integridad:** Integridad de entidad, toda afiliación debe corresponder a una persona real.

#### AFILIADO_A.codigo_institucion → INSTITUCION_SALUD.codigo_ministerial
```sql
FOREIGN KEY (codigo_institucion) REFERENCES INSTITUCION_SALUD(codigo_ministerial)
```

**Justificación:**
Garantiza que las afiliaciones solo pueden ser hacia instituciones de salud válidas y registradas oficialmente. Previene afiliaciones hacia instituciones inexistentes.

**Restricción de Integridad:** Integridad referencial, solo permite afiliaciones a instituciones autorizadas por el Ministerio de Salud.

#### AFILIADO_A.run_titular → PERSONA.run
```sql
FOREIGN KEY (run_titular) REFERENCES PERSONA(run)
```

**Justificación:**
Asegura que cuando un beneficiario es dependiente, su titular debe ser una persona válida en el sistema. Esta restricción es crucial para mantener la coherencia de las dependencias familiares en el sistema previsional.

**Restricción de Integridad:** Integridad referencial de dependencia, todo beneficiario dependiente debe tener un titular válido.

### 3. Integridad Referencial de Entidades Débiles

#### PLAN_ISAPRE.codigo_institucion → INSTITUCION_SALUD.codigo_ministerial
```sql
FOREIGN KEY (codigo_institucion) REFERENCES INSTITUCION_SALUD(codigo_ministerial) ON DELETE CASCADE
```

**Justificación:**
Como PLAN_ISAPRE es una entidad débil, su existencia depende completamente de la ISAPRE propietaria. La eliminación en cascada es esencial porque un plan no puede existir sin su ISAPRE correspondiente.

**Restricción de Integridad:** Integridad de dependencia existencial, los planes solo pueden existir mientras exista su ISAPRE propietaria.

### 4. Integridad Referencial de Consultas Médicas

#### CONSULTA_MEDICA.run_medico → MEDICO.run
```sql
FOREIGN KEY (run_medico) REFERENCES MEDICO(run)
```

**Justificación:**
Implementa la regla de negocio fundamental "Solo los médicos realizan consultas médicas". Esta restricción previene que personal administrativo u otros roles realicen consultas médicas.

**Restricción de Integridad:** Integridad de autorización, solo personal médico certificado puede realizar actos médicos.

#### CONSULTA_MEDICA.run_paciente → PACIENTE.run
```sql
FOREIGN KEY (run_paciente) REFERENCES PACIENTE(run)
```

**Justificación:**
Asegura que las consultas solo pueden ser realizadas a personas registradas como pacientes en el sistema. Garantiza trazabilidad médica completa.

**Restricción de Integridad:** Integridad de destinatario, las consultas médicas solo pueden ser realizadas a pacientes válidos.

### 5. Integridad Referencial de Documentos Médicos

#### RECETA.id_consulta → CONSULTA_MEDICA.id_consulta
```sql
FOREIGN KEY (id_consulta) REFERENCES CONSULTA_MEDICA(id_consulta)
```

**Justificación:**
Implementa la dependencia existencial de las recetas respecto a las consultas. Toda receta debe originarse de una consulta médica válida, cumpliendo con las normativas médicas legales.

**Restricción de Integridad:** Integridad de origen, toda receta debe tener respaldo en una consulta médica documentada.

#### ORDEN.id_consulta → CONSULTA_MEDICA.id_consulta
```sql
FOREIGN KEY (id_consulta) REFERENCES CONSULTA_MEDICA(id_consulta)
```

**Justificación:**
Similar a las recetas, garantiza que toda orden de exámenes o procedimientos tenga su origen en una consulta médica válida, cumpliendo con protocolos médicos establecidos.

**Restricción de Integridad:** Integridad de origen, toda orden debe estar respaldada por una consulta médica.

### 6. Integridad Referencial de Contenido de Medicamentos

#### CONTIENE_NORMAL.id_receta → RECETA.id_receta
```sql
FOREIGN KEY (id_receta) REFERENCES RECETA(id_receta) ON DELETE CASCADE
```

**Justificación:**
Asegura que los medicamentos solo pueden ser asociados a recetas válidas. La eliminación en cascada mantiene coherencia cuando se anula una receta.

**Restricción de Integridad:** Integridad de contenido, los medicamentos deben estar asociados a prescripciones válidas.

#### CONTIENE_NORMAL.codigo_generico → MAESTRO_FARMACIA.codigo_generico
```sql
FOREIGN KEY (codigo_generico) REFERENCES MAESTRO_FARMACIA(codigo_generico)
```

**Justificación:**
Garantiza que solo medicamentos registrados oficialmente pueden ser prescritos. Previene prescripciones de medicamentos no autorizados o inexistentes.

**Restricción de Integridad:** Integridad de producto, solo se pueden prescribir medicamentos registrados en el maestro farmacéutico.

### 7. Integridad Referencial de Aranceles

#### ARANCEL_DCCOLITA.codigo_fonasa, codigo_adicional_fonasa → ARANCEL_FONASA(codigo, codigo_adicional)
```sql
FOREIGN KEY (codigo_fonasa, codigo_adicional_fonasa) REFERENCES ARANCEL_FONASA(codigo, codigo_adicional)
```

**Justificación:**
Cuando el centro médico basa sus precios en el arancel FONASA, debe referenciar códigos válidos y existentes. Esta restricción asegura que las equivalencias sean hacia prestaciones oficialmente reconocidas.

**Restricción de Integridad:** Integridad de equivalencia, las referencias al arancel oficial deben ser válidas y existentes.

#### CONSULTA_MEDICA.codigo_interno → ARANCEL_DCCOLITA.codigo_interno
```sql
FOREIGN KEY (codigo_interno) REFERENCES ARANCEL_DCCOLITA(codigo_interno)
```

**Justificación:**
Garantiza que toda consulta sea facturable según un código válido del arancel interno. Previene consultas sin respaldo tarifario.

**Restricción de Integridad:** Integridad de facturación, toda prestación médica debe tener un código de cobro válido.

## Restricciones de Integridad Especiales

**Staff médico como paciente titular:**
```sql
CHECK (NOT EXISTS (SELECT 1 FROM MEDICO M WHERE M.run = AFILIADO_A.run AND tipo_beneficiario = 'dependiente'))
```
Implementa la regla del enunciado que staff médico solo puede ser beneficiario titular.

**Código de autorización psicotrópicos:**
```sql
CHECK ((tipo_receta = 'psicotropicos' AND codigo_autorizacion IS NOT NULL) OR (tipo_receta = 'normal' AND codigo_autorizacion IS NULL))
```
Garantiza código único obligatorio para medicamentos controlados.

**Relación titular-dependiente:**
```sql
CHECK ((tipo_beneficiario = 'titular' AND run_titular IS NULL) OR (tipo_beneficiario = 'dependiente' AND run_titular IS NOT NULL AND run_titular != run))
```
Asegura coherencia lógica: titulares sin titular asignado, dependientes con titular diferente a sí mismos.

# Cómo el Diseño Resuelve los Temas Fidelidad, Redundancia, Anomalías, Simplicidad y Buena Elección de Claves Primarias

El esquema relacional diseñado resuelve efectivamente los problemas fundamentales de calidad de bases de datos. En términos de **fidelidad**, el modelo representa fielmente todas las reglas de negocio del centro médico DCColita de rana, desde la restricción "solo médicos realizan consultas" hasta las complejas relaciones previsionales entre titulares y dependientes, garantizando que el sistema refleje exactamente la realidad operacional. La **redundancia** se elimina mediante la normalización BCNF, donde cada dato se almacena en un único lugar lógico: la información personal está solo en PERSONA, los diagnósticos solo en CONSULTA_MEDICA, y las dependencias funcionales tienen como determinantes únicamente las claves candidatas, evitando duplicación innecesaria de información. Las **anomalías** de inserción, actualización y eliminación se previenen por la estructura normalizada y las restricciones de integridad: no se pueden insertar consultas sin médicos válidos (anomalía de inserción), actualizar un RUN actualiza automáticamente todas las referencias por las claves foráneas (eliminando anomalías de actualización), y las eliminaciones en cascada mantienen coherencia sin dejar registros huérfanos (evitando anomalías de eliminación). La **simplicidad** se logra mediante jerarquías bien diseñadas que evitan tablas innecesarias, relaciones claras que minimizan joins complejos, y una estructura intuitiva donde cada tabla tiene un propósito específico y fácilmente comprensible. Finalmente, la **buena elección de claves primarias** combina estratégicamente claves naturales estables como RUN para personas y códigos ministeriales para instituciones (aprovechando identificadores únicos del dominio), con claves surrogate SERIAL para entidades sin identificadores naturales obvios como consultas, recetas y órdenes (garantizando simplicidad y eficiencia en referencias), mientras que las claves compuestas se utilizan apropiadamente solo donde son conceptualmente necesarias como en ARANCEL_FONASA y PLAN_ISAPRE, resultando en un esquema que maximiza tanto la integridad referencial como el rendimiento del sistema.