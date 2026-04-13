<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

El **polimorfismo** es un principio de la programación orientada a objetos que permite tratar objetos de distintas clases relacionadas mediante un tipo común (normalmente una superclase). De este modo, una misma referencia puede apuntar a objetos diferentes, y cada uno responderá con su comportamiento específico al invocar métodos. Su principal utilidad es permitir escribir código más general, flexible y extensible, evitando dependencias directas de clases concretas.

Este mecanismo facilita que el código cliente no tenga que conocer los detalles de cada subclase, delegando la responsabilidad del comportamiento en cada implementación concreta. Así, se mejora la reutilización y se reduce el acoplamiento, algo que no existe en C y que en C++ requiere mecanismos explícitos.

La **sobreescritura de métodos** (*overriding*) consiste en redefinir en una subclase un método heredado de la clase base, manteniendo la misma firma. Cuando se invoca ese método sobre un objeto, se ejecuta la versión correspondiente a su tipo real en tiempo de ejecución.

---

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La **ligadura dinámica** o **enlace tardío** es el proceso mediante el cual la selección del método a ejecutar se realiza en tiempo de ejecución, en lugar de en tiempo de compilación. Esto significa que la decisión depende del tipo real del objeto al que apunta una referencia, y no del tipo declarado de dicha referencia.

Este mecanismo es esencial para el polimorfismo, ya que permite que una misma llamada a un método tenga comportamientos distintos según el objeto concreto. Sin ligadura dinámica, el polimorfismo quedaría limitado, ya que todas las llamadas se resolverían de forma estática.

En **C++**, es necesario indicar explícitamente qué métodos deben usar ligadura dinámica mediante la palabra clave `virtual`. Si no se hace, el compilador usa enlace temprano (estático). En **Java**, en cambio, todos los métodos (salvo `static`, `final` o `private`) utilizan ligadura dinámica por defecto, por lo que no es necesario indicarlo.

En **Python**, el enlace es dinámico de forma natural, ya que es un lenguaje dinámico. Todas las llamadas a métodos se resuelven en tiempo de ejecución sin necesidad de palabras clave específicas.

---

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

Se define una clase base `Soldado` con un método `saludar`, y dos subclases que proporcionan su propia implementación. En el caso de `Zapador`, se reemplaza completamente el comportamiento heredado:

```java
class Soldado {
    public void saludar() {
        System.out.println("Soy un soldado.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Soy un zapador.");
    }
}

class Artillero extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Soy un artillero.");
    }
}
```

Para ilustrar el polimorfismo, se utiliza un array de tipo `Soldado` que contiene instancias de distintas subclases. Aunque todas las referencias son del tipo base, cada objeto responde con su propia implementación:

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = {
            new Zapador(),
            new Artillero(),
            new Zapador()
        };

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

En tiempo de ejecución, se invoca la versión correcta del método según el tipo real del objeto, demostrando el uso del polimorfismo.

---

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí, es posible invocar el método de la clase base incluso cuando ha sido sobreescrito. Esto permite reutilizar el comportamiento original y ampliarlo con funcionalidad adicional en la subclase, en lugar de reemplazarlo completamente.

En Java, se utiliza la palabra clave `super` para acceder al método de la clase base. De este modo, el `Zapador` puede mantener el saludo original y añadir un mensaje adicional:

```java
class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```

De esta forma, se ejecuta primero el comportamiento heredado y después el específico. La palabra clave utilizada es `super`, que permite acceder explícitamente a la implementación de la superclase.

---

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al sobreescribir un método en Java, es obligatorio mantener la misma lista de parámetros (tipo, número y orden). El tipo de retorno debe ser el mismo o un subtipo compatible (covarianza). Además, no se puede restringir la visibilidad del método respecto al original, aunque sí ampliarla.

La **sobreescritura (overriding)** implica redefinir un método heredado en una subclase, y su resolución ocurre en tiempo de ejecución. La **sobrecarga (overloading)** consiste en definir métodos con el mismo nombre pero diferentes parámetros en la misma clase, y se resuelve en tiempo de compilación.

La anotación `@Override` indica que un método está destinado a sobreescribir otro de la superclase. Su uso es recomendable porque permite al compilador detectar errores (por ejemplo, firmas incorrectas), mejorando la seguridad y la claridad del código.

---

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Sí, el polimorfismo se utiliza desde fases muy tempranas al aprender Java, incluso sin que se haga explícito. Esto ocurre porque muchas clases heredan de `Object`, y al redefinir métodos como `toString` o `equals`, ya se está participando en comportamiento polimórfico.

Cuando estos métodos son invocados, Java decide en tiempo de ejecución qué implementación ejecutar según el tipo real del objeto. Esto es precisamente ligadura dinámica, base del polimorfismo.

Por tanto, aunque no se trabaje aún con jerarquías complejas, el simple hecho de sobreescribir estos métodos ya implica el uso de polimorfismo.

---

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una **clase abstracta** es una clase que no puede ser instanciada directamente y que sirve como base para otras clases. Puede contener tanto métodos con implementación como métodos abstractos. Su propósito es definir una estructura común y obligar a las subclases a implementar ciertos comportamientos.

Un **método abstracto** es un método sin implementación que debe ser definido obligatoriamente en las subclases. Se declara con la palabra clave `abstract` y sin cuerpo. No es posible crear instancias de una clase abstracta.

Ejemplo:

```java
abstract class Soldado {
    public void saludar() {
        System.out.println("Soy un soldado.");
    }

    public abstract void atacar();
}
```

Las subclases deben implementar `atacar`. La palabra clave `abstract` se coloca tanto en la clase como en los métodos que no tienen implementación.

---

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

La palabra clave `final` impide la modificación mediante herencia. Si se aplica a un método, este no puede ser sobreescrito en subclases. Si se aplica a una clase, esta no puede ser extendida.

Esto limita el uso del polimorfismo, ya que impide redefinir comportamientos en subclases. Por tanto, reduce la flexibilidad del diseño basado en herencia.

Un ejemplo conocido es la clase `String`, que es `final`. Esto garantiza que su comportamiento no puede ser alterado, lo cual es importante para la seguridad y la inmutabilidad.

---

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Las **interfaces** son estructuras que definen un conjunto de métodos que una clase debe implementar, actuando como un contrato. Tradicionalmente no contienen implementación ni estado, aunque versiones modernas permiten métodos por defecto.

Son similares a las clases abstractas, pero más restrictivas en cuanto a estado y más flexibles en cuanto a herencia. Las clases abstractas pueden tener implementación y atributos, mientras que las interfaces se centran en definir comportamiento.

Una clase puede implementar múltiples interfaces, lo que permite combinar distintos comportamientos sin recurrir a herencia múltiple de clases. Esto aporta gran flexibilidad en el diseño de software.

---

## 10. Vamos a poner un ejemplo nuevo con polimorfismo...

### Respuesta

Se define una clase abstracta `Punto` con un método abstracto para calcular distancias:

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}
```

Implementaciones concretas con verificación de tipo:

```java
class Punto2D extends Punto {
    double x, y;

    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Tipo incompatible");
        }
        Punto2D p = (Punto2D) otro;
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}

class Punto3D extends Punto {
    double x, y, z;

    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Tipo incompatible");
        }
        Punto3D p = (Punto3D) otro;
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2));
    }
}
```

Clase `Linea` que usa polimorfismo:

```java
class Linea {
    Punto a, b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}
```

La clase `Linea` no necesita conocer el tipo concreto de los puntos, ya que el comportamiento se resuelve dinámicamente.

---

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo...

### Respuesta

La **herencia de interfaces** permite que una interfaz extienda otra, heredando sus métodos. Esto facilita la reutilización y la organización de contratos de comportamiento.

Sí existe **herencia múltiple de interfaces**, ya que una interfaz puede extender varias a la vez. Esto es posible porque no hay conflictos de implementación directa en la mayoría de los casos.

Ejemplo:

```java
interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}
```

Una clase que implemente `FicheroEscribible` deberá implementar todos los métodos definidos en ambas interfaces, combinando así varios comportamientos.
