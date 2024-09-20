# Practica modelado y persistencia

### Ejercicio 1 ) Modelado Final Tinderizr

![](D:\Documentos\Practica_DDS\practica_persistencia_y_modelado_de_datos\modelado_persistencia.jpg)

### Justificaciones:

#### a) Entidades a persistir:

1. **Usuario**: Representa a un usuario del sistema, que puede tener varios perfiles en diferentes dominios.
2. **Perfil**: Los usuarios crean perfiles específicos para cada dominio, que consisten en un nombre, descripción y una relación con los campos del dominio.
3. **Dominio**: Cada dominio tiene su propio conjunto de tipos de perfiles y campos, además de definir el tipo de macheo (simple o múltiple) y el número mínimo de participantes para una vinculación múltiple.
4. **Campo**: Representa los campos variables de un perfil dentro de un dominio, que pueden ser opcionales u obligatorios.
5. **ValorDelCampo**: Almacena el valor de un campo específico para un perfil.
6. **Like**: Registra cuándo un perfil indica interés en otro.
7. **Match**: Representa un "match" entre uno o varios perfiles. Puede ser simple o múltiple.
8. **MatchXParticipante**: Conecta un perfil con otro match específico.

#### b) Impedance mismatch:

-  Las relaciones entre *usuarios, perfiles y dominios* son representadas por asociaciones `1:N` o `N:N` en el diseño relacional. Esto evita la necesidad de modelar herencia en una base de datos relacional, facilitando la implementación de ORM como JPA/Hibernate, que mapea fácilmente las relaciones.

- Las asociaciones entre perfiles y matcheos pueden considerarse de muchos-a-muchos ya que un perfil puede participar en varias vinculaciones  o macheos y una vinculación puede involucrar varios perfiles.

Se adoptó una tabla intermedia para controlar esta situación:

​	**MacheoXParticipante**

En lugar de modelar esto como colecciones dentro de una entidad (como sería típico en un modelo OO), usamos una tabla intermedia que elimina el *impedance mismatch* al mapear las relaciones N.

#### c) Normalización:

**Todas las entidades en el modelo cumplen con 1NF porque:**

- Cada columna contiene un único valor.
- Cada fila es única y tiene una clave primaria que la identifica.
- No se permiten valores repetidos o listas de valores en una sola columna.

**Cada entidad cumple con 2NF porque:**

- No hay dependencias parciales en claves compuestas.
- Todos los atributos dependen de la clave primaria

**Cada entidad cumple con 3NF porque:**

- No hay atributos que dependan de otros atributos no clave.
