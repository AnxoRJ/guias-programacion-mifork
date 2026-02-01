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

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una.

###

## **1. Abstracción**

La abstracción consiste en identificar los elementos esenciales de un problema y representarlos mediante clases que modelan conceptos del mundo real. Se centra en mostrar únicamente la información necesaria, ocultando los detalles internos que no son relevantes para el uso del objeto. En Java, la abstracción se logra definiendo clases con atributos y métodos que representan comportamientos de alto nivel.

Además, permite que el código sea más claro y manejable, porque todas las operaciones complejas quedan encapsuladas dentro de la clase. Así, quien utiliza un objeto no necesita conocer cómo está implementado internamente, sino solo qué puede hacer.

***

## **2. Encapsulación**

La encapsulación se refiere a agrupar datos (atributos) y comportamientos (métodos) dentro de una misma unidad: la clase. Además, protege la información al controlar qué partes del código pueden acceder o modificar dichos datos mediante modificadores de acceso como `private`, `protected` o `public`. Esto evita manipulación indebida y reduce errores.

Este mecanismo ofrece un mayor control sobre el estado del objeto, pues obliga a interactuar con él a través de métodos bien definidos, como getters y setters. Con ello se garantiza que cualquier cambio en el estado interno siga reglas coherentes con el diseño de la clase.

***

## **3. Herencia**

La herencia permite crear nuevas clases basadas en clases existentes, reutilizando sus atributos y métodos. Esto favorece la extensión del comportamiento sin reescribir código, estableciendo relaciones del tipo "es un" (por ejemplo, un `Perro` es un `Animal`). En Java, la herencia se implementa con la palabra clave `extends`.

Gracias a la herencia, es posible construir jerarquías de clases que comparten características comunes, lo que facilita la organización del código y reduce duplicidades. Además, promueve la creación de estructuras más genéricas y flexibles dentro de una aplicación.

***

## **4. Polimorfismo**

El polimorfismo permite que un mismo método pueda comportarse de distintas maneras según el objeto que lo invoque. Esto hace posible escribir código más genérico que trabaje con clases relacionadas sin necesidad de conocer cuál será la implementación exacta que se ejecutará en tiempo de ejecución. En Java, el polimorfismo se manifiesta principalmente mediante la sobrescritura de métodos.

Este principio favorece la extensibilidad del software, ya que permite introducir nuevas clases o comportamientos sin modificar el código que ya funciona. En aplicaciones complejas, esto simplifica la integración de nuevos módulos y promueve diseños más limpios y modulares.



## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### 

1.  **Java**  
    Es uno de los lenguajes orientados a objetos más populares y fue diseñado desde el inicio con este paradigma en mente. Su modelo de clases y objetos es muy consistente y se usa de forma intensiva en aplicaciones empresariales, móviles (Android) y sistemas distribuidos.

2.  **C++**  
    Aunque permite programación estructurada, también incorpora un modelo completo de orientación a objetos, incluyendo clases, herencia múltiple, polimorfismo y encapsulación. Su flexibilidad lo hace adecuado para desarrollo de software de alto rendimiento y sistemas.

3.  **Python**  
    Es un lenguaje multiparadigma, pero ofrece un soporte sencillo y potente para la programación orientada a objetos. Permite definir clases de forma muy intuitiva y aprovechar la herencia, la sobrescritura y el polimorfismo sin complejidad adicional.

4.  **C#**  
    Diseñado por Microsoft, es un lenguaje fuertemente orientado a objetos y muy similar a Java en su estructura. Se utiliza en desarrollo de aplicaciones de escritorio, servicios web, videojuegos (con Unity) y aplicaciones empresariales.




## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### 

## **¿Qué es la programación estructurada?**

La programación estructurada es un paradigma que organiza el código siguiendo un conjunto limitado de estructuras de control básicas: **secuencia**, **selección** (`if`, `switch`) y **repetición** (`for`, `while`). La idea central es evitar el uso de saltos incontrolados como `goto`, que dificultan la lectura y el mantenimiento del programa. Esto promueve un flujo lógico más claro y predecible, facilitando tanto el análisis como la depuración.

Además, la programación estructurada fomenta dividir un programa en pequeñas tareas que se ejecutan de forma lineal y ordenada. El desarrollo se realiza siguiendo un esquema “de arriba hacia abajo” (top‑down), lo que permite comprender primero la lógica general y luego implementar los detalles. Este enfoque facilita que el programa sea más legible, más fácil de probar y menos propenso a errores que las aproximaciones no estructuradas.

***

## **¿Qué es la programación modular?**

La programación modular puede considerarse una evolución natural de la programación estructurada, ya que busca dividir un programa en **módulos independientes**, cada uno encargado de una funcionalidad bien definida. Un módulo actúa como una unidad lógica de código, normalmente implementada como un archivo o biblioteca, que ofrece un conjunto de funciones relacionadas entre sí. Esta separación ayuda a organizar programas grandes y a reducir la complejidad global.

Este enfoque permite trabajar de manera más eficiente, ya que cada módulo puede desarrollarse, probarse y mantenerse por separado. También promueve la reutilización: si un módulo resuelve un problema de forma general, puede utilizarse en distintos programas sin reescribirlo. En lenguajes como C, esta modularidad se implementa mediante archivos `.c` y `.h`, lo que permite controlar la visibilidad de funciones y datos. En cierto modo, este concepto anticipa las ideas que más tarde formaliza la orientación a objetos, como encapsulación y separación clara de responsabilidades.





## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### 

***

## **1. Estado**

El estado de un objeto está formado por sus **atributos**, es decir, las variables que almacenan la información que caracteriza a ese objeto. Cada objeto posee su propio conjunto de valores para esos atributos, lo que permite diferenciarlo de otros objetos pertenecientes a la misma clase. Por ejemplo, dos objetos de la clase `Coche` pueden tener distintos colores o distintas velocidades, y eso forma parte de su estado individual.

Este concepto resulta clave porque permite que un mismo diseño (la clase) produzca múltiples instancias con datos particulares. El estado evoluciona a lo largo del tiempo en respuesta a operaciones realizadas sobre el objeto, lo que ofrece un modelo más cercano a cómo se comportan las entidades del mundo real.

***

## **2. Comportamiento**

El comportamiento de un objeto está definido por sus **métodos**, que especifican las acciones que puede realizar o las operaciones que pueden ejecutarse sobre él. Estos métodos permiten modificar el estado interno o interactuar con otros objetos, proporcionando una interfaz bien definida para usar la instancia sin necesidad de conocer su implementación interna.

Además, el comportamiento establece la lógica asociada a un objeto, determinando cómo responde ante determinadas solicitudes. Gracias a este mecanismo, los objetos pueden colaborar entre sí para resolver problemas complejos, manteniendo al mismo tiempo una estructura organizada y coherente dentro del programa.

***

## **3. Identidad**

La identidad es lo que permite distinguir a cada objeto de todos los demás, incluso si comparten exactamente el mismo estado y el mismo comportamiento. En lenguajes como Java, esta identidad se refleja en que cada objeto reside en una posición de memoria única, de modo que dos objetos diferentes nunca son considerados el mismo aunque tengan atributos idénticos.

Este concepto es esencial para gestionar correctamente referencias, comparaciones y ciclos de vida de las instancias dentro de una aplicación. Sin identidad, no sería posible modelar adecuadamente entidades independientes ni controlar la interacción precisa entre los distintos objetos de un programa.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### 

***

## **¿Qué es una clase?**

Una clase puede entenderse como un **modelo o plantilla** que describe las características y comportamientos que tendrán los objetos creados a partir de ella. Define qué atributos existirán y qué métodos estarán disponibles, pero no contiene valores concretos por sí misma. De manera similar a un `struct` avanzado en C, una clase agrupa datos y funciones, pero además incorpora mecanismos de encapsulación, herencia y polimorfismo.

En esencia, la clase actúa como un plano de construcción. No representa un elemento del programa en ejecución por sí sola, sino que sirve para crear entidades reales llamadas objetos. Gracias a este diseño, múltiples objetos pueden generarse a partir de la misma clase, cada uno con su propio estado independiente.

***

## **¿Es lo mismo una clase que un objeto?**

Una clase **no es** un objeto; son conceptos relacionados pero distintos. La clase es la definición, el diseño, la estructura conceptual. En cambio, un objeto es una **entidad concreta** creada en memoria a partir de esa clase. Mientras la clase existe de forma abstracta, los objetos existen de forma tangible durante la ejecución del programa.

Una forma clásica de visualizar la diferencia es pensar en la clase como el plano de una casa, y el objeto como la casa construida. Puede haber muchas casas distintas construidas con el mismo plano, del mismo modo que puede haber muchos objetos creados a partir de la misma clase.

***

## **¿Qué es una instancia?**

Una instancia es simplemente un **objeto particular** creado a partir de una clase. Cada instancia tiene sus propios valores para los atributos definidos por la clase, lo que permite que dos instancias iguales conceptualmente sean diferentes en la práctica. Por ejemplo, dos instancias de la clase `Persona` pueden compartir el mismo diseño, pero tener nombres y edades distintos.

El proceso de crear una instancia se conoce como *instanciación*, y es en ese momento cuando el programa reserva memoria, inicializa sus atributos y prepara el objeto para ser utilizado. Por tanto, “objeto” e “instancia” suelen usarse como sinónimos, aunque técnicamente instancia enfatiza que proviene de una clase concreta.

***

## **¿Todos los lenguajes orientados a objetos manejan el concepto de clase?**

No todos los lenguajes orientados a objetos utilizan el concepto de clase. Muchos lenguajes sí lo hacen, como Java, C++, C# o Swift, porque se basan en un modelo **clásico** de orientación a objetos en el que la clase es el elemento fundamental. Sin embargo, existen lenguajes que implementan orientación a objetos sin clases, sino mediante **prototipos**, como JavaScript.

En los lenguajes orientados a prototipos, los objetos se crean a partir de otros objetos en lugar de provenir de una clase. Esto permite una mayor flexibilidad, aunque también un modelo conceptual diferente al que ofrecen los lenguajes de clases. Por ello, aunque la mayoría de lenguajes populares utilizan clases, no es un requisito absoluto para implementar el paradigma de objetos.



## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### 

***

## **¿Dónde se almacenan en memoria los objetos?**

En la mayoría de lenguajes orientados a objetos modernos, los objetos se almacenan en una zona de memoria llamada **heap**. Esta área está destinada a la creación dinámica de datos cuyo tamaño o vida útil no se conoce en tiempo de compilación. En Java, por ejemplo, cada vez que se utiliza la palabra clave `new`, el objeto resultante se coloca en el heap y lo que se maneja en el código es una **referencia** a esa ubicación. Esto permite que los objetos vivan más allá del ámbito de un método, siempre que existan referencias apuntando hacia ellos.

En contraste, en C++ con orientación a objetos, un objeto puede almacenarse tanto en el **heap** (si se crea con `new`) como en la **pila** (*stack*) si se declara como variable local normal. Esto significa que su posición en memoria y su ciclo de vida dependen de cómo se cree el objeto. En lenguajes como Java o C#, sin embargo, los objetos siempre residen en el heap, mientras que la pila solo almacena referencias.

***

## **¿Es igual en todos los lenguajes?**

No, no todos los lenguajes almacenan los objetos de la misma manera. Lenguajes como Java y C# siguen un modelo uniforme donde todas las instancias de clases viven en el heap. Python también coloca sus objetos en el heap, aunque internamente maneja un sistema propio de administración de memoria. C++, por su parte, ofrece flexibilidad total al permitir objetos en el stack, en el heap o incluso en memoria estática, dependiendo de cómo se definan.

Esta diversidad muestra que la ubicación de los objetos depende del **modelo de memoria del lenguaje** y de si este cuenta con **gestión automática de memoria**. Cuanto más control tiene el programador (como en C++), más variabilidad hay sobre dónde se guardan los objetos. Cuanto más automático sea el lenguaje (como Java), más uniforme es el modelo y menos responsabilidad recae en el desarrollador.

***

## **¿Qué es la recolección de basura (garbage collection)?**

La **recolección de basura** es un mecanismo automático utilizado por lenguajes como Java, C# o Python para liberar memoria que ya no está siendo utilizada por el programa. Cuando un objeto deja de estar referenciado —es decir, ya no hay ninguna variable que apunte a él—, el recolector de basura identifica que ese objeto ya no es accesible y lo elimina de la memoria para evitar fugas. Este proceso evita que el programador tenga que liberar memoria manualmente, reduciendo errores como *memory leaks* o accesos a memoria liberada.

Este mecanismo implica que la gestión de memoria es más segura, pero también introduce un coste: el recolector se ejecuta periódicamente y puede afectar al rendimiento en ciertos momentos. Aun así, para la mayoría de aplicaciones, el equilibrio entre seguridad y eficiencia es favorable. En contraste, lenguajes como C o C++ no utilizan recolección de basura y dependen por completo de que el programador gestione la memoria correctamente.



## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### 

***

## **¿Qué es un método?**

Un método es una **función asociada a una clase** que describe una acción o comportamiento que los objetos de dicha clase pueden realizar. A diferencia de las funciones sueltas de lenguajes como C, los métodos operan sobre datos propios del objeto, teniendo acceso directo a sus atributos. Esto permite que el comportamiento esté estrechamente ligado al estado de cada instancia, creando un modelo coherente y encapsulado.

Además del acceso al estado, los métodos definen la forma en que otros componentes del programa interactúan con un objeto. En Java, la invocación de un método implica enviar una solicitud al objeto para que ejecute una operación concreta. De este modo, los métodos actúan como la interfaz pública que controla cómo se usa un objeto, manteniendo ocultos los detalles internos de su implementación.

***

## **¿Qué es la sobrecarga de métodos?**

La **sobrecarga de métodos** consiste en definir varios métodos con el **mismo nombre**, pero con **diferentes parámetros** (ya sea en número, tipo o ambos). Este mecanismo permite que un mismo comportamiento conceptual pueda aplicarse en distintas situaciones, adaptándose a diferentes tipos de datos o combinaciones de argumentos. A nivel del lenguaje, Java determina cuál de los métodos sobrecargados debe ejecutarse en función de la lista de parámetros proporcionada en la llamada.

Este concepto no debe confundirse con la sobrescritura (*override*), que ocurre en herencia. La sobrecarga trabaja dentro de la misma clase y se resuelve en **tiempo de compilación**, lo que la convierte en una forma de polimorfismo estático. Gracias a este recurso, los programas pueden ser más expresivos, ofrecer múltiples variantes de una misma operación y mantener nombres consistentes para acciones similares.



## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### 

```java
// Archivo: Punto.java
class Punto {
    // Atributos con visibilidad por defecto (package-private)
    double x;
    double y;

    // Constructor sencillo
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método que calcula la distancia al origen (0,0)
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

A continuación, se incluye un **ejemplo de uso** creando una instancia de `Punto` y llamando al método `calculaDistanciaAOrigen`. Puede compilarse en el mismo paquete (mismo directorio, sin declarar paquetes) para respetar la visibilidad por defecto de los atributos.

```java
// Archivo: EjemploUso.java
public class EjemploUso {
    public static void main(String[] args) {
        Punto p = new Punto(3.0, 4.0);
        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + distancia); // Imprime 5.0
    }
}
```

Este diseño mantiene el código simple y directo para centrarse en el concepto de **clase**, **atributos** y **método**. En contextos reales, sería habitual hacer los atributos `private` y exponer métodos de acceso (*getters/setters*) para practicar **encapsulación**, pero aquí se ha seguido la indicación de visibilidad por defecto para mayor sencillez.



## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### 

***

## **¿Cuál es el punto de entrada en un programa en Java?**

El punto de entrada de una aplicación Java estándar es el método:

```java
public static void main(String[] args)
```

Este método debe ser **`public`** (accesible desde la JVM), **`static`** (no requiere instancia para ser invocado), **`void`** (no devuelve valor) y recibir un **arreglo de `String`** como parámetro (`String[] args`). También se admite la forma con **varargs**: `public static void main(String... args)`.

Es importante notar que **pueden existir múltiples métodos `main`** sobrecargados en una clase, pero **solo** el que cumple exactamente la firma esperada por la JVM se utilizará como **punto de entrada**. Asimismo, pueden coexistir varias clases con `main` dentro del mismo proyecto; en ese caso, la clase concreta que se ejecuta se elige en la configuración de ejecución (o con `java NombreDeClase`).

***

## **¿Qué es `static` y para qué vale?**

`static` indica que un **miembro pertenece a la clase**, no a las instancias. Un **método `static`** puede llamarse sin crear objetos, y un **campo `static`** tiene una única copia compartida por todas las instancias. Esto resulta útil para **métodos de utilidad** (por ejemplo, `Math.sqrt`), **fábricas** (*factory methods*), **contadores globales** o **configuración compartida**.

Además de métodos y campos, `static` puede aplicarse a **bloques estáticos** (se ejecutan una vez al cargar la clase) y a **clases anidadas estáticas** (anidadas que no necesitan referencia implícita a la instancia de la clase envolvente). A nivel conceptual, `static` acerca el uso a “funciones y datos globales controlados”, manteniendo el modelo orientado a objetos.

***

## **¿Sólo se emplea para ese método `main`?**

No. Aunque `static` es necesario en `main` para permitir que la JVM lo invoque **sin instancia**, su uso es mucho más amplio. Se emplea constantemente en **métodos utilitarios**, **constantes compartidas** (`public static final`), **constructores estáticos simulados** (métodos fábrica) y **miembros de clase** cuyo valor debe ser único y común a todas las instancias.

También se usa en **clases anidadas estáticas** para modelar tipos relacionados que no dependen del estado de la instancia exterior, y en **bloques estáticos** para inicialización compleja de campos estáticos. En resumen, `static` es una herramienta general para expresar pertenencia a la clase, no un recurso exclusivo del `main`.

***

## **¿Para qué se combina con `final`?**

La combinación **`static final`** se usa típicamente para declarar **constantes**:

```java
public static final double PI = 3.141592653589793;
```

*   **`static`**: hay **una única copia** asociada a la clase.
*   **`final`**: el **valor no puede reasignarse** una vez inicializado.  
    En tipos por referencia, **`final`** hace inmutable la **referencia** (no se puede cambiar a otro objeto), pero **no** garantiza que el **objeto** sea inmutable, a menos que su tipo lo sea (por diseño).

`final` también se combina con **métodos** y **clases**. Un **método `final`** no puede ser sobrescrito; si además es **`static`**, no puede **ocultarse** (*method hiding*) en subclases. Una **clase `final`** no puede heredarse, lo que resulta útil para asegurar diseños cerrados a extensión o para implementar tipos inmutables. Estas combinaciones ayudan a **proteger invariantes**, **evitar cambios indeseados** y **expresar intención de diseño** con claridad.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### 

***

# **¿Cómo se compila y ejecuta un programa Java desde la línea de comandos?**

Para ejecutar Java desde consola se necesitan dos comandos: **`javac`** (compilador) y **`java`** (ejecutor de la JVM).

### **1. Compilación con `javac`**

Supongamos que se tiene un archivo `Ejemplo.java` con un método `main`:

```bash
javac Ejemplo.java
```

Este comando **no genera un ejecutable**, sino uno o varios ficheros `.class` correspondientes a cada clase del archivo. Cada `.class` contiene *bytecode*, el formato intermedio que entiende la máquina virtual.

### **2. Ejecución con `java`**

Una vez compilado, se ejecuta así:

```bash
java Ejemplo
```

Es importante **no incluir la extensión `.class`** en este comando. Solo se especifica el **nombre de la clase que contiene el método `main`**.

Si el programa está dentro de un paquete, hay que usar la ruta del paquete, por ejemplo:

```bash
java com.miapp.pruebas.Ejemplo
```

***

# **¿Java es un lenguaje compilado?**

Java es **compilado y también interpretado**, dependiendo de qué fase se observe.

*   Es **compilado** porque `javac` traduce el código fuente `.java` a *bytecode* `.class`.
*   Es **interpretado/ejecutado** porque ese bytecode no se ejecuta directamente, sino que lo interpreta o compila en tiempo de ejecución la **máquina virtual de Java (JVM)**.

Por eso se suele decir que Java es un lenguaje de **compilación híbrida**.  
La ventaja es que un mismo programa compilado (.class) puede ejecutarse en cualquier sistema operativo que tenga una máquina virtual compatible (Windows, Linux, macOS…).

***

# **¿Qué es la máquina virtual de Java (JVM)?**

La **JVM** es un programa que actúa como “ordenador virtual” dentro del ordenador real.  
Su función es:

*   Cargar los ficheros `.class`.
*   Interpretar o compilar dinámicamente el *bytecode*.
*   Gestionar memoria (incluyendo el recolector de basura).
*   Asegurar seguridad, aislamiento y portabilidad.

Gracias a la JVM, un mismo programa Java puede ejecutarse en múltiples plataformas sin recompilarse. Esto se resume en el famoso lema:

> *“Write once, run anywhere.”*

***

# **¿Qué es el *bytecode* y qué son los ficheros `.class`?**

El **bytecode** es un **código intermedio** que no es comprensible por el procesador físico (como el de un `.exe`), pero sí por la JVM.  
Tiene varias características:

*   Es compacto y eficiente de interpretar.
*   Es independiente del sistema operativo.
*   Permite optimizaciones en tiempo de ejecución (JIT: *Just‑In‑Time compilation*).

Los ficheros **`.class`** contienen precisamente ese bytecode.  
Después de compilar con `javac`, cada clase genera su archivo `.class`. Incluso si un archivo `.java` contiene varias clases internas, pueden aparecer varios `.class` asociados.





## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### 

***

## **¿Qué es `new`?**

`new` es el operador en Java que **reserva memoria en el heap** y **crea una instancia** (un objeto) de una clase. Al utilizarlo, se invoca internamente un **constructor** de esa clase, que inicializa el estado del nuevo objeto. El resultado de `new` es una **referencia** que apunta al objeto en memoria, y esa referencia se almacena en una variable del tipo de la clase o de un tipo compatible.

Este operador es esencial para trabajar con objetos porque, a diferencia de tipos primitivos, las clases requieren esa creación explícita (salvo casos especiales como *interning* de `String` que no cambia el uso básico). Sin `new`, solo existiría una referencia nula (`null`), incapaz de invocar métodos o acceder a atributos sin provocar un `NullPointerException`.

***

## **¿Qué es un constructor?**

Un **constructor** es un **método especial** de una clase que **no tiene tipo de retorno** y **se llama automáticamente** al crear un objeto con `new`. Su propósito es **inicializar los atributos** del objeto y dejarlo listo para su uso, garantizando que comience su vida con un estado válido. Puede haber **varios constructores** en la misma clase (sobrecarga), con diferentes listas de parámetros para cubrir necesidades de inicialización distintas.

Si no se declara ningún constructor, Java **genera uno por defecto** sin parámetros (constructor por defecto) que no realiza inicialización adicional más allá de los valores por defecto del lenguaje. En cuanto se define **cualquier** constructor, el constructor por defecto **deja de generarse** automáticamente, por lo que, si se necesita, debe declararse explícitamente.

***

## **Ejemplo: clase `Empleado` con constructor**

A continuación, se muestra una clase `Empleado` mínima con tres atributos (`dni`, `nombre`, `apellidos`) y un **constructor** que los recibe como parámetros e inicializa el objeto. Se emplea **visibilidad por defecto** en los atributos para mantener la sencillez del ejemplo (en código real se recomienda `private` y el uso de encapsulación).

```java
// Archivo: Empleado.java
class Empleado {
    // Atributos con visibilidad por defecto (package-private)
    String dni;
    String nombre;
    String apellidos;

    // Constructor que inicializa todos los campos
    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }

    // Método de utilidad opcional para mostrar datos
    String descripcion() {
        return nombre + " " + apellidos + " (DNI: " + dni + ")";
    }
}
```

Y un ejemplo de uso creando una **instancia** con `new` e invocando un método para verificar la inicialización:

```java
// Archivo: DemoEmpleado.java
public class DemoEmpleado {
    public static void main(String[] args) {
        Empleado e = new Empleado("12345678A", "Ana", "Pérez López");
        System.out.println(e.descripcion());
        // Salida: Ana Pérez López (DNI: 12345678A)
    }
}
```

Este patrón muestra cómo `new` **crea** el objeto y cómo el **constructor** **establece** su estado inicial de manera clara y segura.



## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### 

***

## **¿Qué es la referencia `this`?**

`this` es una referencia implícita que apunta al **objeto actual** dentro del contexto de una instancia. Permite acceder a los **atributos** y **métodos** de esa instancia desde dentro de la propia clase. En términos prácticos, sirve para dejar claro que se está hablando del campo del objeto y no de una variable local o un parámetro con el mismo nombre.

Además de desambiguar nombres, `this` se utiliza para **encadenar constructores** en la misma clase (`this(...)` como primera línea de un constructor) y para **pasar la referencia del objeto actual** a otros métodos o componentes que la necesiten (por ejemplo, al registrarse como listener). En métodos `static` no puede usarse `this`, porque no hay instancia asociada.

***

## **¿Se llama igual en todos los lenguajes?**

No exactamente. La idea existe en la mayoría de lenguajes orientados a objetos, pero el **identificador varía**. En Java, C#, Kotlin y JavaScript se utiliza `this`. En C++ se emplea `this` pero como **puntero** (`this->campo`). En Python el concepto se expresa con el primer parámetro explícito del método, habitualmente llamado `self`, que cumple el papel de la instancia actual. En Swift y Scala también se usa `self` o `this` según el lenguaje y el estilo.

Pese a las diferencias de nombre o matiz (referencia vs. puntero vs. parámetro explícito), el propósito es el mismo: **referirse a la instancia en curso** para acceder a su estado y comportamiento, o para diferenciar entre variables con nombres coincidentes.

***

## **Ejemplo de uso de `this` en la clase `Punto`**

En el ejemplo siguiente, `this` se utiliza para:

1.  **Desambiguar** entre los parámetros del constructor y los campos de la instancia, y
2.  Invocar un **constructor sobrecargado** desde otro constructor con `this(...)`.

```java
// Archivo: Punto.java
class Punto {
    double x; // visibilidad por defecto (package-private)
    double y; // visibilidad por defecto

    // Constructor principal
    Punto(double x, double y) {
        // 'this.x' y 'this.y' se refieren a los atributos del objeto actual
        // 'x' y 'y' (sin 'this') son los parámetros del constructor
        this.x = x;
        this.y = y;
    }

    // Constructor adicional: crea el punto en el origen reutilizando el principal
    Punto() {
        this(0.0, 0.0); // llamada a otro constructor de la misma clase
    }

    // Método que calcula la distancia al origen usando los campos de la instancia
    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Método que traslada el punto; 'this' es implícito, pero puede escribirse
    void trasladar(double dx, double dy) {
        this.x += dx;
        this.y += dy;
    }
}
```

Para ilustrar el uso:

```java
// Archivo: DemoPunto.java
public class DemoPunto {
    public static void main(String[] args) {
        Punto a = new Punto(3, 4);
        System.out.println(a.calculaDistanciaAOrigen()); // 5.0

        Punto b = new Punto(); // usa el constructor que llama a this(0.0, 0.0)
        b.trasladar(1, 2);
        System.out.println(b.x + ", " + b.y); // 1.0, 2.0
    }
}
```

Este patrón muestra cómo `this` permite **acceder con claridad** al estado de la instancia y **reutilizar lógica** entre constructores, manteniendo el código legible y evitando ambigüedades.



## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### A continuación se añade a la clase `Punto` un método llamado **`distanciaA`** que recibe otro `Punto` y calcula la distancia entre la instancia actual (`this`) y el punto proporcionado. La fórmula utilizada es la distancia euclidiana en 2D:

$$
\text{distancia} = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}
$$

Se mantiene la **visibilidad por defecto** en los atributos para seguir el estilo de los ejemplos anteriores y el método emplea `this.x` y `this.y` para hacer explícito el uso de la instancia actual.

En el ejemplo de uso se crean dos puntos y se invoca `distanciaA` para obtener la distancia entre ambos. Este patrón resulta útil para mantener la lógica encapsulada en la propia clase, evitando repetir cálculos en distintos lugares del programa y haciendo el código más legible y reutilizable.

```java
// Archivo: Punto.java
class Punto {
    double x; // visibilidad por defecto (package-private)
    double y; // visibilidad por defecto

    // Constructor principal
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Constructor adicional: punto en el origen
    Punto() {
        this(0.0, 0.0);
    }

    // Distancia al origen (0,0)
    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Nuevo método: distancia entre 'this' y el punto 'otro'
    double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    // Traslada el punto
    void trasladar(double dx, double dy) {
        this.x += dx;
        this.y += dy;
    }
}
```

```java
// Archivo: DemoPunto.java
public class DemoPunto {
    public static void main(String[] args) {
        Punto a = new Punto(3, 4);
        Punto b = new Punto(6, 8);

        System.out.println("Distancia de a al origen: " + a.calculaDistanciaAOrigen()); // 5.0
        System.out.println("Distancia entre a y b: " + a.distanciaA(b)); // 5.0
    }
}
```

Si se deseara robustecer el método, podría añadirse una verificación de `null` para el parámetro `otro` y, en caso necesario, lanzar una excepción descriptiva (`IllegalArgumentException`) para evitar errores en tiempo de ejecución. También podría considerarse cambiar a `private` los atributos y exponer getters, pero se ha mantenido la visibilidad por defecto para la sencillez del ejemplo.



## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### 

***

## **¿Es por copia o por referencia al pasar un `Punto`?**

En Java **todo** se pasa **por valor**. Cuando el parámetro es un objeto (por ejemplo, `Punto`), lo que se **copia por valor** es la **referencia** al objeto. Eso significa que **dentro del método** se recibe otra **referencia** que apunta **al mismo objeto** en el heap. Por ello, si se **modifican los atributos** del `Punto` dentro del método (p. ej., `p.x = 10;`), esos cambios se observarán **fuera** del método, porque el objeto subyacente es el mismo.

Sin embargo, si **se reasigna** el parámetro a **otro objeto** dentro del método (por ejemplo, `p = new Punto(0,0);`), **esa reasignación no afecta** a la variable del llamador. Se cambia a qué objeto apunta la **copia local** de la referencia, pero **no** la referencia original fuera del método.

**Ejemplo:**

```java
void cambiaPunto(Punto p) {
    p.x = 10;           // Modifica el mismo objeto: se verá fuera
    p = new Punto(0,0); // Reasigna la referencia local: NO afecta fuera
}
```

***

## **¿Afectan los cambios a los atributos fuera del método?**

Sí, si el método **modifica campos** del objeto pasado, el cambio se refleja fuera porque **ambas referencias** (la del llamador y la del parámetro) apuntan al **mismo objeto**. Esto es un patrón común en Java: “**pass-by-value of the reference**”. Es importante distinguir **mutar el objeto** (se observa fuera) de **reasignar la referencia** (no se observa fuera).

Para evitar efectos laterales, puede optarse por diseños inmutables (objetos cuyos campos no cambian) o por **copias defensivas** (crear un nuevo objeto con los mismos datos y trabajar con él). Estas técnicas ayudan a mantener invariantes y a reducir acoplamiento entre métodos.

**Ejemplo demostrativo:**

```java
Punto a = new Punto(3, 4);
cambiaPunto(a);
// Tras llamar, a.x será 10 (por la mutación). Pero 'a' NO apuntará al (0,0),
// porque la reasignación dentro de cambiaPunto no afecta a la referencia del llamador.
```

***

## **¿Qué ocurre con un `int` (primitivo) pasado como parámetro?**

Para **tipos primitivos** como `int`, Java también pasa **por valor**, pero ahora lo que se copia es el **valor numérico** en sí. Si dentro del método se cambia el parámetro (por ejemplo, `n = 99;`), ese cambio **no afecta** a la variable original del llamador, porque se está modificando **solo la copia local**.

Este comportamiento explica por qué no es posible “devolver” cambios a un primitivo vía parámetros como en C con punteros; en Java se devuelve un nuevo valor **por retorno** o se encapsula el dato en un **objeto contenedor** si se desea mutarlo indirectamente.

**Ejemplo:**

```java
void incrementa(int n) {
    n = n + 1; // Solo cambia la copia local
}

int x = 5;
incrementa(x);
// x sigue valiendo 5
```

***

### **Resumen corto**

*   Java **siempre** pasa **por valor**.
*   Para objetos, se pasa **por valor una referencia** → mutar campos **sí** se ve fuera; **reasignar** el parámetro **no**.
*   Para primitivos (`int`, `double`, etc.), se pasa **por valor el dato** → los cambios **no** se ven fuera.



## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### 

***

## **¿Qué es `toString()` en Java?**

`toString()` es un método heredado de la clase base `java.lang.Object` que devuelve una **representación textual** del objeto. Sirve para obtener una cadena que describa su estado, y se invoca de forma implícita cuando un objeto se concatena con cadenas o se imprime con `System.out.println`. Por defecto, la implementación de `Object` muestra el nombre de la clase y un identificador hash, por lo que suele **sobrescribirse** para que la salida sea más útil y legible.

Sobrescribir `toString()` ayuda a **depurar**, a **registrar logs** y a hacer más clara la interacción con el objeto en herramientas de consola o entornos de pruebas. Un buen `toString()` debe ser **claro, conciso y estable**, mostrando los campos relevantes sin exponer información sensible. Es habitual usar la anotación `@Override` para indicar explícitamente que se está sobrescribiendo el método de la superclase.

***

## **¿Existe en otros lenguajes?**

La idea de una **representación en cadena** existe en la mayoría de lenguajes orientados a objetos, aunque con nombres y matices distintos. En **C#**, el método equivalente también se llama `ToString()`. En **Python**, los métodos relacionados son `__str__` (representación humana) y `__repr__` (representación para depuración); al imprimir un objeto, se usa `__str__` si está definido, y si no, `__repr__`. En **JavaScript**, existe `toString()` en objetos y prototipos, y se invoca en contextos de concatenación o conversión a cadena.

En **C++**, no hay un método estándar `toString()`, pero el patrón equivalente se logra sobrecargando el **operador de inserción** en streams (`operator<<`) o proporcionando funciones auxiliares que devuelvan `std::string`. Otros lenguajes como **Kotlin** y **Swift** también ofrecen mecanismos análogos (`toString()` en Kotlin; conformidad con `CustomStringConvertible` en Swift usando `description`).

***

## **Ejemplo de `toString()` en la clase `Punto`**

En el ejemplo siguiente, se añade un `toString()` que devuelve una forma legible como `Punto(x=3.0, y=4.0)`. Se mantiene la **visibilidad por defecto** en los atributos para ser consistentes con los ejemplos anteriores.

```java
// Archivo: Punto.java
class Punto {
    double x; // visibilidad por defecto (package-private)
    double y; // visibilidad por defecto

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    Punto() {
        this(0.0, 0.0);
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    void trasladar(double dx, double dy) {
        this.x += dx;
        this.y += dy;
    }

    @Override
    public String toString() {
        return "Punto(x=" + x + ", y=" + y + ")";
    }
}
```

```java
// Archivo: DemoPunto.java
public class DemoPunto {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println(p); // Invoca implícitamente p.toString()
        // Salida: Punto(x=3.0, y=4.0)

        System.out.println("Distancia al origen de " + p + ": " + p.calculaDistanciaAOrigen());
        // Salida: Distancia al origen de Punto(x=3.0, y=4.0): 5.0
    }
}
```

Este patrón facilita la depuración y la lectura de logs, ya que muestra el **estado relevante** del objeto de manera compacta y comprensible. En proyectos más grandes, puede considerarse formatear con `String.format` o usar bibliotecas que generen `toString()` automáticamente, pero la versión manual suele ser suficiente y explícita para empezar.



## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### 

***

## **¿Una clase es como un `struct` en C?**

Un `struct` en C puede considerarse como un **antecedente muy simplificado** de lo que luego será una clase en lenguajes orientados a objetos. Ambos permiten **agrupar datos** bajo un mismo tipo, lo que facilita manejar varias variables relacionadas como una unidad coherente. Sin embargo, la similitud termina en ese punto: un `struct` solo contiene **datos**, mientras que una clase combina **datos + comportamientos**, integrando también mecanismos como encapsulación, herencia y polimorfismo, que son esenciales en la orientación a objetos.

En este sentido, una clase no solo describe qué información tendrá un objeto, sino **qué puede hacer** ese objeto y **cómo se comporta**. Esto convierte a la clase en un modelo más completo y expresivo que un simple contenedor de variables. Mientras que el `struct` organiza datos, la clase describe **entidades activas**, capaces de interactuar y colaborar en un programa de forma estructurada.

***

## **¿Qué le falta al `struct` de C para ser como una clase?**

Para que un `struct` se asemejara a una clase orientada a objetos, necesitaría incorporar elementos clave que C no posee. En primer lugar, le falta la capacidad de **definir funciones asociadas directamente al tipo**, es decir, métodos que operen sobre los propios datos del objeto. En C, las funciones que trabajan con un `struct` existen externamente y deben recibir un puntero o la estructura completa, sin la sintaxis ni el modelo mental de “método del objeto”.

Además, al `struct` le faltan mecanismos fundamentales como **encapsulación** (control de acceso mediante `public`, `private`…), **constructores automáticos**, **destructores**, **herencia**, **polimorfismo** y **referencia implícita al objeto** (como `this`). Todos estos aspectos conforman la base del comportamiento de una clase en Java o C++, y permiten que las variables de dicho tipo se consideren **instancias** en el sentido pleno de la orientación a objetos —objetos con estado, identidad y comportamiento.

***

En resumen, un `struct` aporta solo la parte "datos" de una clase. Para convertirse en algo equivalente, necesitaría añadir la parte "comportamiento" y los mecanismos que hacen posible el resto de características de la programación orientada a objetos. Si lo deseas, puedo preparar una tabla comparativa entre `struct` (C), `struct` (C++) y clase (Java/C++), o mostrar cómo C++ amplió el `struct` para hacerlo prácticamente equivalente a una clase.



## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### 

***

## **Emulación básica con `struct` + funciones “método”**

En C no existen métodos asociados al tipo, así que se define un `struct` con los **datos** y funciones externas que operan sobre él. El patrón común es pasar un **puntero al `struct`** como primer parámetro de cada función, lo que emula el objeto receptor del método.

```c
// archivo: punto.c (o punto.h/punto.c si se separa interfaz e implementación)
#include <math.h>
#include <stdio.h>

typedef struct {
    double x;
    double y;
} Punto;

// “Constructor” de conveniencia (inicializador)
Punto Punto_make(double x, double y) {
    Punto p;
    p.x = x;
    p.y = y;
    return p; // retorno por valor (copia pequeña y eficiente en la práctica)
}

// “Método”: distancia al origen (0,0)
// Emula: double Punto::calculaDistanciaAOrigen() const;
double Punto_calculaDistanciaAOrigen(const Punto* self) {
    // 'self' emula a 'this' en C++
    return sqrt(self->x * self->x + self->y * self->y);
}

// Otra función “método” de ejemplo: trasladar
void Punto_trasladar(Punto* self, double dx, double dy) {
    self->x += dx;
    self->y += dy;
}

// Demostración de uso
int main(void) {
    Punto a = Punto_make(3.0, 4.0);                  // “instancia” automática (stack)
    printf("dist(a, origen) = %.2f\n", Punto_calculaDistanciaAOrigen(&a)); // 5.00

    Punto_trasladar(&a, 1.0, -2.0);
    printf("a = (%.1f, %.1f)\n", a.x, a.y);          // (4.0, 2.0)

    // También podría reservarse en heap (como 'new' en Java) si se necesita:
    // Punto* b = malloc(sizeof(Punto));
    // *b = Punto_make(0.0, 0.0);
    // printf("%.2f\n", Punto_calculaDistanciaAOrigen(b));
    // free(b);

    return 0;
}
```

Con este enfoque, **las “instancias”** en C son simplemente **variables** del tipo `Punto` (en stack, estática o heap), y las “operaciones” del tipo se expresan como **funciones que reciben un puntero** al `struct`. Para facilitar el uso, se suele prefijar el nombre del “tipo” en las funciones (`Punto_...`), imitando un espacio de nombres.

***

## **¿Qué ha pasado con `this`?**

En C no existe `this`. Lo que en Java se obtiene de forma **implícita** dentro de un método de instancia, en C se pasa **explícitamente** como un **puntero** al primer parámetro (en el ejemplo se llamó `self`, aunque también se usa `p`, `obj`, etc.). Por eso, el acceso a campos se realiza con `self->x` y `self->y` en lugar de `this.x` o `x`.

Este paso explícito hace visibles detalles que en lenguajes OO quedan ocultos: cada función “método” necesita que se le **provea la dirección** del objeto sobre el que operará. Además, se puede aplicar **const-correctness** para expresar intención: si la función no modifica el objeto, el parámetro se declara `const Punto* self`, igual que un método “const” en C++. Esto ayuda a prevenir modificaciones accidentales y documenta el contrato de la función.

***

## **Notas prácticas (constructor, memoria, estilo)**

C no tiene **constructores** ni **sobrecarga**. Se puede ofrecer un **inicializador** tipo `Punto_make(x, y)` que devuelva un `Punto` ya rellenado, o una función `Punto_init(Punto* self, double x, double y)` si se prefiere inicializar una estructura ya reservada (especialmente útil cuando se trabaja en heap o en arreglos). Para memoria dinámica, se usa `malloc`/`free`; para memoria automática, basta con declarar la variable.

Tampoco hay **encapsulación** nativa. Si se quiere imitarla, puede **ocultarse** la definición del `struct` en el `.c` y exponer un **puntero opaco** en el `.h` (técnica de *opaque pointers*), obligando al uso de funciones de acceso. Sin embargo, para una emulación simple como esta, mantener el `struct` en el `.h` y funciones prefijadas es suficiente para capturar la idea: datos + funciones que operan sobre esos datos, con el “`this`” convertido en un **parámetro explícito**.
