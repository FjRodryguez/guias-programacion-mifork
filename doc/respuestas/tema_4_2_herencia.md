<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo...

### Respuesta

En orientación a objetos, la **herencia** es un mecanismo que permite a una clase (subclase) reutilizar y extender otra (superclase). La relación “A es-un B” implica que un objeto de la subclase puede ser tratado como uno de la superclase. Por ejemplo, un `Artillero` es un `Soldado`, por lo que puede usarse donde se espere un `Soldado`.

Esto implica, por un lado, **compatibilidad de tipos**, permitiendo que una referencia de `Soldado` apunte a objetos de `Artillero` o `Zapador`. Por otro lado, implica **herencia de estado y comportamiento**, ya que las subclases heredan atributos y métodos de la superclase.

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() { return cohetes; }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() { return minas; }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] soldados = {
            new Artillero("Juan", 5),
            new Zapador("Luis", 3)
        };

        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}
```

---

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre?

### Respuesta

Al crear un objeto de una subclase, se ejecutan todos los constructores de la jerarquía, desde la superclase hasta la subclase. Primero se ejecuta el constructor de `Soldado` y después el de la clase concreta como `Artillero`.

La palabra clave `super` se utiliza para invocar el constructor de la superclase y debe aparecer como primera instrucción. Si no se indica, Java intenta llamar automáticamente al constructor sin parámetros.

Si la superclase no tiene un constructor sin parámetros accesible, es obligatorio usar `super(...)` con parámetros. En ese caso, no hacerlo provoca un error de compilación.

---

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase?

### Respuesta

Los atributos privados de la superclase sí forman parte del objeto en memoria, incluso cuando se instancia una subclase. Un objeto de `Artillero` contiene también el atributo `nombre` heredado de `Soldado`.

Sin embargo, esto no implica que puedan usarse directamente desde la subclase. Al ser `private`, solo pueden ser accedidos desde la propia clase `Soldado`.

Por tanto, aunque existan en memoria, la subclase solo puede interactuar con ellos mediante métodos públicos o protegidos definidos en la superclase.

---

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado`...

### Respuesta

La compatibilidad de tipos permite escribir código que depende de la abstracción (`Soldado`) en lugar de implementaciones concretas. Esto facilita la extensibilidad, ya que se pueden añadir nuevas subclases sin modificar el código existente.

Por ejemplo, se puede añadir un nuevo tipo:

```java
class Medico extends Soldado {
    public Medico(String nombre) {
        super(nombre);
    }
}
```

El código que recorre los soldados no cambia:

```java
Soldado[] soldados = {
    new Artillero("Juan", 5),
    new Zapador("Luis", 3),
    new Medico("Carlos")
};

for (Soldado s : soldados) {
    s.saludar();
}
```

Esto demuestra que el sistema es abierto a extensión y cerrado a modificación.

---

## 5. En Java, cuando trabajo con referencias y herencia...

### Respuesta

En Java, una referencia del supertipo puede apuntar a objetos de subtipos. Sin embargo, solo permite acceder a los métodos definidos en el tipo de la referencia.

El **upcasting** es la conversión automática de un subtipo a un supertipo. El **downcasting** es la conversión explícita inversa, que puede fallar si el objeto no es del tipo esperado. Para evitar errores, se utiliza `instanceof`.

```java
for (Soldado s : soldados) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;
        System.out.println(a.getCohetes());
    }
}
```

Esto permite acceder de forma segura a funcionalidades específicas.

---

## 6. Respecto a la ocultación de información y herencia...

### Respuesta

El modificador `protected` permite que un atributo o método sea accesible desde la propia clase, sus subclases y otras clases del mismo paquete.

Se utiliza cuando se quiere permitir a las subclases acceder directamente a ciertos atributos sin hacerlos públicos.

```java
class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }

    public void ponerMina() {
        System.out.println(nombre + " pone una mina");
    }
}
```

---

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

No todos los lenguajes orientados a objetos tienen una clase base común, aunque muchos sí la incluyen.

En Java, todas las clases heredan de `Object`, lo que proporciona métodos comunes a todos los objetos.

Esto facilita el tratamiento uniforme de cualquier objeto dentro del lenguaje.

---

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple permite que una clase herede de varias superclases, lo que puede generar conflictos y ambigüedades.

Java no permite herencia múltiple de clases, solo de interfaces.

Esto simplifica el modelo y evita problemas como el conflicto de métodos heredados.

---

## 9. Las excepciones en los lenguajes orientados a objetos son objetos...

### Respuesta

En Java se pueden crear excepciones personalizadas extendiendo de `RuntimeException` si se desea que sean no controladas.

```java
class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() { return nombre; }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }
}
```

Esto permite añadir información adicional al error.

---

## 10. Herencia vs. Composición...

### Respuesta

La herencia implica una relación fuerte entre clases, por lo que no debe usarse solo para reutilizar código.

Puede generar acoplamiento excesivo y dificultar el mantenimiento del sistema.

Por ello, es preferible considerar la composición como alternativa.

---

## 11. Herencia vs. Composición...

### Respuesta

La composición permite construir clases a partir de otras, delegando comportamiento.

Esto reduce el acoplamiento y mejora la flexibilidad del sistema.

Como resultado, el código es más mantenible y adaptable.

---

## 12. Herencia vs. Composición...

### Respuesta

La herencia rompe la encapsulación porque expone detalles internos de la superclase a las subclases.

Esto puede hacer que cambios internos afecten al comportamiento de las subclases.

Por tanto, debe usarse con precaución.

---

## 13. Pongamos un ejemplo de dos alternativas para lo mismo...

### Respuesta

**Herencia:**

```java
class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
```

**Composición:**

```java
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
```

La composición ofrece mayor flexibilidad y mejor separación de responsabilidades.
