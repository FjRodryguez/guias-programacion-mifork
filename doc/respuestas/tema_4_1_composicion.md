<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

En C, la composición se implementa incluyendo una estructura dentro de otra. De este modo, se puede modelar que una línea “tiene” dos puntos, y que cada punto “tiene” dos coordenadas. Esta relación es directa y no existe encapsulación, por lo que los campos son accesibles desde cualquier parte del programa.

A continuación se muestra un ejemplo funcional donde se definen ambas estructuras y se implementan funciones para calcular la distancia entre puntos y la longitud de una línea:

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    float x;
    float y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

float distancia(Punto a, Punto b) {
    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
}

float longitud(Linea l) {
    return distancia(l.p1, l.p2);
}
```

Este diseño refleja claramente la composición estructural, aunque no permite proteger los datos ni garantizar invariantes como sí ocurre en orientación a objetos.

---

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.

### Respuesta

En Java, la composición se expresa mediante atributos de tipo objeto. A diferencia de C, se puede aplicar encapsulación para ocultar el estado interno y garantizar inmutabilidad, evitando modificaciones tras la construcción de los objetos.

A continuación se presenta una implementación donde tanto `Punto` como `Linea` son inmutables mediante el uso de atributos `private final` y ausencia de métodos modificadores:

```java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) +
                         Math.pow(otro.y - this.y, 2));
    }
}
```

```java
public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```

Gracias a este diseño, se garantiza que ni los puntos ni la línea puedan modificarse tras su creación, lo que mejora la robustez respecto a la versión en C.

---

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad indica cuántas instancias de una clase pueden estar asociadas a otra dentro de una relación. Es un concepto importante en el modelado de sistemas, ya que define restricciones sobre el número de objetos implicados.

En el ejemplo, una `Linea` está compuesta exactamente por dos objetos `Punto`. Por tanto, la multiplicidad de `Linea` a `Punto` es **2..2**, es decir, exactamente dos puntos.

En sentido inverso, un `Punto` puede pertenecer a ninguna, una o varias líneas, ya que no se ha establecido ninguna restricción. Por tanto, la multiplicidad de `Punto` a `Linea` es **0..***.

---

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La composición fuerte implica una relación en la que el objeto contenido depende completamente del contenedor. Su ciclo de vida está ligado: si el contenedor se destruye, también lo hacen sus componentes. Este tipo de relación es lo que se denomina propiamente **composición**.

Por otro lado, la composición débil implica que los objetos pueden existir independientemente del contenedor. El contenedor solo mantiene referencias, pero no controla su existencia. Este tipo se conoce como **agregación** o asociación.

La diferencia fundamental radica en el ciclo de vida: en la composición fuerte existe dependencia total, mientras que en la débil los objetos pueden sobrevivir por separado.

---

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

Cuando una clase utiliza otra únicamente de forma puntual, como en parámetros, valores de retorno o variables locales, se habla de **dependencia** y no de composición.

La composición implica una relación estructural permanente, donde un objeto forma parte del estado interno de otro. En cambio, la dependencia es temporal y no implica propiedad ni pertenencia.

Por tanto, el uso de `new` dentro de un método o el paso de objetos como parámetros no constituye composición, sino una relación más débil de dependencia.

---

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

En composición fuerte, la clase contenedora crea y controla completamente los objetos contenidos. En este caso, la línea crea sus propios puntos, por lo que estos no existen fuera de ella.

```java
public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}
```

En composición débil, los puntos se crean fuera y se pasan a la línea, pudiendo ser compartidos o reutilizados:

```java
public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
}
```

---

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java, la destrucción de objetos no es explícita, sino que se gestiona mediante el recolector de basura. Este elimina automáticamente los objetos que ya no son accesibles desde ninguna referencia activa.

En una composición fuerte, cuando el objeto contenedor deja de ser accesible, los objetos que contiene también lo dejan de ser. Como consecuencia, el recolector de basura los eliminará eventualmente.

No es necesario que la clase `Linea` destruya explícitamente los `Punto`, ya que Java no utiliza gestión manual de memoria como en C o C++.

---

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

En este caso, se modela una composición débil, ya que los profesores pueden existir independientemente del departamento. Se mantiene la encapsulación ocultando el array interno y garantizando las invariantes mediante comprobaciones y excepciones.

```java
public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
public class Departamento {
    private Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor director) {
        this.director = director;
        profesores[numProfesores++] = director;
    }

    public void addProfesor(Profesor p) {
        if (numProfesores >= 50) throw new RuntimeException("Límite alcanzado");
        profesores[numProfesores++] = p;
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) throw new RuntimeException("Posición inválida");
        if (profesores[pos] == director) throw new RuntimeException("No se puede eliminar al director");

        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        numProfesores--;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) throw new RuntimeException("Posición inválida");
        return profesores[pos];
    }

    public void setDirector(Profesor nuevo) {
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevo) {
                encontrado = true;
                break;
            }
        }
        if (!encontrado) throw new RuntimeException("Debe pertenecer al departamento");
        director = nuevo;
    }
}
```

---

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

El uso de `List` simplifica notablemente la implementación, ya que elimina la necesidad de gestionar manualmente el tamaño, los desplazamientos y los límites del array.

```java
import java.util.*;

public class Departamento {
    private List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor director) {
        this.director = director;
        profesores.add(director);
    }

    public void addProfesor(Profesor p) {
        profesores.add(p);
    }

    public void removeProfesor(int pos) {
        if (profesores.get(pos) == director)
            throw new RuntimeException("No se puede eliminar al director");
        profesores.remove(pos);
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}
```

Se evita escribir código para gestionar el array manualmente, como el control de capacidad o el desplazamiento de elementos.

Si se devolviera directamente la lista interna, se rompería la encapsulación, ya que el cliente podría modificarla. Esto se soluciona devolviendo una vista inmutable o una copia de la lista.

---

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

La composición recursiva ocurre cuando una clase contiene una referencia a otra instancia de sí misma. Esto permite modelar estructuras jerárquicas o encadenadas, como árboles o genealogías.

```java
public class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("Bea", abuela);
        Persona hijo = new Persona("Carlos", madre);

        System.out.println(hijo.getMadre().getNombre());
    }
}
```

Otros ejemplos clásicos incluyen árboles binarios, listas enlazadas o estructuras de directorios.

---

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Una relación bidireccional implica que ambas clases mantienen referencias entre sí. Es decir, no solo el `Departamento` conoce a sus `Profesor`, sino que cada `Profesor` también conoce el departamento al que pertenece.

Para implementarlo, se debe añadir un atributo en `Profesor` que referencie al `Departamento`. Además, cada vez que se añada o elimine un profesor del departamento, se debe actualizar también la referencia en el objeto `Profesor`.

Esto introduce mayor complejidad, ya que se debe garantizar la coherencia en ambas direcciones, evitando inconsistencias como un profesor que apunte a un departamento distinto al que realmente pertenece.
