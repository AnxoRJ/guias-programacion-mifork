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

El **polimorfismo** es una propiedad de la programación orientada a objetos que permite tratar de forma uniforme a objetos de distintas clases que comparten un mismo tipo base (una clase padre o una interfaz), aunque su comportamiento concreto sea diferente. En la práctica, significa que una misma referencia puede apuntar a objetos de distintas subclases y que, al invocar un método, se ejecuta la versión adecuada según el tipo real del objeto en tiempo de ejecución. Esto supone un cambio conceptual importante respecto a C/C++ sin orientación a objetos, donde las llamadas se resuelven de forma más estática y explícita.

El polimorfismo sirve principalmente para **desacoplar** el código y hacerlo más flexible y extensible. Gracias a él, se pueden escribir métodos que trabajen con clases base sin conocer los detalles de las clases derivadas, facilitando la reutilización y el mantenimiento del software. En Java, este mecanismo está estrechamente ligado a la herencia y al uso de referencias del tipo de la superclase, permitiendo ampliar comportamientos sin modificar el código existente.

La **sobreescritura de métodos** es el mecanismo que hace posible el polimorfismo dinámico. Consiste en que una subclase proporcione su propia implementación de un método definido en la clase base, respetando la misma firma (nombre, parámetros y tipo de retorno compatible). Cuando un método está sobrescrito y se invoca a través de una referencia del tipo de la superclase, Java decide en tiempo de ejecución qué versión del método ejecutar, en función del tipo real del objeto. De este modo, la sobreescritura permite especializar comportamientos heredados y es una pieza clave para aprovechar correctamente el polimorfismo en el diseño orientado a objetos.

**Anotación:**
* Polimorfismo:
* * Objetivo: faciitar la extensión de los programas.
* * Facilita que esta extensión se haga creando código nuevo frente a editar código existente.



## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La **ligadura dinámica** (o **enlace tardío**) consiste en que la asociación entre una llamada a un método y la implementación concreta que se ejecuta se decide **en tiempo de ejecución**, y no en tiempo de compilación. Esto implica que, cuando se invoca un método a través de una referencia a un tipo base, el sistema determina qué versión ejecutar según el **tipo real del objeto** al que apunta esa referencia. Frente a la ligadura estática (enlace temprano), donde la decisión se toma en compilación, la ligadura dinámica aporta flexibilidad y adaptación del comportamiento en ejecución.

La relación con el **polimorfismo** es directa: el polimorfismo dinámico se apoya precisamente en la ligadura dinámica para funcionar. Gracias a este mecanismo, diferentes subclases pueden responder de forma distinta a la misma llamada de método, manteniendo una interfaz común definida en la superclase. Sin ligadura dinámica, el polimorfismo en el sentido habitual de la orientación a objetos no sería posible, ya que todas las llamadas se resolverían de forma fija y predecible en compilación.

En **C++**, la ligadura dinámica **no es el comportamiento por defecto**. Para que un método se enlace dinámicamente debe declararse explícitamente como `virtual` en la clase base, y la llamada debe realizarse a través de un puntero o referencia al tipo base. Si no se usa `virtual`, se aplica ligadura estática, incluso aunque exista herencia. En **Java**, en cambio, la ligadura dinámica es el **comportamiento por defecto** para los métodos de instancia: todos los métodos no marcados como `final`, `static` o `private` se enlazan dinámicamente sin necesidad de indicarlo explícitamente, lo que facilita el uso del polimorfismo desde etapas tempranas.

En **Python**, la ligadura dinámica también es la norma, pero de una forma todavía más flexible. Al ser un lenguaje dinámico, el método que se ejecuta se decide en tiempo de ejecución según el objeto concreto, sin necesidad de herencia estricta ni declaraciones previas. Este enfoque, conocido como *duck typing*, permite polimorfismo basado en el comportamiento más que en la jerarquía de clases, haciendo que el enlace tardío sea una característica natural del propio modelo del lenguaje.



## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

A continuación se muestra un **ejemplo sencillo en Java** que ilustra el **polimorfismo mediante ligadura dinámica**. Se define una clase base `Soldado` con un método `saludar`. La subclase `Zapador` **sobreescribe completamente** dicho método, mientras que `Artillero` hereda el comportamiento tal cual.

```java
public class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma genérica.");
    }
}
```

```java
public class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("El zapador saluda mientras prepara explosivos.");
    }
}
```

```java
public class Artillero extends Soldado {
    // No sobrescribe el método saludar
}
```

El siguiente código crea un **array de referencias de tipo `Soldado`** que apunta a objetos de distintas subclases. Al recorrerlo y llamar al método `saludar`, Java decide en **tiempo de ejecución** qué versión ejecutar según el tipo real del objeto, demostrando el polimorfismo:

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[2];
        ejercito[0] = new Zapador();
        ejercito[1] = new Artillero();

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

En este ejemplo, aunque todas las referencias del array son de tipo `Soldado`, la llamada a `saludar` ejecuta comportamientos distintos. Esto ocurre porque el método está enlazado dinámicamente y permite que cada subclase defina su propia respuesta, mostrando de forma clara el funcionamiento del polimorfismo en Java.

**Anotación:**

```java
public class Soldado{
    public void saludar;

    public void saluda(){
        sout("saludo 1")
    }
}

private class PruebaPolimorfismo{
    main{
        Soldado[] soldados = new Soldado[2];

        Soldados [0] = new Artillero();
        Soldados [1] = new Zapador();

        for(Soldado soldado: soldados){
            soldado.saluda();
        }
    }
}

public class Zapador extends Soldado{
    public void saluda(){
        sout("Saludo zapador");
    }
}

private class PruebaPolimorfismo{
    main{
        Soldado[] soldados = new Soldado[2];

        Soldados [0] = new Artillero();
        Soldados [1] = new Zapador();

        pasarRevista.soldados();
    }
}

public static void pasarRevista(){
    for(Soldado soldado: soldados){
        soldado.saluda();
    }
}

public class Zapador extends Soldado{
    super.saluda();
    public void saluda(){
        sout("Saludo zapador");
    }
}
```



## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Sí, al **sobreescribir un método** es posible **invocar la implementación de la clase base** y trabajar a partir de ella. Esto resulta útil cuando se desea **ampliar o modificar ligeramente** el comportamiento heredado, en lugar de sustituirlo por completo. De este modo, se mantiene la lógica común definida en la superclase y se añade comportamiento específico en la subclase, favoreciendo la reutilización del código.

En Java, esta invocación se realiza mediante la **palabra clave `super`**, que permite acceder a los métodos (y atributos) de la clase padre. Al usar `super.metodo()`, se ejecuta explícitamente la versión del método definida en la superclase, incluso cuando dicho método ha sido sobreescrito en la subclase. Esta llamada suele colocarse al inicio o al final del método sobrescrito, según el efecto deseado.

El siguiente ejemplo muestra cómo `Zapador` sobrescribe el método `saludar`, pero reutiliza el saludo base del `Soldado` y añade un mensaje propio:

```java
public class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma genérica.");
    }
}
```

```java
public class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();  // Invoca el método de la clase base
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}
```

En este caso, el método sobrescrito no reemplaza por completo el comportamiento original, sino que lo **extiende**. La palabra clave utilizada para invocar el método de la clase base es **`super`**, y su uso es esencial cuando se desea combinar polimorfismo con reutilización controlada del código heredado.



## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al **sobreescribir un método en Java**, deben respetarse ciertas restricciones. La **lista de parámetros debe ser exactamente la misma** (mismo número, orden y tipos), ya que de lo contrario no se considera sobreescritura. El **tipo de retorno** debe ser el mismo o un **tipo covariante**, es decir, un subtipo del devuelto por el método de la clase base. Además, no se puede reducir la visibilidad del método (por ejemplo, no es válido pasar de `public` a `protected`), y tampoco se pueden lanzar excepciones comprobadas (*checked*) más generales que las declaradas en el método original.

La diferencia entre **sobreescritura (overriding)** y **sobrecarga (overloading)** es fundamental. La sobreescritura sucede entre una clase base y una subclase, afecta al **mismo método heredado** y está asociada al **polimorfismo y a la ligadura dinámica**, ya que la versión ejecutada se decide en tiempo de ejecución. La sobrecarga, en cambio, ocurre dentro de una misma clase (o entre clase base y subclase) cuando existen **métodos con el mismo nombre pero distinta lista de parámetros**; en este caso, la elección del método se realiza en **tiempo de compilación** y no interviene el polimorfismo dinámico.

La anotación **`@Override`** sirve para indicar explícitamente que un método está destinado a **sobreescribir** uno heredado de la clase base. Su principal utilidad es que el compilador verifica que la sobreescritura es correcta: si se comete un error en el nombre del método, en los parámetros o en la firma, se producirá un error de compilación. Esto evita fallos sutiles y difíciles de detectar en tiempo de ejecución, por lo que se considera **una buena práctica usar `@Override` siempre** que se sobrescriba un método, mejorando la legibilidad y la seguridad del código.



## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Sí, al estudiar Java se empieza a **usar polimorfismo desde etapas muy tempranas**, aunque no siempre se presente explícitamente con ese nombre. Muchas de las primeras clases que se crean heredan, directa o indirectamente, de `Object`, y al **sobreescribir métodos como `toString` o `equals`** ya se está aprovechando el polimorfismo dinámico. Esto ocurre incluso aunque el concepto todavía no se haya formalizado teóricamente.

Cuando se sobreescribe `toString`, por ejemplo, se define cómo “se presenta” un objeto concreto al ser tratado como un `Object`. Si ese objeto se pasa a un método que espera un `Object` (como ocurre al imprimirlo con `System.out.println`), Java invoca en tiempo de ejecución la versión adecuada de `toString`. Esa decisión automática basada en el tipo real del objeto es precisamente **polimorfismo**, aunque para el programador principiante resulte transparente.

Algo similar sucede con `equals`. Al sobrescribirlo, se redefine el criterio de igualdad para una clase concreta, pero el método sigue siendo invocado a través de referencias del tipo `Object` en muchas APIs de Java (colecciones, comparaciones generales, etc.). De nuevo, el comportamiento correcto se selecciona dinámicamente según el objeto concreto, mostrando que el polimorfismo no es solo un tema “avanzado”, sino una característica central del lenguaje.

Por tanto, puede afirmarse que en Java **se utiliza el polimorfismo casi desde el principio**, incluso antes de estudiarlo de forma explícita. La diferencia es que, al inicio, se usa de manera implícita y natural, y más adelante se profundiza en él como herramienta de diseño, especialmente al trabajar con jerarquías de clases propias, interfaces y arquitecturas más complejas.



## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una **clase abstracta** es una clase que representa un concepto general e incompleto y que está pensada para servir como **base de otras clases**, no para ser utilizada directamente. Puede contener tanto métodos con implementación como métodos sin ella, y permite definir un comportamiento común que las subclases pueden reutilizar o especializar. Una clase abstracta **no puede instanciarse**, ya que no define por completo el comportamiento de los objetos que representa.

Un **método abstracto** es un método que **no tiene cuerpo** y solo define su firma. Su finalidad es obligar a las subclases a proporcionar una implementación concreta de ese comportamiento. Si una clase contiene al menos un método abstracto, **debe declararse también como abstracta**. De este modo, se fuerza contractualmente a que cada subclase implemente el método, garantizando que todas respondan a la misma operación, aunque de formas distintas.

No es posible crear instancias de una clase abstracta usando `new`, ya que su definición no está completa. Sin embargo, **sí pueden usarse referencias del tipo de la clase abstracta**, que apunten a objetos de sus subclases concretas, lo cual encaja perfectamente con el uso del polimorfismo. En este contexto, los métodos abstractos suelen representar acciones que dependen del tipo concreto del objeto.

En Java, la palabra clave **`abstract`** se coloca tanto en la **declaración de la clase** como en la **declaración del método abstracto**. El siguiente ejemplo redefine `Soldado` como clase abstracta e introduce el método `atacar`, que cada tipo de soldado implementa a su manera:

```java
public abstract class Soldado {

    public void saluda() {
        System.out.println("El soldado saluda de forma genérica.");
    }

    public abstract void atacar();
}
```

```java
public class Zapador extends Soldado {

    @Override
    public void atacar() {
        System.out.println("El zapador ataca colocando explosivos.");
    }
}
```

```java
public class Artillero extends Soldado {

    @Override
    public void atacar() {
        System.out.println("El artillero ataca disparando el cañón.");
    }
}
```

En este diseño, `Soldado` define **qué significa atacar**, pero no **cómo se ataca**, delegando esa responsabilidad en las subclases. La palabra clave `abstract` se utiliza, por tanto, para expresar conceptos incompletos y para combinar herencia y polimorfismo de forma segura y explícita.



## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave **`final`** en Java impone **restricciones a la herencia y a la sobrescritura**, y puede aplicarse tanto a métodos como a clases. Cuando un **método se declara como `final`**, se indica que **no puede ser sobrescrito** por las subclases. Esto garantiza que el comportamiento definido en la clase base se mantendrá exactamente igual en toda la jerarquía, evitando modificaciones accidentales o no deseadas en clases derivadas.

Cuando una **clase se declara como `final`**, se prohíbe completamente la herencia: **ninguna otra clase puede extenderla**. Como consecuencia, todos sus métodos quedan implícitamente protegidos frente a la sobrescritura. Este uso suele emplearse cuando una clase representa un concepto cerrado, completo y estable, o cuando es importante preservar invariantes internas por motivos de seguridad, diseño o rendimiento.

La relación con el **polimorfismo** es directa: marcar métodos o clases como `final` **limita el polimorfismo dinámico**. Un método `final` no puede participar en la ligadura dinámica, ya que su implementación queda fijada en la clase base. Del mismo modo, una clase `final` no puede formar parte de una jerarquía extensible, por lo que no puede haber variantes polimórficas basadas en subclases. Aun así, el uso de `final` no elimina el polimorfismo en general, sino que establece límites explícitos allí donde no se desea especialización.

Un ejemplo muy conocido de **clase `final` en la API estándar de Java** es `String`. Esta clase no puede heredarse, lo que garantiza que su comportamiento sea inmutable y seguro, especialmente en contextos como la gestión de cadenas literales, la seguridad y el uso en estructuras como mapas o conjuntos. Otros ejemplos frecuentes son las clases envoltorio (`Integer`, `Double`, `Boolean`, etc.), que también son `final` para asegurar un comportamiento consistente y predecible dentro del núcleo del lenguaje.

**Anotación:**
* `final` en clases: prohibe heredar.
* `final` en métodos: prohibe sobreescribir.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

En Java, una **interfaz** es un tipo que define un **contrato**: especifica qué métodos debe ofrecer una clase, pero no cómo se implementan (al menos en su forma clásica). Las interfaces se utilizan para expresar **capacidades o roles** que una clase puede cumplir, independientemente de su posición en una jerarquía de herencia. Una clase que implementa una interfaz se compromete a proporcionar implementaciones concretas de todos sus métodos abstractos.

Las interfaces **se parecen a las clases abstractas**, pero no son lo mismo. Ambas permiten definir métodos sin implementación y favorecen el polimorfismo, ya que se pueden usar referencias del tipo interfaz para apuntar a objetos de clases distintas. Sin embargo, una clase abstracta representa una relación “**es-un**” (herencia), mientras que una interfaz representa más bien un “**puede-hacer**”. Además, una clase abstracta puede tener estado (atributos) y constructores, mientras que una interfaz no tiene estado propio; sus campos son implícitamente `public static final`.

Una diferencia clave es que **una clase puede implementar más de una interfaz**, algo que Java no permite con las clases (no existe herencia múltiple de clases). Esto resuelve muchos problemas de diseño que en otros lenguajes se abordarían con herencia múltiple. Mediante interfaces, una clase puede combinar comportamientos esperados desde distintos puntos de vista, manteniendo un diseño flexible y desacoplado.

En términos de polimorfismo, las interfaces son una herramienta fundamental. Permiten escribir código que dependa de **qué se puede hacer** un objeto, y no de **qué es exactamente**, facilitando la extensibilidad y el uso de componentes intercambiables. Por ello, en Java moderno se fomenta a menudo “programar contra interfaces y no contra implementaciones”, especialmente en APIs, bibliotecas y diseños orientados a mantenimiento a largo plazo.

**Anotación:**
* Todos los métodos son abstractos, es decir, sin código (a partir de cierta versión de Java, deja meter una implementación "default").
* No tienen estado
* Una clase puede implementar más de una interfaz.

```java
public interface EntradaSalida{
    public String leerEntrada();

    public void escribirEnSalida(String salida);
}

public class TecladoPantalla implements EntradaSalida{

}
```



## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

Una forma clara de aplicar **polimorfismo con clases abstractas** consiste en definir un tipo general `Punto` que represente la idea de punto, pero delegue a las subclases el modo concreto de calcular distancias. Al declarar el método `calcularDistanciaA` como **abstracto**, se obliga a que cada subtipo (`Punto2D`, `Punto3D`) implemente su propia versión. El uso de `instanceof` junto con *downcasting* permite verificar en tiempo de ejecución que la distancia solo se calcule entre puntos **del mismo subtipo**, evitando incoherencias geométricas.

En este diseño, la clase abstracta `Punto` no conoce el número de dimensiones, únicamente define el contrato común. Las subclases concretas almacenan las coordenadas necesarias y aplican la fórmula adecuada. El método recibe un `Punto` genérico, comprueba su compatibilidad con `instanceof` y, si es correcto, realiza el *downcasting* para acceder a los atributos específicos de su tipo concreto.

```java
public abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}
```

```java
public class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Punto incompatible");
        }
        Punto2D p = (Punto2D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

```java
public class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Punto incompatible");
        }
        Punto3D p = (Punto3D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        double dz = this.z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}
```

Este planteamiento permite introducir ahora una clase `Linea` que **trabaja únicamente con referencias de tipo `Punto`**, sin saber si son 2D o 3D. Gracias al polimorfismo, puede delegar el cálculo de la longitud en los propios puntos, manteniéndose completamente desacoplada del número de dimensiones.

```java
public class Linea {
    private Punto inicio;
    private Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.calcularDistanciaA(fin);
    }
}
```

Así, `Linea` puede calcular correctamente su longitud independientemente de si trabaja con puntos 2D o 3D. Este ejemplo muestra cómo el polimorfismo permite diseñar código genérico y extensible, aun utilizando comprobaciones dinámicas (`instanceof`) cuando se desea mantener compatibilidad estricta entre subtipos concretos.

**Anotación:**

```java
public abstract class Punto{
    public abstract double calcularDistanciaA();
}

public class Punto2D extends Punto{
    private double x, y;

    public Punto2D(double x, double y){
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro){
        if(otro instanceof Punto2D){
            return Math.sqrt(Math.pow(this.x - otro.x, 2) + Math.pow(this.y - otro.y, 2));
        }else{
            throw new IllegalArgumentException("Diferente dimensión.");
        }
    }
}

public class Punto3D extends Punto{
    private double x, y, z;

    public Punto2D(double x, double y){
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro){
        if(otro instanceof Punto3D){
            return Math.sqrt(Math.pow(this.x - otro.x, 2) + Math.pow(this.y - otro.y, 2) + Math.pow(this.z - otro.z, 2));
        }else{
            throw new IllegalArgumentException("Diferente dimensión.");
        }
    }
}

public class Linea{
    private Punto puntoInicial, puntoFinal;

    public Linea(Punto puntoInicial, Punto puntoFinal){
        this.puntoInicial = puntoInicial;
        this.puntoFinal = puntoFinal;
    }

    public double calcularLongitud(){
        return this.puntoInicial.calcularDistanciaA(this.puntoFinal);
    }

    main{
        Linea l2d = new Linea(new Punto2D(4, 5), new Punto2D(7, 8));
        Linea l3d = new Linea(new Punto2D(4, 5, 3), new Punto2D(7, 8, 4));
    }
}
```



## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La **herencia de interfaces** en Java consiste en que una interfaz puede **extender a otra interfaz**, heredando todos sus métodos abstractos (y `default`, si existen). De este modo, se pueden construir contratos más específicos a partir de otros más generales, manteniendo una relación jerárquica entre capacidades. No se hereda implementación ni estado, sino únicamente la **obligación de ofrecer ciertos métodos**, ampliando o especializando el comportamiento esperado.

En Java **sí existe herencia múltiple de interfaces**. Una interfaz puede extender **una o varias interfaces a la vez**, y una clase puede implementar **múltiples interfaces simultáneamente**. Esto permite combinar distintos roles sin los problemas clásicos de la herencia múltiple de clases. A diferencia de las clases abstractas, las interfaces están diseñadas precisamente para favorecer esta composición de comportamientos desde distintos puntos de vista.

Un ejemplo sencillo sería una interfaz `Fichero` que define la capacidad básica de leer su contenido, y otra interfaz más específica `FicheroEscribible` que **extiende** a la anterior y añade operaciones de escritura y eliminación. El uso de `extends` en interfaces expresa claramente esta relación de especialización:

```java
public interface Fichero {
    String leerContenido();
}
```

```java
public interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}
```

En este diseño, cualquier clase que implemente `FicheroEscribible` estará obligada a implementar **todos los métodos de ambas interfaces**, tanto los heredados (`leerContenido`) como los nuevos. Esto demuestra cómo la herencia múltiple de interfaces permite construir jerarquías de contratos flexibles, favoreciendo el polimorfismo y el desacoplamiento sin recurrir a jerarquías rígidas de clases.

**Anotación:**
```java
public interface Fichero{
    String leer;
}

public interface FicheroEscribible extends Fichero{
    public void escribir(String string);
    public void eliminar();
}
```

