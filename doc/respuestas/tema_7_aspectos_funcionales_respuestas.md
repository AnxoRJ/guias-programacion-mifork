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

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Un **puntero a una función** es una variable que almacena la dirección de una función concreta, permitiendo invocarla de forma indirecta. Desde el punto de vista del lenguaje C, las funciones también ocupan una dirección de memoria, y dicha dirección puede asignarse a una variable cuyo tipo describe exactamente la firma de la función (tipo de retorno y tipos de parámetros). Este mecanismo resulta especialmente útil para desacoplar la llamada a una función de su implementación concreta, algo habitual en bibliotecas, callbacks o estrategias configurables en tiempo de ejecución.

A diferencia de lenguajes orientados a objetos como Java, donde este tipo de comportamiento suele lograrse mediante interfaces o clases abstractas, en C se alcanza mediante punteros a funciones. Conceptualmente, un puntero a función puede entenderse como una forma primitiva de polimorfismo: el código que realiza la llamada no necesita conocer qué función exacta se ejecutará, solo que cumple una determinada “firma”. Esto encaja bien con conocimientos previos de C estructurado, ya que no introduce nuevos conceptos más allá de los ya conocidos sobre punteros.

En el siguiente ejemplo se define una función que recibe una cadena de caracteres (`char *`) y transforma sus letras a mayúsculas, devolviendo la misma cadena modificada. A continuación, se declara un puntero a dicha función llamado `aMayusculas` como variable local, se le asigna la dirección de la función y se invoca utilizando dicho puntero.

```c
#include <stdio.h>
#include <ctype.h>

char *convertirAMayusculas(char *cadena) {
    int i = 0;
    while (cadena[i] != '\0') {
        cadena[i] = toupper((unsigned char)cadena[i]);
        i++;
    }
    return cadena;
}

int main(void) {
    char texto[] = "Hola mundo";

    char *(*aMayusculas)(char *);
    aMayusculas = convertirAMayusculas;

    char *resultado = aMayusculas(texto);
    printf("%s\n", resultado);

    return 0;
}
```

En este código, el tipo del puntero `aMayusculas` indica claramente que apunta a una función que recibe un `char *` y devuelve un `char *`. La llamada `aMayusculas(texto)` es equivalente a llamar directamente a `convertirAMayusculas(texto)`, pero con la ventaja de que la función concreta podría cambiarse fácilmente asignando otra dirección compatible al puntero.



## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una **función lambda** es una función anónima, es decir, una función sin nombre explícito, que puede definirse de forma concisa y asignarse a una variable o pasarse como argumento. Su objetivo principal es tratar el comportamiento como un valor, permitiendo escribir código más expresivo y flexible. A diferencia de las funciones tradicionales, las lambdas suelen ser breves y se centran en expresar una operación concreta sin necesidad de una definición completa y separada.

Desde una perspectiva cercana a C y Java clásico, una función lambda puede compararse con el uso de punteros a funciones, pero con una sintaxis más simple y segura. En lenguajes modernos, las lambdas se usan habitualmente para callbacks, transformaciones de datos o estrategias intercambiables. En Javascript forman parte esencial del lenguaje, mientras que en Java (a partir de Java 8) se integran mediante interfaces funcionales, lo que encaja bien con los conceptos previos de interfaces y polimorfismo.

En Javascript, las funciones son ciudadanos de primera clase, por lo que una función lambda puede asignarse directamente a una variable local. El siguiente ejemplo define una función lambda que recibe una cadena y devuelve dicha cadena en mayúsculas, usando una variable llamada `aMayusculas`:

```javascript
let aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

let resultado = aMayusculas("Hola mundo");
console.log(resultado);
```

En Java, las funciones lambda no existen de forma aislada, sino que se asocian a un tipo funcional. Para simplificar, se utiliza la interfaz estándar `Function<String, String>`, que representa una función que recibe un `String` y devuelve otro `String`. La variable local `aMayusculas` apunta a la lambda y se invoca mediante el método `apply`:

```java
import java.util.function.Function;

public class Principal {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        String resultado = aMayusculas.apply("Hola mundo");
        System.out.println(resultado);
    }
}
```

En ambos casos, la función lambda permite encapsular una operación concreta y tratarla como un valor, logrando un efecto similar al de los punteros a funciones en C, pero con una sintaxis más clara y un mejor soporte por parte del lenguaje.



## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El **paradigma funcional** es un estilo de programación en el que el programa se construye fundamentalmente mediante funciones matemáticas, evitando en la medida de lo posible el estado mutable y los efectos secundarios. En este paradigma, una función idealmente devuelve siempre el mismo resultado para los mismos argumentos y no modifica variables externas. Este enfoque facilita el razonamiento sobre el código, mejora su testabilidad y favorece la ejecución paralela, ya que se reducen las dependencias entre distintas partes del programa.

A lenguajes como **Java 8** se les denomina **multi‑paradigma** porque, aun habiendo nacido como lenguajes claramente orientados a objetos, incorporan características propias de otros paradigmas, en este caso del funcional. Java sigue basándose en clases, objetos, herencia y polimorfismo, pero desde Java 8 permite expresar comportamientos mediante funciones lambda, usar funciones como valores y trabajar con APIs diseñadas en clave funcional, como *Streams*. Esto permite elegir el paradigma más adecuado según el problema, combinando estilos sin abandonar el lenguaje.

Decir que las funciones son **“ciudadanos de primera clase”** significa que las funciones se tratan igual que cualquier otro valor del lenguaje. Esto implica que pueden asignarse a variables, almacenarse en estructuras de datos, pasarse como argumentos a otras funciones y devolverse como resultado. En lenguajes como C esto se logra mediante punteros a funciones, mientras que en Java moderno se consigue mediante lambdas y tipos funcionales, ocultando gran parte de la complejidad.

En conjunto, el paradigma funcional no reemplaza necesariamente al enfoque orientado a objetos, sino que lo complementa. En lenguajes multi‑paradigma como Java, esta combinación permite escribir código más declarativo y expresivo, especialmente en tareas de transformación de datos o definición de comportamientos, manteniendo a la vez la estructura y el modelo conceptual propios de la programación orientada a objetos.



## 4. Explica la sintaxis básica de una función lambda en Java.

La **sintaxis básica de una función lambda en Java** se introdujo a partir de Java 8 y permite expresar una implementación de una interfaz funcional de forma compacta. Una lambda está formada por tres partes: la lista de parámetros, el operador flecha `->` y el cuerpo de la función. Su forma general es:  
`(parámetros) -> expresión` o bien `(parámetros) -> { bloque de instrucciones }`. El tipo de la lambda no se indica explícitamente, ya que el compilador lo infiere a partir del contexto, normalmente una interfaz funcional.

Los **parámetros** pueden escribirse con o sin tipo. Cuando el compilador puede inferirlos, se suele omitir el tipo para mejorar la legibilidad. Si la lambda recibe un solo parámetro, incluso pueden omitirse los paréntesis. El **cuerpo** puede ser una única expresión (cuyo valor se devuelve implícitamente) o un bloque entre llaves, en cuyo caso debe usarse `return` si se devuelve un valor. Esta dualidad recuerda a la diferencia entre llamar directamente a una función o definirla con un cuerpo completo.

```java
// Un parámetro, expresión única
cadena -> cadena.toUpperCase()

// Varios parámetros, con bloque
(a, b) -> {
    return a.length() + b.length();
}
```

En Java, una función lambda siempre se asocia a una **interfaz funcional**, es decir, una interfaz con un único método abstracto. Por ejemplo, `Function<String, String>` define una función que recibe un `String` y devuelve otro `String`. La lambda es, conceptualmente, una implementación anónima de ese método, pero con una sintaxis mucho más concisa que una clase anónima tradicional, manteniendo la coherencia con el modelo orientado a objetos del lenguaje.



## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

Para **recibir una función como parámetro** y ejecutarla desde dentro de un método, se aprovecha la idea de que las funciones pueden tratarse como valores. Esto permite delegar el comportamiento concreto en código externo, mientras que el método receptor se limita a aplicar dicho comportamiento. Desde el punto de vista conceptual, este mecanismo es equivalente a pasar un puntero a función en C o una estrategia en Java clásico, pero usando lambdas se consigue una sintaxis más clara y expresiva.

En **JavaScript**, esto es un patrón natural, ya que las funciones son ciudadanos de primera clase. El método `transformar` recibe una cadena y una función transformadora, y simplemente invoca dicha función pasando la cadena como argumento. La función concreta (en este caso `aMayusculas`) se define como una lambda y se pasa como parámetro sin necesidad de tipos explícitos.

```javascript
function transformar(cadena, transformador) {
    return transformador(cadena);
}

let aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

let resultado = transformar("Hola mundo", aMayusculas);
console.log(resultado);
```

En **Java**, el mismo concepto se expresa mediante una interfaz funcional. El método `transformar` recibe un `String` y una referencia a una función del tipo `Function<String, String>`, y la ejecuta mediante el método `apply`. Aunque la sintaxis es más verbosa que en JavaScript, el modelo encaja con los conocimientos previos de polimorfismo e interfaces.

```java
import java.util.function.Function;

public class Principal {

    public static String transformar(String cadena, Function<String, String> transformador) {
        return transformador.apply(cadena);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        String resultado = transformar("Hola mundo", aMayusculas);
        System.out.println(resultado);
    }
}
```

En ambos lenguajes, el método `transformar` queda desacoplado de la operación concreta que realiza, pudiendo reutilizarse fácilmente con otras funciones. Este es uno de los beneficios clave del enfoque funcional incorporado en lenguajes multi‑paradigma como Java moderno y JavaScript.



## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

Para **invocar `transformar` pasando una función lambda directamente**, se define el comportamiento en el mismo punto de la llamada, sin asignarlo previamente a una variable. Esto refuerza la idea de tratar a las funciones como valores y es habitual cuando el comportamiento es simple y de uso puntual. Desde el punto de vista conceptual, el método `transformar` no cambia; únicamente se sustituye la referencia a una función previamente definida por una lambda inline.

En **JavaScript**, la función lambda que invierte la cadena se define en el segundo argumento de la llamada a `transformar`. La inversión puede realizarse convirtiendo la cadena en un array de caracteres, invirtiéndolo y uniéndolo de nuevo en una cadena:

```javascript
function transformar(cadena, transformador) {
    return transformador(cadena);
}

let resultado = transformar("Hola mundo", (cadena) => {
    return cadena.split("").reverse().join("");
});

console.log(resultado);
```

En **Java**, el mismo patrón se expresa pasando una lambda compatible con `Function<String, String>` directamente en la llamada. La inversión de la cadena se realiza, por ejemplo, utilizando `StringBuilder`. El cuerpo de la lambda se define en el propio argumento, manteniendo un estilo declarativo y conciso:

```java
import java.util.function.Function;

public class Principal {

    public static String transformar(String cadena, Function<String, String> transformador) {
        return transformador.apply(cadena);
    }

    public static void main(String[] args) {
        String resultado = transformar("Hola mundo",
            cadena -> new StringBuilder(cadena).reverse().toString()
        );

        System.out.println(resultado);
    }
}
```

Este uso directo de funciones lambda mejora la legibilidad cuando la lógica es breve y evita introducir nombres intermedios innecesarios. Además, pone de manifiesto el carácter multi‑paradigma de lenguajes como Java moderno y JavaScript, en los que el comportamiento puede definirse y pasarse de forma inmediata allí donde se necesita.



## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un **cierre** o *closure* se produce cuando una función lambda es capaz de **capturar y utilizar variables definidas en el contexto donde fue creada**, incluso aunque la ejecución de la lambda tenga lugar en otro punto del programa. En otras palabras, la función no solo contiene su propio código, sino también una referencia a los datos externos que necesita para ejecutarse correctamente. Este concepto es fundamental en programación funcional, ya que permite construir funciones más expresivas y dependientes de su entorno.

En **Java**, una lambda puede acceder a variables locales siempre que sean **efectivamente finales**, es decir, que su valor no se modifique después de ser inicializado. Esto garantiza la coherencia del comportamiento capturado por la lambda y evita problemas de sincronización o ambigüedad. Aunque Java no expone el cierre de forma tan flexible como otros lenguajes puramente funcionales, el mecanismo es suficiente para muchos escenarios prácticos y mantiene la seguridad del modelo de ejecución.

Partiendo del ejemplo anterior con el método `transformar`, se introduce ahora una variable local externa que será capturada por la lambda. Dicha lambda concatena una cadena adicional definida fuera de su cuerpo, demostrando así el uso de un cierre:

```java
import java.util.function.Function;

public class Principal {

    public static String transformar(String cadena, Function<String, String> transformador) {
        return transformador.apply(cadena);
    }

    public static void main(String[] args) {
        String sufijo = " !!!";

        String resultado = transformar("Hola mundo",
            texto -> texto + sufijo
        );

        System.out.println(resultado);
    }
}
```

En este ejemplo, la variable local `sufijo` forma parte del **cierre** de la función lambda, ya que es utilizada dentro de ella aunque haya sido definida fuera. La lambda «recuerda» ese valor y lo aplica cuando se ejecuta, ilustrando claramente cómo el contexto de definición influye en el comportamiento de las funciones lambda en Java.



## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

Aunque **una función lambda y un puntero a función en C** permiten almacenar y pasar “comportamiento” como un valor, existen diferencias conceptuales y prácticas importantes entre ambos mecanismos. Un puntero a función en C es, literalmente, una dirección de memoria que apunta al código de una función concreta. No incluye ningún contexto adicional: solo permite llamar a esa función con los parámetros indicados. El lenguaje no incorpora mecanismos extra para asociar datos al puntero más allá de lo que el programador gestione manualmente.

Por el contrario, una **función lambda** es una abstracción de más alto nivel. Además del código ejecutable, una lambda puede **capturar variables del entorno donde fue definida**, formando un cierre (*closure*). Esto significa que la función no solo representa una dirección de código, sino también un contexto asociado que se mantiene vivo mientras la lambda exista. En C, este comportamiento no está soportado de forma nativa y, si se necesita algo equivalente, debe simularse combinando punteros a funciones con estructuras de datos y paso explícito de estado.

Otra diferencia clave está en el **modelo de tipos y seguridad**. En C, la correcta utilización de un puntero a función es responsabilidad total del programador, y errores en la firma pueden provocar comportamientos indefinidos. En Java, las funciones lambda están fuertemente tipadas mediante interfaces funcionales, y el compilador garantiza que la firma sea correcta. Esto las integra de forma natural con conceptos como polimorfismo e interfaces, que ya forman parte del paradigma orientado a objetos.

En síntesis, puede decirse que los punteros a funciones en C son un mecanismo **bajo nivel y explícito**, centrado en direcciones de memoria, mientras que las funciones lambda son un mecanismo **de alto nivel**, integrado en el lenguaje y pensado para expresar comportamiento junto con su contexto. Esta diferencia refleja la evolución desde lenguajes procedimentales hacia lenguajes multi‑paradigma que combinan ideas funcionales y orientadas a objetos.



## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

Para **devolver funciones** se aprovecha la capacidad de las funciones lambda de tratarse como valores de retorno. En Java, esto se consigue devolviendo una instancia de una **interfaz funcional**, como `Function<Double, Double>`. La idea es crear una función `crearDescuento(porcentaje)` que no aplica el descuento directamente, sino que **construye y devuelve** otra función capaz de aplicar ese descuento a cualquier cantidad que se le pase más adelante.

Desde el punto de vista del paradigma funcional, esta técnica permite generar funciones especializadas a partir de parámetros de configuración. En este caso, el porcentaje de descuento actúa como ese parámetro, y el resultado es una función reutilizable que encapsula dicha configuración. Este enfoque evita código duplicado y expresa de manera clara la intención: primero se define el descuento y luego se aplica tantas veces como sea necesario.

El siguiente ejemplo muestra una implementación completa en Java. Se crean dos funciones descuento distintas y se aplican a una misma cantidad para comprobar su efecto:

```java
import java.util.function.Function;

public class Principal {

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad * (1 - porcentaje);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(0.10);
        Function<Double, Double> descuento25 = crearDescuento(0.25);

        double precio = 100.0;

        System.out.println(descuento10.apply(precio)); // 90.0
        System.out.println(descuento25.apply(precio)); // 75.0
    }
}
```

En este contexto, la **closure** se produce porque la función lambda devuelta captura la variable `porcentaje`, que pertenece al contexto de ejecución de `crearDescuento`. Aunque dicha variable es local al método y deja de existir cuando este termina, su valor queda “recordado” dentro de la lambda. Cada llamada a `crearDescuento` genera un cierre distinto, con su propio valor de `porcentaje`, lo que permite que cada función descuento mantenga su comportamiento específico de forma independiente.



## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

En Java, una **interfaz funcional** es una interfaz que representa un **tipo de función**, es decir, describe la firma de una operación que puede implementarse mediante una función lambda o una referencia a método. Su propósito es proporcionar un tipo estático al que pueda asociarse una lambda, permitiendo que el compilador sepa qué parámetros recibe y qué valor devuelve dicha función. Desde el punto de vista conceptual, una interfaz funcional define el *contrato* que debe cumplir la función.

El **requisito fundamental** de una interfaz funcional es que tenga **exactamente un único método abstracto**. Este método es el que la función lambda implementa implícitamente. Pueden existir otros métodos en la interfaz, siempre que no sean abstractos, como métodos `default` o `static`. La restricción de “un solo método abstracto” es clave, ya que elimina cualquier ambigüedad sobre qué método debe implementarse cuando se usa una lambda.

Java proporciona varias **interfaces funcionales estándar** en el paquete `java.util.function`, como `Function<T, R>`, `Predicate<T>` o `Consumer<T>`. Estas interfaces cubren los casos más comunes y evitan la necesidad de crear nuevas interfaces en muchos escenarios. No obstante, también es posible definir interfaces funcionales propias, especialmente cuando la semántica del dominio lo justifica.

De forma opcional, una interfaz funcional puede anotarse con `@FunctionalInterface`, lo que no cambia su funcionamiento, pero permite al compilador verificar que realmente cumple los requisitos. Esta anotación actúa como una garantía explícita de diseño y ayuda a mantener la compatibilidad del código, ya que cualquier intento futuro de añadir un segundo método abstracto provocará un error de compilación.



## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Una **interfaz funcional definida a mano** permite modelar explícitamente un tipo de función adaptado al dominio del problema. En este caso, se desea representar una operación que transforma una cadena de texto (`String`) en otra cadena (`String`). Conceptualmente, esta interfaz cumple el mismo papel que `Function<String, String>`, pero con un nombre más expresivo, lo que mejora la legibilidad y la intención del código.

Para que una interfaz sea funcional en Java, debe cumplir el requisito de **tener un único método abstracto**. Ese método define la firma de la función que implementarán las funciones lambda asociadas a dicha interfaz. De forma opcional, se puede usar la anotación `@FunctionalInterface` para asegurar en tiempo de compilación que la interfaz no deja de ser funcional por error.

La interfaz `Transformador` puede definirse de la siguiente manera:

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}
```

Una vez definida, esta interfaz puede utilizarse como tipo para funciones lambda que conviertan una cadena en otra, por ejemplo para pasar comportamientos como parámetros o devolver funciones. De este modo, se mantiene la comprobación estática de tipos propia de Java y se refuerza la integración del estilo funcional dentro de un lenguaje orientado a objetos.



## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

Al utilizar **genéricos** en una interfaz funcional, se consigue generalizar el concepto de transformación para que no dependa de tipos concretos. En lugar de limitarse a transformar un `String` en otro `String`, la interfaz puede describir una conversión desde un tipo de entrada a un tipo de salida arbitrarios. Esto incrementa la reutilización y encaja con los conocimientos previos sobre *genericidad* en Java.

Una interfaz funcional genérica debe seguir cumpliendo el requisito esencial: **un único método abstracto**. Los parámetros de tipo (por ejemplo, `T` y `R`) representan respectivamente el tipo de entrada y el tipo de salida de la transformación. La anotación `@FunctionalInterface` sigue siendo válida y recomendable, ya que asegura que la interfaz mantiene su naturaleza funcional.

La definición genérica de la interfaz `Transformador` puede escribirse de la siguiente forma:

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}
```

A partir de esta interfaz, es posible definir distintos transformadores mediante funciones lambda. Por ejemplo, un transformador que convierta un `Double` en un `Integer` redondeando el valor puede definirse e invocarse así:

```java
public class Principal {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear =
            valor -> (int) Math.round(valor);

        Integer resultado = redondear.transformar(3.6);
        System.out.println(resultado); // 4
    }
}
```

En este ejemplo, la interfaz genérica permite expresar de forma clara una conversión entre tipos distintos, manteniendo la comprobación estática de tipos de Java. La función lambda actúa como una implementación concreta del método abstracto, demostrando cómo los genéricos y el paradigma funcional se integran de manera natural en el lenguaje.



## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

En Java, las **interfaces funcionales predefinidas** se encuentran principalmente en el paquete **`java.util.function`**, introducido en Java 8 para dar soporte al uso de funciones lambda y programación funcional. Estas interfaces cubren los casos más habituales de paso y composición de funciones, evitando la necesidad de definir interfaces propias como `Transformador<T, R>` en la mayoría de situaciones. Todas ellas cumplen el requisito fundamental: **un único método abstracto**.

La interfaz más general es **`Function<T, R>`**, que representa una función que recibe un valor de tipo `T` y devuelve otro de tipo `R`. Muy cercana a ella está **`UnaryOperator<T>`**, una especialización de `Function<T, T>` para transformaciones donde el tipo de entrada y salida es el mismo, y **`BinaryOperator<T>`**, que extiende `BiFunction<T, T, T>` y representa una operación entre dos valores del mismo tipo que devuelve uno del mismo tipo.

Para funciones que **no devuelven resultado**, existe **`Consumer<T>`**, que recibe un valor y produce un efecto lateral (por ejemplo, imprimir por pantalla). Su versión con dos parámetros es **`BiConsumer<T, U>`**. En el caso opuesto, cuando una función **no recibe parámetros pero devuelve un resultado**, se utiliza **`Supplier<T>`**, muy común para generación diferida de valores.

También existen interfaces funcionales orientadas a **evaluaciones lógicas**, como **`Predicate<T>`**, que recibe un valor y devuelve un `boolean`, y **`BiPredicate<T, U>`**, que evalúa una condición sobre dos valores. Además, Java proporciona versiones especializadas para tipos primitivos (`IntFunction`, `DoublePredicate`, `LongConsumer`, etc.), que evitan el *autoboxing* y mejoran el rendimiento en casos críticos.

En conjunto, este conjunto de interfaces hace que Java sea claramente **multi‑paradigma**, permitiendo expresar comportamientos funcionales de forma segura y reutilizable. En la práctica, `Function<T, R>` sustituye casi por completo a interfaces genéricas como `Transformador<T, R>`, reservando la definición de interfaces funcionales propias para casos donde el **significado semántico del dominio** sea más importante que la reutilización genérica.



## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

En Java, el método **`List.forEach`** es una forma funcional de recorrer una colección sin utilizar explícitamente un bucle `for` tradicional. Este método recibe como parámetro una **función lambda** (o una referencia a método) que será ejecutada para cada elemento de la lista. Conceptualmente, el control de la iteración pasa de ser explícito (índices, condiciones) a ser declarativo: se expresa *qué hacer* con cada elemento, no *cómo recorrer* la colección.

Desde el punto de vista del paradigma funcional, `forEach` ejemplifica el uso de **comportamiento como parámetro**. La lógica que se aplica a cada elemento se encapsula en una función lambda, que es invocada internamente por la lista. Esto reduce el código repetitivo y mejora la legibilidad, especialmente cuando la operación a realizar es simple y está claramente relacionada con cada elemento de la colección.

En el siguiente ejemplo se recorre una lista de `Integer` y se muestra un mensaje únicamente si el valor es positivo. La comprobación se realiza dentro de la función lambda pasada a `forEach`:

```java
import java.util.Arrays;
import java.util.List;

public class Principal {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-2, 5, 0, 3, -1);

        numeros.forEach(numero -> {
            if (numero > 0) {
                System.out.println("El número es positivo: " + numero);
            }
        });
    }
}
```

Este enfoque funcional elimina la necesidad de manejar índices o estructuras de control explícitas y refuerza la idea de que las colecciones “saben” cómo recorrerse. Además, sirve como base conceptual para entender operaciones más avanzadas del estilo funcional en Java, como las que se realizan mediante la API de *Streams*.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

En la firma de `forEach`, que es `void forEach(Consumer<? super T> action)`, se utiliza **`Consumer<? super T>` y no `Consumer<T>`** para hacer el método más flexible desde el punto de vista de la *genericidad*. La clave está en que `forEach` **consume** elementos de la colección (los pasa como argumento a la función), pero nunca los produce ni devuelve. Por tanto, es válido que la función aceptada sea capaz de consumir no solo `T`, sino también **cualquier supertipo de `T`**, como `Object`. Este uso amplía las posibilidades de reutilización sin comprometer la seguridad de tipos.

Este principio se resume en la regla conocida como **PECS**: *Producer Extends, Consumer Super*. Según PECS, cuando un parámetro genérico **produce** valores de tipo `T`, debe usarse `? extends T`; cuando **consume** valores de tipo `T`, debe usarse `? super T`. En el caso de `Consumer`, su único método es `accept(T value)`, lo que significa que recibe valores, es decir, **consume** datos. Por ello, resulta correcto usar `? super T`, permitiendo consumidores más generales que el tipo exacto de la colección.

Aplicando esta idea al método `transformar`, se puede mejorar su firma cuando se trabaja con funciones transformadoras. El método recibe un valor de tipo `T` y una función que lo **consume** como entrada y **produce** un resultado de tipo `R`. Según PECS, el tipo de la función debería expresarse como `Function<? super T, ? extends R>`, ya que la función consume valores de tipo `T` (o supertipos) y produce valores de tipo `R` (o subtipos). Esto permite aceptar funciones más generales como transformadoras sin perder seguridad de tipos.

En definitiva, el uso de comodines con PECS no es un detalle sintáctico, sino una decisión de diseño orientada a maximizar la **flexibilidad y reutilización** de APIs genéricas. `forEach` es un ejemplo clásico de esta filosofía, y el método `transformar` puede beneficiarse del mismo razonamiento para integrarse correctamente en un estilo funcional y genérico en Java.


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Las **referencias a métodos** permiten tratar un método ya existente como un valor, de forma similar a una función lambda, sin necesidad de volver a definir su cuerpo. En lugar de escribir una lambda que invoque al método, se toma directamente una referencia al mismo. Esto resulta útil para simplificar el código y mejorar su legibilidad, especialmente cuando la lambda solo delega en un método existente.

En **JavaScript**, las funciones son ciudadanos de primera clase, pero es importante tener en cuenta el contexto (`this`). Para obtener una referencia a un método de un objeto y poder invocarlo correctamente, suele usarse `bind`, que fija el valor de `this`. En el siguiente ejemplo se define una clase `Persona` con un método `saludar`, se crea un objeto y se obtiene una referencia a dicho método:

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const persona = new Persona("Ana");
const referenciaSaludar = persona.saludar.bind(persona);

referenciaSaludar();
```

En **Java**, las referencias a métodos forman parte del soporte funcional introducido en Java 8. Estas referencias se combinan con interfaces funcionales y mantienen el enlace con el objeto o la clase correspondiente. A continuación se muestra una clase `Persona` y cómo obtener una referencia a su método `saludar` desde el código principal:

```java
public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}
```

```java
public class Principal {
    public static void main(String[] args) {
        Persona persona = new Persona("Ana");

        Runnable referenciaSaludar = persona::saludar;
        referenciaSaludar.run();
    }
}
```

En este caso, `persona::saludar` es una referencia directa al método de instancia, y `Runnable` se usa como interfaz funcional compatible porque define un método sin parámetros y sin valor de retorno. Tanto en JavaScript como en Java, este mecanismo refuerza la idea de que los métodos pueden manejarse como valores, facilitando un estilo de programación más funcional y expresivo.



## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

En Java existen **cuatro tipos principales de referencias a método**, todas ellas introducidas en Java 8 como una forma concisa y expresiva de reutilizar métodos existentes en un contexto funcional. Una referencia a método es, conceptualmente, una shorthand de una función lambda cuyo cuerpo simplemente delega la llamada en un método concreto. Todas ellas se usan siempre en combinación con una **interfaz funcional** compatible con la firma del método referenciado.

El **primer tipo** es la **referencia a un método estático**, cuya forma general es `Clase::metodoEstatico`. Se utiliza cuando el método no depende de ninguna instancia. Por ejemplo, una referencia al método `parseInt` de `Integer`, compatible con `Function<String, Integer>`:

```java
Function<String, Integer> convertir = Integer::parseInt;
Integer valor = convertir.apply("42");
```

El **segundo tipo** es la **referencia a un método de instancia de un objeto concreto**, con la forma `objeto::metodo`. En este caso, el método se invoca siempre sobre esa instancia específica, y los parámetros del método coinciden con los de la interfaz funcional:

```java
Persona persona = new Persona("Ana");

Runnable saludar = persona::saludar;
saludar.run();
```

El **tercer tipo** es la **referencia a un método de instancia sobre cualquier instancia de una clase**, con la forma `Clase::metodo`. Aquí, el objeto sobre el que se invoca el método se pasa implícitamente como primer parámetro de la función. Este tipo es común en operaciones sobre colecciones:

```java
Function<String, Integer> longitud = String::length;
Integer l = longitud.apply("Hola");
```

Por último, existe la **referencia a un constructor**, cuya forma es `Clase::new`. Este tipo permite tratar la creación de objetos como una función, lo que resulta muy útil en código genérico y en APIs funcionales:

```java
Function<String, Persona> crearPersona = Persona::new;
Persona p = crearPersona.apply("Luis");
```

En conjunto, estos cuatro tipos cubren los casos habituales de reutilización de comportamiento en Java. Las referencias a método mejoran la legibilidad del código y refuerzan el enfoque funcional del lenguaje, sin romper su modelo de comprobación estática de tipos ni su base orientada a objetos.



## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

Este ejemplo muestra un uso expresivo de las **expresiones lambda** para definir criterios de ordenación sin necesidad de clases auxiliares. El método `Collections.sort` recibe un **comparador**, cuyo objetivo es indicar cómo se comparan dos objetos del mismo tipo. Desde Java 8, dicho comparador puede definirse fácilmente mediante una lambda, lo que encaja muy bien con el estilo funcional y reduce código ceremonial.

En primer lugar, se presenta la versión **manual** del comparador, implementando explícitamente la lógica de comparación. Se compara primero la edad, y solo en caso de igualdad se compara el nombre alfabéticamente. La lambda recibe dos objetos `Persona` y devuelve un entero siguiendo el contrato de `Comparator`.

```java
import java.util.*;

class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}

public class Principal {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Luis", 30),
            new Persona("Ana", 25),
            new Persona("Pedro", 30),
            new Persona("Marta", 25)
        );

        Collections.sort(personas, (p1, p2) -> {
            int cmpEdad = Integer.compare(p1.getEdad(), p2.getEdad());
            if (cmpEdad != 0) {
                return cmpEdad;
            }
            return p1.getNombre().compareTo(p2.getNombre());
        });

        System.out.println(personas);
    }
}
```

A continuación, se muestra una versión más **declarativa y expresiva**, utilizando métodos auxiliares de la interfaz `Comparator`. Aquí se encadenan comparadores mediante `comparingInt` y `thenComparing`, lo que hace que la intención del código sea más clara y reduce la lógica imperativa explícita.

```java
Collections.sort(personas,
    Comparator
        .comparingInt(Persona::getEdad)
        .thenComparing(Persona::getNombre)
);
```

Esta segunda versión es generalmente preferible en Java moderno, ya que mejora la legibilidad y reutiliza utilidades estándar bien probadas. Ambos enfoques son correctos, pero el uso de `Comparator` refuerza el estilo funcional y demuestra cómo Java combina programación orientada a objetos y funcional de forma natural.

