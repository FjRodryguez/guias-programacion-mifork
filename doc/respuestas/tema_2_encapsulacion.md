<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

La encapsulación en Programación Orientada a Objetos persigue agrupar en una misma unidad (la clase) los datos y las operaciones que actúan sobre ellos. La ocultación de información busca restringir el acceso directo a los detalles internos de la clase, exponiendo únicamente aquello que es necesario para usarla correctamente. De este modo, el uso de un objeto se basa en su comportamiento observable y no en su implementación interna.

Este enfoque permite que el código cliente interactúe con los objetos sin conocer cómo están implementados internamente sus atributos o algoritmos. En comparación con C/C++ sin POO, donde las estructuras de datos suelen ser accesibles directamente, la ocultación reduce la dependencia entre módulos y promueve un diseño más modular y mantenible.

Entre las ventajas principales se encuentran: la reducción del acoplamiento entre clases, la posibilidad de cambiar la implementación interna sin afectar al código que la utiliza, la mejora en el control de los estados válidos de los objetos (invariantes), y una mayor facilidad para detectar errores al centralizar el acceso a los datos a través de métodos bien definidos.

---

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La interfaz pública de una clase está formada por el conjunto de métodos y miembros accesibles desde el exterior, normalmente declarados como `public`. Esta interfaz define cómo puede ser utilizado un objeto por otras partes del programa, especificando qué operaciones están disponibles y qué garantías ofrece la clase sobre su comportamiento.

Desde el punto de vista conceptual, la interfaz pública actúa como un “contrato” entre la clase y su entorno. El código cliente se apoya en esa interfaz para interactuar con los objetos, sin necesidad de conocer cómo están implementados internamente los datos o los algoritmos que dan soporte a dichas operaciones.

La relación con la ocultación de información es directa: todo aquello que no forma parte de la interfaz pública se mantiene oculto, evitando accesos directos a los atributos internos. Esto permite modificar la implementación interna sin romper el código que utiliza la clase, siempre que la interfaz pública se mantenga estable.

---

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

La interfaz pública debe diseñarse con cuidado porque constituye el punto de contacto entre la clase y el resto del programa. Cualquier decisión tomada en la interfaz pública condiciona cómo se utilizará la clase y qué dependencias se crearán entre módulos. Una interfaz mal diseñada puede obligar a exponer detalles innecesarios o dificultar el uso correcto de la clase.

Una vez que una interfaz pública es utilizada por otras partes del sistema, modificarla suele ser costoso. Cambiar nombres de métodos, tipos de parámetros o eliminar operaciones puede romper código existente y generar errores en tiempo de compilación o ejecución. Por tanto, la estabilidad de la interfaz pública es clave para la mantenibilidad del software.

Por esta razón, se recomienda diseñar interfaces públicas mínimas, claras y coherentes, exponiendo solo lo estrictamente necesario. Esto reduce el impacto de posibles cambios internos y facilita la evolución de la implementación sin afectar a los usuarios de la clase.

---

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las invariantes de clase son condiciones que deben cumplirse siempre para que un objeto de esa clase se considere en un estado válido. Estas condiciones definen propiedades que deben mantenerse a lo largo de toda la vida del objeto, antes y después de la ejecución de cualquier método público. Por ejemplo, que una coordenada no sea negativa o que un contador no descienda por debajo de cero.

La ocultación de información ayuda a preservar estas invariantes al impedir que el código externo modifique directamente los atributos internos. En lugar de permitir accesos directos, se obliga a que cualquier cambio pase por métodos que pueden comprobar y garantizar que las invariantes se siguen cumpliendo.

Este control centralizado reduce la probabilidad de que un objeto quede en un estado inconsistente. En lenguajes sin encapsulación estricta, como C con estructuras accesibles públicamente, resulta más difícil asegurar que se respeten estas condiciones, ya que cualquier parte del programa puede modificar los datos sin restricciones.

---

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

En el ejemplo se define una clase `Punto` cuyos atributos internos representan las coordenadas. Estos atributos se declaran como privados para evitar su modificación directa desde el exterior. El acceso a los datos se realiza a través de métodos públicos, que forman la interfaz pública de la clase.

La interfaz pública de `Punto` está compuesta por el constructor y el método `calcularDistanciaAOrigen`. Estos elementos son los únicos accesibles desde otras clases y constituyen la forma oficial de interactuar con los objetos de tipo `Punto`. Los atributos `x` e `y` quedan ocultos, permitiendo cambiar su representación interna sin afectar al código cliente.

En Java, `public` indica que un miembro es accesible desde cualquier clase, mientras que `private` limita el acceso exclusivamente al interior de la propia clase. Esta distinción implementa directamente la ocultación de información, separando la interfaz pública de la implementación interna.

```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

---

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores de visibilidad `public` y `private` se pueden aplicar a clases (en el caso de `public`), atributos, métodos y constructores. Estos modificadores controlan desde qué partes del programa es posible acceder a cada elemento. La elección del modificador afecta directamente al nivel de encapsulación que ofrece la clase.

Los atributos y métodos suelen declararse `private` cuando forman parte de la implementación interna y no deben ser utilizados directamente por el exterior. Los métodos que forman parte de la interfaz pública se declaran `public`, permitiendo su uso desde cualquier otro paquete o clase.

En el caso de las clases, solo es posible declarar una clase de nivel superior como `public` o sin modificador (visibilidad por defecto). El modificador `private` se utiliza para clases internas, limitando su uso al interior de la clase que las contiene.

---

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

En POO existen varios niveles de visibilidad además de pública y privada. Estos niveles permiten un control más fino sobre qué partes del código pueden acceder a determinados miembros. La idea general es ofrecer mecanismos intermedios entre el acceso totalmente abierto y el acceso totalmente restringido.

En Java existen cuatro niveles de visibilidad: `public`, `protected`, `private` y la visibilidad por defecto (también llamada de paquete). `protected` permite el acceso desde clases del mismo paquete y desde subclases, mientras que la visibilidad por defecto limita el acceso a clases del mismo paquete. Esto resulta útil para organizar módulos relacionados sin exponer detalles al resto del programa.

En otros lenguajes orientados a objetos también existen mecanismos similares, aunque con diferencias en la semántica concreta. Por ejemplo, en C++ existen los modificadores `public`, `protected` y `private`, aplicables tanto a miembros como a herencia, lo que proporciona un control detallado del acceso en jerarquías de clases.

---

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

En Java, los miembros de instancia privados están ocultos para otras clases, pero no para otras instancias de la misma clase. Esto significa que un método de una clase puede acceder a los atributos privados de otro objeto de la misma clase, ya que la restricción de `private` se aplica a nivel de clase, no de instancia.

Este comportamiento permite implementar métodos que comparen o combinen el estado de varios objetos del mismo tipo sin exponer los atributos al exterior. Desde el punto de vista del diseño, sigue respetándose la encapsulación, ya que el acceso continúa estando controlado por la propia clase.

En el ejemplo, el método `calcularDistanciaAPunto` puede acceder a `otro.x` y `otro.y` aunque sean privados, porque se está dentro de la misma clase `Punto`. Sin embargo, ninguna clase externa podría acceder directamente a esos atributos.

```java
public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

---

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Los métodos “getter” y “setter” son métodos públicos que permiten, respectivamente, leer y modificar el valor de un atributo que suele estar declarado como privado. Su objetivo principal es proporcionar un acceso controlado a los datos internos de un objeto, en lugar de permitir el acceso directo a los atributos.

Un “getter” devuelve el valor de un atributo sin permitir su modificación directa, mientras que un “setter” permite cambiar el valor del atributo. En el interior del “setter” se pueden incluir validaciones para asegurar que el nuevo valor respeta las invariantes de la clase. Esto ofrece una ventaja clara frente a los atributos públicos.

Estos métodos forman parte de la interfaz pública de la clase y constituyen un mecanismo habitual para aplicar encapsulación en lenguajes como Java. No obstante, su uso indiscriminado puede llevar a exponer demasiados detalles de la implementación interna si no se diseña con cuidado la interfaz pública.

---

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

Cuando se afirma que la ocultación de información mejora la “seguridad” del programa, no se está hablando de seguridad en el sentido de protección frente a ataques externos o “hackeo”. Se trata de seguridad desde el punto de vista del diseño del software y la corrección interna del programa.

La ocultación de información reduce la probabilidad de que el código cliente utilice incorrectamente los objetos, accediendo a estados internos de forma indebida o dejando los objetos en estados inconsistentes. Al obligar a interactuar a través de métodos bien definidos, se limita el margen de uso incorrecto.

Este tipo de “seguridad” se relaciona con la robustez y fiabilidad del sistema. Se busca evitar errores de programación y facilitar el mantenimiento, no proteger el programa frente a ataques de seguridad informática o accesos maliciosos desde el exterior.

---

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Un miembro de instancia pertenece a cada objeto creado a partir de una clase, de modo que cada instancia tiene su propia copia de ese atributo o método. En cambio, un miembro de clase pertenece a la clase en sí y es compartido por todas las instancias. En Java, los miembros de clase se declaran con la palabra clave `static`.

Desde el punto de vista conceptual, los miembros de instancia representan el estado particular de cada objeto, mientras que los miembros de clase representan información o comportamiento común a todos los objetos de ese tipo. Ambos tipos de miembros pueden formar parte de la interfaz pública o permanecer ocultos.

Los miembros de clase también se pueden ocultar mediante modificadores de visibilidad como `private`. Esto permite encapsular información global de la clase y controlar su acceso de la misma forma que se hace con los miembros de instancia, manteniendo coherencia en el diseño orientado a objetos.

---

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Tiene sentido que los constructores sean privados en determinados patrones de diseño o situaciones específicas. Al declarar un constructor como privado, se impide que el código externo cree instancias directamente de la clase. Esto permite controlar estrictamente cómo y cuándo se crean los objetos.

Un uso habitual de constructores privados aparece en el patrón Singleton, donde se garantiza que solo exista una única instancia de una clase. También se emplean en clases que ofrecen métodos factoría estáticos para crear instancias de forma controlada, aplicando validaciones o lógica adicional durante la creación.

En estos casos, la ocultación del constructor refuerza la encapsulación, ya que se evita que el usuario de la clase cree objetos en estados no válidos o de formas no previstas por el diseño de la clase.

---

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

En Java, los miembros de clase se indican mediante la palabra clave `static`. Estos miembros pertenecen a la clase y no a cada instancia concreta, por lo que su valor es compartido por todos los objetos creados a partir de esa clase. Su uso es apropiado para almacenar información global relacionada con la clase.

En el caso de la clase `Punto`, se pueden añadir atributos de clase para registrar los valores máximos de `x` e `y` observados hasta el momento. Estos atributos se actualizan en el constructor cada vez que se crea un nuevo objeto. De este modo, se centraliza el seguimiento de esta información.

Estos miembros de clase también pueden ocultarse utilizando `private`, exponiendo únicamente métodos públicos para consultar sus valores. Así se mantiene la encapsulación incluso para datos compartidos por todas las instancias.

```java
private static double maxX = Double.NEGATIVE_INFINITY;
private static double maxY = Double.NEGATIVE_INFINITY;

public Punto(double x, double y) {
    this.x = x;
    this.y = y;
    if (x > maxX) maxX = x;
    if (y > maxY) maxY = y;
}

public static double getMaxX() {
    return maxX;
}

public static double getMaxY() {
    return maxY;
}
```

---

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`?

### Respuesta

Un método factoría es un método que encapsula la lógica de creación de objetos, permitiendo controlar cómo se construyen las instancias. En este caso, el método recibe dos valores reales y los redondea antes de crear el objeto `Punto`. Esto evita que el código cliente tenga que conocer los detalles de esta transformación.

El método se declara como `static` porque no depende de una instancia concreta de la clase para crear un nuevo objeto. Se invoca directamente sobre la clase, lo que refuerza la idea de que la creación de objetos está centralizada en la propia clase.

Este enfoque permite modificar la lógica de creación en el futuro sin afectar al código que utiliza el método factoría. Así se mantiene la encapsulación del proceso de construcción de objetos.

```java
public static Punto crearRedondeado(double x, double y) {
    double xr = Math.round(x);
    double yr = Math.round(y);
    return new Punto(xr, yr);
}
```

---

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

El cambio de la representación interna de los datos sin modificar la interfaz pública es un ejemplo directo del beneficio de la encapsulación. Al mantener los atributos privados y acceder a ellos únicamente a través de métodos públicos, es posible alterar la implementación interna sin afectar al código cliente.

En este caso, las coordenadas `x` e `y` pasan a almacenarse en un array interno de dos posiciones. Desde el exterior, la clase sigue ofreciendo los mismos métodos y el mismo comportamiento observable. El usuario de la clase no necesita conocer este cambio interno.

Este tipo de modificación demuestra cómo la ocultación de información permite evolucionar el diseño interno de una clase sin romper el código existente. La interfaz pública permanece estable, mientras que la implementación se adapta a nuevas necesidades.

```java
private double[] coords = new double[2];

public Punto(double x, double y) {
    coords[0] = x;
    coords[1] = y;
}

public double calcularDistanciaAOrigen() {
    double x = coords[0];
    double y = coords[1];
    return Math.sqrt(x * x + y * y);
}
```

---

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Aunque un atributo tenga un “getter” y un “setter” públicos, no es recomendable declararlo público. Declarar el atributo como público expone directamente el estado interno del objeto, impidiendo controlar cómo y cuándo se modifica. En cambio, mediante métodos se puede validar la entrada y mantener las invariantes de la clase.

La convención más habitual en Java y en POO en general es declarar los atributos como `private` y exponer únicamente los métodos necesarios para interactuar con ellos. Esto refuerza la encapsulación y permite cambiar la implementación interna sin afectar al código cliente.

Esta práctica está directamente relacionada con las invariantes de clase, ya que los métodos “setter” pueden comprobar que los nuevos valores no violan las condiciones que deben mantenerse. Con atributos públicos, no existe un punto centralizado donde imponer estas restricciones.

---

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase inmutable es aquella cuyos objetos no pueden cambiar de estado una vez creados. Es decir, después de construir una instancia, sus atributos no se modifican. Cualquier operación que conceptualmente “modifique” el objeto produce en realidad una nueva instancia con el nuevo estado.

Un método modificador es un método que cambia el estado interno de un objeto. Un “setter” es un tipo particular de método modificador, ya que asigna un nuevo valor a un atributo. Sin embargo, no todos los métodos modificadores son “setters”, ya que también pueden existir métodos que alteren el estado de forma más compleja.

Las clases inmutables tienen ventajas como una mayor simplicidad en el razonamiento del programa, ausencia de efectos secundarios inesperados y una mejor seguridad en entornos concurrentes. Además, facilitan el mantenimiento de invariantes, ya que el estado solo se establece en el constructor.

---

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No es recomendable incluir métodos “setter” de forma automática como convención general. Exponer “setters” para todos los atributos puede llevar a un diseño pobre de la interfaz pública, ya que se permite modificar libremente el estado interno del objeto sin restricciones semánticas claras.

En muchos casos, resulta preferible proporcionar métodos que representen operaciones con significado en el dominio del problema, en lugar de simples asignaciones de valores. Esto permite mantener las invariantes de la clase y expresar mejor la intención del diseño.

La inclusión de “setters” debe responder a una necesidad real del diseño. En clases que representan conceptos que no deberían cambiar una vez creados, o en clases inmutables, no tiene sentido proporcionar métodos modificadores de este tipo.

---

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase String en Java es inmutable. Esto significa que una vez creada una cadena, su contenido no puede cambiar. Cualquier operación que parezca modificar una cadena en realidad crea un nuevo objeto con el nuevo contenido, dejando intacto el objeto original.

Al concatenar dos cadenas con el operador `+`, se crea un nuevo objeto `String` que contiene el resultado de la concatenación. Este proceso implica la creación de objetos intermedios, lo que puede resultar ineficiente si se realizan muchas concatenaciones en un bucle o en una operación iterativa larga.

Cuando se necesita construir una cadena muy larga mediante múltiples concatenaciones, se recomienda utilizar la clase StringBuilder, que es mutable y permite modificar su contenido de forma eficiente. Una vez finalizada la construcción, se puede obtener el resultado final como un objeto `String`.

---

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java?

### Respuesta

En POO, la comparación entre objetos puede hacerse por identidad (si dos referencias apuntan al mismo objeto) o por contenido (si dos objetos distintos representan el mismo valor lógico). La elección depende del significado que se quiera dar a la comparación en el dominio del problema. En Java, el operador `==` compara la identidad de las referencias.

El método `equals` en Java se utiliza para comparar el contenido lógico de los objetos. Por defecto, la implementación heredada de `Object` compara la identidad, es decir, se comporta igual que `==`. Para comparar por contenido, es necesario sobrescribir `equals` en la clase correspondiente, definiendo qué significa que dos objetos sean “iguales”.

En el caso de las cadenas, no se debe usar `==` para comparar su contenido, ya que solo comprueba si ambas referencias apuntan al mismo objeto. Se debe utilizar el método `equals` de la clase `String`, que compara el contenido de las cadenas carácter a carácter.

---

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers?

### Respuesta

Las clases “wrapper” son clases que envuelven valores de tipos primitivos para tratarlos como objetos. En Java, ejemplos de wrappers son `Integer`, `Double` o `Boolean`, que encapsulan valores primitivos `int`, `double` o `boolean`. Esto permite utilizar estos valores en contextos donde se requieren objetos, como colecciones genéricas.

La conversión entre tipos primitivos y sus wrappers puede hacerse explícitamente mediante constructores o métodos de fábrica, pero en Java también existe un proceso automático llamado “autoboxing” y “unboxing”. Este mecanismo convierte de forma transparente entre primitivos y objetos wrapper cuando es necesario.

Las ventajas de los wrappers incluyen la posibilidad de utilizar tipos primitivos en estructuras orientadas a objetos, beneficiarse de métodos asociados a esos valores y permitir la utilización de valores nulos para representar ausencia de valor. No todos los lenguajes orientados a objetos distinguen entre tipos primitivos y objetos; algunos lenguajes tratan todos los valores como objetos y no necesitan wrappers explícitos.

---

## 22. En POO ¿qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un tipo de dato enumerado representa un conjunto finito y cerrado de valores posibles. En POO, los enumerados permiten modelar dominios donde solo existen ciertas opciones válidas, como los días de la semana o los meses del año. Esto evita el uso de constantes dispersas o valores “mágicos” en el código.

En Java, un tipo enumerado es en realidad una clase especial. Cada valor del enumerado es una instancia única de esa clase, y se pueden definir atributos, métodos y constructores privados dentro del propio enumerado. Esto permite asociar comportamiento y datos a cada valor posible.

En términos de encapsulación, los enumerados en Java ofrecen un alto nivel de seguridad y claridad. Solo pueden existir las instancias definidas en el propio enumerado, lo que garantiza que no se crearán valores inválidos. Además, los detalles internos del enumerado pueden ocultarse mediante atributos privados, exponiendo únicamente la interfaz pública necesaria.

---

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta

El tipo enumerado `Mes` permite modelar de forma segura los meses del año como un conjunto cerrado de valores. Cada instancia del enumerado representa un mes concreto y almacena internamente información asociada, como el número de días y su posición en el año. Estos datos se definen mediante atributos privados y se inicializan en el constructor del enumerado.

La interfaz pública del enumerado expone métodos para consultar el número de días y el ordinal del mes. De este modo, el código cliente no necesita conocer cómo se almacenan internamente estos valores, manteniéndose la encapsulación. Cualquier cambio en la implementación interna no afecta al uso del enumerado.

Este diseño evita el uso de constantes enteras sueltas para representar meses y reduce la posibilidad de errores, ya que solo existen las doce instancias válidas definidas en el propio tipo enumerado.

```java
public enum Mes {
    ENERO(1, 31), FEBRERO(2, 28), MARZO(3, 31), ABRIL(4, 30),
    MAYO(5, 31), JUNIO(6, 30), JULIO(7, 31), AGOSTO(8, 31),
    SEPTIEMBRE(9, 30), OCTUBRE(10, 31), NOVIEMBRE(11, 30), DICIEMBRE(12, 31);

    private final int ordinalEnAnio;
    private final int dias;

    private Mes(int ordinalEnAnio, int dias) {
        this.ordinalEnAnio = ordinalEnAnio;
        this.dias = dias;
    }

    public int getOrdinalEnAnio() {
        return ordinalEnAnio;
    }

    public int getDias() {
        return dias;
    }
}
```

---

## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

Estos métodos permiten encapsular la lógica que relaciona cada mes con las estaciones del año, teniendo en cuenta el hemisferio. De este modo, el código cliente no necesita conocer las reglas de correspondencia entre meses y estaciones, ya que la responsabilidad se delega en el propio enumerado `Mes`.

El uso de un parámetro booleano para indicar el hemisferio permite reutilizar la misma lógica para ambos casos sin duplicar código. La interfaz pública ofrece métodos expresivos que mejoran la legibilidad del programa y reducen la probabilidad de errores en el uso de condiciones complejas en el código cliente.

La implementación interna puede cambiar si se desean modificar los criterios de asignación de estaciones, sin afectar al resto del programa. Esto refuerza la encapsulación y mantiene el diseño modular.

```java
public boolean esDePrimavera(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return this == MARZO || this == ABRIL || this == MAYO;
    } else {
        return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
    }
}

public boolean esDeVerano(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return this == JUNIO || this == JULIO || this == AGOSTO;
    } else {
        return this == DICIEMBRE || this == ENERO || this == FEBRERO;
    }
}

public boolean esDeOtoño(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
    } else {
        return this == MARZO || this == ABRIL || this == MAYO;
    }
}

public boolean esDeInvierno(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return this == DICIEMBRE || this == ENERO || this == FEBRERO;
    } else {
        return this == JUNIO || this == JULIO || this == AGOSTO;
    }
}
```

