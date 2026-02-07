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

En POO, **la encapsulación** y **la ocultación de información** buscan limitar el acceso directo a los datos internos de un objeto, de modo que solo puedan manipularse mediante métodos definidos por la propia clase. Esto permite controlar estrictamente cómo se modifican y consultan los atributos, evitando comportamientos no deseados o incoherentes. En esencia, ambas prácticas buscan proteger la integridad de los datos, estableciendo una frontera clara entre el interior del objeto (su implementación) y el exterior (su interfaz pública).

Además, la ocultación de información evita que otros componentes del programa dependan de detalles internos que podrían cambiar en el futuro. Al exponer solo lo necesario —por ejemplo, métodos `get` y `set` o funciones más elaboradas— se facilita la evolución del código sin romper otras partes del sistema. De este modo, se mejora la mantenibilidad y se reduce el acoplamiento entre módulos o clases.

Entre las ventajas de la ocultación de información pueden enumerarse las siguientes:

1.  **Protección del estado interno**, evitando modificaciones incorrectas o inconsistentes.
2.  **Reducción del acoplamiento**, ya que otras partes del código dependen solo de lo que la clase expone públicamente.
3.  **Mayor flexibilidad**, permitiendo cambiar la implementación interna sin afectar el uso externo.
4.  **Mejor control sobre el acceso**, al poder validar o filtrar datos desde los métodos de la clase antes de aplicarlos.

Estas ventajas hacen que la encapsulación sea uno de los pilares fundamentales de la POO y un recurso clave para escribir software más robusto, legible y fácil de mantener.



## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

En POO, la **interfaz pública** de un objeto o clase se entiende como el conjunto de métodos y atributos accesibles desde fuera de dicha clase. Constituye la “puerta de entrada” a las funcionalidades que el objeto ofrece y define cómo debe interactuarse con él. Mientras que los detalles internos pueden variar, la interfaz pública permanece estable y sirve como contrato que indica qué operaciones pueden realizarse sin necesidad de conocer cómo están implementadas.

Esta interfaz está directamente relacionada con la **ocultación de información**, ya que permite separar lo que debe saberse del objeto (su comportamiento visible) de lo que debe mantenerse privado (su estructura interna y lógica de funcionamiento). Gracias a esta separación, se ocultan los datos y procedimientos que no deberían modificarse directamente, obligando a acceder al estado interno únicamente mediante los métodos definidos. Así se consigue un diseño más seguro, mantenible y menos propenso a errores, ya que cualquier cambio interno no afecta a quienes utilizan la clase mientras la interfaz pública se mantenga estable.



## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Diseñar con cuidado la **interfaz pública** de una clase es fundamental porque actúa como el contrato que otros módulos, clases u objetos utilizarán para interactuar con ella. Si este contrato está mal definido —por ejemplo, si expone más de lo necesario o si permite modificar el estado de forma insegura— se corre el riesgo de generar dependencias indeseadas, acoplamiento excesivo y dificultades para mantener la coherencia interna del objeto. Una interfaz pública clara, mínima y bien pensada contribuye a que la clase sea más robusta y su uso más intuitivo.

Por otro lado, cambiar una **interfaz pública** no suele ser fácil, ya que cualquier modificación puede afectar a todos los componentes que la estén utilizando. Incluso cambios aparentemente pequeños, como renombrar un método o alterar sus parámetros, pueden romper código existente y obligar a realizar ajustes en múltiples lugares. Por este motivo, es habitual considerar la interfaz pública como la parte más estable del diseño, mientras que la implementación interna puede evolucionar con mucha más libertad.



## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las **invariantes de clase** son condiciones lógicas que deben cumplirse siempre para que un objeto se considere válido durante toda su vida útil. Normalmente describen restricciones sobre los atributos —por ejemplo, que un valor nunca sea negativo, que una colección no tenga elementos nulos o que dos campos mantengan una relación coherente entre sí. Estas invariantes deben cumplirse tras la construcción del objeto y después de la ejecución de cualquier método público que pueda modificar su estado.

La **ocultación de información** ayuda porque impide que el código externo modifique directamente los atributos, lo cual podría romper estas invariantes sin control. Al obligar a que los cambios se realicen únicamente a través de métodos definidos por la clase, es posible validar los datos, comprobar las restricciones y garantizar que las invariantes se cumplan siempre. De esta forma, se mantiene la coherencia interna del objeto y se evita que su estado quede en situaciones inconsistentes o erróneas.

Además, gracias a la encapsulación, la propia clase puede reorganizar cómo mantiene internamente estos datos sin afectar a quienes la usan. Esto permite evolucionar la implementación, mejorarla o reforzar las invariantes sin necesidad de modificar el código externo. En conjunto, la ocultación de información se convierte en una herramienta clave para preservar la integridad lógica del objeto y facilitar un diseño más robusto y confiable.



## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

A modo de ejemplo, se muestra una clase `Punto` en Java que **oculta** sus datos (`x` e `y`) marcándolos como `private`, y expone una **interfaz pública** mínima para construir el punto, consultarlo, modificarlo de forma controlada y calcular su distancia al origen:

```java
public final class Punto {
    // Estado interno oculto (encapsulación)
    private double x;
    private double y;

    // Invariante simple de ejemplo: NaN no permitido
    private static void validar(double valor, String nombre) {
        if (Double.isNaN(valor) || Double.isInfinite(valor)) {
            throw new IllegalArgumentException(nombre + " no puede ser NaN ni infinito");
        }
    }

    // Constructor
    public Punto(double x, double y) {
        validar(x, "x");
        validar(y, "y");
        this.x = x;
        this.y = y;
    }

    // Métodos de acceso controlado (interfaz pública)
    public double getX() { return x; }
    public double getY() { return y; }

    public void setX(double x) {
        validar(x, "x");
        this.x = x;
    }

    public void setY(double y) {
        validar(y, "y");
        this.y = y;
    }

    // Comportamiento observable
    public double calcularDistanciaAOrigen() {
        // Math.hypot evita overflow/underflow mejor que sqrt(x*x + y*y)
        return Math.hypot(x, y);
    }

    // Opcional: método de ayuda para depuración/registro
    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}
```

La **interfaz pública** de `Punto` está compuesta por aquello que el código externo puede invocar: el **constructor público** `Punto(double, double)`, los **accesores** `getX()` y `getY()`, los **mutadores** `setX(double)` y `setY(double)`, el **método de comportamiento** `calcularDistanciaAOrigen()`, y el `toString()` sobrescrito. Todo ello constituye el “contrato” estable mediante el cual otros módulos interactúan con la clase, mientras que los detalles internos (los campos `x` e `y` y la política de validación) permanecen **ocultos** y pueden cambiarse sin romper a los clientes, siempre que la interfaz pública se mantenga.

En Java, `public` indica que el miembro (clase, método o campo) es **accesible desde cualquier lugar** donde la clase sea visible, formando parte de su interfaz pública. En cambio, `private` limita el acceso **exclusivamente** al interior de la propia clase, lo que posibilita la **ocultación de información**: se evita que código externo modifique directamente el estado interno y se obliga a pasar por métodos controlados (por ejemplo, `setX`/`setY`), donde puede **validarse** y **preservarse** la invariante (como “no permitir NaN/∞”). Para quien venga de C/C++ sin OOP, puede verse como una `struct` cuyos campos no son accesibles directamente y sólo se manipulan mediante funciones asociadas que hacen de “guardas” del estado.



## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores de acceso **`public`** y **`private`** pueden aplicarse a **clases**, **constructores**, **métodos** y **atributos (campos)**. Su función es controlar desde dónde pueden ser utilizados dichos elementos, favoreciendo la encapsulación y la ocultación de información. De este modo, se limita o amplía el acceso según el diseño deseado de la interfaz pública de la clase.

El modificador `public` puede aplicarse tanto a **clases de primer nivel** (siempre y cuando el archivo solo contenga una clase pública) como a **miembros internos** de una clase (métodos, campos y constructores). Por el contrario, `private` **no puede** aplicarse a clases de primer nivel, pero sí a **clases internas**, además de a métodos, atributos y constructores. Cualquier miembro marcado como `private` solo será accesible desde dentro de la propia clase que lo declara, convirtiéndose así en un mecanismo clave para proteger el estado interno y evitar usos indebidos.

En consecuencia, Java permite un control granular sobre la visibilidad de cada una de las piezas que componen una clase. Esto posibilita construir una interfaz pública clara —formada por elementos `public`— y mantener ocultos los detalles internos mediante miembros `private`, lo que constituye una de las bases fundamentales de la programación orientada a objetos.



## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

En POO existen más tipos de visibilidad además de la pública y la privada. Aunque el concepto general es siempre el mismo —controlar quién puede acceder a qué parte de una clase— distintos lenguajes ofrecen distintos niveles y matices. Estos niveles adicionales permiten afinar mejor la encapsulación, ajustando la accesibilidad a las necesidades de diseño de cada sistema.

En Java, además de `public` y `private`, existen dos visibilidades adicionales: **`protected`** y el llamado **“paquete”** o *package‑private* (la visibilidad por defecto, cuando no se escribe ningún modificador). `protected` permite el acceso desde la propia clase, sus subclases y cualquier clase del mismo paquete, lo que resulta útil para herencia. La visibilidad de paquete permite el acceso solo a clases del mismo paquete, lo que favorece agrupar componentes relacionados y compartir detalles internos sin exponerlos al exterior. Así, Java ofrece **cuatro niveles en total** de visibilidad para miembros y **dos** para clases de primer nivel (`public` y paquete).

En otros lenguajes, la situación varía. Por ejemplo, C++ ofrece `public`, `private` y `protected`, pero además permite granularidad a nivel de herencia mediante **modos de herencia pública, privada o protegida**, que afectan cómo se “heredan” estas visibilidades. Lenguajes como Python no imponen visibilidad estricta, pero usan convenciones como `_atributo` (protegido) y `__atributo` (pseudo‑privado mediante *name mangling*). Otros lenguajes modernos, como Swift o Kotlin, incorporan aún más niveles, incluyendo visibilidades como `internal` o `fileprivate`, orientadas a modularizar mejor el acceso dentro de proyectos grandes.

En conjunto, prácticamente todos los lenguajes orientados a objetos amplían la visibilidad más allá de lo público y privado para ofrecer un control más fino sobre la encapsulación. Java se sitúa en un punto intermedio, con un conjunto claro y relativamente simple de niveles, suficiente para mantener interfaces públicas limpias y ocultar de forma robusta los detalles internos.



## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

En Java, los miembros de instancia `private` están ocultos para **(a) otras clases**, pero **no** para **(b) otras instancias de la misma clase**. Dentro del código de la propia clase, cualquier método puede acceder a los campos privados de **cualquier** instancia de esa clase (incluida otra distinta de `this`). Esto permite, por ejemplo, implementar operaciones entre dos objetos del mismo tipo sin exponer su estado.

A continuación, un ejemplo de `Punto` con un método `calcularDistanciaAPunto(Punto otro)` que accede directamente a los campos privados `x` e `y` tanto del receptor como del parámetro `otro`. Este acceso es válido porque ocurre **dentro de la misma clase** `Punto`. En cambio, si se intentara leer `otro.x` desde **otra clase** distinta, el compilador lo impediría al ser `private`.

```java
public final class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        validar(x, "x");
        validar(y, "y");
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.hypot(x, y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto 'otro' no puede ser null");
        }
        // Acceso a campos privados de otra instancia de la MISMA clase: permitido en Java
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.hypot(dx, dy);
    }

    // Getters opcionales (no necesarios para el ejemplo)
    public double getX() { return x; }
    public double getY() { return y; }

    private static void validar(double v, String nombre) {
        if (Double.isNaN(v) || Double.isInfinite(v)) {
            throw new IllegalArgumentException(nombre + " no puede ser NaN ni infinito");
        }
    }
}
```

En suma, `private` implementa **ocultación frente a otras clases**, no frente a **otras instancias de la misma clase**. La razón es que la unidad de encapsulación es la **clase**: el compilador permite que el código de `Punto` acceda a los detalles privados de **cualquier** `Punto`, lo cual simplifica operaciones internas (comparaciones, copias, cálculos) sin necesidad de exponer getters o ampliar la visibilidad. Esta propiedad hace posible mantener una interfaz pública mínima y, a la vez, implementar lógica rica y segura dentro de la clase.



## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

En los lenguajes orientados a objetos, los métodos **getter** y **setter** son funciones diseñadas para **acceder** y **modificar**, respectivamente, los atributos privados de una clase. Su uso está directamente relacionado con la encapsulación, ya que permiten controlar cómo se consulta y actualiza el estado interno sin exponer directamente los campos. Así, en lugar de acceder a los atributos de forma pública, se obliga a pasar por métodos que pueden validar datos, mantener invariantes o aplicar reglas de negocio.

Un **getter** es un método que devuelve el valor de un atributo privado. Normalmente su nombre comienza por `get` seguido del nombre del atributo con mayúscula inicial, como `getX()` o `getNombre()`. Por otro lado, un **setter** permite modificar el valor de un campo, y suele llamarse `setX()` o `setNombre()`. Dentro del setter puede incluirse cualquier control necesario para evitar valores inválidos o inconsistentes, con lo cual se refuerza la integridad del objeto.

Estos métodos sirven también para desacoplar la interfaz pública de la implementación interna. Si en el futuro se cambia la forma en la que un atributo se almacena, calcula o valida, los clientes de la clase no necesitan modificar su código siempre que sigan usando los mismos getters y setters. De esta manera, se consigue una mayor flexibilidad y se facilita el mantenimiento del programa.



## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No: cuando en POO se dice que la **ocultación de información mejora la “seguridad”**, no se está hablando de evitar que un programa sea *hackeado* en el sentido tradicional de la ciberseguridad. El término se refiere a un tipo de **seguridad interna o lógica**, cuyo objetivo es impedir que otras partes del propio programa utilicen mal un objeto o lo dejen en un estado inconsistente. Se trata de proteger la **integridad del objeto**, no de bloquear ataques externos.

La idea principal es que si los atributos internos están ocultos y solo pueden modificarse mediante métodos controlados, entonces es mucho más difícil que el propio desarrollador (u otro módulo del programa) provoque errores involuntarios. Por ejemplo, si una clase necesita que un valor nunca sea negativo, un setter puede validar eso, pero si los atributos fueran públicos, cualquier parte del código podría romper esa regla sin querer. Ese tipo de fallos no son un “hackeo”, sino errores de diseño o uso.

En este sentido, la encapsulación actúa como una barrera que **evita accesos indebidos dentro del propio software**, reduciendo el acoplamiento y permitiendo mantener invariantes y comportamientos coherentes. La “seguridad” alude, por tanto, a un diseño más robusto, predecible y resistente a errores internos. Aunque esta robustez pueda contribuir indirectamente a reducir ciertos fallos explotables, la encapsulación **no sustituye** a medidas reales de ciberseguridad como cifrado, firewalls o control de permisos del sistema.

En resumen, cuando se habla de “seguridad” en encapsulación se hace referencia a la protección del **estado interno** y a la prevención de **errores lógicos** dentro del propio programa, no a la defensa contra ataques externos. Es una forma de seguridad estructural o de diseño, no de seguridad informática en sentido estricto.



## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

En POO, un **miembro de instancia** es aquel que pertenece a cada **objeto concreto** creado a partir de una clase. Cada instancia tiene su propia copia de esos atributos, de modo que modificar el valor en un objeto no afecta a los demás. Por ejemplo, en una clase `Punto`, los campos `x` e `y` suelen ser miembros de instancia porque cada punto tiene sus propias coordenadas. Lo mismo sucede con los métodos que operan sobre datos específicos del objeto, aunque los métodos en Java no se replican por objeto, sí actúan sobre el estado particular de cada instancia mediante `this`.

Por contraste, un **miembro de clase** (en Java marcado con `static`) pertenece a la **clase en sí misma**, no a las instancias. Esto implica que existe **una única copia compartida** entre todos los objetos. Este tipo de miembros se usa cuando el dato o comportamiento no depende del estado individual de cada objeto, sino de algo común al conjunto de instancias, como un contador estático, una constante o una función utilitaria. Así, incluso sin crear objetos, es posible acceder a estos miembros a través del nombre de la clase.

Respecto a la ocultación, los **miembros de clase también pueden ocultarse** utilizando modificadores de acceso como `private`, igual que los miembros de instancia. Esto permite, por ejemplo, mantener una constante interna o un método auxiliar estático que no tiene por qué formar parte de la interfaz pública. La encapsulación sigue funcionando del mismo modo: la clase controla quién puede ver o utilizar esos elementos, ya sean de instancia o de clase, garantizando una implementación más limpia y menos expuesta.

En conjunto, ambos tipos de miembros participan en la encapsulación: los atributos de instancia protegen el estado individual de cada objeto, mientras que los miembros de clase protegen o comparten información relevante para la clase completa. Tener control sobre su visibilidad permite definir con precisión qué forma parte de la interfaz pública y qué debe permanecer como detalle interno.



## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene sentido que los **constructores sean privados** en ciertos patrones de diseño y situaciones concretas en POO. Aunque lo habitual es que los constructores sean públicos para permitir crear instancias libremente, hacerlos privados permite **controlar estrictamente** cómo y cuándo se crean los objetos. Esto ayuda a mantener invariantes más fuertes y a evitar usos indebidos, especialmente cuando no interesa permitir que cualquier código externo cree nuevas instancias sin control.

Un caso típico es el **patrón Singleton**, donde solo debe existir una única instancia de la clase. Marcando el constructor como `private`, se impide la creación directa de objetos y se obliga a obtener la instancia a través de un método estático de la propia clase. Otro caso habitual son las **fábricas estáticas (factory methods)**, donde se oculta la lógica de creación para poder decidir dinámicamente el tipo de objeto que se devuelve o aplicar validaciones previas. Este enfoque permite aislar detalles internos y mantener una interfaz pública más limpia y estable.

También tiene sentido usar constructores privados en clases que solo proporcionan **métodos utilitarios estáticos**, como `java.lang.Math`, donde no tiene sentido crear instancias porque la clase no representa objetos con estado. De este modo, hacer el constructor privado evita que otros desarrolladores creen instancias accidentales o inútiles, reforzando así la intención de diseño.

En resumen, aunque no es lo más frecuente en clases corrientes, los constructores privados son una herramienta útil para **controlar la creación de objetos**, **reforzar la encapsulación** y **expresar más claramente la intención** de cómo debe utilizarse una clase.



## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

En Java, los **miembros de clase** se indican con la palabra clave `static`. Esto hace que el miembro pertenezca a la **clase** y no a cada **instancia**: existe una única copia compartida por todos los objetos. Pueden ser campos (`static` fields), métodos (`static` methods), bloques estáticos o constantes (`static final`). Se accede a ellos preferentemente mediante el **nombre de la clase** (por ejemplo, `Punto.getMaxXGlobal()`), incluso aunque no exista ninguna instancia creada.

A continuación, se muestra una versión de la clase `Punto` que incluye **miembros de clase** para llevar un registro del valor máximo de `x` y `y` observado en **todas** las instancias creadas y modificadas. Para mantener la encapsulación, los campos estáticos son `private` y se exponen mediante **métodos estáticos públicos**. Se actualizan tanto en el **constructor** como en los **setters**, de modo que los máximos globales reflejan la evolución de todos los puntos a lo largo del tiempo.

```java
public final class Punto {
    // ===== Miembros de instancia (estado propio de cada objeto) =====
    private double x;
    private double y;

    // ===== Miembros de clase (compartidos por TODAS las instancias) =====
    private static double maxXGlobal = Double.NEGATIVE_INFINITY;
    private static double maxYGlobal = Double.NEGATIVE_INFINITY;

    // (Opcional) sincronización si se espera uso concurrente
    private static synchronized void actualizarMaximos(double x, double y) {
        if (x > maxXGlobal) maxXGlobal = x;
        if (y > maxYGlobal) maxYGlobal = y;
    }

    // ===== Constructor =====
    public Punto(double x, double y) {
        validar(x, "x");
        validar(y, "y");
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y); // registra en los máximos globales
    }

    // ===== Getters/Setters de instancia =====
    public double getX() { return x; }
    public double getY() { return y; }

    public void setX(double x) {
        validar(x, "x");
        this.x = x;
        actualizarMaximos(this.x, this.y);
    }

    public void setY(double y) {
        validar(y, "y");
        this.y = y;
        actualizarMaximos(this.x, this.y);
    }

    // ===== Comportamiento de instancia =====
    public double calcularDistanciaAOrigen() {
        return Math.hypot(x, y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        if (otro == null) throw new IllegalArgumentException("El punto 'otro' no puede ser null");
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.hypot(dx, dy);
    }

    // ===== Interfaz pública para los máximos globales =====
    public static double getMaxXGlobal() { return maxXGlobal; }
    public static double getMaxYGlobal() { return maxYGlobal; }

    // ===== Utilidad interna =====
    private static void validar(double v, String nombre) {
        if (Double.isNaN(v) || Double.isInfinite(v)) {
            throw new IllegalArgumentException(nombre + " no puede ser NaN ni infinito");
        }
    }

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}
```

En este diseño, `maxXGlobal` y `maxYGlobal` son **detalle interno** (`private static`) y **no forman parte del estado de cada objeto**, sino del estado **compartido** de la clase. La interfaz pública se limita a los métodos estáticos `getMaxXGlobal()` y `getMaxYGlobal()`, que permiten consultar los máximos actuales. Si se prevé **concurrencia**, conviene proteger las actualizaciones (como se ha mostrado con `synchronized`) o utilizar tipos atómicos/estrategias equivalentes; si la aplicación es monohilo, puede prescindirse de la sincronización para simplificar.



## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

```java
// Método factoría estático: redondea al entero más cercano y crea el Punto
public static Punto crearRedondeado(double x, double y) {
    double rx = (double) Math.round(x); // redondeo half-up
    double ry = (double) Math.round(y);
    return new Punto(rx, ry);
}
// Sí: se usa `static` porque es un método factoría de clase.
```



## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

A continuación se muestra una versión de `Punto` que **mantiene la misma interfaz pública** (constructor, getters/setters, `calcularDistanciaAOrigen`, `calcularDistanciaAPunto`, los métodos estáticos de máximos y la factoría `crearRedondeado`), pero **cambia la representación interna**: en lugar de dos `double` separados, utiliza un **array interno** de dos posiciones. Este cambio ilustra cómo la **ocultación de información** permite evolucionar la implementación sin afectar al código cliente que ya usa la clase.

Se respetan las mismas validaciones, la actualización de máximos globales y el acceso a los campos privados de **otra instancia de la misma clase** dentro de `calcularDistanciaAPunto`. La referencia al array es `final` para dejar claro que no se reasigna (solo cambian sus contenidos), y no se expone en ningún punto de la interfaz pública.

```java
public final class Punto {
    // ===== Representación interna cambiada: array de 2 posiciones =====
    private final double[] coords; // coords[0] = x, coords[1] = y

    // ===== Miembros de clase (compartidos) =====
    private static double maxXGlobal = Double.NEGATIVE_INFINITY;
    private static double maxYGlobal = Double.NEGATIVE_INFINITY;

    private static synchronized void actualizarMaximos(double x, double y) {
        if (x > maxXGlobal) maxXGlobal = x;
        if (y > maxYGlobal) maxYGlobal = y;
    }

    // ===== Constructor (interfaz pública sin cambios) =====
    public Punto(double x, double y) {
        validar(x, "x");
        validar(y, "y");
        this.coords = new double[] { x, y };
        actualizarMaximos(coords[0], coords[1]);
    }

    // ===== Getters/Setters (interfaz pública sin cambios) =====
    public double getX() { return coords[0]; }
    public double getY() { return coords[1]; }

    public void setX(double x) {
        validar(x, "x");
        coords[0] = x;
        actualizarMaximos(coords[0], coords[1]);
    }

    public void setY(double y) {
        validar(y, "y");
        coords[1] = y;
        actualizarMaximos(coords[0], coords[1]);
    }

    // ===== Comportamiento de instancia =====
    public double calcularDistanciaAOrigen() {
        return Math.hypot(coords[0], coords[1]);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto 'otro' no puede ser null");
        }
        // Acceso permitido: miembros privados de otra instancia de la misma clase
        double dx = this.coords[0] - otro.coords[0];
        double dy = this.coords[1] - otro.coords[1];
        return Math.hypot(dx, dy);
    }

    // ===== Interfaz pública para los máximos globales =====
    public static double getMaxXGlobal() { return maxXGlobal; }
    public static double getMaxYGlobal() { return maxYGlobal; }

    // ===== Método factoría (sin cambios) =====
    public static Punto crearRedondeado(double x, double y) {
        double rx = (double) Math.round(x);
        double ry = (double) Math.round(y);
        return new Punto(rx, ry);
    }

    // ===== Utilidades internas =====
    private static void validar(double v, String nombre) {
        if (Double.isNaN(v) || Double.isInfinite(v)) {
            throw new IllegalArgumentException(nombre + " no puede ser NaN ni infinito");
        }
    }

    @Override
    public String toString() {
        return "Punto(" + coords[0] + ", " + coords[1] + ")";
    }
}
```

Con este enfoque, cualquier cliente que ya dependiera de la **interfaz pública** puede seguir compilando y funcionando sin cambios. La clase conserva sus invariantes mediante validación centralizada y controla su estado interno a través de métodos, demostrando la utilidad práctica de la encapsulación: el **qué** expone la clase permanece estable, mientras que el **cómo** se implementa puede evolucionar libremente.



## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

En POO, incluso cuando un atributo va a tener *getter* y *setter* públicos, **no es mejor declararlo público**. Si un atributo fuese público, cualquier parte del programa podría modificarlo directamente, sin pasar por ningún control. En cambio, mediante getters y setters es posible validar valores, mantener restricciones internas y reaccionar a los cambios (por ejemplo, actualizar otros atributos dependientes). Esto preserva la integridad del objeto y permite modificar la implementación interna sin afectar al código cliente que usa la clase.

La **convención más habitual** —tanto en Java como en la mayoría de lenguajes orientados a objetos— es declarar los **atributos como `private`**. Es, de hecho, una regla de estilo clásica: *“los campos siempre deben ser privados”*. De esa manera, la clase controla completamente el acceso a su estado. Los getters y setters públicos forman parte de la interfaz, pero la representación interna sigue oculta. Incluso cuando un atributo pueda parecer trivial ahora, mantenerlo privado permite modificarlo mañana sin romper compatibilidad.

Esto tiene una relación directa con las **invariantes de clase**. Las invariantes son condiciones que deben cumplirse siempre para que el objeto sea válido, y sólo pueden garantizarse si los cambios pasan por puntos de control —como setters o métodos específicos— donde puede verificarse que el nuevo estado cumple las restricciones. Si los atributos fueran públicos, cualquiera podría asignar valores que violaran dichas invariantes (como números negativos, valores no permitidos o incoherencias entre atributos), dejando al objeto en un estado inválido.

En conjunto, declarar los atributos como privados, incluso cuando existan getters y setters, es una práctica fundamental para mantener encapsulación, proteger invariantes y permitir que la clase evolucione sin romper su contrato público.



## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

En POO, una clase **inmutable** es aquella cuyo estado **no puede cambiar** después de haberse creado. Todos sus atributos quedan fijados en el constructor y no existen métodos que modifiquen ese estado. Esto implica que no hay setters y que cualquier intento de “cambiar” el objeto produce uno nuevo. Para lograrlo, sus atributos suelen ser `private` y `final`, y la propia clase no expone ninguna vía para alterar su contenido. La inmutabilidad es muy común en tipos como `String` en Java, donde los datos no cambian una vez construido el objeto.

Se llama **método modificador** a cualquier método que **altera el estado interno** del objeto. Este tipo de métodos cambia los valores de sus atributos, ya sea directa o indirectamente. Un **setter** es un caso particular de método modificador, pero **no todo método modificador es un setter**. Por ejemplo, un método como `incrementarX(double cantidad)` o `mover(double dx, double dy)` también modifica el estado, aunque no se llame `setX`. En general, cualquier método que altere los atributos —sea directamente o a través de operaciones— es un modificador.

Una clase inmutable tiene varias **ventajas** importantes. En primer lugar, simplifica el razonamiento sobre el programa: si un objeto no puede cambiar, no hay riesgo de que otra parte del código (u otro hilo) lo altere inesperadamente, facilitando evitar errores y mantener invariantes de forma automática. Además, las clases inmutables son intrínsecamente **seguras para la concurrencia**, ya que distintos hilos pueden compartirlas sin necesidad de sincronización. También facilitan la depuración, ya que su comportamiento es más predecible, y permiten optimizaciones internas como el *caching* o la reutilización segura de instancias.

En conjunto, la inmutabilidad reduce el riesgo de estados inconsistentes, elimina muchos problemas de sincronización y hace que la lógica de un programa sea más robusta y fácil de mantener. Por ello, aunque no siempre es apropiada, resulta muy útil en modelos matemáticos, valores simples, objetos compartidos y programación funcional integrada en lenguajes orientados a objetos.



## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No, **no es recomendable incluir métodos *setter* siempre ni como convención general**. En la mayoría de los diseños orientados a objetos, añadir setters de forma automática suele ser una mala práctica, precisamente porque contradice la filosofía de la **encapsulación**: si todo atributo tiene su setter público, la clase deja de controlar su propio estado y las invariantes pueden romperse fácilmente.

En lugar de añadir setters sin pensarlo, lo habitual es preguntarse **si el atributo debe poder cambiar** y **quién debería tener permiso para cambiarlo**. En muchos casos, no hay razón para modificar ciertos valores después de crear el objeto, lo cual sugiere que **no debería existir un setter**. En otros casos, puede ser preferible proporcionar métodos más específicos (como `mover(dx, dy)`), que expresan mejor la intención y permiten validar cambios de forma coherente con la lógica interna del objeto, en lugar de exponer setters genéricos que permitan valores arbitrarios.

Además, la ausencia de setters facilita crear **clases inmutables**, que ofrecen múltiples ventajas: simplicidad conceptual, seguridad frente a errores, facilidad de uso en entornos concurrentes y garantía automática de invariantes. Incluso cuando la clase no sea totalmente inmutable, restringir los setters o reemplazarlos por métodos bien diseñados mejora la robustez del sistema.

En resumen, los setters **no deben añadirse por convención**, sino **solo cuando sean necesarios** y encajen con el diseño de la clase. La encapsulación, las invariantes y la claridad de intención deben guiar la decisión, no una regla automática.



## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase **`String` en Java es inmutable**. Esto significa que, una vez creada una cadena, su contenido **no puede modificarse**. Cualquier operación que parezca alterar una cadena —como concatenar, reemplazar o convertir mayúsculas/minúsculas— en realidad **crea un nuevo objeto `String`** y deja intacto el original. Esta inmutabilidad contribuye a la seguridad, a la facilidad de razonamiento y a que `String` pueda usarse de forma segura en entornos concurrentes.

Cuando se **concatenan dos cadenas**, por ejemplo con `s1 + s2`, Java crea internamente **un nuevo objeto `String`** que contiene el resultado, mientras que `s1` y `s2` se mantienen sin cambios. Esta operación es relativamente costosa si se realiza repetidas veces, porque implica crear muchos objetos intermedios y copiar caracteres repetidamente. Por ello, concatenar dentro de un bucle puede volverse muy ineficiente.

Si se necesita **construir una cadena muy larga paso a paso**, lo recomendable es usar **`StringBuilder`** (o `StringBuffer` si se necesita sincronización). `StringBuilder` es **mutable**, lo que permite ir añadiendo fragmentos en memoria sin crear objetos adicionales innecesarios. Al finalizar, se puede convertir el resultado a un `String` mediante `.toString()`. Esta práctica evita costes de tiempo y memoria cuando las concatenaciones son numerosas.

En conjunto, la inmutabilidad de `String` es una ventaja en muchos contextos, pero para operaciones intensivas de construcción de texto conviene utilizar estructuras mutables como `StringBuilder`, que están precisamente diseñadas para ese propósito.



## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En POO, los objetos pueden compararse **por identidad** o **por contenido**, pero ambas comparaciones representan conceptos distintos. La **identidad** indica si dos referencias apuntan al **mismo objeto en memoria**, mientras que el **contenido** indica si los datos internos de ambos objetos son equivalentes aunque sean objetos distintos. En Java, estas dos formas se distinguen claramente: el operador `==` compara **identidad**, mientras que el método `equals` compara **contenido** (si está bien redefinido).

El **método `equals`** en Java forma parte de la clase base `Object`. Su propósito es permitir comparar si **dos objetos “son iguales” según la semántica de su clase**. Sin embargo, **por defecto**, la implementación heredada de `Object` compara **identidad**, es decir, actúa como `==`. Para que la comparación sea por contenido, cada clase debe **sobrescribir** `equals` y definir qué significa la igualdad para ella (por ejemplo, que dos puntos tengan las mismas coordenadas o que dos fechas representen el mismo instante).

En el caso concreto de las **cadenas en Java**, no se deben comparar mediante `==`, porque este operador solo indica si dos referencias apuntan al mismo objeto. Aunque Java hace *interning* de literales, este comportamiento no es universal ni seguro para comparar contenido. La forma correcta de comparar cadenas es mediante **`s1.equals(s2)`**, que sí compara carácter a carácter el contenido de las cadenas. También existen variantes como `equalsIgnoreCase` cuando se desea ignorar mayúsculas/minúsculas.

En resumen:

*   `==` → compara **identidad**, si son el mismo objeto.
*   `equals` → compara **contenido**, si está bien redefinido.
*   Por defecto, `equals` usa la identidad (como `==`).
*   Las cadenas en Java deben compararse con **`.equals()`**, nunca con `==` si interesa su contenido.



## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

En los lenguajes orientados a objetos, las **clases *wrapper*** (o *envoltorio*) son clases cuyo propósito es **representar valores de tipos primitivos como objetos**. En Java, por ejemplo, los tipos primitivos (`int`, `double`, `boolean`, etc.) no son objetos, así que existen clases específicas —como `Integer`, `Double`, `Boolean`— que permiten tratarlos como tales. Estas clases envuelven el valor primitivo y lo proporcionan dentro de un objeto, permitiendo usarlo en estructuras o contextos que solo aceptan objetos.

En Java, este proceso puede realizarse tanto de forma **manual** como de forma **automática**. Manualmente, se crea un objeto wrapper de la forma `Integer i = new Integer(5);` (aunque está desaconsejado desde Java 9) o usando métodos como `Integer.valueOf(5)`. Automáticamente, Java realiza lo que se conoce como **autoboxing** y **unboxing**: convierte un valor primitivo en su wrapper correspondiente cuando el contexto lo requiere, y viceversa, sin que el programador tenga que escribir código explícito. Por ejemplo, en `Integer x = 5;`, el `5` se convierte automáticamente en un `Integer`.

Las clases wrapper tienen varias **ventajas**. Permiten almacenar valores primitivos en colecciones como `ArrayList`, porque estas solo aceptan objetos. También proporcionan métodos útiles —como conversiones, comparaciones o operaciones auxiliares— que los tipos primitivos no tienen. Además, facilitan trabajar con APIs más abstractas, ya que muchos marcos y librerías necesitan objetos para funcionar correctamente. Esta capacidad de convertir tipos simples en objetos útiles mejora la flexibilidad del lenguaje sin renunciar a la eficiencia de los primitivos.

No todos los lenguajes orientados a objetos tienen tipos primitivos como Java, y por tanto **no todos necesitan wrappers**. Lenguajes como Python o Ruby consideran prácticamente todo como objeto, incluidos números y booleanos, por lo que no requieren clases especiales para envolver valores básicos. Otros, como C++ o Kotlin, proporcionan envoltorios más integrados o realizan estas conversiones de forma transparente. En general, los wrappers son necesarios en lenguajes que, como Java, distinguen entre valores primitivos y objetos por razones de eficiencia y diseño.



## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

En POO, un **tipo de dato enumerado** (o *enum*) es un tipo cuyos valores posibles están **predefinidos y limitados**. Representa un conjunto cerrado de constantes con significado semántico, como días de la semana, estados de una máquina o tipos de dirección. Usar enumerados evita emplear números o cadenas “mágicas”, dando más claridad y seguridad al código porque solo se aceptan valores válidos del conjunto.

En Java, un enumerado **sí es una clase**, aunque con una sintaxis especial. Cada valor del `enum` es en realidad una **instancia única** y estática de esa clase. Esto significa que un `enum` puede tener **campos privados, métodos, constructores privados e incluso implementar interfaces**. De hecho, Java genera automáticamente varios métodos útiles (`values()`, `valueOf()`) y garantiza que cada constante sea un objeto singleton. Por ello, los enumerados en Java son mucho más potentes que los simples enteros simbólicos de lenguajes como C.

En términos de **encapsulación**, los enumerados en Java ofrecen ventajas importantes. En primer lugar, permiten **ocultar la representación interna** de cada constante: se puede añadir información adicional o lógica interna sin que el código externo se vea afectado. También ayudan a mantener **invariantes**, ya que es imposible crear nuevos valores fuera del conjunto permitido; el constructor del enum es siempre privado, lo que evita usos indebidos. Además, pueden encapsular **comportamientos específicos** asociados a cada valor, manteniendo los detalles internos protegidos y exponiendo únicamente la interfaz pública deseada.

En conjunto, los enumerados en Java combinan la seguridad y claridad de un conjunto limitado de valores con la potencia de una clase totalmente encapsulable, haciendo el código más robusto, legible y fácil de mantener.



## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

A continuación se define un **tipo enumerado** `Mes` con las doce constantes. Cada constante encapsula su **ordinal 1–12** y el **número de días en año no bisiesto**; se ofrece un método para obtener los días en un **año concreto** (considerando bisiestos) y otro para consultar el **ordinal**. Además, se implementan los cuatro métodos de estación que devuelven `true` si **ese mes contiene algunos días** de la estación indicada, teniendo en cuenta el **hemisferio** (en el hemisferio norte se incluyen los meses fronterizos: mar/jun/sep/dic, porque contienen días de dos estaciones).

Se usa un **constructor privado** (propio de los `enum` en Java), atributos privados y métodos públicos como interfaz. La lógica de año bisiesto sigue la regla gregoriana habitual (divisible por 4, salvo excluidos por 100, excepto si también divisible por 400). La implementación de estaciones en el hemisferio sur invierte las del norte.

```java
public enum Mes {
    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    // --- Estado encapsulado ---
    private final int numeroMes;        // 1-12
    private final int diasNoBisiesto;   // días típicos (febrero=28)

    // --- Constructor privado ---
    private Mes(int numeroMes, int diasNoBisiesto) {
        this.numeroMes = numeroMes;
        this.diasNoBisiesto = diasNoBisiesto;
    }

    // --- Interfaz pública ---
    /** Ordinal del mes en el año (1-12). */
    public int ordinalMes() {
        return numeroMes;
    }

    /** Días del mes en un año concreto (considera bisiesto). */
    public int diasEn(int anio) {
        if (this == FEBRERO && esBisiesto(anio)) return 29;
        return diasNoBisiesto;
    }

    /** ¿Este mes tiene algunos días de PRIMAVERA en el hemisferio indicado? */
    public boolean esDePrimavera(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            // Norte: parte de MAR y JUN, enteros ABR-MAY
            return switch (this) {
                case MARZO, ABRIL, MAYO, JUNIO -> true;
                default -> false;
            };
        } else {
            // Sur: primavera = SEP, OCT, NOV y parte de DIC
            return switch (this) {
                case SEPTIEMBRE, OCTUBRE, NOVIEMBRE, DICIEMBRE -> true;
                default -> false;
            };
        }
    }

    /** ¿Este mes tiene algunos días de VERANO en el hemisferio indicado? */
    public boolean esDeVerano(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            // Norte: parte de JUN y SEP, enteros JUL-AGO
            return switch (this) {
                case JUNIO, JULIO, AGOSTO, SEPTIEMBRE -> true;
                default -> false;
            };
        } else {
            // Sur: verano = DIC, ENE, FEB y parte de MAR
            return switch (this) {
                case DICIEMBRE, ENERO, FEBRERO, MARZO -> true;
                default -> false;
            };
        }
    }

    /** ¿Este mes tiene algunos días de OTOÑO en el hemisferio indicado? */
    public boolean esDeOtoño(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            // Norte: parte de SEP y DIC, enteros OCT-NOV
            return switch (this) {
                case SEPTIEMBRE, OCTUBRE, NOVIEMBRE, DICIEMBRE -> true;
                default -> false;
            };
        } else {
            // Sur: otoño = MAR, ABR, MAY y parte de JUN
            return switch (this) {
                case MARZO, ABRIL, MAYO, JUNIO -> true;
                default -> false;
            };
        }
    }

    /** ¿Este mes tiene algunos días de INVIERNO en el hemisferio indicado? */
    public boolean esDeInvierno(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            // Norte: DIC, ENE, FEB y parte de MAR
            return switch (this) {
                case DICIEMBRE, ENERO, FEBRERO, MARZO -> true;
                default -> false;
            };
        } else {
            // Sur: JUN, JUL, AGO y parte de SEP
            return switch (this) {
                case JUNIO, JULIO, AGOSTO, SEPTIEMBRE -> true;
                default -> false;
            };
        }
    }

    // --- Utilidad interna ---
    private static boolean esBisiesto(int anio) {
        return (anio % 4 == 0) && (anio % 100 != 0 || anio % 400 == 0);
    }
}
```

> Nota: se ha interpretado “algunos días” de cada estación para **incluir** los meses de cambio estacional (marzo/junio/septiembre/diciembre) en **ambas** estaciones afectadas del hemisferio correspondiente. Si se prefiriese una asignación por **meses completos**, bastaría con ajustar los `switch`.

