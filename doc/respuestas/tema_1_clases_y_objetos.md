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

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Las cuatro características básicas de la programación orientada a objetos son **encapsulación**, **abstracción**, **herencia** y **polimorfismo**. Estas ideas buscan organizar mejor el código y hacerlo más fácil de mantener, reutilizar y entender en programas grandes.

La **encapsulación** consiste en agrupar datos y operaciones que trabajan sobre esos datos dentro de una misma unidad, el objeto. Además, permite controlar el acceso a los datos internos, evitando que se modifiquen de forma incorrecta desde fuera. Esto recuerda a ocultar detalles de implementación tras una interfaz bien definida.

La **abstracción** permite centrarse en qué hace un objeto y no en cómo lo hace. Se modelan entidades del mundo real o conceptos del problema mediante clases, ignorando detalles irrelevantes. Esto reduce la complejidad y facilita el razonamiento sobre el sistema.

La **herencia** permite crear nuevas clases a partir de otras existentes, reutilizando su código y ampliándolo o modificándolo. El **polimorfismo** permite tratar objetos de distintas clases relacionadas de forma uniforme, haciendo que una misma operación se comporte de manera diferente según el objeto concreto.

---

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Existen muchos lenguajes que soportan la programación orientada a objetos, algunos de ellos de forma pura y otros de manera híbrida. Este paradigma es especialmente común en lenguajes modernos de uso general.

Entre los lenguajes más populares se encuentra **Java**, que está diseñado desde el principio como un lenguaje orientado a objetos. Prácticamente todo en Java gira en torno a clases y objetos, salvo algunos tipos básicos.

Otro lenguaje muy usado es **C++**, que añade características de orientación a objetos sobre el lenguaje C. También destacan **Python**, que soporta POO de forma flexible y dinámica, y **C#**, muy similar a Java y ampliamente utilizado en el ecosistema de Microsoft.

---

## 3. Los paradigmas anteriores a la POO, ¿Qué es la programación estructurada? y, todavía mejor, ¿Qué es la programación modular?

La **programación estructurada** es un paradigma que propone organizar el código usando estructuras de control claras como secuencias, condicionales y bucles, evitando saltos incontrolados como `goto`. El objetivo es mejorar la legibilidad y facilitar el mantenimiento del código.

En este paradigma, el programa se divide en funciones o procedimientos, cada uno con una tarea concreta. C es un ejemplo típico de lenguaje que favorece la programación estructurada, donde los datos suelen ser globales o se pasan explícitamente a las funciones.

La **programación modular** va un paso más allá y propone dividir el programa en módulos independientes, cada uno responsable de una parte del sistema. Un módulo agrupa funciones y datos relacionados, y expone solo lo necesario al resto del programa, acercándose conceptualmente a la encapsulación.

---

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Un objeto en programación orientada a objetos se define por **estado**, **comportamiento** e **identidad**. Estos tres elementos permiten diferenciar un objeto de otro y describir su papel dentro del sistema.

El **estado** está formado por los datos internos del objeto, normalmente representados por atributos o variables miembro. Estos valores pueden cambiar a lo largo del tiempo y representan la situación actual del objeto.

El **comportamiento** corresponde a las operaciones que el objeto puede realizar, es decir, sus métodos. La **identidad** implica que cada objeto es único, incluso aunque tenga el mismo estado que otro, algo que no ocurre con simples variables en C.

---

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una **clase** es una plantilla o definición que describe cómo serán los objetos de un determinado tipo. En ella se especifican los atributos que tendrán y los métodos que podrán ejecutar. No ocupa memoria para datos concretos, solo define la estructura.

Un **objeto** es una entidad concreta creada a partir de una clase. Crear un objeto se denomina **instanciar** la clase, y el objeto resultante es una **instancia** de dicha clase. Por tanto, clase y objeto no son lo mismo: la clase es el molde y el objeto el resultado.

Aunque la mayoría de lenguajes orientados a objetos usan clases, no todos lo hacen. Algunos lenguajes, como JavaScript, utilizan modelos basados en prototipos en lugar de clases tradicionales, aunque el resultado práctico sea similar.

---

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la recolección de basura?

En muchos lenguajes orientados a objetos, como Java, los objetos se almacenan en el **heap**, una zona de memoria dinámica. Las variables que se usan en el programa no contienen el objeto en sí, sino una **referencia** a él.

Esto no es igual en todos los lenguajes. En C++, por ejemplo, un objeto puede estar en la pila (stack) o en el heap, dependiendo de cómo se cree. Java, en cambio, gestiona automáticamente la memoria y no permite al programador decidirlo explícitamente.

La **recolección de basura** (*garbage collection*) es un mecanismo automático que libera la memoria ocupada por objetos que ya no se usan. El programador no tiene que liberar memoria manualmente, reduciendo errores como fugas de memoria o accesos inválidos.

---

## 7. ¿Qué es un método? ¿Qué es la sobrecarga de métodos?

Un **método** es una función asociada a una clase u objeto. Define una operación que puede realizarse sobre los datos del objeto o relacionada con la clase. Conceptualmente es similar a una función en C, pero ligada a un tipo concreto.

Los métodos pueden acceder directamente a los atributos del objeto al que pertenecen, sin necesidad de pasarlos como parámetros. Esto refuerza la idea de que los datos y el comportamiento van juntos.

La **sobrecarga de métodos** consiste en definir varios métodos con el mismo nombre pero con diferentes parámetros. El lenguaje decide cuál usar según el número y tipo de argumentos, lo que permite interfaces más claras y expresivas.

---

## 8. Ejemplo mínimo de clase en Java: `Punto`

```java
class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

class PruebaPunto {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        double d = p.calculaDistanciaAOrigen();
        System.out.println(d);
    }
}
```

---

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para `main`? ¿Para qué se combina con `final`?

El punto de entrada de un programa en Java es el método `main`, con una firma muy concreta. La máquina virtual comienza la ejecución del programa llamando a ese método, sin necesidad de crear un objeto previamente.

La palabra clave `static` indica que un método o atributo pertenece a la **clase** y no a una instancia concreta. Por eso `main` es `static`: no existe aún ningún objeto cuando el programa arranca.

`static` no se usa solo en `main`; también se emplea para métodos utilitarios o constantes compartidas. Combinado con `final`, se usa habitualmente para definir constantes, cuyo valor no puede cambiar.

---

## 10. Compilación y ejecución en Java. JVM, bytecode y `.class`

Un programa Java se compila usando el comando `javac`, que transforma el código fuente (`.java`) en archivos `.class`. Estos archivos no contienen código máquina, sino **bytecode**.

La ejecución se realiza con el comando `java`, que lanza la **máquina virtual de Java (JVM)**. La JVM interpreta o compila dinámicamente el bytecode para la plataforma concreta.

Java es, por tanto, un lenguaje **compilado e interpretado**: se compila a bytecode y luego se ejecuta sobre una máquina virtual. Esto permite que el mismo `.class` funcione en distintos sistemas.

---

## 11. `new`, constructores y ejemplo con `Empleado`

La palabra clave `new` se utiliza para crear un nuevo objeto en memoria. Reserva espacio en el heap y devuelve una referencia al objeto recién creado.

Un **constructor** es un método especial que se ejecuta automáticamente al crear un objeto. Sirve para inicializar los atributos y asegurar que el objeto nace en un estado válido.

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

## 12. ¿Qué es `this`? ¿Se llama igual en todos los lenguajes? Ejemplo en `Punto`

`this` es una referencia al objeto actual, es decir, a la instancia sobre la que se está ejecutando el método. Permite distinguir entre atributos del objeto y variables locales con el mismo nombre.

No se llama igual en todos los lenguajes. En C++ se utiliza `this`, en Python se suele usar `self`, aunque el concepto es el mismo.

```java
class Punto {
    double x;
    double y;

    void mover(double x, double y) {
        this.x = x;
        this.y = y;
    }
}
```

---

## 13. Método `distanciaA`

```java
double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

---

## 14. Paso de parámetros: objetos vs tipos primitivos

En Java, los parámetros se pasan **por valor**, pero en el caso de los objetos, el valor que se copia es la **referencia**. Esto implica que modificar los atributos del objeto dentro del método sí afecta al objeto original.

Sin embargo, si se cambia la referencia para que apunte a otro objeto, ese cambio no se refleja fuera del método. Esto suele generar confusión y se describe informalmente como “paso por referencia”, aunque técnicamente no lo sea.

En el caso de tipos primitivos como `int`, se copia directamente el valor. Modificar ese valor dentro del método no tiene ningún efecto fuera de él.

---

## 15. ¿Qué es `toString()`? Ejemplo en `Punto`

`toString()` es un método que existe en todas las clases de Java, heredado de `Object`. Se utiliza para obtener una representación en forma de texto del objeto.

Muchos lenguajes tienen mecanismos similares, como `__str__` en Python. Sobrescribir este método facilita la depuración y la impresión de objetos.

```java
@Override
public String toString() {
    return "Punto(" + x + ", " + y + ")";
}
```

---

## 16. ¿Una clase es como un `struct` en C?

Una clase se parece a un `struct` en C en cuanto a que ambos agrupan datos relacionados bajo un mismo nombre. Sin embargo, el `struct` es una estructura pasiva, sin comportamiento asociado.

A un `struct` le faltan métodos integrados, control de visibilidad, constructores, herencia y polimorfismo. Además, no existe el concepto de instancia con identidad propia gestionada por el lenguaje.

Las clases combinan datos y comportamiento, y el lenguaje las trata como entidades de primer nivel, algo que no ocurre en C.

---

## 17. Emulación de `Punto` en C con `struct`

```c
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

double calculaDistanciaAOrigen(Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}
```

En este caso, `this` se sustituye explícitamente por un puntero pasado como parámetro. La función no “pertenece” al `struct`, sino que se le pasa manualmente.

La diferencia clave es que en C el programador gestiona explícitamente esa relación, mientras que en POO el lenguaje la integra de forma automática y más segura.
