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

A falta de excepciones en C, una forma habitual de **separar el resultado del estado** consiste en devolver un código de error y entregar el valor por un **parámetro de salida**. De ese modo, la función `raiz` no imprime ni informa nada; solo valida, calcula y comunica si fue bien o mal. Quien llama decide cómo informar al usuario. Esta estrategia escala bien cuando hay múltiples causas de error (p. ej., códigos `enum`) y evita ambigüedades con valores de retorno “sentinela”.

```c
#include <stdio.h>
#include <math.h>

typedef enum {
    RAIZ_OK = 0,
    RAIZ_ERR_NEGATIVO,
    RAIZ_ERR_NULO
} RaizStatus;

RaizStatus raiz(double x, double *out) {
    if (out == NULL) return RAIZ_ERR_NULO;
    if (x < 0.0)     return RAIZ_ERR_NEGATIVO;
    *out = sqrt(x);
    return RAIZ_OK;
}

int main(void) {
    double valor = -9.0;
    double r;
    RaizStatus st = raiz(valor, &r);
    if (st == RAIZ_OK) {
        printf("La raíz es %.4f\n", r);
    } else if (st == RAIZ_ERR_NEGATIVO) {
        printf("Error: no se puede calcular la raíz de un número negativo (%.2f)\n", valor);
    } else {
        printf("Error interno: parámetro de salida nulo\n");
    }
    return 0;
}
```

Otra opción típica en C es usar `errno` (definido en `<errno.h>`) para **señalizar errores globalmente**, devolviendo un valor especial (por ejemplo, `NAN`) cuando falla. La función `raiz` se mantiene “silenciosa” y el llamador consulta `errno` para decidir cómo informar. Esta técnica es familiar en APIs de la biblioteca estándar y permite integrar diagnósticos con `perror`/`strerror`. Eso sí, requiere resetear `errno` antes de llamar y manejar la posibilidad de que `NAN` también aparezca en otros contextos, por lo que conviene comprobar **ambas** cosas: el valor devuelto y `errno`.

```c
#define _GNU_SOURCE
#include <stdio.h>
#include <math.h>
#include <errno.h>
#include <string.h>

double raiz(double x) {
    if (x < 0.0) {
        errno = EDOM;           // Dominio matemático inválido
        return NAN;             // Valor sentinela
    }
    errno = 0;                  // No estrictamente necesario aquí, pero prudente si la función creciera
    return sqrt(x);
}

int main(void) {
    double valor = -9.0;
    errno = 0;                  // Reset antes de la llamada
    double r = raiz(valor);
    if (errno != 0) {
        // El mensaje al usuario se decide aquí, fuera de `raiz`
        fprintf(stderr, "No se pudo calcular la raíz: %s (valor=%.2f)\n", strerror(errno), valor);
    } else {
        printf("La raíz es %.4f\n", r);
    }
    return 0;
}
```

Como criterio práctico, conviene elegir **código de estado + parámetro de salida** cuando se desea una API explícita, fácil de probar y sin efectos globales; y **`errno` + sentinela** cuando se alinea con convenciones POSIX/stdlib o se quiere interoperar con utilidades existentes. En ambos casos, la responsabilidad de **informar al usuario** recae en el llamador, que es quien conoce el contexto y el canal de comunicación apropiado.

**Anotaciones:**

Dos formas:
### A:
```c++
float raiz(float num){
    if(num < 0){
        return -1.0;
    }
    return sqrt(num);
}

main(){
    float num = leerDeTeclado();
    float resultado = raiz(num);

    if(resultado == -1.0){
        printf("Error");
    }
    else{
        printf("%d", resultado);
    }
}
```

### B:
```c++
float raiz (float num, int *error){
    if(num < 0){
        *error = 1;
        return 0;
    }
    else{
        *error = 0;
        return sqrt(num);
    }
}

main(){
    int error = 0;
    float num = leerDeTeclado();
    float resultado = raiz(num, &error);
    if(error != 0){
        printf("Error");
    }
    else{
        printf("%d", resultado);
    }
}
```



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una **excepción** es un mecanismo que permite *interrumpir el flujo normal de un programa* cuando ocurre una condición anómala, y transferir el control hasta un punto preparado para gestionar ese problema. En lugar de devolver códigos de error o valores especiales, la función que detecta la situación anormal “lanza” una excepción y el sistema de ejecución busca un manejador apropiado para tratarla. Este proceso hace que la detección del error quede separada del modo en que se gestiona, lo que da más claridad al código.

El objetivo principal al usarlas al **implementar funciones** es disponer de una forma clara y estructurada de informar de fallos sin mezclar la lógica principal con el manejo de errores. La función se concentra en su tarea y, si algo no puede completarse, lanza una excepción que describe el problema. Esto evita cargar cada llamada con comprobaciones manuales y disminuye la probabilidad de que el programador olvide validar un resultado.

Desde el punto de vista de quien **llama a la función**, las excepciones permiten encapsular toda la lógica de tratamiento de fallos en bloques bien definidos (`try/catch`). De ese modo, es posible agrupar y gestionar varios tipos de errores de forma ordenada y en un único lugar, lo que mejora la legibilidad y facilita el mantenimiento del código. Además, la propagación automática de las excepciones permite que el error sea atendido por el nivel adecuado de la aplicación, en lugar de obligar a cada función intermedia a reenviar manualmente códigos de error.

En conjunto, el uso de excepciones proporciona una forma más expresiva y menos propensa a errores para tratar situaciones excepcionales, comparada con las técnicas clásicas de retorno de valores o uso de banderas globales.

**Anotaciones:**
* Excepciones -> en situaciones atípicas
* Cuando las implementamos -> nos permite indicar más claramente el error.
* Cuando las llamamos -> facilita la lógica normal de la reacción o manejo de las situaciones complejas.



## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

Aprovechando excepciones en Java, la validación puede encapsularse dentro de `Calculadora.raiz(double)` y delegar en el llamador la responsabilidad de informar. Si el argumento es negativo, se lanza una `IllegalArgumentException` (excepción no comprobada, apropiada para argumentos inválidos). Así se evita mezclar la lógica de cálculo con la de notificación, y el método se mantiene enfocado en su contrato: devolver la raíz o fallar con un motivo claro.

En el `main`, el control se ejerce con un bloque `try/catch`. El código que invoca `raiz` se mantiene limpio y, en caso de error, se captura la excepción para decidir el mensaje al usuario y el flujo posterior (por ejemplo, continuar, pedir otro número o terminar). Si se quisiera obligar a manejar el error de forma explícita, podría usarse una excepción *checked* propia; sin embargo, para argumentos inválidos en APIs internas, `IllegalArgumentException` suele ser suficiente y más idiomática.

```java
public class Calculadora {

    /**
     * Devuelve la raíz cuadrada de x.
     * @param x número de entrada; debe ser >= 0
     * @return sqrt(x)
     * @throws IllegalArgumentException si x es negativo
     */
    public static double raiz(double x) {
        if (x < 0.0) {
            throw new IllegalArgumentException(
                "No se puede calcular la raíz de un número negativo: " + x
            );
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        double valor = -9.0;

        try {
            double r = Calculadora.raiz(valor);
            System.out.printf("La raíz es %.4f%n", r);
        } catch (IllegalArgumentException e) {
            // Control desde fuera del método: decidir qué y cómo informar
            System.err.println("Error de entrada: " + e.getMessage());
            // Aquí podría registrarse, pedir otro valor, o finalizar con código de salida, etc.
        }
    }
}
```

Si se prefiere una variante con excepción *checked* para forzar el manejo en tiempo de compilación, bastaría con declarar y lanzar, por ejemplo, `class NumeroNegativoException extends Exception` y firmar el método como `public static double raiz(double x) throws NumeroNegativoException`. El `main` cambiaría únicamente el tipo de excepción en el `catch` (o añadiría un `catch` adicional), manteniendo el mismo control externo y la separación de responsabilidades.

**Anotación:**

```java
class Calculadora{
    public static double raiz(double num){
        if(num < 0.0){
            throw new IlegalArgumentException("num negativo.");
        }
        else{
            return Math.sqrt(num);
        }
    }
}

class App{
    public static void main(String[] args){
        double num = leerTeclado();
        try{
            double resultado = Calculadora.raiz(num);
            sout(resultado);
        }catch(IlegalArgumentException e){
            sout("Número negativo.");
        }
    }
}
```



## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

“Lanzar” una excepción consiste en indicar explícitamente que ha ocurrido una condición anómala y que la ejecución normal no puede continuar. En Java esto se hace usando la palabra clave `throw`. Cuando `Calculadora.raiz` detecta un número negativo, no devuelve un valor especial; en su lugar lanza una `IllegalArgumentException`. Ese lanzamiento hace que el método finalice inmediatamente y transfiere el control a la búsqueda de un manejador adecuado en la pila de llamadas. El código que iba después dentro del método ya no se ejecuta, porque se asume que la situación es excepcional y debe tratarse fuera.

“Controlar” o “capturar” una excepción significa colocar un bloque `try/catch` que pueda recibir y gestionar esa excepción. El programador decide en ese punto cómo reaccionar: informar al usuario, registrar el error, intentar una alternativa, etc. En el ejemplo de la raíz cuadrada, el método `main` contiene un `catch (IllegalArgumentException e)` que actúa como manejador. Si `raiz` lanza la excepción, la ejecución salta desde el punto del fallo directamente a ese bloque `catch`, donde se decide qué hacer con el mensaje de error. Gracias a esto, la lógica del cálculo se mantiene separada de la lógica de presentación o comunicación.

Cuando una excepción **no** es capturada en un método, se dice que **se propaga**. Esto significa que la excepción “sube” al método que llamó al actual. Java revisa la pila de llamadas buscando un `catch` compatible; si no lo encuentra en el método actual, destruye el marco de pila de ese método y continúa en el siguiente nivel. Este proceso se repite hasta encontrar un manejador o hasta llegar al nivel superior del programa. En el ejemplo, si `main` no tuviera un bloque `try/catch`, la excepción lanzada en `raiz` se propagaría más arriba, produciendo la finalización del programa con un mensaje de error.

Durante la propagación, las funciones por las que pasa la excepción **no se reanudan**: cada una termina abruptamente y su marco de pila se descarta. Es decir, ninguna parte de esas funciones continúa ejecutándose después de lanzar o recibir la excepción; solo se ejecutan los bloques `finally` si existen. En el ejemplo de la raíz cuadrada, una vez que `raiz` lanza la excepción, ese método no vuelve a ejecutarse ni retoma la instrucción siguiente al `throw`. Del mismo modo, si hubiera métodos intermedios entre `main` y `raiz` que no controlasen la excepción, todos ellos se cerrarían inmediatamente sin continuar con sus instrucciones pendientes. Solo el primer punto donde exista un `catch` adecuado recuperará el control de la ejecución.



## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La **propagación natural** de excepciones en lenguajes como Java aporta varias ventajas importantes respecto a la gestión manual de errores típica en C. Al aprovechar la pila de llamadas, el flujo de error se maneja de forma automática y estructurada, eliminando buena parte del código repetitivo y del riesgo de errores lógicos que aparecen cuando los fallos se propagan “a mano”.

En primer lugar, permite **separar claramente la lógica normal de la lógica de errores**. En C, cada función debe devolver un código de error y cada llamador debe comprobarlo explícitamente. Esto genera código ruidoso y repetitivo, y además es fácil olvidarse de validar algún resultado. Con excepciones, el código de la ruta correcta queda limpio, y los errores se gestionan solo donde corresponde, sin contaminar cada nivel de la llamada. Esto mejora notablemente la legibilidad y reduce la probabilidad de que un fallo quede sin tratar.

En segundo lugar, la propagación automática evita la necesidad de **reenviar manualmente el error a través de cada función intermedia**. En C, si `f3()` detecta un error, `f2()` debe devolverlo a `f1()` y esta a su vez al punto superior. Si alguno olvida reenviar el error, se pierde el diagnóstico y el programa puede comportarse de manera incorrecta. En Java, en cuanto se lanza la excepción, la máquina virtual abandona cada función en la pila hasta encontrar un manejador adecuado, garantizando que el error no quede oculto. Las funciones intermedias no necesitan hacer nada especial para “pasar” el error.

Además, cada función que se abandona de forma abrupta puede ejecutar automáticamente bloques `finally`, lo que facilita la **liberación segura de recursos** (archivos, conexiones, memoria gestionada por objetos). En C, esta tarea debe programarse en cada punto donde pueda ocurrir un error, lo que multiplica los lugares donde se podría producir una fuga si el programador no es extremadamente cuidadoso. Las excepciones proporcionan una estructura uniforme para limpiar recursos sin duplicación de código.

Por último, la propagación a través del *stack* asegura que el error llegue al **nivel adecuado** de la aplicación. A menudo, el lugar donde se detecta el fallo no es donde se debe decidir cómo reaccionar. La excepción viaja automáticamente hasta el contexto apropiado—por ejemplo, el método `main` en el caso de la raíz negativa—sin necesidad de programar manualmente una cadena de retornos. Esto da flexibilidad y permite centralizar el tratamiento de fallos en puntos bien definidos de la aplicación.



## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En la mayoría de lenguajes orientados a objetos modernos, como Java, **las excepciones son efectivamente objetos**. Cada excepción es una instancia de alguna clase que deriva de `Throwable`, lo que permite que transporten no solo un mensaje, sino también metadatos relevantes: el tipo concreto del error, la traza de la pila, causas encadenadas y cualquier información adicional que se quiera encapsular en atributos internos. Esto convierte el manejo de errores en una parte integrada del modelo de objetos en lugar de depender de códigos numéricos o valores especiales sin estructura.

El hecho de que las excepciones sean objetos aporta claras ventajas en términos de **encapsulación**. Cada tipo de error puede definirse como una clase que agrupa su estado interno y su comportamiento específico. Así, se consigue que la información relevante del fallo viaje junto con la excepción, sin necesidad de dispersarla por varias funciones o variables globales. Además, se permite que el código capturador conozca solo el tipo de excepción que quiere manejar, sin preocuparse por los detalles internos: la interfaz pública (mensaje, tipo, métodos auxiliares) actúa como frontera de encapsulación, tal y como sucede con cualquier otro objeto.

Dado que una excepción es una clase, el programador puede **crear excepciones personalizadas** para representar problemas propios del dominio de la aplicación. Esto resulta útil cuando se desea describir errores con mayor precisión que las excepciones estándar (`IllegalArgumentException`, `IOException`, etc.). Basta con definir una clase que herede de `Exception` (si se quiere que sea *checked*) o de `RuntimeException` (si se prefiere que sea *unchecked*), y luego lanzar instancias de ella. De esta forma, se mantiene un diseño más expresivo y coherente, ya que cada tipo de error tiene su propia identidad y puede ser capturado de manera específica.

Por ejemplo, en el contexto del cálculo de raíces cuadradas, podría definirse una excepción propia `NumeroNegativoException`, encapsulando el valor negativo que originó el problema. Esto permite que el bloque `catch` acceda a dicho valor sin necesidad de analizar cadenas de texto ni estructuras externas, reforzando todavía más la encapsulación y la claridad del código.



## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

En comparación con C, donde el programador debe construir *a mano* cualquier información sobre el error (códigos numéricos, mensajes sueltos, variables globales, etc.), un **objeto excepción en Java siempre incorpora información esencial encapsulada** que llega automáticamente al manejador. Esa información viaja unificada, sin depender de que cada función intermedia la reenvíe explícitamente.

La pieza más valiosa es la **traza de pila (stack trace)**. Cada excepción lleva consigo el registro completo de *dónde* se produjo exactamente el error y *qué funciones estaban activas* en ese momento. En C, si se olvida reenviar el detalle del fallo o no se guarda el contexto, esa información se pierde y puede ser muy difícil reconstruir la causa real. En Java, en cambio, el objeto excepción conserva esa traza desde el punto de lanzamiento, permitiendo diagnosticar fácilmente la ruta que siguió el programa hasta fallar.

Además, una excepción empaqueta de forma encapsulada un **mensaje descriptivo** y un **tipo concreto** (su clase). El manejador puede distinguir de manera clara qué tipo de error se produjo sin analizar códigos numéricos ni condicionar todo el programa con valores especiales. Esto facilita la escritura de manejadores selectivos (`catch` por tipo), mejora la expresividad y permite una jerarquía de errores coherente. En el ejemplo de la raíz cuadrada, el manejador recibe una `IllegalArgumentException` con el mensaje y con la traza completa, lo que le permite tanto informar al usuario como registrar el problema sin necesidad de lógica adicional.

En conjunto, la **traza de pila**, el **tipo de la excepción** y el **mensaje encapsulado** constituyen un paquete de información esencial que se transmite de forma automática, segura y sin pérdida entre el punto del error y el manejador. Esta capacidad es una de las mayores ventajas del enfoque orientado a objetos frente al tratamiento manual de errores en C.

**Anotaciones:**
La información esencial que lleva cualquier objeto excepción puede ser:
* Un mensaje (getMessage()).
* La traza de la pila, vale para depurar.
* Opcionalmente, una "causa".



## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Sí, en Java un bloque `try` puede ir seguido de **varios bloques `catch`**, cada uno destinado a manejar un tipo distinto de excepción. Esto permite reaccionar de forma diferenciada según el problema detectado, manteniendo la lógica de tratamiento bien organizada y acorde con la jerarquía de clases de excepciones.

De esos múltiples bloques, **solo se ejecuta uno**. Cuando se lanza una excepción dentro del `try`, la JVM busca el **primer** bloque `catch` cuyo tipo sea compatible con la excepción lanzada. En cuanto encuentra uno adecuado, ejecuta **únicamente ese** bloque y **omiten** todos los siguientes. No existe un “encadenamiento” automático entre `catch`: se captura una sola vez y la propagación se detiene allí, salvo que dentro del `catch` se vuelva a lanzar otra excepción.

Un ejemplo breve que sigue el estilo del caso de la raíz cuadrada sería:

```java
try {
    double r = Calculadora.raiz(valor);
    System.out.println("Resultado: " + r);
} catch (IllegalArgumentException e) {
    System.err.println("Error en el argumento: " + e.getMessage());
} catch (RuntimeException e) {
    System.err.println("Otro error no esperado: " + e.getMessage());
}
```

Aquí, si `raiz` lanza `IllegalArgumentException`, se ejecuta únicamente el **primer** `catch`. El segundo no se evalúa porque la excepción ya ha sido manejada. Esta estructura permite un control fino y ordenado de errores, especialmente útil cuando se quiere distinguir entre causas específicas y situaciones más generales.


**Anotaciones:**
 Sí que se puede tener más de uno:
* Solo se ejecuta uno.
* Se van comprobando por orden hasta el primero que "encaje".
* Se deben poner de más específico a más general (sino se ejecutará el primero que encaje).



## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Sí: para garantizar la liberación de recursos aunque haya rupturas por excepciones, se usa el bloque **`finally`**. El código dentro de `finally` se ejecuta *siempre* tras el `try` (y tras el `catch` si lo hay), tanto si hubo excepción como si no, incluso cuando la excepción se siga propagando. Solo situaciones extraordinarias como `System.exit(...)`, caída de la JVM o interrupciones de bajo nivel impedirían su ejecución. Esto permite centralizar cierres de ficheros, liberación de sockets o desbloqueos de *locks* sin duplicar lógica en cada rama de error.

Se puede combinar `try-catch-finally` para **manejar** la excepción localmente y, después, **cerrar** recursos en `finally`. Alternativamente, se puede usar `try-finally` **sin `catch`** cuando se quiera *solo* asegurar la limpieza y dejar que la excepción se **propague** al llamador. En ambos casos, el cierre debe ser a prueba de fallos (por ejemplo, comprobando `null` y envolviendo el `close()` en su propio `try`), porque incluso el cierre puede lanzar otra excepción.

**Ejemplo con `catch` + `finally` (se informa y se limpia):**

```java
import java.io.*;

public class DemoCatchFinally {
    public static void main(String[] args) {
        FileReader fr = null;
        try {
            fr = new FileReader("datos.txt");      // Puede lanzar FileNotFoundException
            int c = fr.read();                      // Puede lanzar IOException
            System.out.println("Primer carácter: " + (char)c);
        } catch (FileNotFoundException e) {
            System.err.println("No se encontró el fichero: " + e.getMessage());
        } catch (IOException e) {
            System.err.println("Error de E/S leyendo el fichero: " + e.getMessage());
        } finally {
            // Se ejecuta ocurra lo que ocurra arriba
            if (fr != null) {
                try { fr.close(); } 
                catch (IOException e) { System.err.println("Fallo al cerrar: " + e.getMessage()); }
            }
        }
    }
}
```

**Ejemplo con `finally` sin `catch` (se limpia y la excepción se propaga):**

```java
import java.io.*;

public class DemoTryFinally {
    static int leerPrimero(String ruta) throws IOException {
        FileReader fr = null;
        try {
            fr = new FileReader(ruta);   // Si falla, saltará al finally igualmente
            return fr.read();            // Si falla aquí, también
        } finally {
            // Garantía de liberación aunque se esté propagando una excepción
            if (fr != null) {
                try { fr.close(); } 
                catch (IOException e) { System.err.println("Fallo al cerrar: " + e.getMessage()); }
            }
        }
    }

    public static void main(String[] args) throws IOException {
        int c = leerPrimero("datos.txt");   // Si hay error, la IOException se propaga hasta aquí
        System.out.println("Primer carácter: " + (char)c);
    }
}
```

En código moderno, también resulta idiomático usar **try-with-resources** (`try (var r = ...) { ... }`), que cierra recursos automáticamente y simplifica el `finally`. Aun así, el concepto clave es el mismo: el **código de limpieza va en `finally`** (o implícitamente en try-with-resources) para garantizar la liberación de recursos incluso cuando el flujo normal se rompe por una excepción.

**Anotación:**
El bloque finally se ejecuta siempre que se ejecute el bloque try.



## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, en Java un bloque `finally` puede ir **sin** `catch`: la forma `try { ... } finally { ... }` es válida y muy común cuando solo se quiere **asegurar la liberación de recursos** y dejar que cualquier excepción se **propague** al llamador. De igual modo, puede usarse `try { ... } catch (...) { ... } finally { ... }` cuando además se desea manejar el error localmente. En ambos casos, `finally` es el lugar apropiado para cerrar ficheros, soltar *locks* o limpiar estructuras, manteniendo la separación entre lógica de negocio, manejo de errores y limpieza.

El bloque `finally` se ejecuta **tanto si ocurre como si no ocurre una excepción**, y también si dentro del `try` hay `return`, `break` o `continue`. La semántica es: primero se evalúa el `try` (y, si corresponde, el `catch`), **después** se ejecuta `finally`, y solo entonces se completa el retorno o la propagación de la excepción. Existen pocas excepciones a esta regla: no se ejecutará si la JVM finaliza abruptamente (`System.exit`/`Runtime.getRuntime().halt`), si el proceso se aborta por un error fatal (por ejemplo, `OutOfMemoryError` en momentos críticos), si el hilo es terminado de forma no controlada, o si se produce un fallo externo (apagado, *kill -9*, etc.).

Concretamente respecto a `return`, el `finally` **se ejecuta antes de que el método devuelva**. Por ejemplo:

```java
static int demoReturn(boolean devolverAntes) {
    try {
        if (devolverAntes) return 1;   // Intenta salir aquí
        throw new IllegalStateException("Fallo");
    } catch (IllegalStateException e) {
        return 2;                      // Intenta salir también aquí
    } finally {
        System.out.println("Limpieza en finally (siempre se imprime salvo casos extremos)");
    }
}
```

En este método, el `println` del `finally` se ejecuta tanto cuando se hace `return 1`, como cuando se captura la excepción y se hace `return 2`, como cuando no hay `catch` y la excepción se propaga. Nótese que si el propio `finally` lanza otra excepción o hace su propio `return`, **anulará** el flujo anterior (lo cual es desaconsejable por ocultar la causa original). Para código moderno, `try-with-resources` es preferible cuando se trabaja con recursos *AutoCloseable*, ya que expresa la misma garantía de limpieza de forma más concisa.

**Anotaciones:**
Si que puede ir sin catch:
* Se ejecuta, puesto que es finally.
* Si hubo excepción, como no tenemos catch, se propaga.



## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java, las excepciones **controladas** (*checked*) son las que el compilador exige declarar o capturar: si un método puede lanzarlas, debe incluirlas en su firma con `throws`, y el código llamador debe tratarlas con `try/catch` o seguir propagándolas. Representan situaciones recuperables y esperables en tiempo de ejecución (p. ej., E/S, red, formato de datos). En cambio, las **no controladas** (*unchecked*) son subclases de `RuntimeException` (o de `Error`) y no requieren declaración ni captura; suelen indicar errores de programación o precondiciones incumplidas (p. ej., índices fuera de rango, `null`, argumentos inválidos). El papel de **`RuntimeException`** es actuar como raíz de estas excepciones no controladas: todo lo que hereda de ella se considera un fallo que el código debería prevenir con validaciones, no algo que obligue a un manejo ceremonial en cada llamada.

Ejemplos típicos de **controladas** (checked) que se usan a diario incluyen `IOException` (fallos de archivos/streams), `SQLException` (errores de BD), `ParseException` (fallo al interpretar cadenas con un formato esperado) o `ClassNotFoundException` (carga dinámica). Entre las **no controladas** (unchecked), resultan comunes `IllegalArgumentException` (argumentos inválidos), `NullPointerException` (acceso a referencia nula), `IndexOutOfBoundsException` (índices fuera de rango) y `ArithmeticException` (división por cero). En una API propia, es habitual lanzar `IllegalArgumentException` cuando se detecta un número negativo en `Calculadora.raiz(double)`, y, si se quisiera forzar manejo explícito, definir una `NumeroNegativoException extends Exception` para hacerla *checked*.

**Cuándo preferir excepciones *controladas* (checked):**

*   Operaciones de **E/S, red o BD** donde el fallo es normal y el llamador puede **recuperarse** (reintentar, ruta alternativa, informar al usuario).
*   **APIs de librería** que deben **forzar** al consumidor a considerar el fallo (p. ej., abrir un fichero, conectar a un servicio).
*   **Validaciones externas** dependientes del entorno (permiso denegado, recurso no disponible, datos externos mal formados).
*   **Flujos de negocio** donde existe un **plan de contingencia** definido (p. ej., reintentos, compensaciones, fallback).

**Cuándo preferir excepciones *no controladas* (unchecked):**

*   **Errores de programación** o violaciones de contrato que deben **corregirse en el código** (pre/postcondiciones incumplidas).
*   **Argumentos inválidos** o **estado ilegal** detectados en métodos públicos (`IllegalArgumentException`, `IllegalStateException`).
*   Fallos que **no tienen recuperación significativa** en el punto de llamada (p. ej., `NullPointerException`, `IndexOutOfBoundsException`).
*   Para **evitar ruido ceremonial** cuando obligar `try/catch` haría el código menos claro sin aportar un plan real de recuperación.

**Anotaciones:**
* **No controladas:** No necesita un bloque try-catch/throws. Ejemplos: RuntimeException (IllegalArgumentException, NullPointerException, ...).
* **Controladas:** Es necesario usar un try-catch/throws. Ejemplo: IOException (AccesDeniedException), ...



## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

En Java, la palabra clave **`throws`** se usa en la *declaración* de un método para indicar que dicho método **puede lanzar una o varias excepciones controladas (checked)**. Su función es advertir al compilador y al llamador de que existe una condición excepcional que *debe ser tenida en cuenta*. De esta manera, el método no es responsable de capturar esa excepción, sino únicamente de declararla, dejando la gestión a quien invoque el método.

El uso de `throws` permite **propagar** la excepción hacia arriba en la pila de llamadas sin necesidad de manejarla localmente. Esto tiene valor cuando el método no sabe, no puede o no debe decidir cómo actuar ante ese error. En operaciones como acceso a ficheros, red o parsing, la recuperación suele depender del nivel superior de la aplicación (interfaz de usuario, controlador, etc.), por lo que declarar `throws` resulta más apropiado que insertar un `try-catch` que solo ocultaría el error o complicaría el código.

`throws` es, por tanto, **alternativa a capturar una excepción controlada** porque el compilador exige que *todas* las excepciones checked sean gestionadas de una de dos formas:

1.  **Capturándolas** con un bloque `try-catch`.
2.  **Declarando** que el método las puede lanzar (`throws`), delegando así la responsabilidad al llamador.

Si una excepción es checked y el código ni la captura ni la declara, el compilador marca error. Por ello, `throws` actúa como una forma completamente válida de cumplir el contrato de manejo obligatorio, evitando manejar localmente algo que quizá no tenga sentido gestionar al nivel en el que se detecta.

Un ejemplo sencillo:

```java
public static int leerPrimero(String ruta) throws IOException {
    FileReader fr = new FileReader(ruta); 
    return fr.read();
}
```

Aquí el método **no captura** la `IOException`. En lugar de eso, declara `throws IOException`, lo que obliga al llamador a decidir cómo tratar la situación. Así, la excepción se gestiona en el nivel adecuado, manteniendo la claridad y la responsabilidad bien distribuida dentro del programa.



## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

A continuación se muestra un ejemplo donde el método **declara** con `throws` que puede producir una `IOException` (incluye el caso de *fichero no existe* mediante `FileNotFoundException`, que hereda de `IOException`). El método **no la captura**: deja que se **propague hacia arriba**. Aun así, se garantiza la **liberación del recurso** en un bloque `finally`, para que el fichero se cierre ocurra lo que ocurra dentro del `try`. El `main` ilustra cómo el nivel superior decide qué hacer con la excepción (capturarla o volver a declararla).

```java
import java.io.FileReader;
import java.io.BufferedReader;
import java.io.IOException;

public class Lector {

    // Firma que declara la excepción: no se maneja aquí, se propaga
    public static String leerPrimeraLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta)); // Puede lanzar FileNotFoundException
            return br.readLine();                           // También puede lanzar IOException
        } finally {
            // Garantía de cierre incluso si hubo return o excepción
            if (br != null) {
                try { br.close(); }
                catch (IOException e) {
                    // En limpieza se registra, pero no se enmascara el error principal
                    System.err.println("Fallo al cerrar el fichero: " + e.getMessage());
                }
            }
        }
    }

    public static void main(String[] args) {
        try {
            String linea = leerPrimeraLinea("datos.txt");
            System.out.println("Primera línea: " + linea);
        } catch (IOException e) {
            // Decisión de manejo fuera: informar, reintentar, o finalizar
            System.err.println("No se pudo leer el fichero: " + e.getMessage());
        }
    }
}
```

En código moderno, también es habitual usar **try-with-resources**, que equivale a poner el cierre en un `finally` pero de forma declarativa (cualquier `AutoCloseable` se cierra automáticamente al salir del bloque). Aun así, el objetivo de la pregunta se cumple con `throws` + `finally`: el método **no maneja** la ausencia del fichero y **propaga** la excepción, mientras asegura la liberación de recursos. Si se prefiere la sintaxis abreviada, bastaría con:

```java
public static String leerPrimeraLinea(String ruta) throws IOException {
    try (BufferedReader br = new BufferedReader(new FileReader(ruta))) {
        return br.readLine();
    } // br.close() implícito aquí, equivalente al finally anterior
}
```

Ambas variantes mantienen la separación de responsabilidades: la función declara el posible fallo con `throws` y garantiza el cierre del recurso; el llamador decide cómo tratar la situación según el contexto de la aplicación.



## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Sí, técnicamente se pueden listar **excepciones no controladas** (subclases de `RuntimeException`) en la cláusula `throws`, pero es **opcional y redundante** desde el punto de vista del compilador: no obliga al llamador a capturarlas ni a declararlas. El valor de hacerlo es **documentar** explícitamente el contrato de la API (p. ej., “este método puede lanzar `IllegalStateException` si el recurso no está inicializado”), lo que mejora la comunicación y la generación de *javadoc*, pero no cambia las reglas de comprobación en tiempo de compilación.

El método llamador **no está obligado** a usar `try-catch` por el hecho de que se liste una `RuntimeException` en `throws`. De hecho, en diseño idiomático se prefiere **no capturar** excepciones no controladas en los niveles intermedios, sino dejar que se **propaguen** hasta un **punto de frontera** (por ejemplo, el bucle principal, un filtro web, un manejador global) donde tenga sentido registrar, transformar o mostrar un mensaje genérico. Capturar *unchecked* localmente solo tiene sentido si aporta una acción concreta de recuperación o para **traducir** la excepción a otra más significativa del dominio.

El uso de `throws` con *unchecked* tiene sentido en varios casos: como **documentación** de precondiciones/poscondiciones; para **señalar** que una operación puede fallar por estado ilegal sin obligar a ceremonia de `try-catch`; o en **sobrescritura** de métodos donde no se pueden añadir excepciones *checked*, pero sí se desea avisar de posibles *unchecked*. Un ejemplo minimalista:

```java
public String obtenerToken() throws IllegalStateException {
    if (!inicializado) throw new IllegalStateException("Servicio no inicializado");
    return token;
}

// En el llamador (no obligado a capturar):
String t = servicio.obtenerToken();  // Se deja propagar o se gestiona en un manejador global
```

En resumen: se **puede** poner *unchecked* en `throws`, pero no **debe** esperarse que fuerce `try-catch`. Se usa para **expresar el contrato** y para guiar a quien llama; y se captura solo cuando exista una **estrategia de recuperación real** o se necesite **traducción/registro** en un punto apropiado del sistema.

**Anotaciones:**
* Se puede, pero el compilador no va a obligar al bloque try-catch.
* No es habitual.
* A veces se ponen para documentación.



## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Las **excepciones controladas** (*checked*) y las **no controladas** (*unchecked*) representan dos enfoques distintos para expresar fallos en un programa, y su uso depende de la *naturaleza del error* y del *tipo de recuperación esperada* en el código cliente. Esta distinción no existe en todos los lenguajes, y donde no existe suele adoptarse por convención el estilo “no controlado”.

***

### ✔️ ¿Cuándo usar **excepciones controladas** (*checked*)?

Las excepciones controladas se recomiendan cuando el error:

1.  **Es esperable y frecuente en condiciones normales**, no necesariamente refleja un fallo de programación.  
    Ejemplos: problemas de disco, red, bases de datos, falta de permisos.

2.  **Tiene un plan de recuperación realista** en el nivel llamador.  
    El llamador puede reintentar, usar un fichero alternativo, pedir otro dato al usuario, etc.

3.  **Depende de factores externos** al flujo interno del programa.  
    Por ejemplo, un fichero que no existe, una conexión que se cae, datos externos corruptos.

Por eso, en Java, excepciones como `IOException`, `SQLException` o `ParseException` son **checked**: obligan al programador a *decidir* qué hacer, porque el fallo forma parte del flujo normal del sistema.

***

### ✔️ ¿Cuándo usar **excepciones no controladas** (*unchecked*)?

Las no controladas se recomiendan cuando el error:

1.  **Representa un fallo de programación** o violación de contrato.  
    Ejemplo: pasar un argumento negativo cuando se exige positivo.

2.  **No tiene sentido intentar recuperarlo localmente**, porque indica un error lógico.  
    Ejemplo: índice fuera de rango, acceder a un `null`, estado ilegal del objeto.

3.  **Añadir `try-catch` sería ruido**, sin opciones reales de manejo.

Por ejemplo, `IllegalArgumentException`, `IllegalStateException`, `NullPointerException` y `ArithmeticException` son **unchecked** porque indican errores del propio código o mal uso de la API.

***

### ✔️ ¿Existen estas dos categorías en todos los lenguajes?

No. La distinción **checked / unchecked** es relativamente rara.  
**Java es el ejemplo más conocido** de un lenguaje con excepciones controladas.

Otros lenguajes modernos (Python, C#, C++, JavaScript, Go —aunque este usa retornos múltiples—) **no tienen excepciones checked**. Todas son esencialmente “no controladas”.

***

### ✔️ En lenguajes donde solo existe un tipo, ¿cuál es la más habitual?

En la mayoría de lenguajes **solo existen excepciones no controladas (unchecked)**.

Esto significa:

*   No es obligatorio capturarlas.
*   Se propagan libremente hasta un manejador global.
*   El programador las usa cuando lo considera necesario, sin restricciones del compilador.

Esto refleja un diseño que favorece la simplicidad y evita el “ruido” de capturas obligatorias, dejando al programador decidir cuándo tiene sentido tratar un error.

***

### ✔️ Resumen claro

*   **Checked → problemas externos, esperables, con posible recuperación.**  
    Ej.: `IOException`.

*   **Unchecked → fallos de programación o mal uso de la API.**  
    Ej.: `IllegalArgumentException`.

*   **No todos los lenguajes tienen checked.**  
    Donde no existen, se usa el estilo **unchecked** como estándar.




## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí, tiene sentido **lanzar excepciones dentro de un `catch`** cuando se desea *traducir* el error a un tipo más significativo para la capa actual, **añadir contexto** (mensaje, datos del dominio) o **normalizar** la jerarquía de errores expuesta por una API. Esta práctica se conoce como *exception translation* o *wrapping*. En Java, es recomendable **encadenar la causa** (`cause`) para no perder el origen: `new MiExcepcion(..., cause)`. Así, el manejador superior puede leer el mensaje de negocio y, si es necesario, inspeccionar la causa original.

También es posible **relanzar la misma excepción capturada** (re-throw) cuando el `catch` realiza una acción local útil—por ejemplo, **registrar**, **liberar recursos no gestionados** (si no se usa `finally`/try-with-resources) o **anotar métricas**—y luego decide que la **decisión de recuperación** corresponde a un nivel superior. En Java, relanzar la misma instancia con `throw e;` **preserva la traza original**, mientras que crear una nueva excepción sin encadenar la causa la **oculta** (desaconsejado). Tiene sentido re-lanzar cuando el método intermedio **no** puede completar su contrato, pero sí puede dejar el sistema en un estado consistente o enriquecer la observabilidad del fallo.

**Ejemplo 1 — traducir la excepción (lanzar otra dentro del `catch`)**  
Se captura una `IOException` y se traduce a una excepción de dominio que el resto de la aplicación entiende mejor, **encadenando** la causa para conservar el rastro técnico:

```java
class DatosNoDisponiblesException extends Exception {
    public DatosNoDisponiblesException(String mensaje, Throwable cause) {
        super(mensaje, cause);
    }
}

String cargarPerfil(String ruta) throws DatosNoDisponiblesException {
    try (var br = new java.io.BufferedReader(new java.io.FileReader(ruta))) {
        return br.readLine();
    } catch (java.io.IOException e) {
        // Traducción con contexto de negocio y causa encadenada
        throw new DatosNoDisponiblesException("No fue posible cargar el perfil desde: " + ruta, e);
    }
}
```

**Ejemplo 2 — re-lanzar la misma excepción capturada (añadir logging/limpieza local)**  
Se hace un **log** y quizá se ejecuta limpieza adicional, pero se deja que el nivel superior decida la recuperación. Obsérvese el `throw e;` para **preservar** la traza original:

```java
double raiz(double x) {
    try {
        return Calculadora.raiz(x); // Puede lanzar IllegalArgumentException (unchecked)
    } catch (IllegalArgumentException e) {
        // Acción local útil (registro/telemetría), sin absorber el error
        System.err.println("Entrada inválida en raiz(): x=" + x + " -> " + e.getMessage());
        throw e;  // Re-lanzar la MISMA excepción; mantiene stack trace original
    }
}
```

En resumen: **lanzar otra excepción** desde un `catch` es útil para **encapsular** y **comunicar** mejor el fallo al dominio (traducción), mientras que **re-lanzar la misma** sirve para **no ocultar** errores tras realizar tareas locales valiosas (registro, limpieza) cuando el método **no puede** resolver el problema. En ambos casos, conviene **preservar la causa** (ya sea con `throw e;` o con `new ... (mensaje, e)`) para mantener diagnósticos fiables.

**Anotación:**
Si que tiene sentido:
* Relanzar la misma exception.
```java
try{

}
catch(NumberFormatException e){
    throw e;
}
```
* Lanzar otra excepción totalmente nueva.
```java
try{

}
catch(IOException e){
    throw new RunTimeException("Excepción", e);
}
```



## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Ser la **“causa”** de otra excepción significa **encadenar** excepciones: una excepción de bajo nivel (p. ej., `IOException`) se **adjunta** como origen (`cause`) dentro de una excepción de más alto nivel (p. ej., `DatosNoDisponiblesException`). En Java esto se modela con `Throwable` y su constructor/campo `cause`. Al crear la excepción de alto nivel se pasa la original como segundo argumento (`new MiExcepcion(msg, cause)`), preservando así el contexto técnico mientras se expone un tipo más significativo para la capa de negocio. Esta práctica se conoce como *exception chaining* y mejora el diagnóstico sin filtrar detalles innecesarios a capas superiores.

Un ejemplo típico consiste en **capturar** una excepción técnica y **encapsularla** en una personalizada que la API expone hacia fuera. El llamador puede entonces manejar el tipo del dominio (más expresivo) y, si necesita detalles, consultar `getCause()` para llegar al fallo original. La ventaja frente a “tirar” la excepción técnica tal cual es que se mantiene la **encapsulación**: la capa superior no depende de detalles de infraestructura y recibe un contrato estable.

Cuando una excepción encadenada se imprime (por ejemplo, con `printStackTrace()` o porque no se captura y la JVM la vuelca), **sí se ve la causa**: la salida muestra la traza de la excepción de alto nivel seguida de líneas con `Caused by: ...` y la traza de la causa, y así sucesivamente si hay varias. Esto facilita rastrear el error desde el síntoma de alto nivel hasta el origen de bajo nivel.

```java
// Excepción de alto nivel (dominio)
class DatosNoDisponiblesException extends Exception {
    public DatosNoDisponiblesException(String mensaje, Throwable cause) {
        super(mensaje, cause); // Encadena la causa
    }
}

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ServicioPerfiles {

    // Capa de dominio: expone una checked propia y oculta detalles técnicos
    public static String cargarPerfil(String ruta) throws DatosNoDisponiblesException {
        try (BufferedReader br = new BufferedReader(new FileReader(ruta))) {
            return br.readLine();
        } catch (IOException e) {
            // Traducción + encadenamiento de causa
            throw new DatosNoDisponiblesException(
                "No fue posible cargar el perfil desde: " + ruta, e
            );
        }
    }

    public static void main(String[] args) {
        try {
            System.out.println(cargarPerfil("perfil.txt"));
        } catch (DatosNoDisponiblesException e) {
            // Al imprimir, se verá la excepción de alto nivel y, debajo, "Caused by: IOException ..."
            e.printStackTrace();
            // También puede inspeccionarse programáticamente:
            Throwable causa = e.getCause();
            if (causa != null) {
                System.err.println("Causa técnica: " + causa.getClass().getSimpleName()
                                   + " -> " + causa.getMessage());
            }
        }
    }
}
```

En este patrón, la capa inferior aporta el detalle técnico y la superior mantiene un contrato estable y expresivo. Al mismo tiempo, la traza impresa revela toda la cadena (`Caused by`) para depuración, evitando perder información diagnóstica mientras se conserva la separación de responsabilidades.

**Anotaciones:**
Causa de excepción:
* Se ve cuando la excepción se muestra por pantalla.
* Se puede obtener con el método getCause().
