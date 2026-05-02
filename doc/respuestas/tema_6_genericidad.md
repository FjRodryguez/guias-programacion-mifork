<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

En C, una forma clásica de simular genericidad es mediante punteros genéricos (`void*`). Se puede implementar, por ejemplo, un vector dinámico que internamente almacene un array de `void*`, de manera que cada posición pueda apuntar a cualquier tipo de dato.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    void** datos;
    int capacidad;
    int size;
} Vector;

Vector* crearVector(int capacidad) {
    Vector* v = malloc(sizeof(Vector));
    v->datos = malloc(sizeof(void*) * capacidad);
    v->capacidad = capacidad;
    v->size = 0;
    return v;
}

void add(Vector* v, void* elemento) {
    if (v->size < v->capacidad) {
        v->datos[v->size++] = elemento;
    }
}
```

En Java, el equivalente sería usar `Object` como tipo base, ya que todas las clases heredan de él. Se puede construir una estructura similar usando un array de `Object`.

```java
class Vector {
    private Object[] datos;
    private int size = 0;

    public Vector(int capacidad) {
        datos = new Object[capacidad];
    }

    public void add(Object elemento) {
        datos[size++] = elemento;
    }

    public Object get(int i) {
        return datos[i];
    }
}
```

En ambos casos, la estructura permite almacenar cualquier tipo, pero se pierde información concreta del tipo en tiempo de compilación, lo cual tiene implicaciones importantes en seguridad de tipos.

---

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica?

### Respuesta

La programación genérica consiste en escribir código que pueda trabajar con distintos tipos de datos sin necesidad de duplicarlo para cada tipo concreto. Se basa en abstraer el tipo de los datos, permitiendo que una misma estructura o algoritmo funcione con enteros, cadenas u otros tipos definidos por el usuario.

El objetivo principal es reutilizar código manteniendo la seguridad de tipos. En lenguajes modernos como Java o C++, esto se consigue mediante mecanismos como parámetros de tipo o plantillas, que permiten especificar el tipo en el momento de uso, manteniendo comprobaciones en compilación.

El ejemplo anterior puede considerarse una forma muy básica y primitiva de programación genérica, ya que permite almacenar cualquier tipo. Sin embargo, no es una solución completa, porque no garantiza seguridad de tipos ni evita errores en tiempo de ejecución, lo cual sí logran los mecanismos modernos de genericidad.

---

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas.

### Respuesta

El principal problema es la pérdida de información de tipo en tiempo de compilación. Al usar `void*` en C o `Object` en Java, el compilador ya no puede verificar que los datos almacenados y recuperados sean del tipo correcto. Esto implica que muchos errores pasan desapercibidos hasta la ejecución.

Otro problema importante es la necesidad de realizar conversiones explícitas (casting). En Java, al recuperar un elemento de tipo `Object`, es necesario hacer un *downcasting* al tipo original. Si el tipo no coincide, se produce una excepción en tiempo de ejecución (`ClassCastException`), lo que introduce fragilidad en el código.

Además, se pierde expresividad: no se puede restringir qué tipos son válidos para una estructura. Por ejemplo, no se puede declarar que una lista solo contenga números o cadenas. Esto dificulta el mantenimiento y aumenta la probabilidad de errores difíciles de detectar.

---

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**?

### Respuesta

Los parámetros de tipo son una forma de definir clases, interfaces o métodos que trabajan con tipos genéricos, especificando dichos tipos como parámetros formales. Se suelen representar mediante letras como `<T>`, `<E>` o `<K, V>`, y se concretan al instanciar la clase o invocar el método.

Este mecanismo permite que el compilador conozca el tipo concreto en el momento de uso, lo que posibilita realizar comprobaciones estáticas de tipo. De esta manera, se evita el uso de conversiones explícitas y se reduce el riesgo de errores en tiempo de ejecución.

Por ejemplo, una lista genérica en Java se define como `List<T>`, y al usarla como `List<String>`, el compilador garantiza que solo se almacenarán cadenas y que los elementos recuperados serán de tipo `String`.

---

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En Java, se puede usar `ArrayList<String>` para asegurar que solo se almacenan cadenas. El compilador garantiza que no se insertarán otros tipos y que no será necesario hacer casting al recuperar los elementos.

```java
import java.util.*;

public class Ejemplo {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();
        lista.add("Hola");
        lista.add("Mundo");

        for (String s : lista) {
            System.out.println(s.toUpperCase());
        }
    }
}
```

En C++, se puede usar `std::vector<std::string>`, donde la plantilla asegura el tipo en compilación.

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> v;
    v.push_back("Hola");
    v.push_back("Mundo");

    for (const std::string& s : v) {
        std::cout << s << std::endl;
    }
}
```

En ambos casos, el tipo de los elementos está completamente determinado en compilación, evitando errores de tipo y eliminando la necesidad de conversiones explícitas.

---

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Cuando se instancia una clase genérica, el compilador utiliza la información del parámetro de tipo para verificar que todas las operaciones son válidas. Sin embargo, la forma en que esto se implementa internamente difiere entre Java y C++.

En Java, se utiliza el mecanismo de *type erasure* (borrado de tipos). Esto significa que, tras la compilación, la información de los parámetros de tipo se elimina y se reemplaza por su límite superior (por defecto `Object`). Por tanto, en tiempo de ejecución no existen realmente los tipos genéricos, aunque el compilador haya comprobado su corrección.

En C++, en cambio, se realiza una instanciación de plantillas. El compilador genera una versión concreta del código para cada tipo utilizado. Por ejemplo, `vector<int>` y `vector<string>` son implementaciones distintas generadas automáticamente, lo que mantiene la información de tipo incluso en tiempo de ejecución.

---

## 7. Vamos a crear una nueva clase con parámetros de tipo...

### Respuesta

Se puede definir una clase genérica `Par` con dos parámetros de tipo distintos:

```java
class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}
```

Un ejemplo de uso sería devolver media y desviación típica:

```java
public static Par<Double, Double> calcular(double[] datos) {
    double suma = 0;
    for (double d : datos) suma += d;
    double media = suma / datos.length;

    double var = 0;
    for (double d : datos) var += Math.pow(d - media, 2);
    double desviacion = Math.sqrt(var / datos.length);

    return new Par<>(media, desviacion);
}
```

En este caso, el tipo de cada componente está claramente definido (`Double`), evitando errores y eliminando la necesidad de conversiones.

---

## 8. Método genérico `seleccionaUno`

### Respuesta

Sin genéricos, se podría definir usando `Object`, pero esto obliga a hacer casting:

```java
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}
```

Esto permite mezclar tipos distintos y requiere *downcasting*, lo cual es inseguro.

Con genéricos:

```java
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}
```

Aquí el compilador garantiza que ambos parámetros son del mismo tipo y que el valor devuelto también lo es, eliminando conversiones y errores de tipo.

---

## 9. Restricciones en parámetros de tipo

### Respuesta

Sí, se pueden restringir los parámetros de tipo mediante límites. Por ejemplo:

```java
class Punto<T extends Number> {
    private T x, y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto<T> p) {
        double dx = x.doubleValue() - p.x.doubleValue();
        double dy = y.doubleValue() - p.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

Sin genéricos:

```java
class Punto {
    private Number x, y;
}
```

Tras compilación, debido al *type erasure*, el tipo real se convierte en `Number`, que es el límite superior especificado.

---

## 10. Reflexión sobre chequeo de tipos

### Respuesta

Ambas soluciones permiten reutilización, pero difieren en seguridad de tipos. Sin genéricos, es posible mezclar tipos distintos (por ejemplo, un entero y un real), ya que ambos son `Number`.

Con genéricos (`<T extends Number>`), se fuerza que ambas coordenadas sean del mismo tipo, lo que mejora la coherencia del objeto y evita mezclas accidentales.

Además, el método `getX` devuelve `Number` en la versión sin genéricos, mientras que en la versión genérica devuelve `T`, proporcionando mayor precisión de tipos en tiempo de compilación.

---

## 11. Ejemplo avanzado con `Punto`

### Respuesta

Se puede parametrizar la interfaz para asegurar tipos homogéneos:

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

Implementación:

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x; this.y = y;
    }

    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}
```

Esto evita `instanceof` y casting, ya que el compilador garantiza que solo se comparan puntos del mismo tipo.

---

## 12. Covarianza e invariancia

### Respuesta

`List<String>` no es subtipo de `List<Object>` porque los genéricos en Java son invariantes. Permitirlo rompería la seguridad de tipos, ya que se podría insertar un `Integer` en una lista de `String`.

Sin embargo, los arrays sí son covariantes: `String[]` es subtipo de `Object[]`. Esto puede provocar errores en ejecución, como `ArrayStoreException`, si se intenta insertar un tipo incorrecto.

Un tipo es covariante si preserva la relación de subtipos, contravariante si la invierte, e invariante si no permite ninguna relación entre distintos parámetros de tipo.

---

## 13. Wildcards

### Respuesta

Un wildcard (`?`) representa un tipo desconocido. Permite recuperar cierta flexibilidad en genéricos sin perder completamente la seguridad de tipos.

`List<? extends T>` se usa cuando solo se quiere leer (covarianza). Por ejemplo:

```java
public static double suma(List<? extends Number> lista) {
    double total = 0;
    for (Number n : lista) total += n.doubleValue();
    return total;
}
```

`List<? super T>` se usa cuando se quiere escribir (contravarianza):

```java
public static void addEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
}
```

El primero permite tratar listas de subtipos como listas de lectura, mientras que el segundo permite insertar elementos de un tipo específico en estructuras más generales.
