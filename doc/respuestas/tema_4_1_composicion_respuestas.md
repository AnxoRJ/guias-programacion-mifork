<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

La **composición** en C se refleja de forma natural con `struct`: una estructura “tiene-un” (o “tiene-varios”) campos que, a su vez, pueden ser otras estructuras. En este ejemplo, un **Punto** tiene dos coordenadas (`x`, `y`) y una **Línea** “tiene-dos” puntos (sus extremos). Este patrón modela relaciones de pertenencia sin necesidad de orientación a objetos, y es perfectamente compatible con el manejo de funciones que operan sobre dichas estructuras, como calcular la distancia entre dos puntos o la longitud de una línea.

A continuación se muestra una implementación simple en C. Se define `struct Punto` y `struct Linea`, junto con dos funciones: `distancia_puntos` (usa la fórmula euclídea √((x2−x1)² + (y2−y1)²)) y `longitud_linea` (que no es más que la distancia entre los extremos de la línea). Se incluye un `main` de ejemplo para ilustrar su uso; puede omitirse si solo se requieren las definiciones y funciones.

```c
#include <stdio.h>
#include <math.h>

/* Un punto en 2D */
struct Punto {
    double x;
    double y;
};

/* Una línea compuesta por dos puntos: inicio y fin */
struct Linea {
    struct Punto a;
    struct Punto b;
};

/* Distancia euclídea entre dos puntos */
double distancia_puntos(struct Punto p1, struct Punto p2) {
    double dx = p2.x - p1.x;
    double dy = p2.y - p1.y;
    return sqrt(dx*dx + dy*dy);
    /* Alternativa numéricamente robusta: return hypot(dx, dy); */
}

/* Longitud de una línea: distancia entre sus extremos */
double longitud_linea(struct Linea l) {
    return distancia_puntos(l.a, l.b);
}

/* Ejemplo de uso */
int main(void) {
    struct Punto p1 = { .x = 1.0, .y = 2.0 };
    struct Punto p2 = { .x = 4.0, .y = 6.0 };
    struct Linea l = { .a = p1, .b = p2 };

    printf("Distancia P1-P2: %.3f\n", distancia_puntos(p1, p2));
    printf("Longitud de la línea: %.3f\n", longitud_linea(l));
    return 0;
}
```

Este diseño permite extender la composición fácilmente: por ejemplo, una polilínea “tiene-varios” puntos (un arreglo dinámico o estático de `struct Punto`), y su longitud total sería la suma de las distancias entre puntos consecutivos. En C, además, puede pasarse por valor como arriba (cómodo y claro) o por puntero constante (`const struct Punto*`) si se desea evitar copias en estructuras mayores; con puntos pequeños de tipo `double`, el coste de copiar suele ser despreciable y la versión por valor mantiene la interfaz simple.

**Anotación:**
```c
struct Punto{
    double x;
    double y;
}

struct Linea{
    struct Punto p1;
    struct Punto p2
}

double distancia(struct Punto p1, struct Punto p2){

}

double longitud(struct Linea l){
    return distancia(l.p1, l.p2);
}
```


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

La **composición** en orientación a objetos permite modelar que una `Linea` “tiene-dos” `Punto`. Gracias a la **ocultación de información** (encapsulación) y al diseño **inmutable**, se garantiza que ni las coordenadas de un `Punto` ni los extremos de una `Linea` puedan modificarse después de creados. Esto se logra haciendo sus campos `private final`, sin métodos *setter*, validando en el constructor, y devolviendo únicamente vistas seguras a través de *getters*. Al ser `Punto` inmutable, `Linea` puede exponer referencias directas a sus extremos sin riesgo de modificaciones externas.

En este ejemplo, `Punto` ofrece el método `distanciaA(Punto otro)` usando `Math.hypot` (más robusto numéricamente que la raíz cuadrada de sumas de cuadrados en algunos casos). Por su parte, `Linea` encapsula dos `Punto` finales (`a` y `b`) y calcula su longitud como la distancia entre ambos. La inmutabilidad simplifica el razonamiento y mejora la seguridad: una vez creada una línea, no cambia “de qué a qué” puntos va. Se incluyen validaciones de no-nulidad para mantener objetos siempre en estados válidos.

```java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    /** Distancia euclídea a otro punto. */
    public double distanciaA(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto objetivo no puede ser null");
        }
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.hypot(dx, dy); // Robusto numéricamente
    }
}

public final class Linea {
    private final Punto a;
    private final Punto b;

    public Linea(Punto a, Punto b) {
        if (a == null || b == null) {
            throw new IllegalArgumentException("Los puntos de la línea no pueden ser null");
        }
        this.a = a; // Seguro porque Punto es inmutable
        this.b = b;
    }

    public Punto getA() { return a; }
    public Punto getB() { return b; }

    /** Longitud de la línea: distancia entre sus extremos. */
    public double longitud() {
        return a.distanciaA(b);
    }
}

// Ejemplo de uso
class Demo {
    public static void main(String[] args) {
        Punto p1 = new Punto(1.0, 2.0);
        Punto p2 = new Punto(4.0, 6.0);
        Linea l = new Linea(p1, p2);

        System.out.printf("Distancia p1-p2: %.3f%n", p1.distanciaA(p2));
        System.out.printf("Longitud de la línea: %.3f%n", l.longitud());
    }
}
```

Este diseño puede ampliarse de forma natural: por ejemplo, una `Polilinea` inmutable podría componer una lista inmutable de `Punto` y calcular su longitud sumando distancias consecutivas. Si en el futuro se quisieran optimizaciones (p. ej., *caching* de la longitud), podrían añadirse campos `transient` o cálculos perezosos sin romper la inmutabilidad observable. La clave es mantener los estados válidos únicamente desde los constructores y exponer solo operaciones que no mutan el estado interno.

**Anotación:**
```java
class Punto{
    private double x;
    private double y;

    public Punto(double x, double y){

    }

    public distancia(Punto p2){
        return Math.sqrt(this.x - p2.x ...);
    }
}

class Linea{
    private Punto p1;
    private Punto p2;

    public Linea(Punto p1, Punto p2){

    }

    public longitud(){
        return this.p1.distancia(this.p2);
    }
}
```



## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La **multiplicidad** en composición describe *cuántas instancias* de una clase están asociadas con *cuántas instancias* de otra dentro de una relación “tiene‑un/tiene‑varios”. En UML suele representarse con números como **1**, **0..1**, **0..***, **1..***, etc., indicando cuántos objetos de un tipo participan respecto del otro. En relaciones de composición, estas multiplicidades reflejan además una pertenencia fuerte: la “parte” no existe sin el “todo” o se considera conceptualmente subordinada a él.

En el ejemplo anterior, una `Linea` está compuesta **exactamente por dos** objetos `Punto` que representan sus extremos. Esto implica que, desde el punto de vista de `Linea`, la multiplicidad hacia `Punto` es **2** (o también **2..2** si se quiere ser explícito). Dicho de otra forma: *cada `Linea` tiene exactamente dos `Punto`* y no podría existir con un número distinto, lo que refuerza la idea de composición rígida.

Visto desde el otro lado, cada `Punto` puede participar en **ninguna, una o varias** líneas. No existe una restricción en la clase `Punto` que impida compartirlo entre líneas distintas; de hecho, la clase es inmutable, así que usar la misma instancia en varias líneas no genera problemas. Por tanto, desde `Punto` hacia `Linea`, la multiplicidad es **0..**\*. Esto expresa que un punto puede no pertenecer a ninguna línea o bien servir como extremo de todas las que se desee.

Resumiendo la multiplicidad en ambas direcciones:

*   **De `Linea` a `Punto`:** **2 o 2.2** (una línea se relaciona con dos Puntos y como máximo con dos Puntos).
*   **De `Punto` a `Linea`:** **0..**\* (un punto se relaciona como mínimo con cero Líneas y como máximo con muchas Lineas).

Si en un futuro se modelaran polilíneas o figuras más complejas, estas multiplicidades cambiarían: una polilínea tendría una multiplicidad 2..\* hacia `Punto`, mientras que un punto seguiría pudiendo pertenecer a muchas estructuras superiores.




## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La **composición fuerte** describe una relación en la que el objeto “todo” es dueño exclusivo de sus partes y controla completamente su ciclo de vida. En este tipo de relación, si el objeto contenedor deja de existir, sus partes también desaparecen conceptualmente y no se reutilizan en ningún otro contexto. Esto implica dependencia vital estricta: las partes no solo se crean para el todo, sino que además no tienen sentido fuera de él.

Por el contrario, la **composición débil** describe una relación donde el objeto “todo” agrupa o utiliza objetos que existen de forma independiente. En esta situación, las partes pueden tener un ciclo de vida propio: pueden existir antes, durante y después del objeto contenedor sin que su existencia dependa de él. No hay propiedad exclusiva ni destrucción conjunta: las partes pueden compartirse entre varios objetos o reutilizarse libremente.

Desde el punto de vista terminológico, la composición débil suele denominarse **asociación o agregación**, porque describe una relación de uso o agrupación sin implicar pertenencia vital. La composición fuerte recibe simplemente el nombre de **composición**, ya que expresa la forma más estricta de relación parte‑todo, donde se entiende que las partes no existen conceptualmente sin el conjunto. En UML, agregación se representa con un rombo blanco y composición con un rombo negro, destacando esta diferencia en la intensidad del vínculo.

Como consecuencia práctica, la elección entre composición fuerte y débil influye en cómo se construyen, validan y gestionan los objetos. Un diseño orientado a composición fuerte tiende a favorecer inmutabilidad y control claro de estados, mientras que la composición débil encaja mejor con objetos compartibles, reutilizables o gestionados mediante servicios externos. En ambos casos, la multiplicidad y la semántica del dominio guían la elección adecuada.

**Anotación:**
* **Fuerte:** el contenedor (P. ej. Linea) es el que crea los objetos que contiene (P.ej. Punto) y estos no van más allá del contenedor.
* * El ciclo de vida del contenido está vinculado al contenedor.

* **Débil:** el contenedor y contenido tienen ciclos de vida independientes(P.ej. los objetos Punto pueden vivir sin estar en objeto Linea).

Se representan graficamente con un rombo oscuro y vacio respectivamente.



## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

En ese caso no se habla de **composición**, sino de **dependencia**. La dependencia es la relación más débil entre clases: expresa simplemente que una clase *usa* a otra de manera puntual para realizar alguna operación, sin que ello implique pertenencia, responsabilidad sobre su ciclo de vida ni parte‑todo. Este uso puede producirse al recibir un objeto como parámetro, al devolverlo, al crear instancias dentro de métodos o al emplearlo como variable local en una operación concreta.

La composición, en cambio, implica que una clase contiene y gestiona objetos que forman parte de su estructura interna y cuya existencia está ligada de forma fuerte o débil al objeto contenedor. En la dependencia no existe tal vínculo permanente: la clase solo necesita a la otra mientras ejecuta una operación concreta, y fuera de ese contexto los objetos no están asociados estructuralmente. Por eso se suele describir la dependencia como una relación “usa‑a” o “conoce‑a”, en contraste con la composición, que es una relación “tiene‑un/tiene‑varios”.

En términos de diseño, reconocer una dependencia es útil para mantener clases poco acopladas y con responsabilidades claras. Una clase puede depender de muchas otras sin que ello implique que su estructura de datos incluya instancias de ellas. Por ejemplo, un método que recibe un `Punto` para calcular algo puntual establece una dependencia, pero no una composición, porque el `Punto` no forma parte del estado interno del objeto ni su ciclo de vida está controlado por él.

En resumen, cuando una clase interactúa con otra solo durante la ejecución de métodos —como parámetros, valores de retorno, variables locales o instancias temporales— se habla estrictamente de **dependencia**, no de composición.

**Anotación:**
Ejemplo de dependencia, no de composición.

```java
class Punto{
    public String toString(){
        StringBuider sb = new StringBuilder();
    }
}
class OperadorFichero{
    public static String leeFichero(Punto p)
}
```
Punto depende de String y StringBuilder.



## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

A continuación se programan dos variantes de la relación entre `Linea` y `Punto` en Java. En **composición fuerte**, `Linea` *posee* sus puntos y controla su ciclo de vida: se copian los datos recibidos y no se exponen referencias internas (se devuelven copias defensivas). Así se garantiza que los extremos de la línea no se comparten ni se reutilizan fuera del “todo”. En **composición débil** (agregación), `Linea` solo *agrupa/usa* puntos existentes: guarda y expone las mismas referencias, permitiendo que un mismo `Punto` participe en varias líneas y que su ciclo de vida sea independiente.

En ambos casos, `Punto` se mantiene inmutable para simplificar el razonamiento y evitar efectos colaterales. Las validaciones de no‑nulidad se realizan en los constructores, y se usan `private final` sin *setters*. La diferencia clave recae en `Linea`: en la versión fuerte se **clonan/copias** las coordenadas y se devuelven **copias** en los *getters*; en la versión débil se **almacenan** y **devuelven** las **mismas referencias** recibidas.

### Variante 1: **Composición fuerte** (el ciclo de vida de los puntos depende de `Linea`)

```java
import java.util.Objects;

public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    /** Distancia euclídea a otro punto. */
    public double distanciaA(Punto otro) {
        Objects.requireNonNull(otro, "El punto destino no puede ser null");
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.hypot(dx, dy);
    }
}

/**
 * Composición fuerte: Linea "posee" sus puntos.
 * - Copia los puntos (no comparte referencias externas).
 * - No expone estado interno: getters devuelven copias defensivas.
 */
public final class LineaFuerte {
    private final Punto a; // estado interno propio (copias)
    private final Punto b;

    /** Constructor a partir de dos puntos (se copian las coordenadas). */
    public LineaFuerte(Punto a, Punto b) {
        Objects.requireNonNull(a, "El punto A no puede ser null");
        Objects.requireNonNull(b, "El punto B no puede ser null");
        // Copia defensiva: se rompe cualquier aliasing con el exterior
        this.a = new Punto(a.getX(), a.getY());
        this.b = new Punto(b.getX(), b.getY());
    }

    /** Alternativa: constructor desde coordenadas primitivas. */
    public LineaFuerte(double ax, double ay, double bx, double by) {
        this.a = new Punto(ax, ay);
        this.b = new Punto(bx, by);
    }

    /** Devuelve una copia para no exponer el estado interno. */
    public Punto getA() { return new Punto(a.getX(), a.getY()); }
    public Punto getB() { return new Punto(b.getX(), b.getY()); }

    /** Longitud de la línea: distancia entre sus extremos internos. */
    public double longitud() { return a.distanciaA(b); }
}
```

### Variante 2: **Composición débil (agregación)** (el ciclo de vida de los puntos es independiente)

```java
import java.util.Objects;

/**
 * Se reutiliza la misma clase Punto inmutable.
 * En agregación, compartir instancias es seguro al ser inmutable.
 */

/**
 * Composición débil (agregación): Linea referencia puntos existentes.
 * - No copia: guarda y expone las mismas referencias recibidas.
 * - Un Punto puede pertenecer a 0..* líneas sin dependencia vital.
 */
public final class LineaDebil {
    private final Punto a; // referencias compartibles
    private final Punto b;

    public LineaDebil(Punto a, Punto b) {
        this.a = Objects.requireNonNull(a, "El punto A no puede ser null");
        this.b = Objects.requireNonNull(b, "El punto B no puede ser null");
    }

    /** Getters devuelven las mismas referencias (no hay copias). */
    public Punto getA() { return a; }
    public Punto getB() { return b; }

    public double longitud() { return a.distanciaA(b); }
}
```

**Diferencia práctica:** en `LineaFuerte`, si se crea con `new LineaFuerte(p1, p2)`, los objetos internos son *copias* de `p1` y `p2`; por tanto, no se comparten con otras líneas y `getA()/getB()` devuelven **nuevas** instancias cada vez. En `LineaDebil`, en cambio, `new LineaDebil(p1, p2)` guarda exactamente `p1` y `p2`; el mismo `Punto` puede formar parte de muchas líneas y los *getters* devuelven esas referencias, reflejando que el ciclo de vida de los puntos no está ligado al de la línea.

**Anotación:**

### **Composición fuerte: **
```java
class Linea{
    private Punto1;
    private Punto2;

    public Linea(double x1, double x2, double y1, double y2){
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}
```



## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java, incluso en un caso de **composición fuerte**, el contenedor *no destruye explícitamente* sus objetos parte. Esto contrasta con lenguajes donde la destrucción es manual, como C++. En Java, **la destrucción no forma parte del modelo de programación** porque la liberación de memoria está gestionada por el **Garbage Collector (GC)**. Por tanto, aunque conceptualmente se diga que “la vida de los puntos depende de la línea”, no existe un código explícito donde `Linea` destruya los `Punto`.

En términos prácticos, la destrucción llega cuando **no quedan referencias accesibles** a los objetos. En composición fuerte, esto suele ocurrir cuando el contenedor se vuelve inalcanzable: al desaparecer la única referencia a `Linea`, sus campos privados `Punto` también quedan sin referencias externas y pasan a ser candidatos para el GC. El recolector detecta que ya no hay camino desde el *root set* a esos objetos y recupera la memoria, lo que materializa la “destrucción” implícita.

Este comportamiento explica por qué no se escribe código en `Linea` para eliminar manualmente sus puntos. En Java, el ciclo de vida se maneja mediante **alcance de referencias**, no mediante llamadas explícitas a destructores. La composición fuerte se expresa entonces en términos conceptuales: el contenedor es el único dueño de sus partes, y solo él mantiene referencias a ellas. Una vez que el contenedor ya no existe lógicamente (porque no hay referencias hacia él), sus partes se vuelven automáticamente inaccesibles.

En definitiva, en Java la relación de composición fuerte no se implementa mediante destrucción manual, sino mediante **encapsulación de referencias** y **inaccesibilidad automática**. El GC se encarga del resto, liberando memoria cuando ya no puede alcanzarse ni el contenedor ni sus componentes. Así, el diseño expresa la dependencia vital, mientras que el lenguaje evita la gestión manual del ciclo de vida.

**Anotación:**

En java, la vida de Punto termina cuando es inaccesible, y en el ejemplo, ocurre cuando Linea deja de serlo a su vez. Por tanto, cuando Linea "es basura", también lo serán sus puntos y serán eliminando de memoria por el recolector de basura.



## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

Se muestra una **composición débil (agregación)** entre un `Departamento` y sus `Profesor`. El departamento *usa/agrupa* profesores ya existentes (no los “posee”), manteniendo referencias sin copiar ni “destruir” objetos; por tanto, su ciclo de vida es independiente. Se implementan dos relaciones a la vez: (1) el departamento tiene **varios** profesores (hasta 50) y (2) el departamento tiene **un** director que **siempre** debe pertenecer a la lista de profesores. Se garantiza la **invariante** con validaciones y excepciones: el director existe desde la construcción, no puede cambiarse a alguien externo al departamento, y no se permite eliminar al profesor que es director hasta que se designe otro director. No se expone la estructura interna (el array), solo una API: `numeroProfesores()`, `profesorEn(pos)`, `anadirProfesor(p)`, `eliminarProfesorEn(pos)` y `setDirector(p)`.

Para no romper la encapsulación, se usa internamente un `Profesor[]` de longitud fija (máximo 50) y un contador; desde fuera no se revela que la estructura sea un array. La adición inserta **al final**; la eliminación por posición compacta el array sin dejar huecos. Se previenen `null`, desbordes de capacidad y duplicados por **identidad** de referencia (si se desea, podría cambiarse a igualdad por identificador). Al cambiar el director, se exige que el nuevo ya pertenezca al departamento; al eliminar, si el índice apunta al director, se lanza excepción para que el cliente cambie antes la dirección.

```java
import java.util.Objects;

public final class Profesor {
    private final String id;     // p. ej., DNI o código interno
    private final String nombre;

    public Profesor(String id, String nombre) {
        this.id = Objects.requireNonNull(id, "id no puede ser null");
        this.nombre = Objects.requireNonNull(nombre, "nombre no puede ser null");
    }

    public String getId()     { return id; }
    public String getNombre() { return nombre; }

    @Override public String toString() {
        return nombre + " [" + id + "]";
    }
}

public final class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final Profesor[] profesores = new Profesor[MAX_PROFESORES];
    private int numProfesores = 0;
    private Profesor director; // Debe estar siempre en 'profesores'

    /** Siempre arranca con un director, que además pasa a la lista de profesores. */
    public Departamento(Profesor director) {
        this.director = Objects.requireNonNull(director, "director no puede ser null");
        profesores[numProfesores++] = director;
    }

    /** Nº de profesores del departamento. */
    public int numeroProfesores() {
        return numProfesores;
    }

    /** Acceso por posición (0..n-1). No se expone la estructura interna. */
    public Profesor profesorEn(int posicion) {
        comprobarRango(posicion);
        return profesores[posicion];
    }

    /** Director actual (pertenece a la lista). */
    public Profesor getDirector() {
        return director;
    }

    /** Cambia el director por otro profesor que ya esté en el departamento. */
    public void setDirector(Profesor nuevoDirector) {
        Objects.requireNonNull(nuevoDirector, "nuevoDirector no puede ser null");
        if (indiceDe(nuevoDirector) < 0) {
            throw new IllegalArgumentException(
                "El nuevo director debe pertenecer al departamento");
        }
        this.director = nuevoDirector;
    }

    /** Añade al final. No admite null, duplicados (por identidad) ni sobrepasar 50. */
    public void anadirProfesor(Profesor p) {
        Objects.requireNonNull(p, "profesor no puede ser null");
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("Capacidad máxima (" + MAX_PROFESORES + ") alcanzada");
        }
        if (indiceDe(p) >= 0) {
            throw new IllegalArgumentException("El profesor ya pertenece al departamento");
        }
        profesores[numProfesores++] = p;
    }

    /**
     * Elimina por posición y devuelve el profesor eliminado.
     * Si es el director, se exige cambiar la dirección antes (invariante).
     */
    public Profesor eliminarProfesorEn(int posicion) {
        comprobarRango(posicion);
        Profesor eliminado = profesores[posicion];
        if (eliminado == director) {
            throw new IllegalStateException(
                "No se puede eliminar al director actual; cámbiese primero el director");
        }
        // Compactación: desplazar a la izquierda
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--numProfesores] = null; // ayuda al GC
        return eliminado;
    }

    // ----------------- utilidades privadas -----------------

    private int indiceDe(Profesor p) {
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == p) { // identidad de referencia
                return i;
            }
        }
        return -1;
    }

    private void comprobarRango(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException(
                "Posición fuera de rango: " + posicion + " (0.." + (numProfesores - 1) + ")");
        }
    }
}

// ----------------- Ejemplo de uso -----------------
class Demo {
    public static void main(String[] args) {
        Profesor p1 = new Profesor("111A", "Ana");
        Profesor p2 = new Profesor("222B", "Berto");
        Profesor p3 = new Profesor("333C", "Carmen");

        Departamento dpto = new Departamento(p1); // p1 es director y profesor
        dpto.anadirProfesor(p2);
        dpto.anadirProfesor(p3);

        // Cambiar director a alguien del propio departamento:
        dpto.setDirector(p3);

        // Intento de eliminar al director: lanza excepción
        try {
            int posDirector = buscarPosicion(dpto, dpto.getDirector());
            dpto.eliminarProfesorEn(posDirector); // IllegalStateException
        } catch (IllegalStateException ex) {
            System.out.println("Correcto: " + ex.getMessage());
        }

        // Eliminar a otro profesor y consultar
        int posBerto = buscarPosicion(dpto, p2);
        dpto.eliminarProfesorEn(posBerto);
        System.out.println("Profesores restantes: " + dpto.numeroProfesores());
        for (int i = 0; i < dpto.numeroProfesores(); i++) {
            System.out.println(" - " + dpto.profesorEn(i));
        }
        System.out.println("Director: " + dpto.getDirector());
    }

    private static int buscarPosicion(Departamento d, Profesor objetivo) {
        for (int i = 0; i < d.numeroProfesores(); i++) {
            if (d.profesorEn(i) == objetivo) return i;
        }
        return -1;
    }
}
```

Este diseño materializa la **agregación** (composición débil): el departamento no “posee” los `Profesor` (no clona ni destruye), solo mantiene referencias, y hace cumplir la invariante de que **siempre** existe un director perteneciente a su lista. Las operaciones de alta/baja y cambio de director preservan consistentemente dicho contrato mediante excepciones cuando procede.

**Anotación:**

```java
public class Profesor{

}

public class Departamento{

    //Composición debil 1:
    //Un departamento: como minimo 0 y como máximo muchos Profesor
    //Un profesor: como mínimo 0 y como máximo muchos Departamento
    private Profesor[] profesores = new Profesor[50];
    private int numProfesor = 0;

    //Composición débil 2:
    //Un departamento tiene como mínimo 1 y como máximo 1 Profesor
    //Un Profesor puede ser director mínimo 0 y máximo muchos Departamento.

    private Profesor director;

    public Departamento(Profesor director){
        //0. Si director es null, lanzamos IllegalArgumentException.
        //1. Añadimos el director al conjunto de profesores.
        //2. Establecemos ese profesor como director.
    }

    public int getNumProfesor(){
        return this.numProfesor;
    }

    public Profesor getProfesor(int pos){
        //0. Validamos pos, y si no valida, lanzar IAE.

        return this.profesores[pos];
    }

    public void addProfesor(Profesor p){
        //0. Si ya no hay más sitio, lanzar IAE o AIOBE (ArrayIndexOutOfBoundsException).
    }

    public void eliminarProfesor(int pos){
        //0. Si pos no está en el rango correcto (0 - numProfesor), lanzar IAE.
        //1. Si el profesor en pos es el director, lanzar IAE.
    }

    public void cambiarDirector(Profesor nuevoDirector){
        //0. Si nuevoDirector es null, lanzar IAE.
        //1. Si nuevoDirector no lo encuentro (bucle de busqueda), lanzo IAE, diciendo que hay que meterlo en el departamento primero.
    }

    public Profesor getDirector(){
        return this.director;
    }

}
```

* Hay dos composiciones débiles.
* No se expone el array al exterior (imposible garantizar invariante de clase).
* En los métodos que gestionan el departamento se controla que no se viole la invariante de clase.



## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

A continuación se reescribe el ejemplo usando **`List`** (agregación: composición débil). El uso de `List` elimina la gestión manual del array (desplazamientos, posiciones libres, *nulling*, y utilidades como `indiceDe`), manteniendo las mismas invariantes: **(1)** capacidad máxima 50, **(2)** el **director** existe desde el inicio y **siempre** forma parte del departamento, **(3)** no se permite eliminar al director sin cambiarlo antes a otro profesor ya incluido, y **(4)** no se admiten `null` ni duplicados (por identidad, al no redefinir `equals`). Se mantiene la API: `numeroProfesores()`, `profesorEn(int)`, `anadirProfesor(Profesor)`, `eliminarProfesorEn(int)`, `getDirector()` y `setDirector(Profesor)`.

Respecto a **qué código se ahorra** al pasar de arrays a `List`: ya no se necesita compactar manualmente al eliminar (no hay bucles de desplazamiento), no hay que **anular** posiciones con `null` para ayudar al GC, no se requiere una función `comprobarRango` (lo hace `List.get/remove` lanzando `IndexOutOfBoundsException`), ni un `indiceDe` propio si basta con `contains`/`indexOf`. Además, el contador `numProfesores` desaparece porque `list.size()` cubre ese rol. La capacidad máxima sigue siendo una regla de negocio explícita (no conviene confiar en la capacidad interna del `ArrayList`).

Sobre **exponer todos los profesores a la vez**: **devolver la lista interna directamente** rompería la encapsulación, pues el cliente podría modificarla (`add/remove/clear`) y violar invariantes (por ejemplo, eliminar al director). Para evitarlo, existen dos enfoques: **(a)** devolver una **vista inmodificable** con `Collections.unmodifiableList(profesores)` (el cliente no puede mutar, pero verá cambios futuros del departamento), o **(b)** devolver una **copia defensiva** `new ArrayList<>(profesores)` (el cliente puede mutar su copia sin afectar al interno; además sirve como *snapshot*). Dado que `Profesor` es inmutable, no hace falta *deep copy* de elementos, solo proteger la colección.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Objects;

public final class Profesor {
    private final String id;
    private final String nombre;

    public Profesor(String id, String nombre) {
        this.id = Objects.requireNonNull(id, "id no puede ser null");
        this.nombre = Objects.requireNonNull(nombre, "nombre no puede ser null");
    }

    public String getId()     { return id; }
    public String getNombre() { return nombre; }

    @Override public String toString() {
        return nombre + " [" + id + "]";
    }
}

public final class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final List<Profesor> profesores = new ArrayList<>(MAX_PROFESORES);
    private Profesor director; // siempre pertenece a 'profesores'

    /** Siempre arranca con un director, que pasa a ser parte de la lista. */
    public Departamento(Profesor director) {
        this.director = Objects.requireNonNull(director, "director no puede ser null");
        profesores.add(director);
    }

    /** Nº de profesores del departamento. */
    public int numeroProfesores() {
        return profesores.size();
    }

    /** Acceso por posición (0..n-1). */
    public Profesor profesorEn(int posicion) {
        return profesores.get(posicion); // lanza IndexOutOfBoundsException si procede
    }

    /** Director actual (miembro de la lista). */
    public Profesor getDirector() {
        return director;
    }

    /** Cambia el director por otro profesor que ya esté en el departamento. */
    public void setDirector(Profesor nuevoDirector) {
        Objects.requireNonNull(nuevoDirector, "nuevoDirector no puede ser null");
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe pertenecer al departamento");
        }
        this.director = nuevoDirector;
    }

    /** Añade al final. Sin null, sin duplicados por identidad, y sin superar MAX_PROFESORES. */
    public void anadirProfesor(Profesor p) {
        Objects.requireNonNull(p, "profesor no puede ser null");
        if (profesores.size() >= MAX_PROFESORES) {
            throw new IllegalStateException("Capacidad máxima (" + MAX_PROFESORES + ") alcanzada");
        }
        if (profesores.contains(p)) { // identidad por defecto (equals no redefinido)
            throw new IllegalArgumentException("El profesor ya pertenece al departamento");
        }
        profesores.add(p);
    }

    /**
     * Elimina por posición y devuelve el profesor eliminado.
     * No se permite eliminar al director actual (invariante).
     */
    public Profesor eliminarProfesorEn(int posicion) {
        Profesor eliminado = profesores.get(posicion); // valida rango
        if (eliminado == director) {
            throw new IllegalStateException(
                "No se puede eliminar al director actual; cámbiese primero el director");
        }
        return profesores.remove(posicion);
    }

    // ----------- Opciones para exponer TODOS los profesores -----------

    /** Versión 1: vista inmodificable (refleja cambios futuros del departamento). */
    public List<Profesor> getProfesoresVista() {
        return Collections.unmodifiableList(profesores);
    }

    /** Versión 2: copia defensiva (snapshot; el cliente puede modificar su copia). */
    public List<Profesor> getProfesoresCopia() {
        return new ArrayList<>(profesores);
    }
}

// ----------------- Ejemplo de uso -----------------
class Demo {
    public static void main(String[] args) {
        Profesor p1 = new Profesor("111A", "Ana");
        Profesor p2 = new Profesor("222B", "Berto");
        Profesor p3 = new Profesor("333C", "Carmen");

        Departamento dpto = new Departamento(p1); // p1 es director
        dpto.anadirProfesor(p2);
        dpto.anadirProfesor(p3);

        // Cambio de director (debe estar ya en la lista)
        dpto.setDirector(p3);

        // Eliminación segura: no se puede eliminar al director actual
        int posBerto =  dpto.getProfesoresVista().indexOf(p2);
        dpto.eliminarProfesorEn(posBerto);

        // Lecturas
        System.out.println("Profesores: " + dpto.numeroProfesores());
        for (int i = 0; i < dpto.numeroProfesores(); i++) {
            System.out.println(" - " + dpto.profesorEn(i));
        }
        System.out.println("Director: " + dpto.getDirector());

        // Si se expusiera la lista interna, se podrían violar invariantes:
        // dpto.getProfesores().clear(); // ¡Peligroso si fuese la lista real!
        // Con vista inmodificable, esta línea lanzaría UnsupportedOperationException.
    }
}
```

En resumen, con `List` se simplifica notablemente la implementación al delegar en la colección las operaciones de **acceso por índice**, **eliminación y desplazamiento**, y **contaje de elementos**. Para un método que devuelva “todos los profesores”, **no** debe retornarse la lista interna: o bien se entrega una **vista inmodificable** para lectura segura, o bien una **copia defensiva** si se desea aislar por completo la estructura interna y ofrecer una instantánea independiente.

**Anotación:**

```java
public class Profesor{

}

public class Departamento{

    //Composición debil 1:
    //Un departamento: como minimo 0 y como máximo muchos Profesor
    //Un profesor: como mínimo 0 y como máximo muchos Departamento
    private Profesor[] profesores = new Profesor[50];
    private int numProfesor = 0;

    //Composición débil 2:
    //Un departamento tiene como mínimo 1 y como máximo 1 Profesor
    //Un Profesor puede ser director mínimo 0 y máximo muchos Departamento.

    private Profesor director;

    public Departamento(Profesor director){
        //0. Si director es null, lanzamos IllegalArgumentException.
        //1. Añadimos el director al conjunto de profesores.
        //2. Establecemos ese profesor como director.
    }

    public int getNumProfesor(){
        return this.numProfesor;
    }

    public Profesor getProfesor(int pos){
        //0. Validamos pos, y si no valida, lanzar IAE.

        return this.profesores[pos];
    }

    public void addProfesor(Profesor p){
        //0. Si ya no hay más sitio, lanzar IAE o AIOBE (ArrayIndexOutOfBoundsException).
    }

    public void eliminarProfesor(int pos){
        //0. Si pos no está en el rango correcto (0 - numProfesor), lanzar IAE.
        //1. Si el profesor en pos es el director, lanzar IAE.
    }

    public void cambiarDirector(Profesor nuevoDirector){
        //0. Si nuevoDirector es null, lanzar IAE.
        //1. Si nuevoDirector no lo encuentro (bucle de busqueda), lanzo IAE, diciendo que hay que meterlo en el departamento primero.
    }

    public Profesor getDirector(){
        return this.director;
    }

    public List<Profesor> getProfesores(){
        //No usar: return this.profesores;

        //Solución clásica, con copia (penalización pequeña en rendimiento).
        return new ArrayList<>(this.profesores);
        
        //Solución alternativa sin copia:
        return Collections.unmodificableList(this.profesores);

        //Que problema hay? -> La lista es mutable, perdemos el control sobre la invariante de clase.

        //Si insisto en que me gusta esta interfaz, pero quiero garantizar que solo se use para recorrerlos y no modificar, puedo:
        //  1. Devolver una copia de la lista.
        //  2. Devolver un envoltorio no mutable.
    }

}
```

Con List<Profesor>:
* 1. No cambia la interfaz pública.
* 2. Es más facil implementar algunos métodos, delegando en métodos de List. 



## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

Las **composiciones recursivas** son aquellas en las que una clase se compone de instancias de **sí misma** (directa o indirectamente). En Java, un ejemplo clásico es la cadena de causas en las excepciones (`Throwable cause`), que permite encadenar errores. A continuación se muestra una `Persona` **inmutable** que “tiene-una” madre, que es otra `Persona`. Para que el modelo sea utilizable, se permite que la madre sea `null` como **caso base** (por ejemplo, el ancestro más alto conocido). La inmutabilidad se asegura con campos `private final`, sin *setters*, validaciones en el constructor y exposición solo mediante *getters*. Se añade un método `lineaMaterna()` que recorre recursivamente hasta la ancestro más antigua.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Objects;

public final class Persona {
    private final String nombre;
    private final int    anioNacimiento;
    private final Persona madre; // Composición recursiva: otra Persona (puede ser null como base)

    public Persona(String nombre, int anioNacimiento, Persona madre) {
        this.nombre = Objects.requireNonNull(nombre, "nombre no puede ser null");
        this.anioNacimiento = anioNacimiento;
        this.madre = madre; // puede ser null para terminar la recursión (abuela sin madre conocida)
    }

    public String getNombre() { return nombre; }
    public int getAnioNacimiento() { return anioNacimiento; }
    public Persona getMadre() { return madre; }

    /** 
     * Devuelve la línea materna desde esta persona hasta el ancestro más alto (incluyéndola).
     * Ej.: [Nieto, Madre, Abuela, ...]
     */
    public List<Persona> lineaMaterna() {
        List<Persona> camino = new ArrayList<>();
        Persona actual = this;
        while (actual != null) {
            camino.add(actual);
            actual = actual.madre;
        }
        return Collections.unmodifiableList(camino);
    }

    @Override
    public String toString() {
        return nombre + " (nac. " + anioNacimiento + ")";
    }

    // equals/hashCode opcionales si se desea comparar por identidad lógica (omitidos para brevedad)
}

// ----------------- Ejemplo de uso -----------------
class Demo {
    public static void main(String[] args) {
        // Abuela (caso base: madre desconocida/null)
        Persona abuela = new Persona("María", 1948, null);
        // Madre: su madre es 'abuela'
        Persona madre  = new Persona("Lucía", 1975, abuela);
        // Nieto: su madre es 'madre'
        Persona nieto  = new Persona("Diego", 2005, madre);

        // Mostrar la línea materna del nieto
        System.out.println("Línea materna de " + nieto.getNombre() + ":");
        for (Persona p : nieto.lineaMaterna()) {
            System.out.println(" - " + p);
        }

        // Acceso directo a la madre y a la abuela
        System.out.println("Madre del nieto: " + nieto.getMadre());
        System.out.println("Abuela del nieto: " + nieto.getMadre().getMadre());
    }
}
```

Este patrón recursivo refuerza la idea de **composición**: una `Persona` *tiene-una* `Persona` (su madre), y así sucesivamente, hasta un caso base. Al ser inmutable, se evitan ciclos o inconsistencias por mutación posterior (por ejemplo, cambiar la madre). El recorrido de la cadena puede implementarse de forma iterativa (como arriba) o recursiva; la versión iterativa evita riesgos de desbordamiento de pila si la cadena fuera muy larga, aunque en datos reales es segura cualquiera de las dos.

Otros ejemplos clásicos de **composición recursiva**: (1) el patrón *Composite* con **carpetas** que contienen carpetas/hijos (árbol de directorios); (2) **árboles de sintaxis** o **expresiones aritméticas** donde una expresión contiene subexpresiones; (3) **listas enlazadas** y **árboles binarios** (un nodo contiene referencias a nodos del mismo tipo); (4) **organigramas** (empleado con *manager* y *reportes* que son empleados); (5) **comentarios/anidaciones** (un comentario tiene respuestas que son comentarios); y (6) como ya se mencionó, **excepciones encadenadas** en Java (`Throwable` con `getCause()`), que modelan causas anidadas del mismo tipo.


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Las **relaciones de composición bidireccionales** (o asociaciones bidireccionales) son aquellas en las que **cada extremo mantiene una referencia al otro** y la **invariante** del modelo exige que ambas estén **consistentes** al mismo tiempo. En el ejemplo de `Profesor` y `Departamento`, implicaría que:

*   `Departamento` *tiene-varios* `Profesor` (agregación/composición débil en nuestro caso).
*   **Cada `Profesor` sabe a qué `Departamento` pertenece** (back‑reference).
*   Debe mantenerse la **consistencia**: si un profesor se añade al departamento, su `departamento` debe apuntar a ese mismo departamento; si se elimina, su `departamento` debe quedar a `null`. El **director** siempre debe ser uno de los profesores del departamento.

Esto obliga a gestionar cuidadosamente la **sincronización** de ambos lados para evitar estados intermedios incoherentes, bucles recursivos entre setters y fugas de encapsulación.

***

### Puntos clave para implementarlo correctamente

1.  **Back‑reference en `Profesor`:** añadir un campo `Departamento departamento` con *getter*, sin *setter* público. Solo el `Departamento` lo actualiza para mantener la consistencia.
2.  **Operaciones atómicas en `Departamento`:** al **añadir/eliminar** un profesor, actualizar también su back‑reference. Al **cambiar el director**, exigir que sea ya miembro del departamento.
3.  **Evitar bucles recursivos:** no llamar desde `Profesor` a métodos públicos de `Departamento` que vuelvan a tocar al profesor. En su lugar, usar métodos **privados** (o de paquete) internos que solo actualizan una cara sin reentradas.
4.  **Invariantes:**
    *   Capacidad máxima (si aplica).
    *   No `null` ni duplicados.
    *   El **director siempre pertenece** a la lista.
    *   Un `Profesor` **no puede pertenecer simultáneamente a dos** departamentos.
    *   No se puede **eliminar al director** sin antes designar otro director.
5.  **Encapsulación:** no exponer la colección interna; si hace falta devolver “todos”, usar **vista inmodificable** o **copia defensiva**.
6.  **Cuidado con `toString/equals/hashCode`:** evitar recorrer ambos lados y crear ciclos (p. ej., `toString` de `Profesor` que imprime el departamento, y el del departamento que imprime profesores → bucle). Mantenerlos simples.

***

### Implementación de ejemplo (agregación bidireccional con `List`)

> Se usa composición débil (agregación): el departamento no “posee” vitalmente al profesor, pero mantiene la **consistencia bidireccional**. `Profesor` es inmutable en identidad (id, nombre) y **mutable** en su back‑reference controlada (solo el `Departamento` la cambia).

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Objects;

public final class Profesor {
    private final String id;
    private final String nombre;

    // Back-reference: gestionada sólo por Departamento
    private Departamento departamento; // puede ser null si no pertenece a ninguno

    public Profesor(String id, String nombre) {
        this.id = Objects.requireNonNull(id, "id no puede ser null");
        this.nombre = Objects.requireNonNull(nombre, "nombre no puede ser null");
    }

    public String getId()     { return id; }
    public String getNombre() { return nombre; }

    /** Departamento actual (puede ser null). */
    public Departamento getDepartamento() { return departamento; }

    // ----------- API restringida: sólo Departamento debe llamar a esto -----------
    /** Uso interno (package-private o private con anidación): NO llamar desde fuera. */
    void __setDepartamentoInterno(Departamento dpto) {
        this.departamento = dpto;
    }

    @Override public String toString() {
        // Evitar imprimir el departamento para no crear recursión accidental.
        return nombre + " [" + id + "]";
    }
}

public final class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final String nombre;
    private final List<Profesor> profesores = new ArrayList<>(MAX_PROFESORES);
    private Profesor director; // debe pertenecer a 'profesores'

    /** Crea un departamento con un director inicial que pasa a ser profesor del departamento. */
    public Departamento(String nombre, Profesor directorInicial) {
        this.nombre = Objects.requireNonNull(nombre, "nombre no puede ser null");
        Objects.requireNonNull(directorInicial, "directorInicial no puede ser null");
        anadirProfesor(directorInicial); // esto setea el back-reference
        this.director = directorInicial;
    }

    public String getNombre() { return nombre; }

    // ------------- Lecturas seguras -------------
    public int numeroProfesores() { return profesores.size(); }

    public Profesor profesorEn(int pos) { return profesores.get(pos); } // valida rango

    public Profesor getDirector() { return director; }

    /** Vista inmodificable para no romper invariantes. */
    public List<Profesor> getProfesoresVista() {
        return Collections.unmodifiableList(profesores);
    }

    /** Opción snapshot: copia defensiva. */
    public List<Profesor> getProfesoresCopia() {
        return new ArrayList<>(profesores);
    }

    // ------------- Escrituras con mantenimiento bidireccional -------------

    /** Añade al final, respetando invariantes y back-reference. */
    public void anadirProfesor(Profesor p) {
        Objects.requireNonNull(p, "profesor no puede ser null");
        if (profesores.size() >= MAX_PROFESORES) {
            throw new IllegalStateException("Capacidad máxima (" + MAX_PROFESORES + ") alcanzada");
        }
        if (profesores.contains(p)) {
            throw new IllegalArgumentException("El profesor ya pertenece a este departamento");
        }
        // No permitir que un profesor pertenezca a dos departamentos a la vez
        if (p.getDepartamento() != null && p.getDepartamento() != this) {
            throw new IllegalStateException(
                "El profesor ya pertenece a otro departamento: " + p.getDepartamento().getNombre());
        }
        profesores.add(p);
        // Back-reference consistente
        p.__setDepartamentoInterno(this);
    }

    /**
     * Elimina por posición y devuelve el profesor eliminado.
     * No permite eliminar al director sin sustituirlo antes.
     */
    public Profesor eliminarProfesorEn(int pos) {
        Profesor eliminado = profesores.get(pos); // valida rango
        if (eliminado == director) {
            throw new IllegalStateException(
                "No se puede eliminar al director actual; cámbiese primero el director");
        }
        profesores.remove(pos);
        // Limpiar back-reference
        eliminado.__setDepartamentoInterno(null);
        return eliminado;
    }

    /**
     * Cambia el director por otro profesor que ya esté en el departamento.
     * Mantiene la invariante: el director siempre pertenece a la lista.
     */
    public void setDirector(Profesor nuevoDirector) {
        Objects.requireNonNull(nuevoDirector, "nuevoDirector no puede ser null");
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException(
                "El nuevo director debe pertenecer al departamento");
        }
        this.director = nuevoDirector;
    }

    @Override public String toString() {
        return "Departamento{" + nombre + ", numProfesores=" + profesores.size()
               + ", director=" + (director != null ? director.getNombre() : "N/D") + "}";
    }
}
```

### Ejemplo de uso

```java
class Demo {
    public static void main(String[] args) {
        Profesor p1 = new Profesor("111A", "Ana");
        Profesor p2 = new Profesor("222B", "Berto");
        Profesor p3 = new Profesor("333C", "Carmen");

        Departamento dpto = new Departamento("Informática", p1); // p1 director y miembro
        dpto.anadirProfesor(p2);
        dpto.anadirProfesor(p3);

        // Bidireccionalidad: profesor conoce su departamento
        System.out.println(p2.getNombre() + " pertenece a " + p2.getDepartamento().getNombre());

        // Cambio de director (debe ser miembro)
        dpto.setDirector(p3);

        // Intento de mover un profesor a otro departamento sin eliminarlo primero -> excepción
        Departamento dpto2 = new Departamento("Matemáticas", new Profesor("444D", "Diana"));
        try {
            dpto2.anadirProfesor(p2); // p2 ya pertenece a "Informática"
        } catch (IllegalStateException ex) {
            System.out.println("Movimiento inválido: " + ex.getMessage());
        }

        // Eliminación válida (no es el director)
        int posBerto = dpto.getProfesoresVista().indexOf(p2);
        dpto.eliminarProfesorEn(posBerto);
        System.out.println(p2.getNombre() + " ahora pertenece a: " + p2.getDepartamento());
    }
}
```

***

### ¿Qué cambia respecto a la versión unidireccional?

*   Se **añade el back‑reference** en `Profesor` y su gestión controlada (`__setDepartamentoInterno`).
*   Los métodos de modificación del `Departamento` ahora son responsables de mantener la **consistencia bidireccional** (añadir → set back‑reference; eliminar → limpiar back‑reference).
*   Se **bloquea** la pertenencia simultánea a dos departamentos.
*   Se mantiene la invariante del **director** y la **encapsulación** (no se expone la lista interna).

***

### Buenas prácticas adicionales

*   Considerar hacer `__setDepartamentoInterno` **package‑private** (sin modificador) para limitar su uso solo a clases del mismo paquete (o anidar `Profesor` dentro de `Departamento` si encaja).
*   Documentar las **pre/postcondiciones** de cada método que muta las asociaciones.
*   Si se usan frameworks (JPA/Hibernate), configurar correctamente la **propiedad dueña** de la relación para que las actualizaciones persistan coherentemente (aunque esto ya es fuera del alcance del ejemplo puro de Java).
*   Evitar ciclos en `equals/hashCode` si alguna vez se basan en relaciones navegables; preferir identificadores estables.

