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

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

En orientación a objetos, la **herencia** es un mecanismo que permite definir una clase nueva a partir de otra ya existente, estableciendo una relación conceptual del tipo **“A es-un B”**. Esto significa que el subtipo (A) es una especialización del tipo más general (B) y puede ser tratado como tal sin romper el significado del programa. Por ejemplo, afirmar que *un Artillero es-un Soldado* implica que todo artillero comparte las características comunes de cualquier soldado y puede utilizarse allí donde se espera un soldado genérico.

La primera implicación importante de la herencia es la **compatibilidad de tipos**. En Java, una referencia de una superclase puede apuntar a un objeto de cualquiera de sus subclases. Esto permite escribir código más general y reutilizable, como colecciones o métodos que trabajan con el tipo base sin conocer los detalles concretos de cada subtipo. Gracias a esta compatibilidad, se puede almacenar en un mismo array distintos tipos de soldados y tratarlos de forma uniforme, invocando métodos comunes definidos en la clase base.

La segunda implicación es la **herencia de estado y comportamiento**. Las subclases heredan los atributos y métodos accesibles de la superclase, evitando duplicar código. En el ejemplo, tanto el artillero como el zapador heredan el atributo `nombre` (encapsulado como privado) y el método `saludar()`, que define un comportamiento común. Cada subtipo añade su propio estado específico (cohetes o minas) y expone ese estado mediante métodos propios, respetando la encapsulación y enriqueciendo el modelo sin modificar la clase base.

```java
// Superclase
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

// Subclase Artillero
public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

// Subclase Zapador
public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

// Uso de la compatibilidad de tipos
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Soldado("Carlos");
        ejercito[1] = new Artillero("Ana", 5);
        ejercito[2] = new Zapador("Luis", 10);

        for (Soldado s : ejercito) {
            s.saludar(); // Todos pueden saludar
        }
    }
}
```



## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Al crear un objeto de una subclase (por ejemplo, `Artillero` o `Zapador`), **se ejecutan siempre dos constructores**: el de la clase base (`Soldado`) y el de la clase concreta. El **orden de ejecución es fijo**: primero se ejecuta el constructor de la superclase y después el de la subclase. Esto ocurre porque, antes de que un objeto pueda inicializar su parte específica, debe estar correctamente inicializada la parte que pertenece a la clase base. En Java, este encadenamiento de constructores es obligatorio y automático desde el punto de vista del lenguaje.

La palabra clave **`super` dentro de un constructor** sirve para invocar explícitamente un constructor de la clase base. Mediante `super(...)` se pasan los parámetros necesarios para inicializar el estado heredado, como el atributo `nombre` en el ejemplo. Conceptualmente, `super` representa “la parte del objeto que pertenece a la superclase”. Esta llamada debe ser siempre la **primera instrucción del constructor**, ya que Java exige que la inicialización de la superclase se realice antes que cualquier otra acción en la subclase.

Si la clase base **no tiene visible un constructor sin parámetros**, entonces **es obligatorio llamar a `super` de forma explícita**. En caso contrario, el código no compilará. Esto se debe a que, si no se escribe `super(...)`, Java intenta insertar implícitamente una llamada a `super()` sin argumentos; si ese constructor no existe o no es accesible, se produce un error de compilación. Por tanto, en diseños donde la clase base requiere información mínima para construirse, las subclases están forzadas a proporcionar esa información al invocar el constructor adecuado.

En cambio, si la clase base **sí dispone de un constructor sin parámetros accesible**, la llamada a `super()` es opcional, ya que el compilador la inserta automáticamente. Aun así, es habitual escribirla explícitamente cuando se quiere dejar claro el flujo de inicialización o cuando se desea llamar a un constructor concreto de la superclase con argumentos. Esta regla refuerza la idea de que la herencia no solo reutiliza código, sino que también impone un orden y una disciplina en la construcción de los objetos.


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Sí, **los atributos privados de la superclase forman parte de la instancia de la subclase en memoria**. Cuando se crea un objeto de una subclase en Java, la instancia resultante contiene **toda la estructura definida por la jerarquía de herencia**: primero los campos de la superclase y después los de la subclase. Desde el punto de vista de la memoria, no existen “dos objetos” separados, sino un único objeto cuya primera parte corresponde a `Soldado` y cuya extensión corresponde, por ejemplo, a `Artillero` o `Zapador`.

Sin embargo, que esos atributos existan físicamente dentro del objeto **no implica que puedan usarse directamente desde el código de la subclase**. El modificador `private` es una restricción de acceso de tipo léxico (de código), no de memoria. Un atributo privado solo puede ser accedido desde el código de la clase que lo declara, aunque el objeto concreto sea de una subclase. Por tanto, aunque un `Artillero` tenga en memoria el atributo `nombre` heredado de `Soldado`, el código de `Artillero` no puede referirse a él directamente.

Esto se aprecia claramente en el siguiente ejemplo: el método `saludar()` puede usar `nombre` porque está definido en `Soldado`, pero un método nuevo definido en `Artillero` no puede hacerlo. Si la subclase necesita interactuar con ese estado heredado, debe hacerlo **a través de métodos accesibles** de la superclase (por ejemplo, métodos `protected` o `public`), respetando la encapsulación.

```java
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }

    protected String getNombre() {
        return nombre;
    }
}

public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public void informar() {
        // System.out.println(nombre); // ERROR: nombre es private en Soldado
        System.out.println("Soy el artillero " + getNombre()
                           + " y tengo " + cohetes + " cohetes");
    }
}
```

En resumen, **los atributos privados de la superclase sí existen dentro de las instancias de las subclases**, pero **no son accesibles directamente desde el código de dichas subclases**. Esta separación refuerza uno de los principios fundamentales de la orientación a objetos: la encapsulación, asegurando que cada clase controla el acceso a su propio estado incluso cuando participa en una relación de herencia.


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La **compatibilidad de tipos** entre una superclase y sus subclases tiene una consecuencia directa y muy importante en términos de **extensibilidad del código**. Permite escribir código que depende únicamente del tipo base (`Soldado`) y que **no necesita modificarse** cuando aparecen nuevas especializaciones. Esto reduce el acoplamiento y evita tener que reescribir lógica existente cada vez que se amplía el sistema. El diseño queda así *abierto a la extensión, pero cerrado a la modificación*, una idea clave en orientación a objetos.

Gracias a esta compatibilidad, el código que trabaja con `Soldado` no necesita conocer los subtipos concretos. El bucle que recorre un array de `Soldado` y llama a `saludar()` funciona porque todos los objetos almacenados **son tratados como soldados**, independientemente de que internamente sean artilleros, zapadores u otros tipos aún no definidos. Añadir un nuevo tipo no rompe el código existente ni obliga a añadir condiciones (`if`, `switch`) basadas en el tipo concreto.

Para ilustrarlo, se puede añadir un nuevo subtipo, por ejemplo un `Medico`, que también es un `Soldado` y hereda su nombre y su capacidad de saludar. Este nuevo tipo introduce su propio estado específico, pero **no requiere ningún cambio** en el código que pide el saludo a todos los soldados. El mecanismo de herencia garantiza que el nuevo objeto siga siendo compatible a nivel de tipos con el resto del sistema.

```java
public class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}

// Código existente: NO SE MODIFICA
Soldado[] ejercito = new Soldado[4];
ejercito[0] = new Soldado("Carlos");
ejercito[1] = new Artillero("Ana", 5);
ejercito[2] = new Zapador("Luis", 10);
ejercito[3] = new Medico("Marta", 3);

for (Soldado s : ejercito) {
    s.saludar();
}
```

En conclusión, la compatibilidad de tipos convierte a la herencia en una herramienta clave para la extensibilidad: se pueden añadir nuevos comportamientos especializados creando nuevas subclases, **sin tocar el código que ya funciona y ha sido probado**. Esto hace que los sistemas orientados a objetos escalen mejor y sean más fáciles de mantener conforme evolucionan.



## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Sí, en Java **es posible tener una referencia del supertipo que apunte a un objeto real de un subtipo**. Esto es una consecuencia directa de la compatibilidad de tipos en herencia: como un `Artillero` *es-un* `Soldado`, puede almacenarse en una variable declarada como `Soldado`. Sin embargo, al trabajar con esa referencia, el compilador solo permite acceder a los **métodos definidos en el tipo de la referencia**, no a los métodos específicos del subtipo, aunque el objeto real sí los tenga. Esta restricción garantiza seguridad de tipos en tiempo de compilación.

Por tanto, **no se pueden invocar directamente métodos públicos exclusivos del subtipo usando una referencia del supertipo**. Por ejemplo, desde una referencia de tipo `Soldado` no se puede llamar a `getCohetes()`, porque ese método no forma parte de la interfaz pública de `Soldado`. Para poder acceder a funcionalidades específicas del subtipo, es necesario realizar una conversión explícita de tipos, siempre de forma controlada.

El **upcasting** consiste en tratar un objeto de un subtipo como si fuese del supertipo, y es una conversión **implícita y segura**. Ocurre, por ejemplo, al asignar un `Artillero` a una variable de tipo `Soldado`. El **downcasting**, en cambio, consiste en convertir una referencia del supertipo a una referencia de un subtipo concreto, y es una operación **explícita y potencialmente peligrosa**, ya que puede provocar errores en tiempo de ejecución si el objeto real no es de ese tipo. Para evitar estos errores se utiliza el operador **`instanceof`**, que permite comprobar el tipo real del objeto antes de realizar la conversión.

```java
Soldado[] ejercito = new Soldado[3];
ejercito[0] = new Soldado("Carlos");       // upcasting implícito
ejercito[1] = new Artillero("Ana", 5);     // upcasting implícito
ejercito[2] = new Zapador("Luis", 10);     // upcasting implícito

for (Soldado s : ejercito) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // downcasting explícito
        System.out.println("Cohetes disponibles: " + a.getCohetes());
    }
}
```

En este ejemplo se observa que el código puede recorrer un array homogéneo de `Soldado`, beneficiándose del upcasting para el tratamiento común, y utilizar `instanceof` junto con downcasting únicamente cuando necesita acceder a información específica de un subtipo concreto. Este patrón es habitual cuando se combina polimorfismo con lógica dependiente del tipo real del objeto.



## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso **protegido** forma parte de los mecanismos de ocultación de información y define un punto intermedio entre `private` y `public`. Un atributo o método protegido **no es accesible desde cualquier clase**, pero **sí lo es desde las subclases** y desde las clases del mismo paquete. Su objetivo es permitir que una clase base comparta parte de su estado o comportamiento con sus derivadas sin exponerlo completamente al exterior, manteniendo así un control razonable sobre su uso.

En Java, el acceso protegido se implementa mediante la palabra clave **`protected`**. A diferencia de `private`, que limita el acceso exclusivamente al código de la propia clase, `protected` reconoce la relación de herencia como un criterio válido de acceso. Esto es especialmente útil cuando el estado de la superclase debe ser utilizado o ampliado por las subclases como parte de su comportamiento específico, sin necesidad de usar siempre getters públicos.

Aplicado al ejemplo, hacer que el atributo `nombre` de `Soldado` sea protegido permite que una subclase como `Zapador` lo utilice directamente en sus métodos. De este modo, el zapador puede referirse a su propio nombre al poner bombas, sin romper la encapsulación frente al resto del sistema. El atributo sigue sin ser accesible desde código externo que no forme parte de la jerarquía.

```java
public class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerBomba() {
        System.out.println("El zapador " + nombre + " pone una bomba");
    }

    public int getMinas() {
        return minas;
    }
}
```

En resumen, el acceso protegido permite **compartir información dentro de una jerarquía de herencia** sin hacerla pública, equilibrando reutilización y encapsulación. Es una herramienta pensada específicamente para el diseño orientado a objetos, donde las subclases forman parte del contrato interno de la superclase, pero el resto del código no.



## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En los lenguajes orientados a objetos **no existe una regla universal** que obligue a que todos los objetos desciendan de una única clase base, aunque sí es una **decisión de diseño muy habitual**. Depende del lenguaje y de cómo haya sido concebido su modelo de objetos. Algunos lenguajes definen explícitamente una raíz común para toda la jerarquía, mientras que otros permiten clases sin ningún ancestro común explícito o dejan este aspecto más difuso.

En varios lenguajes orientados a objetos **sí existe una clase base implícita** para todos los objetos. Por ejemplo, en Java y C#, todas las clases heredan, directa o indirectamente, de una clase raíz común. En otros lenguajes como C++, aunque se puede diseñar una jerarquía con una base común, **no es obligatorio**: una clase puede no heredar de ninguna otra. En lenguajes más dinámicos como Python, también existe una clase base común, aunque el sistema de tipos y herencia es más flexible.

En **Java**, la clase base de todas las clases es `java.lang.Object`. Si una clase no extiende explícitamente a otra, el compilador hace que herede automáticamente de `Object`. Esto implica que **todos los objetos en Java comparten un conjunto mínimo de métodos**, como `toString()`, `equals()`, `hashCode()` y `getClass()`. Incluso las clases definidas por el programador más simples, como `Soldado`, forman siempre parte de esta jerarquía.

Esta decisión tiene consecuencias importantes: permite tratar cualquier instancia como un `Object`, almacenar objetos heterogéneos en estructuras comunes y definir comportamientos generales. Al mismo tiempo, refuerza la uniformidad del lenguaje y facilita el uso de la reflexión, la gestión de colecciones y la interoperabilidad entre librerías. En resumen, en Java **sí existe una clase base universal**, mientras que en otros lenguajes orientados a objetos esto depende del modelo elegido por el propio lenguaje.



## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La **herencia múltiple** es un mecanismo de la orientación a objetos mediante el cual una clase puede **heredar directamente de más de una clase base**. Conceptualmente, esto significa que un tipo concreto puede ser a la vez una especialización de varios tipos distintos, heredando estado (atributos) y comportamiento (métodos) de todos ellos. Por ejemplo, una clase podría ser simultáneamente un `Vehiculo` y una `MaquinaElectrica`. Aunque esta idea puede resultar potente, también introduce problemas de complejidad, como ambigüedades cuando varias superclases definen el mismo método o atributo (el conocido *problema del diamante*).

La existencia y el soporte de la herencia múltiple **depende del lenguaje**. Algunos lenguajes como C++ permiten herencia múltiple completa entre clases, lo que da gran flexibilidad pero exige al programador gestionar explícitamente las ambigüedades. Otros lenguajes, por razones de simplicidad y seguridad, deciden restringirla o eliminarla, ofreciendo mecanismos alternativos para lograr reutilización de comportamiento sin los riesgos asociados.

En **Java no existe herencia múltiple de clases**: una clase solo puede extender (`extends`) de **una única clase base**. Esta decisión elimina de raíz los problemas de ambigüedad y simplifica el modelo de herencia. Sin embargo, Java **sí permite herencia múltiple de interfaces**, lo que significa que una clase puede implementar (`implements`) varias interfaces al mismo tiempo. De este modo, una clase puede comprometerse a varios contratos de comportamiento sin heredar estado, lo que evita los conflictos típicos de la herencia múltiple clásica.

En consecuencia, en Java la combinación de **herencia simple + múltiples interfaces + composición** cubre la mayoría de casos prácticos. Se hereda estado y comportamiento común desde una única superclase, y se amplían capacidades desde varias interfaces, manteniendo un diseño más claro y controlado. Esta restricción no limita la expresividad del lenguaje, sino que orienta hacia diseños más robustos y mantenibles.



## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

En los lenguajes orientados a objetos, las **excepciones son objetos**, lo que permite modelarlas como cualquier otro elemento del dominio. En Java, esto posibilita crear **excepciones personalizadas** que transportan información adicional sobre el error ocurrido. Una excepción personalizada mejora la expresividad del código, ya que comunica claramente qué ha fallado y permite reaccionar de forma más precisa. Además, al formar parte de una jerarquía de clases, puede integrarse con los mecanismos estándar de manejo de excepciones del lenguaje.

Una excepción es **no controlada (unchecked)** cuando hereda de `RuntimeException`. Este tipo de excepciones no obliga a ser capturada ni declarada con `throws`, y suele emplearse para errores de programación o situaciones imposibles de manejar localmente. En este caso, una excepción `UsuarioNoEncontradoException` resulta apropiada como no controlada, ya que indica un fallo lógico en el flujo del programa (por ejemplo, acceder a un usuario inexistente).

El hecho de que la excepción esté **compuesta con un `Usuario`** significa que mantiene una referencia al objeto que causó el problema. Esto permite, por ejemplo, registrar información detallada, depurar con mayor facilidad o mostrar mensajes de error más ricos. Desde el punto de vista del diseño, se trata de encapsular tanto el problema como su contexto en un único objeto coherente.

Java permite además encadenar excepciones mediante una **causa subyacente** (`cause`). Para ello, se sobrecargan los constructores y se delega en los constructores de la superclase. De este modo, la excepción personalizada puede indicar tanto el error concreto de dominio como el error técnico que lo originó.

```java
// Clase de dominio usada por la excepción
public class Usuario {
    private String id;
    private String nombre;

    public Usuario(String id, String nombre) {
        this.id = id;
        this.nombre = nombre;
    }

    public String getId() {
        return id;
    }

    public String getNombre() {
        return nombre;
    }
}

// Excepción personalizada no controlada
public class UsuarioNoEncontradoException extends RuntimeException {
    private final Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getId());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable cause) {
        super("Usuario no encontrado: " + usuario.getId(), cause);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

En este ejemplo, la excepción es un objeto completo del sistema: contiene estado propio, referencia a un objeto de dominio y soporte para el encadenamiento de errores. Esto refleja claramente cómo las excepciones, al ser objetos, participan plenamente en el modelo orientado a objetos.



## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

Se afirma que no debe emplearse la herencia únicamente para reutilizar código porque la herencia **impone una relación semántica fuerte** del tipo *“es-un”*. Al heredar, no solo se reutiliza implementación, sino que se establece un **compromiso conceptual y de tipos**: la subclase pasa a ser sustituible por la superclase. Si esa relación no es verdadera desde el punto de vista del dominio, el diseño se vuelve incorrecto, aunque “funcione”. Reutilizar código sin una relación *es‑un* real conduce a jerarquías artificiales y difíciles de entender.

Además, la herencia introduce un **acoplamiento muy fuerte** entre la clase base y las subclases. Los detalles de implementación de la superclase dejan de ser internos, porque cualquier cambio puede afectar al comportamiento de todas las clases derivadas. Esto provoca el llamado *diseño frágil*: pequeñas modificaciones en la superclase pueden romper invariantes o provocar errores sutiles en subclases que dependen implícitamente de su funcionamiento. Cuando la herencia se usa solo para copiar código, este riesgo no se justifica.

La **composición**, en cambio, permite reutilizar código manteniendo **relaciones más débiles** del tipo *“tiene-un / usa-un”*. Al componer, una clase delega parte de su comportamiento en otros objetos, sin asumir su identidad ni quedar atada a su jerarquía. Esto hace el código más flexible, más fácil de probar y de modificar, ya que los componentes pueden cambiarse sin afectar al tipo público de la clase que los usa.

Por ello, la herencia debe reservarse para modelar **especialización auténtica**, donde la sustitución sea correcta y esperable. Cuando la necesidad principal es reutilizar funcionalidad, la composición suele ser la primera y mejor opción. En orientación a objetos, heredar no significa “copiar código”, sino **extender un concepto**, y confundir ambos objetivos conduce a diseños rígidos y poco mantenibles.



## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

Se dice que se debe **favorecer la composición frente a la herencia** porque la composición produce diseños **más flexibles y menos acoplados**. La herencia establece una relación rígida y permanente entre clases, donde la subclase queda fuertemente ligada a la implementación y decisiones internas de la superclase. Esto implica que cualquier cambio en la clase base puede afectar de forma inesperada a todas las subclases, incluso aunque estas no se modifiquen. En sistemas grandes o en evolución, este acoplamiento dificulta el mantenimiento y la evolución del código.

La composición, en cambio, se basa en relaciones del tipo **“tiene-un” o “usa-un”**, donde una clase delega parte de su comportamiento en otros objetos. Esta delegación no impone una relación de tipos ni de identidad, lo que permite cambiar la implementación interna sin afectar a la interfaz pública de la clase. Como consecuencia, el código resulta más modular: los componentes pueden sustituirse, extenderse o reutilizarse en otros contextos con menor riesgo.

Otro motivo clave es que la herencia **expone implícitamente detalles de implementación**. Las subclases no solo reutilizan el comportamiento público, sino que a menudo dependen de comportamientos protegidos o del orden interno de ejecución de la superclase. Esto viola el principio de encapsulación y conduce a diseños frágiles. La composición, por el contrario, fomenta el uso de interfaces bien definidas y contratos explícitos, reduciendo dependencias ocultas.

Por todo ello, la recomendación no implica que la herencia sea incorrecta, sino que debe usarse **solo cuando existe una verdadera relación de especialización** y cuando la sustitución sea semánticamente correcta. En ausencia de una relación clara *es‑un*, la composición suele ofrecer una solución más segura, extensible y mantenible. Favorecer la composición es, en esencia, una forma de diseñar sistemas que evolucionan mejor con el tiempo.



## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

Cuando se dice que la **herencia rompe la encapsulación**, se hace referencia a que la herencia **expone internamente detalles de implementación de la clase base a las subclases**. La encapsulación persigue que una clase oculte su estado y su funcionamiento interno, comunicándose solo a través de una interfaz bien definida. Sin embargo, al heredar, la subclase no solo accede al comportamiento público, sino que suele depender también de métodos `protected`, del orden de ejecución interno o de supuestos no explícitos de la superclase.

Esta dependencia implícita hace que la subclase conozca “demasiado” sobre cómo funciona la clase base por dentro. Aunque ese conocimiento no sea visible desde fuera del sistema, **sí rompe la encapsulación interna**, porque la implementación deja de ser un detalle privado de la superclase. Si la clase base cambia su comportamiento interno —aunque mantenga la misma interfaz pública—, las subclases pueden dejar de funcionar correctamente. Esto genera el llamado *acoplamiento frágil*, donde las clases derivadas son sensibles a cambios que, en principio, no deberían afectarles.

En otras palabras, la herencia permite acceder a una **interfaz ampliada y no siempre explícita**, formada por campos protegidos, métodos auxiliares y efectos secundarios internos. Esta interfaz no está documentada formalmente, pero las subclases acaban dependiendo de ella para funcionar. Así, la encapsulación se debilita porque la superclase ya no puede evolucionar libremente sin considerar a todas sus subclases.

Por el contrario, la composición mantiene la encapsulación porque la clase reutilizada se trata como una **caja negra**: solo se usa a través de su interfaz pública y no se asumen detalles internos. Por eso se afirma que no es que la herencia sea incorrecta, sino que **debe usarse con cuidado**, entendiendo que implica compartir implementación y, con ello, perder parte del aislamiento que la encapsulación pretende garantizar.



## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

A continuación se muestran **dos formas alternativas de modelar el mismo problema**, poniendo en contraste **herencia** y **composición**. El objetivo es representar a un `Estudiante` y un `Trabajador` que comparten datos comunes (DNI y nombre), destacando cómo cambia el diseño según el mecanismo elegido, sin alterar el dominio del problema.

En el enfoque basado en **herencia**, se introduce una superclase `Persona` que agrupa los datos comunes. Tanto `Estudiante` como `Trabajador` **son personas**, por lo que heredan directamente esos atributos. Esta solución expresa explícitamente una relación *“es‑un”* y permite reutilizar el estado compartido de forma directa. Sin embargo, implica acoplar a ambas clases a la jerarquía y asumir que cualquier estudiante o trabajador debe ser tratado como una persona en todos los contextos.

```java
// Enfoque con herencia
public class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

public class Estudiante extends Persona {
    private String carrera;

    public Estudiante(String dni, String nombre, String carrera) {
        super(dni, nombre);
        this.carrera = carrera;
    }
}

public class Trabajador extends Persona {
    private double salario;

    public Trabajador(String dni, String nombre, double salario) {
        super(dni, nombre);
        this.salario = salario;
    }
}
```

En el enfoque basado en **composición**, los datos comunes se encapsulan en una clase independiente (`DatosPersonales`) y tanto `Estudiante` como `Trabajador` **tienen** esos datos en lugar de heredarlos. Este diseño no impone una relación de tipos entre las clases y permite reutilizar la información personal en otros contextos sin forzar una jerarquía común. Además, reduce el acoplamiento y preserva mejor la encapsulación, ya que cada clase decide cómo usar internamente esos datos.

```java
// Enfoque con composición
public class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Estudiante {
    private DatosPersonales datos;
    private String carrera;

    public Estudiante(DatosPersonales datos, String carrera) {
        this.datos = datos;
        this.carrera = carrera;
    }
}

public class Trabajador {
    private DatosPersonales datos;
    private double salario;

    public Trabajador(DatosPersonales datos, double salario) {
        this.datos = datos;
        this.salario = salario;
    }
}
```

Ambas soluciones son válidas, pero transmiten **decisiones de diseño distintas**. La herencia enfatiza la identidad común y la sustitución por el tipo base, mientras que la composición prioriza flexibilidad, reutilización desacoplada y mejor evolución del código. Elegir una u otra depende de si la relación conceptual *“es‑un”* es realmente esencial en el modelo del dominio.

