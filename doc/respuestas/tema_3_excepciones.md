<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

En C, al no existir excepciones, los errores se indican mediante valores de retorno o mecanismos externos a la función. La idea es que la función `raiz` no imprima mensajes ni termine el programa, sino que comunique al código llamador que algo ha fallado, para que este decida cómo informar al usuario. Este diseño desacopla el cálculo del manejo del error, de forma similar a cómo en Java se separa el `throw` del `catch`.

Una opción clásica consiste en devolver un valor especial que indique error, por ejemplo `-1.0`, junto con una convención documentada. El llamador debe comprobar ese valor y actuar en consecuencia. El inconveniente es que el valor especial puede confundirse con un resultado válido en otros problemas más generales, por lo que esta técnica requiere cuidado en el diseño de la interfaz.

```c
#include <math.h>

double raiz(double x) {
    if (x < 0.0) {
        return -1.0;  // valor especial que indica error
    }
    return sqrt(x);
}

int main() {
    double r = raiz(-9.0);
    if (r < 0.0) {
        printf("Error: no se puede calcular la raíz de un número negativo\n");
    }
}
```

Otra opción es usar un parámetro adicional para comunicar el estado de error (por ejemplo, un entero que actúe como “bandera”), dejando el valor de retorno solo para el resultado válido. Este enfoque evita la ambigüedad de los valores especiales y se parece más a la idea de separar “resultado” de “estado”, que luego en Java se consigue con excepciones.

```c
#include <math.h>

double raiz(double x, int *ok) {
    if (x < 0.0) {
        *ok = 0;
        return 0.0;
    }
    *ok = 1;
    return sqrt(x);
}

int main() {
    int ok;
    double r = raiz(-9.0, &ok);
    if (!ok) {
        printf("Error: argumento negativo\n");
    }
}
```

---

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un mecanismo del lenguaje que permite señalar que ha ocurrido una situación anómala durante la ejecución de un programa, interrumpiendo el flujo normal de instrucciones. En lugar de devolver códigos de error de forma manual, el lenguaje proporciona una vía estructurada para “saltar” desde el punto donde ocurre el problema hasta un manejador adecuado en otro nivel de la pila de llamadas.

El objetivo principal al implementar funciones es separar claramente la lógica normal del tratamiento de errores. El código que detecta el problema se limita a indicar que la operación no puede continuar, mientras que el código llamador decide cómo reaccionar: informar al usuario, reintentar, liberar recursos o abortar la operación. Esto mejora la legibilidad del programa y reduce la cantidad de comprobaciones repetitivas tras cada llamada.

Desde el punto de vista del código que llama, las excepciones permiten centralizar el manejo de errores en un solo punto, evitando tener que comprobar manualmente cada valor de retorno. Este enfoque se alinea con el principio de encapsulación: cada función expone su comportamiento normal y, si algo falla, delega la decisión de qué hacer a niveles superiores, sin mezclar ambas responsabilidades.

---

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java, el control de errores se implementa de forma estructurada mediante excepciones. El método que calcula la raíz no devuelve un valor especial en caso de error, sino que “lanza” una excepción cuando recibe un argumento inválido. De este modo, el contrato del método queda claro: si se proporciona un número negativo, la operación no es válida y se comunica mediante una excepción.

La clase `Calculadora` encapsula la operación de cálculo, mientras que el método `main` asume la responsabilidad de decidir qué hacer cuando ocurre el problema. Esta separación hace que el código sea más limpio que en C, ya que no es necesario comprobar manualmente valores de retorno tras cada llamada. El control del error se concentra en el bloque `try-catch`.

```java
public class Calculadora {
    public static double raiz(double x) {
        if (x < 0.0) {
            throw new IllegalArgumentException("Argumento negativo");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double r = Calculadora.raiz(-9.0);
            System.out.println("Resultado: " + r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

Este diseño evita que el método `raiz` tenga que decidir cómo se informa al usuario. El método solo expresa que no puede realizar la operación, y el código llamador es el que define la política de manejo del error. Esto se ajusta bien al estilo orientado a objetos, donde cada clase se responsabiliza de su comportamiento y no del contexto en el que se usa.

---

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

Lanzar una excepción consiste en crear o activar un objeto de tipo excepción para indicar que se ha producido un error y que la ejecución normal no puede continuar en ese punto. En el ejemplo de la raíz cuadrada, el método `raiz` lanza una `IllegalArgumentException` cuando recibe un número negativo. A partir de ese momento, se interrumpe el flujo normal del método y no se ejecutan las instrucciones siguientes.

Capturar o controlar una excepción significa definir un bloque `catch` que recibe esa excepción y ejecuta un código alternativo para gestionar la situación de error. En el `main`, el bloque `catch` captura la excepción lanzada por `raiz` y muestra un mensaje al usuario. Este es el punto donde se decide qué hacer ante el fallo, separando la detección del problema de su tratamiento.

La propagación ocurre cuando una excepción no es capturada en el método donde se produce y “sube” por la pila de llamadas buscando un manejador adecuado. Cada función por la que pasa se abandona inmediatamente, sin reanudar su ejecución normal. Las funciones que no controlan la excepción no continúan después de la línea que causó el error; su ejecución se corta y solo se reanuda el flujo cuando se alcanza un `catch` compatible o cuando el programa termina si nadie la captura.

```java
public static double raiz(double x) {
    if (x < 0.0) {
        throw new IllegalArgumentException("Argumento negativo");
    }
    return Math.sqrt(x);
}
```

---

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La propagación natural de las excepciones permite que un error detectado en un nivel bajo de la aplicación se comunique automáticamente a niveles superiores sin necesidad de pasar códigos de error manualmente por cada llamada intermedia. En C, cada función debe devolver un valor que indique éxito o fallo y cada llamador debe comprobarlo explícitamente, lo que genera código repetitivo y propenso a olvidos.

Gracias a la propagación, el código intermedio que no sabe cómo resolver el problema no necesita intervenir. Este código se mantiene limpio y enfocado en su lógica principal, mientras que el manejo del error se centraliza en los puntos donde realmente tiene sentido decidir qué hacer, por ejemplo en la interfaz con el usuario o en la capa de control de la aplicación.

Otra ventaja importante es la mejora de la legibilidad y del mantenimiento del programa. Al no estar mezcladas las comprobaciones de error con la lógica normal, el código resulta más claro y menos cargado de condiciones. Además, la propagación automática reduce el riesgo de que un error se ignore por descuido, ya que si no se captura, el programa falla de forma explícita en lugar de continuar en un estado inconsistente.

---

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

En orientación a objetos, las excepciones suelen ser objetos que pertenecen a una jerarquía de clases. Esto significa que una excepción no es solo un código numérico, sino una entidad con estado y comportamiento, capaz de almacenar información relevante sobre el error ocurrido. Este diseño encaja con el modelo de clases y objetos que ya se utiliza para representar otros conceptos del programa.

Desde el punto de vista de la encapsulación, el objeto excepción puede contener dentro de sí todos los datos necesarios para describir el problema: un mensaje, el tipo concreto de error y, opcionalmente, otra excepción que lo haya provocado. De este modo, la información del error viaja junto con la excepción sin necesidad de variables globales ni parámetros adicionales, lo que reduce el acoplamiento entre funciones.

Este enfoque permite además crear excepciones personalizadas que representen errores propios del dominio de la aplicación. Definir una clase de excepción específica hace que el código sea más expresivo y que los manejadores puedan diferenciar con precisión entre distintos tipos de fallos. Así, se consigue un diseño más claro que cuando se utilizan códigos genéricos de error, como ocurre habitualmente en C.

---

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

Un objeto excepción en Java transporta información esencial que resulta muy útil cuando se alcanza un manejador. En primer lugar, contiene el tipo de la excepción, que permite distinguir entre diferentes clases de errores y reaccionar de manera específica. Este tipo forma parte de la jerarquía de clases de excepciones y facilita capturar solo aquellas que interesan.

Además, el objeto excepción incluye un mensaje descriptivo que explica la causa del problema. Este mensaje puede mostrarse al usuario, registrarse en un log o utilizarse para depuración. En C, este tipo de información suele gestionarse mediante cadenas sueltas o códigos de error que requieren documentación externa para interpretarse correctamente.

Otro elemento clave es la traza de la pila de llamadas asociada a la excepción. Esta información indica qué métodos estaban en ejecución cuando ocurrió el error y resulta fundamental para depurar. Gracias a la encapsulación, todos estos datos viajan juntos dentro del objeto excepción, sin necesidad de mecanismos adicionales, lo que supone una ventaja clara frente a los enfoques manuales habituales en C.

---

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

En Java es posible definir varios bloques `catch` asociados a un mismo bloque `try`. Cada bloque `catch` puede capturar un tipo distinto de excepción, lo que permite tratar de forma diferenciada varios tipos de errores que puedan producirse dentro del mismo bloque de código. Este diseño hace que el manejo de errores sea más preciso y expresivo.

Cuando se lanza una excepción dentro del `try`, Java busca el primer bloque `catch` cuyo tipo sea compatible con la excepción producida. Solo se ejecuta un bloque `catch`, el primero que coincide en el orden en que están escritos. Los bloques posteriores no se ejecutan, ya que la excepción queda considerada como manejada en ese punto.

Este comportamiento obliga a ordenar los `catch` desde los tipos más específicos hasta los más generales. De lo contrario, un `catch` genérico podría capturar una excepción antes de que llegue a uno más específico. Este orden refuerza la idea de jerarquía de excepciones y de manejo selectivo de los errores según su naturaleza.

---

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

Para garantizar la ejecución de código de limpieza, Java proporciona el bloque `finally`, que se ejecuta siempre tras el `try`, haya ocurrido o no una excepción. Este mecanismo permite cerrar ficheros, liberar recursos o realizar tareas de finalización sin depender de que el flujo normal del programa se complete correctamente. De este modo, se evita que una excepción deje recursos abiertos.

Cuando existe un bloque `catch`, el `finally` se ejecuta después de que el `catch` haya gestionado la excepción. Cuando no hay `catch`, el `finally` se ejecuta igualmente antes de que la excepción se propague al llamador. Esto garantiza que las acciones críticas de limpieza se realizan incluso cuando el error no se controla en ese nivel.

```java
try {
    double r = Calculadora.raiz(-9.0);
    System.out.println(r);
} catch (IllegalArgumentException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("Liberando recursos...");
}

try {
    double r = Calculadora.raiz(-9.0);
    System.out.println(r);
} finally {
    System.out.println("Este código se ejecuta siempre");
}
```

Este patrón es especialmente importante en operaciones con recursos externos, como ficheros o conexiones, donde la omisión del cierre puede provocar fugas de recursos. El uso de `finally` permite separar claramente la lógica de limpieza del tratamiento del error en sí.

---

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

En Java, el bloque `finally` puede aparecer sin un bloque `catch`. En este caso, el código del `finally` se ejecuta tanto si el bloque `try` finaliza con normalidad como si se produce una excepción que se propaga al llamador. Este diseño permite garantizar la ejecución de tareas de limpieza incluso cuando no se desea manejar la excepción en ese nivel.

El bloque `finally` se ejecuta siempre que el flujo de ejecución salga del `try`, independientemente de si ha ocurrido una excepción o no. Esto incluye el caso en que dentro del `try` se ejecute un `return`. Antes de que el método retorne realmente al llamador, el código del `finally` se ejecuta, asegurando que los recursos se liberen correctamente.

Este comportamiento hace que `finally` sea una herramienta fiable para asegurar invariantes del programa, como el cierre de recursos. No obstante, se debe evitar introducir en `finally` lógica compleja que pueda lanzar nuevas excepciones, ya que esto puede ocultar la excepción original y dificultar el diagnóstico de errores.

---

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

En Java, las excepciones controladas son aquellas que el compilador obliga a manejar o declarar mediante `try-catch` o `throws`. Representan condiciones que el programador puede prever razonablemente y ante las que se espera una reacción por parte del código llamador. Las excepciones no controladas, en cambio, son subclases de `RuntimeException` y no requieren ser declaradas ni capturadas obligatoriamente.

`RuntimeException` actúa como base para errores de programación o violaciones de precondiciones, como pasar argumentos inválidos o acceder fuera de los límites de un array. Estas excepciones suelen indicar fallos lógicos que deben corregirse durante el desarrollo, más que situaciones recuperables en tiempo de ejecución. Por este motivo, no se obliga a capturarlas.

Ejemplos típicos de excepciones controladas son `IOException` o `FileNotFoundException`, que representan fallos externos previsibles. Ejemplos de no controladas son `IllegalArgumentException`, `NullPointerException` o `IndexOutOfBoundsException`. Estas últimas también pueden ser lanzadas por código propio para indicar un uso incorrecto de una API.

* Situaciones donde se prefiere una excepción controlada:

  * Fallos al abrir o leer un fichero.
  * Errores de comunicación de red.
  * Acceso a recursos externos no disponibles.
  * Operaciones que dependen del entorno.
* Situaciones donde se prefiere una excepción no controlada:

  * Argumentos inválidos a un método.
  * Estados internos incoherentes.
  * Errores de lógica en el código.
  * Violación de precondiciones documentadas.

---

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

`throws` se utiliza en la firma de un método para declarar que dicho método puede producir una o varias excepciones y que no se encarga de manejarlas. Con esta declaración, el método informa a sus llamadores de que deben estar preparados para tratar esa excepción, ya sea capturándola o volviéndola a declarar en su propia firma.

Este mecanismo es una alternativa a capturar una excepción controlada dentro del propio método cuando dicho método no tiene suficiente contexto para decidir cómo resolver el problema. En lugar de forzar una política de manejo, se delega la responsabilidad al nivel superior, donde puede existir más información sobre qué hacer en caso de error.

El uso de `throws` favorece un diseño más flexible y modular, ya que permite que las capas inferiores de la aplicación se centren en su lógica principal sin imponer decisiones sobre el tratamiento de errores. De este modo, se mantiene la separación de responsabilidades entre la detección del problema y la decisión de cómo reaccionar ante él.

---

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

Un método que abre un fichero puede declarar con `throws` que no se responsabiliza de manejar el caso en que el fichero no exista. De este modo, el método se limita a realizar su tarea principal y deja que el código llamador decida qué hacer si ocurre el error. Esta decisión es habitual cuando el método pertenece a una capa baja de la aplicación.

Aunque la excepción se propague, sigue siendo necesario garantizar la liberación de recursos. Para ello se emplea un bloque `finally`, que se ejecuta siempre antes de que el método termine, tanto si la operación tiene éxito como si se produce una excepción. Esto asegura que el fichero se cierre correctamente si llegó a abrirse.

```java
import java.io.*;

public static void leerArchivo(String ruta) throws IOException {
    BufferedReader br = null;
    try {
        br = new BufferedReader(new FileReader(ruta));
        System.out.println(br.readLine());
    } finally {
        if (br != null) {
            br.close();
        }
    }
}
```

Este diseño permite que la política de manejo del error se sitúe en un nivel superior del programa, por ejemplo en la interfaz con el usuario, mientras que el método de bajo nivel se mantiene simple y centrado en su responsabilidad principal.

---

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Es posible declarar en `throws` excepciones no controladas, como las que heredan de `RuntimeException`, aunque no es obligatorio. Esta declaración no tiene efecto sobre el compilador, ya que este no exige que se capturen ni se declaren estas excepciones. Su inclusión en la firma es, por tanto, meramente informativa.

El método llamador no está obligado a poner un `try-catch` para una excepción no controlada, incluso si aparece en la cláusula `throws`. Capturarla puede tener sentido cuando se desea transformar un fallo de programación en un mensaje más claro para el usuario o en un mecanismo de recuperación controlada en una capa superior.

En general, declarar `RuntimeException` en `throws` se utiliza como documentación para indicar que un método puede fallar si se violan ciertas precondiciones. No se considera una práctica necesaria en la mayoría de los casos, ya que el uso correcto del método debería evitar que estas excepciones se produzcan.

---

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Las excepciones controladas se recomiendan para situaciones en las que el error es una condición esperable del entorno de ejecución y el programa puede reaccionar de alguna forma razonable. Ejemplos típicos son fallos de entrada/salida, problemas de red o ausencia de recursos externos. Estas situaciones no suelen ser errores de programación, sino contingencias que deben gestionarse.

Las excepciones no controladas se recomiendan para indicar errores de uso de una API o fallos lógicos en el programa, como pasar argumentos inválidos o violar invariantes internas. En estos casos, se considera que el problema debe corregirse en el código y no gestionarse como una condición normal de ejecución. Por ello, no se obliga a capturarlas.

No todos los lenguajes distinguen entre excepciones controladas y no controladas. En muchos lenguajes solo existe un tipo de excepción que no requiere declaración explícita, lo que se parece más al modelo de las excepciones no controladas de Java. Este enfoque es más habitual en lenguajes modernos, ya que simplifica las firmas de los métodos y reduce el acoplamiento entre capas.

---

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Tiene sentido lanzar excepciones dentro de un `catch` cuando se desea transformar una excepción de bajo nivel en otra de mayor nivel de abstracción. Esto permite ocultar detalles de implementación y ofrecer al llamador una visión más acorde con el dominio del problema. El bloque `catch` actúa entonces como un punto de traducción entre capas.

También es posible relanzar la misma excepción capturada, ya sea tal cual o después de realizar alguna acción adicional, como registrar información en un log. Este patrón resulta útil cuando se quiere añadir contexto o realizar tareas de limpieza sin ocultar la excepción original. De este modo, el manejo final del error se delega a niveles superiores.

```java
try {
    leerArchivo("datos.txt");
} catch (IOException e) {
    throw new RuntimeException("Error al acceder a los datos", e);
}

try {
    leerArchivo("datos.txt");
} catch (IOException e) {
    System.out.println("Registrando error...");
    throw e;  // relanzar la misma excepción
}
```

En ambos casos, el `catch` no resuelve el problema definitivamente, sino que lo adapta o lo propaga, manteniendo la separación entre la detección del fallo y la política final de manejo.

---

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Que una excepción sea la causa de otra significa que una excepción de nivel superior encapsula dentro de sí la excepción original que provocó el fallo. Este mecanismo permite conservar la información del error de bajo nivel al tiempo que se presenta al llamador una excepción más adecuada al dominio de la aplicación. Así, no se pierde el detalle técnico del problema, pero se ofrece una interfaz más clara.

En Java, esto se logra pasando la excepción original como parámetro al constructor de la nueva excepción. De este modo, la relación de causa queda registrada y puede consultarse posteriormente. Este patrón es habitual cuando se captura una excepción técnica, como una de entrada/salida, y se traduce a una excepción propia de la lógica de negocio.

```java
class ErrorDeNegocio extends Exception {
    public ErrorDeNegocio(String msg, Throwable causa) {
        super(msg, causa);
    }
}

try {
    leerArchivo("datos.txt");
} catch (IOException e) {
    throw new ErrorDeNegocio("No se pudieron cargar los datos", e);
}
```

Cuando una excepción con causa se muestra por pantalla o se imprime su traza, la información de la causa aparece encadenada, mostrando tanto la excepción de alto nivel como la original. Esto facilita enormemente la depuración, ya que se puede ver el contexto lógico del fallo y, al mismo tiempo, el detalle técnico de su origen.


