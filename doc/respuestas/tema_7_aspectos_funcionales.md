<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C...

Un puntero a función en C es una variable que almacena la dirección de memoria de una función. Permite tratar a las funciones como valores, pudiendo pasarlas como parámetros, almacenarlas en estructuras o invocarlas indirectamente. Su sintaxis puede resultar menos intuitiva que la de variables normales, ya que incluye tanto el tipo de retorno como la lista de parámetros de la función apuntada.

En esencia, se define indicando el tipo de retorno y los tipos de los parámetros entre paréntesis. Posteriormente, se puede asignar la dirección de una función compatible y usar el puntero como si fuese la propia función. Esto es una forma primitiva de programación funcional en C, aunque sin soporte directo para cierres o contexto.

```c
#include <stdio.h>
#include <ctype.h>

void aMayusculas(char *cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
}

int main() {
    void (*ptr)(char *) = aMayusculas;  // puntero a función

    char texto[] = "hola mundo";
    ptr(texto);  // invocación mediante el puntero

    printf("%s\n", texto);
    return 0;
}
```

---

## 2. ¿Qué es una función lambda?... ejemplos en JS y Java

Una función lambda es una función anónima (sin nombre) que puede definirse inline y asignarse a variables o pasarse como argumento. Es habitual en lenguajes modernos y facilita un estilo más declarativo y funcional, reduciendo la necesidad de definir funciones completas cuando solo se necesitan pequeñas transformaciones.

En JavaScript, las lambdas (arrow functions) son muy naturales, mientras que en Java se introducen a partir de Java 8 y requieren una interfaz funcional como tipo objetivo.

```javascript
const aMayusculas = (cadena) => cadena.toUpperCase();

console.log(aMayusculas("hola mundo"));
```

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas =
            cadena -> cadena.toUpperCase();

        System.out.println(aMayusculas.apply("hola mundo"));
    }
}
```

---

## 3. ¿Qué es el paradigma funcional?... ciudadanos de primera clase

El paradigma funcional es un estilo de programación donde las funciones son el elemento principal. Se basa en evitar estados mutables y efectos secundarios, favoreciendo funciones puras que siempre producen el mismo resultado para los mismos datos de entrada.

Lenguajes como Java 8 se consideran multi-paradigma porque combinan orientación a objetos con características funcionales, como lambdas, streams o funciones de orden superior. Esto permite elegir el enfoque más adecuado según el problema.

Decir que las funciones son "ciudadanos de primera clase" significa que pueden almacenarse en variables, pasarse como argumentos, devolverse desde funciones y manipularse como cualquier otro valor.

---

## 4. Sintaxis básica de una función lambda en Java

La sintaxis de una lambda en Java consiste en una lista de parámetros, seguida de una flecha (`->`) y un cuerpo. Puede ser una expresión simple o un bloque de código. El tipo de los parámetros suele inferirse automáticamente.

Cuando el cuerpo es una sola expresión, no se necesitan llaves ni `return`. Si contiene varias instrucciones, se usan llaves y se debe especificar el retorno explícitamente si corresponde.

```java
// Forma simple
cadena -> cadena.toUpperCase()

// Forma completa
(String cadena) -> {
    return cadena.toUpperCase();
}
```

---

## 5. Método transformar con función como parámetro

Se puede definir un método que reciba una función como parámetro, lo que permite aplicar distintas transformaciones sin cambiar el método. Este patrón es fundamental en programación funcional.

En Java, se usa `Function<String, String>`, mientras que en JavaScript se pasa directamente una función.

```javascript
function transformar(texto, funcion) {
    return funcion(texto);
}

const aMayusculas = (cadena) => cadena.toUpperCase();

console.log(transformar("hola", aMayusculas));
```

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String texto,
                                     Function<String, String> f) {
        return f.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas =
            s -> s.toUpperCase();

        System.out.println(transformar("hola", aMayusculas));
    }
}
```

---

## 6. Lambda directamente en la llamada

Es posible definir la función lambda directamente al invocar el método, evitando variables intermedias. Esto es útil cuando la lógica es sencilla y no se reutilizará.

En este caso, se define una lambda que invierte la cadena en el mismo momento en que se pasa como argumento.

```javascript
console.log(transformar("hola", s =>
    s.split("").reverse().join("")
));
```

```java
System.out.println(transformar("hola", s ->
    new StringBuilder(s).reverse().toString()
));
```

---

## 7. Closure (cierre) en lambdas

Un closure es una función que captura variables de su entorno de definición. Esto permite que la función utilice valores externos incluso después de que ese contexto haya finalizado.

En Java, las variables capturadas deben ser efectivamente finales. Aun así, permiten construir funciones dependientes de un contexto.

```java
String sufijo = "!!!";

Function<String, String> concatenar =
    s -> s + sufijo;

System.out.println(transformar("hola", concatenar));
```

---

## 8. Diferencia entre lambda y punteros a función en C

Los punteros a función en C solo almacenan direcciones de funciones, sin ningún contexto adicional. No pueden capturar variables locales ni mantener estado asociado.

En cambio, las funciones lambda pueden capturar variables del entorno (closures), lo que permite construir funciones dinámicas y más expresivas. Además, las lambdas suelen integrarse con el sistema de tipos del lenguaje, ofreciendo mayor seguridad y flexibilidad.

---

## 9. Funciones que devuelven funciones (descuento)

Se puede crear una función que genere otras funciones parametrizadas. En este caso, una función que recibe un porcentaje y devuelve otra función que aplica ese descuento.

La closure se produce porque la lambda resultante captura el valor del porcentaje.

```java
import java.util.function.Function;

public class Main {
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return precio -> precio * (1 - porcentaje);
    }

    public static void main(String[] args) {
        var d10 = crearDescuento(0.10);
        var d20 = crearDescuento(0.20);

        System.out.println(d10.apply(100.0));
        System.out.println(d20.apply(100.0));
    }
}
```

---

## 10. ¿Qué es una interfaz funcional?

Una interfaz funcional es una interfaz que contiene exactamente un método abstracto. Se utiliza como tipo objetivo para expresiones lambda.

Puede tener métodos `default` o `static`, pero solo un método abstracto. Se suele anotar con `@FunctionalInterface` para mayor claridad y comprobación.

---

## 11. Interfaz funcional Transformador

Se puede definir manualmente una interfaz funcional para transformar cadenas.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String entrada);
}
```

Esto permite usar lambdas compatibles con esa firma.

---

## 12. Transformador genérico

Se puede generalizar usando generics para permitir transformar cualquier tipo en otro.

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}
```

Ejemplo:

```java
Transformador<Double, Integer> redondear =
    d -> (int) Math.round(d);

System.out.println(redondear.transformar(3.7));
```

---

## 13. Interfaces funcionales predefinidas

Java proporciona varias interfaces funcionales en `java.util.function`, como `Function`, `Consumer`, `Supplier`, `Predicate`, entre otras.

Estas cubren casos comunes: transformar valores, consumirlos, generarlos o evaluarlos. Su uso evita tener que definir interfaces propias en muchos casos.

---

## 14. Ejemplo con forEach

El método `forEach` permite recorrer colecciones de forma funcional, aplicando una acción a cada elemento.

```java
import java.util.*;

List<Integer> lista = Arrays.asList(1, -2, 3, -4);

lista.forEach(n -> {
    if (n > 0) {
        System.out.println("Positivo: " + n);
    }
});
```

---

## 15. PECS y Consumer<? super T>

PECS significa "Producer Extends, Consumer Super". Es una regla para usar comodines en generics.

`Consumer<? super T>` se usa porque el consumidor acepta elementos de tipo T o de sus supertipos. Esto aporta mayor flexibilidad.

En `transformar`, se podría usar:

```java
Function<? super String, ? extends String>
```

para permitir mayor reutilización.

---

## 16. Referencias a métodos (JS y Java)

Se pueden referenciar métodos directamente sin lambdas.

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const p = new Persona("Ana");
const ref = p.saludar.bind(p);

ref();
```

```java
class Persona {
    String nombre;
    Persona(String n) { nombre = n; }

    void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

Persona p = new Persona("Ana");
Runnable ref = p::saludar;

ref.run();
```

---

## 17. Tipos de referencias a método en Java

Java permite varios tipos: a métodos estáticos, a métodos de instancia de un objeto concreto, a métodos de instancia genéricos y a constructores.

```java
// Estático
Function<String, Integer> f1 = Integer::parseInt;

// Instancia concreta
Persona p = new Persona("Ana");
Runnable f2 = p::saludar;

// Instancia arbitraria
Function<String, String> f3 = String::toUpperCase;

// Constructor
Supplier<Persona> f4 = () -> new Persona("Luis");
```

---

## 18. Ordenar lista de Persona

Se puede ordenar con una lambda personalizada o usando utilidades de `Comparator`.

```java
Collections.sort(lista, (p1, p2) -> {
    if (p1.edad != p2.edad) {
        return p1.edad - p2.edad;
    }
    return p1.nombre.compareTo(p2.nombre);
});
```

```java
Collections.sort(lista,
    Comparator.comparingInt((Persona p) -> p.edad)
              .thenComparing(p -> p.nombre)
);
```

Este enfoque funcional mejora la legibilidad y reutilización del código.
