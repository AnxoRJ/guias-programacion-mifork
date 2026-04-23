<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

La **genericidad** busca permitir que una misma estructura de datos pueda trabajar con distintos tipos sin duplicar código. En lenguajes sin genéricos nativos (como C) o antes de introducir genéricos formales (como Java previo a `<>`), esta idea se emula tratando todos los elementos como un tipo común “general”. En C ese papel lo cumple `void*`, que puede apuntar a cualquier tipo de dato, y en Java lo cumple `Object`, la superclase de todas las clases.

En C, una estructura de datos genérica puede implementarse usando un **array primitivo de punteros** (`void*`). Cada posición del array almacena la dirección de memoria de un dato cualquiera. La estructura no conoce el tipo concreto; esta responsabilidad recae en quien inserta o extrae los datos, que debe realizar las conversiones (`cast`) adecuadas. Esta solución es flexible pero insegura a nivel de tipos, ya que errores de conversión solo se detectan en tiempo de ejecución.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    void** elementos;   // Array primitivo de punteros genéricos
    int capacidad;
    int tamaño;
} ListaGenerica;

ListaGenerica* crearLista(int capacidad) {
    ListaGenerica* lista = malloc(sizeof(ListaGenerica));
    lista->capacidad = capacidad;
    lista->tamaño = 0;
    lista->elementos = malloc(sizeof(void*) * capacidad);
    return lista;
}

void agregar(ListaGenerica* lista, void* dato) {
    lista->elementos[lista->tamaño++] = dato;
}
```

En Java, antes del uso de genéricos, se resolvía el mismo problema mediante un **array de `Object`**, ya que cualquier objeto puede almacenarse como tal. Esta técnica encaja bien con los conocimientos de clases, herencia y polimorfismo: todos los objetos comparten una interfaz común implícita (`Object`). Al recuperar un elemento es necesario hacer un *cast* explícito al tipo original, lo que nuevamente introduce riesgo de errores en tiempo de ejecución.

```java
public class ListaGenerica {
    private Object[] elementos;
    private int size = 0;

    public ListaGenerica(int capacidad) {
        elementos = new Object[capacidad];
    }

    public void add(Object obj) {
        elementos[size++] = obj;
    }

    public Object get(int index) {
        return elementos[index];
    }
}
```

Estas soluciones ilustran claramente la idea de genericidad “manual” y permiten entender por qué los genéricos modernos mejoran la seguridad de tipos. Al eliminar la necesidad de conversiones explícitas, los genéricos trasladan muchos errores potenciales del tiempo de ejecución al tiempo de compilación, manteniendo la flexibilidad mostrada en estos ejemplos.


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La **programación genérica** es un paradigma que permite definir algoritmos y estructuras de datos **independientes del tipo concreto** de los datos que manipulan. El objetivo es reutilizar código sin perder expresividad, de modo que el mismo componente pueda funcionar con distintos tipos manteniendo el mismo comportamiento lógico. Idealmente, esta independencia de tipos debería ser **verificable en tiempo de compilación**, aumentando la seguridad y reduciendo errores.

El ejemplo anterior basado en `void*` en C o en `Object` en Java **sí representa un ejemplo muy básico de programación genérica**, ya que permite almacenar y manejar datos de cualquier tipo con una única estructura de datos. En ambos casos se abstrae el tipo concreto y se trabaja con un “tipo común” que actúa como contenedor universal. Desde el punto de vista conceptual, se está aplicando genericidad porque el código no depende de un tipo específico.

Sin embargo, se trata de una **genericidad rudimentaria y no tipada**, ya que la comprobación de tipos se delega completamente al programador y se realiza en tiempo de ejecución mediante conversiones explícitas. Esto significa que el compilador no puede detectar usos incorrectos de la estructura. Por esta razón, aunque el ejemplo ilustra la idea fundamental de la programación genérica, no alcanza el nivel de los **genéricos paramétricos modernos** (como `List<T>` en Java), que ofrecen generalidad con seguridad de tipos en tiempo de compilación.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El principal problema respecto al **chequeo de tipos** al emplear `void*` en C o `Object` en Java es la **pérdida de información de tipo en tiempo de compilación**. Al almacenar los elementos como un tipo genérico común, el compilador deja de saber cuál es el tipo real de los datos, por lo que no puede verificar si las operaciones realizadas sobre ellos son correctas. Esto elimina una de las funciones más importantes del sistema de tipos: detectar errores antes de ejecutar el programa.

Como consecuencia directa, el **control de tipos se traslada al tiempo de ejecución**. Al recuperar un elemento de la estructura es necesario realizar conversiones explícitas (*casts*) al tipo esperado. Si el tipo real del objeto no coincide con el asumido, el error solo se manifestará en ejecución: en C puede provocar comportamientos indefinidos o fallos de memoria, y en Java se producirá una excepción como `ClassCastException`. Estos errores son más difíciles de detectar y depurar que los errores de compilación.

Otro problema relevante es que **no existe garantía de homogeneidad** dentro de la estructura de datos. Nada impide mezclar tipos incompatibles en la misma colección, lo que obliga a documentar cuidadosamente el uso esperado de la estructura y a confiar en la disciplina del programador. Esto reduce la robustez del código y aumenta el acoplamiento entre quien usa la estructura y su implementación.

Por último, este enfoque dificulta el **mantenimiento y la evolución del código**. Cambios en los tipos almacenados o en el uso de la estructura pueden introducir errores silenciosos que el compilador no detecta. Este conjunto de problemas justifica la introducción de genéricos paramétricos en lenguajes modernos, donde la genericidad se combina con un chequeo de tipos estático que evita estas situaciones.



## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los **parámetros de tipo** son un mecanismo de la programación genérica que permite **parametrizar clases, interfaces o métodos con uno o varios tipos**, sin especificar de antemano cuáles serán esos tipos concretos. Se introducen como identificadores formales (por ejemplo, `T`, `E`, `K`) que actúan como *marcadores de tipo*, y que se sustituyen por tipos reales cuando el componente genérico se utiliza. De este modo, el código se escribe una sola vez y se adapta a distintos tipos de forma segura.

La aportación fundamental de los parámetros de tipo es que permiten mantener la **información de tipo en tiempo de compilación**. A diferencia del uso de `void*` o `Object`, el compilador conoce el tipo concreto asociado al parámetro y puede comprobar que las operaciones realizadas son correctas. Esto evita conversiones explícitas (*casts*) y numerosos errores que, en enfoques anteriores, solo se detectaban en tiempo de ejecución.

Desde el punto de vista conceptual, los parámetros de tipo hacen posible una **genericidad fuerte y tipada**, en la que una estructura de datos genérica puede garantizar homogeneidad interna (por ejemplo, “una lista de elementos de tipo T”). El programador especifica el tipo deseado al usar la clase genérica, y el sistema de tipos se encarga de imponer esa restricción en todo el código relacionado.

En resumen, los parámetros de tipo son la base de los **genéricos paramétricos modernos**, ya que combinan reutilización, abstracción y seguridad de tipos. Gracias a ellos, la programación genérica deja de ser una técnica basada en convenciones y pasa a estar integrada de forma formal en el lenguaje, con soporte directo del compilador.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

La programación genérica se materializa de forma distinta en Java y en C++, pero en ambos casos el objetivo es el mismo: **definir contenedores reutilizables con seguridad de tipos en tiempo de compilación**. En Java se utilizan *generics* basados en **parámetros de tipo**, mientras que en C++ se emplean **templates**, que permiten generar código especializado para cada tipo concreto. En los dos lenguajes, el compilador impide insertar o usar tipos incorrectos.

En Java, una lista genérica que solo admite `String` se instancia indicando el tipo entre `< >`. El compilador garantiza que todos los elementos son `String`, por lo que no son necesarias conversiones explícitas al recorrerla. Cualquier intento de insertar otro tipo produciría un error de compilación, demostrando el chequeo estático de tipos.

```java
import java.util.ArrayList;
import java.util.List;

public class EjemploGenerics {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Programación");
        lista.add("Genérica");

        for (String s : lista) {
            System.out.println(s.toUpperCase()); // s es String con total seguridad
        }
    }
}
```

En C++, el mismo concepto se expresa mediante *templates*, usando por ejemplo `std::vector`. Al instanciar el vector con `std::string`, el compilador genera una versión del contenedor que solo admite ese tipo. Al recorrerlo, cada elemento ya es de tipo `std::string`, sin necesidad de comprobaciones adicionales ni conversiones.

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> v;

    v.push_back("Hola");
    v.push_back("Programación");
    v.push_back("Genérica");

    for (const std::string& s : v) {
        std::cout << s << std::endl; // s es std::string con seguridad
    }

    return 0;
}
```

En ambos ejemplos se observa cómo la programación genérica moderna elimina los problemas asociados a `void*` u `Object`: **el tipo concreto se conoce y se verifica en compilación**, la estructura es homogénea y el código resultante es más seguro, legible y fácil de mantener.



## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

Cuando se **instancia una clase con parámetros de tipo**, el compilador adapta la definición genérica al tipo concreto indicado por el programador y comprueba que su uso es coherente. A partir de ese momento, todas las operaciones sobre esa instancia quedan restringidas a dicho tipo, lo que permite detectar usos incorrectos durante la compilación. El código fuente genérico no cambia, pero su interpretación sí queda condicionada por los tipos reales usados en cada instanciación.

Sin embargo, **Java y C++ no hacen lo mismo internamente**. Aunque ambos ofrecen programación genérica con chequeo estático de tipos, el mecanismo de implementación es distinto. En Java, la genericidad está pensada principalmente como una ayuda al compilador, mientras que en C++ es un mecanismo de generación real de código. Esta diferencia tiene implicaciones importantes en rendimiento, expresividad y en la información de tipos disponible en tiempo de ejecución.

En Java se emplea el mecanismo conocido como **type erasure** (borrado de tipos). Durante la compilación, el compilador verifica los tipos genéricos, pero luego **elimina la información de los parámetros de tipo**, sustituyéndolos normalmente por `Object` (o por el límite superior si existe). En tiempo de ejecución no existe diferencia entre `List<String>` y `List<Integer>`, lo que garantiza compatibilidad con versiones antiguas del lenguaje, pero impide, por ejemplo, preguntar por el tipo genérico real mediante reflexión.

En C++, en cambio, se utiliza la **instanciación de plantillas** (*template instantiation*). Para cada tipo concreto usado con una plantilla, el compilador **genera una versión específica del código**, como si se hubiera escrito a mano para ese tipo. Esto hace que el tipo se mantenga completamente en compilación y ejecución, permite mayor optimización y expresividad, pero puede aumentar el tamaño del binario. En resumen, Java aplica genericidad “por comprobación” y C++ genericidad “por generación de código”.



## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

La creación de una clase con **parámetros de tipo** permite definir estructuras reutilizables capaces de almacenar valores de distintos tipos de forma **segura**, conservando el chequeo de tipos en tiempo de compilación. Una clase `Par<T, U>` representa un contenedor que aloja dos valores, cada uno con su propio tipo, sin necesidad de recurrir a `Object` ni a conversiones explícitas. Este patrón es habitual para modelar retornos múltiples de una función.

En Java, la clase `Par` se define declarando dos parámetros de tipo formales y usándolos como tipos de los atributos. El constructor recibe valores de ambos tipos y los métodos *getter* devuelven cada uno de ellos con su tipo concreto. El compilador garantiza que los valores almacenados y recuperados respetan los tipos indicados al instanciar la clase.

```java
public class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}
```

Un uso típico de esta clase es como **tipo de retorno** de una función que necesita devolver más de un resultado. Por ejemplo, una función que calcule la media y la desviación típica de un array de `double` puede devolver ambos valores empaquetados en un `Par<Double, Double>`. El código cliente recibe el resultado con tipos completamente conocidos y sin *casts*.

```java
public class Estadistica {

    public static Par<Double, Double> mediaYDesviacion(double[] datos) {
        double suma = 0.0;
        for (double d : datos) {
            suma += d;
        }
        double media = suma / datos.length;

        double sumaCuadrados = 0.0;
        for (double d : datos) {
            sumaCuadrados += Math.pow(d - media, 2);
        }
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] valores = {2.0, 4.0, 6.0, 8.0};
        Par<Double, Double> resultado = mediaYDesviacion(valores);

        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación típica: " + resultado.getSegundo());
    }
}
```

Este ejemplo muestra cómo los parámetros de tipo permiten expresar con claridad la intención del código, mejorar la legibilidad y evitar errores de tipo, manteniendo una solución genérica y reutilizable.



## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

En Java, un **método genérico** declara sus propios parámetros de tipo de forma independiente a la clase que lo contiene. Esto permite escribir métodos reutilizables y **con seguridad de tipos**, incluso dentro de clases no genéricas. El parámetro de tipo se declara antes del tipo de retorno del método y se usa para expresar relaciones de tipos entre los parámetros y el valor devuelto.

Si el método se define usando `Object`, se pierde información de tipo. El método puede devolver cualquier objeto, pero quien lo llama debe hacer *downcasting*, y no existe ninguna garantía de que ambos argumentos sean del mismo tipo. El compilador acepta combinaciones incoherentes, dejando los errores para el tiempo de ejecución.

```java
import java.util.Random;

public class EjemploObject {

    public static Object seleccionaUno(Object a, Object b) {
        return new Random().nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        String s = (String) seleccionaUno("hola", "adiós"); // requiere cast
        Object o = seleccionaUno("hola", 10); // permitido, pero peligroso
    }
}
```

Al definir el método con un **parámetro de tipo**, el compilador puede comprobar dos aspectos clave: (i) el valor devuelto es exactamente del tipo esperado, evitando *downcasting*, y (ii) ambos argumentos deben ser del **mismo tipo**, ya que comparten el mismo parámetro de tipo formal. Cualquier uso incorrecto se detecta en compilación.

```java
import java.util.Random;

public class EjemploGenerico {

    public static <T> T seleccionaUno(T a, T b) {
        return new Random().nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        String s = seleccionaUno("hola", "adiós"); // sin cast
        // seleccionaUno("hola", 10); // error de compilación
    }
}
```

En resumen, el uso de **parámetros de tipo a nivel de método** aporta seguridad y expresividad: elimina conversiones explícitas y fuerza relaciones de tipo correctas entre los parámetros. Frente a `Object`, el método genérico es más robusto, más legible y coherente con los objetivos de la programación genérica moderna.



## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Sí, en Java es posible **establecer restricciones en los parámetros de tipo** mediante *bounded type parameters*. Esto permite indicar que un parámetro genérico debe ser, como mínimo, una subclase de una clase concreta o implementar una interfaz determinada. De este modo, el compilador garantiza que sobre el tipo genérico se pueden aplicar las operaciones definidas en ese límite. Para el caso de valores numéricos, se puede imponer que el parámetro de tipo extienda `Number`, lo que habilita el uso de métodos como `doubleValue()` con seguridad en tiempo de compilación.

Una **primera solución**, sin usar genéricos, consiste en definir las coordenadas directamente como `Number`. De esta forma, el punto puede trabajar con cualquier tipo numérico (`Integer`, `Double`, `Float`, etc.), pero se pierde información sobre el tipo concreto y se mezcla esa flexibilidad con menor precisión en el chequeo de tipos. El compilador solo sabe que se trata de algún número, no de cuál.

```java
public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Una **segunda solución**, más robusta, utiliza **genéricos con restricción** para expresar explícitamente el tipo numérico con el que trabaja cada instancia de `Punto`. Aquí se emplea `<T extends Number>`, lo que fuerza que ambas coordenadas sean del mismo tipo y permite al compilador verificar esa coherencia. Además, el código cliente sabe exactamente si está trabajando con `Double`, `Integer`, etc.

```java
public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Respecto al **type erasure**, en ambos casos el tipo genérico **se elimina tras la compilación**. En la versión genérica, `T` se sustituye por su límite superior, es decir, `Number`. Por tanto, el tipo final generado por el compilador es equivalentemente un `Punto` que trabaja con `Number`, aunque el chequeo estricto de tipos ya se haya realizado en tiempo de compilación. La ventaja de los genéricos no está en el bytecode final, sino en la **seguridad y expresividad añadidas durante la compilación**.



## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

Ambas soluciones permiten reutilizar la clase `Punto` para distintos tipos numéricos, pero **no refuerzan el chequeo de tipos al mismo nivel**. En la solución sin genéricos, al usar directamente `Number`, **sí es posible** crear un punto con una coordenada entera y otra real, por ejemplo `new Punto(3, 4.5)`. El compilador lo acepta porque ambas coordenadas cumplen ser subclases de `Number`, aunque conceptualmente se esté mezclando el tipo numérico de cada eje sin control adicional.

En cambio, la solución con genéricos (`Punto<T extends Number>`) **no permite** esa mezcla. Al instanciar la clase, se fija un único tipo concreto para `T`, por ejemplo `Punto<Integer>` o `Punto<Double>`. Esto fuerza que **ambas coordenadas sean del mismo tipo numérico**, y cualquier intento de mezclar (`Integer` y `Double`) provoca un error de compilación. Aquí los genéricos expresan una restricción semántica que no puede representarse usando solo `Number`.

Respecto a los métodos de acceso, en la solución sin genéricos el método `getX` devuelve siempre un `Number`. Esto obliga al código cliente a trabajar al nivel de abstracción de `Number` y, si necesita un tipo concreto, a realizar conversiones explícitas. El tipo preciso del dato se pierde desde el punto de vista del compilador, aunque esté presente en tiempo de ejecución.

En la solución con genéricos, `getX` devuelve el **tipo genérico `T`**, es decir, exactamente el tipo con el que se parametrizó el `Punto`. Esto permite al compilador conocer y verificar el tipo concreto (`Integer`, `Double`, etc.) sin *casts*. Aunque tras la compilación Java aplique *type erasure* y el tipo resultante sea `Number`, el **chequeo reforzado ya se ha producido en tiempo de compilación**, que es precisamente la ventaja clave de los genéricos.



## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

Para reforzar el chequeo de tipos y evitar tanto el uso de `instanceof` como el *downcasting*, se puede aplicar **polimorfismo acotado con genéricos** (*F-bounded polymorphism*). La idea consiste en parametrizar la interfaz `Punto` con su propio subtipo concreto, de forma que el método `distanciaA` solo acepte puntos del **mismo tipo exacto**. Así, la relación de tipos queda expresada en la firma del método y verificada por el compilador.

La interfaz se redefine introduciendo un parámetro de tipo que se acota a sí mismo. De este modo, cualquier clase que implemente `Punto<T>` se compromete a que `T` sea su propio tipo. El contrato garantiza que la distancia siempre se calcula entre puntos homogéneos (2D con 2D, 3D con 3D).

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

La implementación para `Punto2D` fija el parámetro genérico a su propio tipo. La sobreescritura del método es ahora completamente segura: el parámetro es necesariamente un `Punto2D`, sin comprobaciones dinámicas ni conversiones explícitas. Cualquier intento de pasar un `Punto3D` provocaría un error de compilación.

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(
            Math.pow(x - p.x, 2) +
            Math.pow(y - p.y, 2)
        );
    }
}
```

Análogamente, `Punto3D` queda restringido a operar exclusivamente con otros `Punto3D`. El diseño expresa en el sistema de tipos una regla que antes solo podía comprobarse en tiempo de ejecución. Aunque tras la compilación Java aplique *type erasure* y los tipos genéricos se borren, el **refuerzo del chequeo ya se ha realizado en compilación**, que es precisamente la ventaja buscada.

```java
public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(
            Math.pow(x - p.x, 2) +
            Math.pow(y - p.y, 2) +
            Math.pow(z - p.z, 2)
        );
    }
}
```

Este enfoque convierte un posible error en ejecución en un **error de compilación**, mejora la claridad del diseño y utiliza los genéricos no solo para reutilización, sino para **expresar invariantes semánticos** del dominio del problema.



## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

Aunque `String` sea subtipo de `Object`, **no significa que `List<String>` sea subtipo de `List<Object>`**. En Java, los tipos genéricos son **invariantes** respecto a su parámetro de tipo: `List<String>` y `List<Object>` son tipos no relacionados entre sí. Permitir esa relación rompería la seguridad de tipos, ya que a una `List<Object>` se le podrían añadir objetos que no fuesen `String`, violando la restricción original de la lista de cadenas.

En cambio, **`String[]` sí es subtipo de `Object[]`**. Los arrays en Java son **covariantes**, lo que significa que si `String` es subtipo de `Object`, entonces `String[]` se considera subtipo de `Object[]`. Esta decisión se tomó por compatibilidad histórica, pero introduce un problema grave: el error no se detecta en compilación, sino en ejecución. Por ejemplo, se puede asignar un `String[]` a una referencia `Object[]` y luego intentar almacenar un `Integer`, lo que compila pero lanza una `ArrayStoreException` en tiempo de ejecución.

```java
String[] arr = new String[2];
Object[] objArr = arr;      // permitido (covarianza)
objArr[0] = 10;             // error en tiempo de ejecución
```

Este contraste explica por qué los genéricos no son covariantes por defecto. A partir de aquí, se definen tres conceptos clave: un tipo genérico es **covariante** si `G<Sub>` es subtipo de `G<Super>` (como los arrays en Java); es **contravariante** si ocurre lo contrario (`G<Super>` es subtipo de `G<Sub>`); y es **invariante** si no existe relación de subtipo entre `G<Sub>` y `G<Super>`, que es el caso de los genéricos en Java (`List<T>`).

En resumen, los arrays priorizan flexibilidad a costa de seguridad en tiempo de ejecución, mientras que los genéricos priorizan **seguridad de tipos en compilación**. Por ello, Java prefiere la invariancia en los genéricos y solo permite variancia de forma controlada mediante comodines (`? extends`, `? super`), evitando así los errores que en los arrays solo pueden detectarse en ejecución.



## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un **wildcard** (`?`) en Java representa un **tipo desconocido** dentro de un tipo genérico y se utiliza para introducir **variancia controlada**. En lugar de fijar un tipo concreto como `List<Integer>`, se permite expresar relaciones de subtipado de forma segura. Los wildcards no indican “cualquier tipo”, sino “algún tipo que cumple una determinada relación con otro”, y su función principal es permitir leer o escribir colecciones sin romper el chequeo estático de tipos.

La diferencia clave está entre `? extends T` y `? super T`. Con `List<? extends T>` se expresa **covarianza**: la lista contiene elementos de *alguna subclase de `T`*. Es adecuada cuando el código **solo necesita leer** elementos como `T`, no modificarlos, ya que no se puede garantizar qué subtipo concreto contiene la lista. En cambio, con `List<? super T>` se expresa **contravarianza**: la lista admite elementos de tipo `T` o de cualquiera de sus supertipos, lo que resulta adecuada cuando el código **necesita escribir (insertar)** valores de tipo `T` en la colección.

Un ejemplo típico de uso de `? extends` es un método que **lee números** para calcular su suma. El método acepta listas de `Integer`, `Double`, `Float`, etc., y trata todos los elementos como `Number`. No se añaden elementos a la lista; únicamente se consumen.

```java
public static double sumarLista(List<? extends Number> lista) {
    double suma = 0.0;
    for (Number n : lista) {
        suma += n.doubleValue();
    }
    return suma;
}
```

Por otro lado, `? super` se emplea cuando el método **produce** elementos y los inserta en la lista. En el siguiente ejemplo se añaden enteros a una lista que puede ser de `Integer`, `Number` u `Object`. El método no necesita conocer el tipo exacto de los elementos ya presentes; solo necesita garantizar que insertar `Integer` es seguro.

```java
public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}
```

En conjunto, los wildcards permiten expresar correctamente la intención del código: **“PECS” (Producer Extends, Consumer Super)**. Si una estructura produce valores, se usa `extends`; si los consume, se usa `super`. Así se recupera covarianza y contravarianza sin perder la seguridad de tipos que caracteriza a los genéricos en Java.

