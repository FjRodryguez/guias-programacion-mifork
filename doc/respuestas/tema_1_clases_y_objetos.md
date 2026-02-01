<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

Perfecto, voy punto por punto y de forma clara, como suele pedirse en una asignatura de introducción a POO 👌

---

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos?

### Respuesta

1. **Encapsulación**
   Consiste en agrupar datos (atributos) y comportamiento (métodos) en una misma unidad: el objeto. Además, se controla el acceso a los datos para protegerlos.

2. **Abstracción**
   Permite representar entidades del mundo real de forma simplificada, quedándonos solo con lo esencial y ocultando los detalles internos.

3. **Herencia**
   Permite crear nuevas clases a partir de otras, reutilizando código y estableciendo relaciones “es-un”.

4. **Polimorfismo**
   Permite que un mismo método tenga distintos comportamientos según el objeto que lo implemente.

---

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

* Java
* C++
* Python
* C#

---

## 3. Paradigmas anteriores a la POO: programación estructurada y modular

### Respuesta

* **Programación estructurada**
  Se basa en el uso de estructuras de control (`if`, `while`, `for`) y funciones, evitando saltos incontrolados como `goto`.

* **Programación modular**
  Divide el programa en módulos independientes, cada uno con una responsabilidad concreta, facilitando el mantenimiento y la reutilización.

---

## 4. ¿Qué tres elementos definen a un objeto en POO?

### Respuesta

1. **Identidad**: lo diferencia de otros objetos
2. **Estado**: valores de sus atributos
3. **Comportamiento**: métodos que puede ejecutar

---

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes usan clases?

### Respuesta

* Una **clase** es un molde o plantilla que define atributos y métodos.
* Un **objeto** es una entidad concreta creada a partir de una clase.
* Una **instancia** es un objeto creado desde una clase.
* No todos los lenguajes orientados a objetos usan clases (por ejemplo, JavaScript usa prototipos).

---

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la recolección de basura?

### Respuesta

* Normalmente los objetos se almacenan en el **heap**.
* No es igual en todos los lenguajes (en C++ pueden estar en stack o heap).
* La **recolección de basura (garbage collection)** es el mecanismo automático que libera memoria de objetos que ya no se usan.

---

## 7. ¿Qué es un método? ¿Qué es la sobrecarga de métodos?

### Respuesta

* Un **método** es una función asociada a una clase u objeto.
* La **sobrecarga de métodos** consiste en definir varios métodos con el mismo nombre pero diferentes parámetros.

---

## 8. Ejemplo mínimo de clase `Punto` en Java

### Respuesta

```java
class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        System.out.println(p.calculaDistanciaAOrigen());
    }
}
```

---

## 9. Punto de entrada en Java, `static` y `final`

### Respuesta

* El punto de entrada es el método:

  ```java
  public static void main(String[] args)
  ```
* `static` indica que el método pertenece a la clase, no a un objeto.
* No se usa solo en `main`, también en atributos y métodos.
* Combinado con `final` sirve para definir constantes (`static final`).

---

## 10. Compilación y ejecución de Java

### Respuesta

```bash
javac Main.java
java Main
```

* Java es **compilado e interpretado**.
* La **máquina virtual (JVM)** ejecuta el programa.
* El *byte-code* es el código intermedio.
* Los ficheros `.class` contienen ese byte-code.

---

## 11. `new`, constructores y ejemplo `Empleado`

### Respuesta

* `new` reserva memoria y crea un objeto.
* Un **constructor** inicializa el objeto.

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

---

## 12. ¿Qué es `this`? ¿Es igual en todos los lenguajes?

### Respuesta

* `this` es la referencia al objeto actual.
* No se llama igual en todos los lenguajes (`self` en Python).

Ejemplo en `Punto`:

```java
double calculaDistanciaAOrigen() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
}
```

---

## 13. Método `distanciaA`

### Respuesta

```java
double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

---

## 14. Paso de parámetros: objetos vs primitivos

### Respuesta

* En Java, los objetos se pasan **por copia de la referencia**.
* Cambiar atributos del objeto afecta al original.
* Los tipos primitivos (`int`) se pasan **por copia**, los cambios no afectan fuera del método.

---

## 15. Método `toString()`

### Respuesta

* `toString()` devuelve una representación en texto del objeto.
* Existe en muchos lenguajes.

Ejemplo:

```java
@Override
public String toString() {
    return "Punto(" + x + ", " + y + ")";
}
```

---

## 16. ¿Una clase es como un `struct` en C?

### Respuesta

Se parecen, pero a un `struct` le faltan:

* Métodos
* Encapsulación
* Constructores
* Polimorfismo
  Las variables de tipo `struct` no son instancias en el sentido de la POO.

---

## 17. Emulación de `Punto` con `struct` en C

### Respuesta

```c
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

double distanciaAOrigen(Punto p) {
    return sqrt(p.x * p.x + p.y * p.y);
}
```

* `this` desaparece: hay que pasar explícitamente el `struct` como parámetro.
* La “magia” de la POO se convierte en convenciones manuales.
