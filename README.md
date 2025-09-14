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
**3.1 Jerarquia Persona**
- **Paciente**: Especialización de Persona que recibe atención médica
- **Trabajador**: Especialización de Persona que trabaja en el centro
- **Medico**: Especializacion de Trabajador que realiza consultas médicas
- **Adiministrativo**: Especializacion de Trabajador con funciones administrativo

> Explicacion: Una persona puede ser paciente, trabajador o ambos. Es 
> una jerarquia solapada (una persona puede ser ambos) y parcial.
> La jeraquia desde trabajadores se crea ya que hay dos tipos de 
> trabajadores, se dividen en staff medico y adminitrativo. Esta 
> subjerarquia es disjunta ya que un trabajor no es medico y 
> administrativo a la vez, es uno o lo otro y es total, ya que si es 
> trabajador es uno o lo otro (medico, admin.)


**Restricción adicional** : según el enunciado "un staff medico puede ser paciente , per en este caso solo puede ser titular", entonces si una persona es medico y paciente (por la jerarquia solapada) y ademas esa persona está afiliada a una institucion previsional de salud entonces en la relación afiliado debe tener "tipo_beneficiario" = titular.




