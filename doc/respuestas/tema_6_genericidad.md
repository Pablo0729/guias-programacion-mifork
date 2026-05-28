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

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato

A continuación se presenta una respuesta ajustada a tu estilo solicitado (2–4 párrafos, tono impersonal, rigor académico y ejemplos claros). No se emplean preguntas finales.

---

En C, antes de la existencia de mecanismos de genericidad, la forma habitual de almacenar valores de tipos heterogéneos consistía en utilizar punteros de tipo `void*`. Este tipo permite referenciar cualquier dato, ya que el compilador no impone restricciones sobre el tipo apuntado. Sin embargo, la responsabilidad de convertir de vuelta al tipo correcto recae completamente en el programador, lo que introduce riesgos de seguridad y errores en tiempo de ejecución. Aun así, este enfoque permite construir estructuras de datos genéricas basadas en arrays primitivos.

```c
typedef struct {
    void** datos;
    int capacidad;
    int usados;
} ArrayGenerico;

void init(ArrayGenerico* a, int capacidad) {
    a->datos = malloc(sizeof(void*) * capacidad);
    a->capacidad = capacidad;
    a->usados = 0;
}

void add(ArrayGenerico* a, void* elemento) {
    a->datos[a->usados++] = elemento;
}
```

En Java, antes de la introducción de la genericidad en Java 5, la única forma de almacenar elementos heterogéneos en una estructura basada en arrays era utilizar el tipo `Object`. Dado que todas las clases derivan de `Object`, cualquier referencia puede almacenarse en un array de este tipo. Al recuperar los elementos, es necesario realizar un *cast* explícito, lo que puede provocar errores en tiempo de ejecución si el tipo no coincide. Este mecanismo es conceptualmente similar al uso de `void*` en C, aunque más seguro debido al sistema de tipos de Java.

```java
public class ArrayGenerico {
    private Object[] datos;
    private int usados;

    public ArrayGenerico(int capacidad) {
        datos = new Object[capacidad];
        usados = 0;
    }

    public void add(Object elem) {
        datos[usados++] = elem;
    }

    public Object get(int i) {
        return datos[i];
    }
}
```

Este tipo de soluciones, tanto en C como en Java, permiten almacenar cualquier tipo de dato, pero sacrifican seguridad de tipos y obligan a realizar conversiones manuales. La genericidad moderna en Java surge precisamente para resolver estas limitaciones, proporcionando verificación en tiempo de compilación y evitando errores derivados de *casts* incorrectos.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica?

A continuación tienes la respuesta con el formato solicitado: 2–4 párrafos, tono impersonal, rigor académico y sin preguntas finales.

---

La **programación genérica** es un paradigma que permite escribir código capaz de operar con distintos tipos de datos sin necesidad de duplicar implementaciones. Su objetivo es abstraer el comportamiento común de una estructura o algoritmo, dejando que el tipo concreto se especifique posteriormente. Esto proporciona reutilización, seguridad de tipos en tiempo de compilación y evita errores derivados de conversiones manuales. En lenguajes modernos como Java, esta técnica se implementa mediante parámetros de tipo (`<T>`), que permiten expresar relaciones entre tipos sin perder información durante la compilación.

El ejemplo anterior basado en `void*` en C o `Object` en Java **no constituye programación genérica en sentido estricto**, aunque históricamente se utilizó como sustituto. En ambos casos se permite almacenar cualquier tipo, pero a costa de perder seguridad de tipos y obligar a realizar *casts* manuales. No existe verificación en tiempo de compilación que garantice que el tipo recuperado coincide con el almacenado, lo que contradice uno de los principios fundamentales de la genericidad moderna.

Por tanto, el ejemplo anterior debe considerarse una **simulación rudimentaria** de genericidad, basada en la eliminación del tipo en tiempo de compilación. La verdadera programación genérica, como la introducida en Java 5, mantiene la información de tipo durante la compilación, detecta errores antes de ejecutar el programa y permite expresar relaciones entre tipos de forma declarativa. El uso de `void*` u `Object` es simplemente una técnica previa que resolvía el problema de la heterogeneidad, pero sin las garantías formales que caracterizan a la genericidad real.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas

A continuación se presenta la respuesta con el formato solicitado: 2–4 párrafos, tono impersonal, rigor académico y sin preguntas finales.

---

El uso de `void*` en C o `Object` en Java introduce problemas significativos en el **chequeo de tipos**, ya que ambos mecanismos eliminan la información de tipo en tiempo de compilación. Al almacenar datos de cualquier clase sin restricciones, el compilador no puede verificar si el tipo recuperado coincide con el tipo originalmente insertado. Esto implica que los errores relacionados con incompatibilidades de tipos no se detectan hasta la ejecución, donde pueden provocar fallos graves como accesos inválidos a memoria en C o excepciones `ClassCastException` en Java.

Además, este enfoque obliga a realizar conversiones explícitas (*casts*) al recuperar los elementos. Dichas conversiones son inherentemente inseguras, porque el compilador no puede comprobar su validez. En C, un *cast* incorrecto puede generar comportamientos indefinidos, mientras que en Java produce errores en tiempo de ejecución. En ambos casos, la responsabilidad recae completamente en el programador, lo que incrementa la probabilidad de errores sutiles y difíciles de depurar.

Por último, la ausencia de información de tipo impide expresar relaciones semánticas entre los elementos almacenados. No es posible, por ejemplo, garantizar que todos los elementos de la estructura implementen una interfaz concreta o que sean comparables entre sí. Esta limitación contrasta con la genericidad moderna, donde los parámetros de tipo permiten imponer restricciones y asegurar que el código genérico se utilice de forma coherente y segura.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**?

A continuación se presenta la respuesta con el formato solicitado: 2–4 párrafos, tono impersonal, rigor académico y sin preguntas finales.

---

Los **parámetros de tipo** son identificadores simbólicos que permiten definir clases, interfaces o métodos cuyo comportamiento es independiente del tipo concreto de los datos con los que operan. En lugar de fijar un tipo específico en la declaración, se introduce un parámetro —como `T`, `E`, `K`, `V`— que actúa como marcador de posición. Este parámetro será sustituido por un tipo real en el momento de instanciar la clase o de invocar el método, permitiendo así reutilizar la misma implementación para múltiples tipos sin duplicar código.

El uso de parámetros de tipo proporciona **seguridad de tipos en tiempo de compilación**, ya que el compilador puede verificar que los valores utilizados coinciden con el tipo elegido por el usuario de la clase genérica. Esto elimina la necesidad de emplear `Object` y realizar *casts* manuales, reduciendo el riesgo de errores en tiempo de ejecución. Además, los parámetros de tipo pueden incorporar restricciones mediante *bounds*, lo que permite exigir que el tipo real cumpla ciertas propiedades, como implementar una interfaz o extender una clase concreta.

En términos conceptuales, los parámetros de tipo constituyen el mecanismo fundamental que habilita la **programación genérica moderna**. Gracias a ellos, es posible diseñar estructuras de datos y algoritmos que funcionan con cualquier tipo compatible, manteniendo al mismo tiempo un control estricto sobre el uso correcto del código. Este enfoque combina flexibilidad y robustez, y representa una mejora sustancial respecto a técnicas previas basadas en `void*` o `Object`, donde la información de tipo se perdía durante la compilación.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad

A continuación se presenta la respuesta con el formato solicitado: 2–4 párrafos, tono impersonal, rigor académico y ejemplos claros. No se incluyen preguntas finales.

---

En Java, la genericidad permite declarar colecciones parametrizadas que solo aceptan un tipo concreto, garantizando seguridad de tipos en tiempo de compilación. Al instanciar una lista como `List<String>`, el compilador impide insertar elementos que no sean cadenas y asegura que cada acceso devuelve un `String` sin necesidad de *casts*. Esto permite recorrer la colección con total seguridad, sabiendo que todos los elementos cumplen el tipo especificado. El siguiente ejemplo muestra la creación de la lista, la inserción de valores y su recorrido:

```java
import java.util.ArrayList;
import java.util.List;

public class EjemploJava {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();
        lista.add("Hola");
        lista.add("Mundo");
        lista.add("Genéricos");

        for (String s : lista) {
            System.out.println(s.toUpperCase());
        }
    }
}
```

En C++, los *templates* permiten generar código especializado para cada tipo en tiempo de compilación. Al utilizar `std::vector<std::string>`, el compilador garantiza que el vector solo contendrá objetos `std::string`, y cada acceso devuelve directamente una referencia a una cadena sin conversiones. Esto proporciona un recorrido seguro y eficiente, manteniendo la coherencia del tipo en toda la estructura. El siguiente ejemplo ilustra este uso:

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> v;
    v.push_back("Hola");
    v.push_back("Mundo");
    v.push_back("Templates");

    for (const std::string& s : v) {
        std::cout << s << std::endl;
    }
}
```

Ambos lenguajes muestran cómo la programación genérica permite definir estructuras de datos flexibles sin sacrificar la seguridad del sistema de tipos. Tanto Java como C++ garantizan que los elementos almacenados coinciden con el tipo parametrizado, evitando errores en tiempo de ejecución y proporcionando un modelo más robusto que las aproximaciones basadas en `Object` o `void*`.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

## ANOTACIONES CLASE


```java
Class Par<P,Q>{

private final P primero;
private final Q segundo;

public Par(P primero, Q segundo){

this.primero=primero;
this.segundo=segundo;

}

public P getPrimero(){
    return this.primero
}

public P getSegundo(){
    return this.segundo
}

}


class Estadisticas{
    public static Par<Double, Double> mediaYDesviacionTipica(List<Double> valores)
{

//.....
double media = ...
double stddev = ...

return new Par<Double, Double>(media, stddev)

}

}


/////////////////
 
Class Par{
private primero;
private segundo;

public Par(Object primero, Object segundo);



}



///////////////// 
class Autobox






```


## RESPUESTA IA

Cuando se instancia una clase con parámetros de tipo en Java, el compilador utiliza un mecanismo llamado **type erasure**. Este proceso consiste en eliminar toda la información de los parámetros de tipo durante la compilación, sustituyéndolos por su *bound* (por defecto, `Object`) y generando una única versión de la clase para todos los tipos posibles. Esto significa que `List<String>` y `List<Integer>` comparten exactamente el mismo bytecode, y la seguridad de tipos se garantiza exclusivamente en tiempo de compilación. El borrado de tipos permite mantener compatibilidad con versiones antiguas de Java, pero limita ciertas operaciones, como crear arrays de tipos genéricos o comprobar el tipo concreto en tiempo de ejecución.

En contraste, C++ utiliza **instanciación de plantillas**, un mecanismo completamente distinto. Cada vez que se usa un `template` con un tipo concreto, el compilador genera una copia especializada del código para ese tipo. Esto implica que `vector<int>` y `vector<double>` son implementaciones distintas, con código generado específicamente para cada una. La ventaja es que no existe borrado de tipos y el compilador puede optimizar cada versión, pero a costa de aumentar el tamaño del ejecutable y del tiempo de compilación. En resumen, Java usa un único código genérico con borrado de tipos, mientras que C++ genera múltiples versiones concretas mediante instanciación de plantillas.



## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`

A continuación tienes la **respuesta a la Pregunta 7 sobre Genericidad**, redactada en **tono impersonal**, con **2 párrafos**, y adaptada a tu recorrido previo (C/C++ sin POO y Java con clases, encapsulación, herencia y polimorfismo).

---

## **7. Clase genérica `Par<T, U>` y ejemplo de uso**

La creación de una clase genérica como `Par<T, U>` permite encapsular dos valores de tipos potencialmente distintos sin necesidad de definir una clase específica para cada combinación. Desde la perspectiva de alguien que viene de C/C++ sin orientación a objetos, puede verse como una generalización segura de lo que en C se intentaba con `struct` y punteros `void*`, pero evitando la pérdida de tipo. En Java, los parámetros de tipo permiten que el compilador garantice que cada componente del par mantiene su tipo concreto, evitando conversiones inseguras y errores en tiempo de ejecución. La clase suele incluir un constructor y getters para acceder a cada elemento, manteniendo la encapsulación.

Un ejemplo típico consiste en usar `Par<Double, Double>` como tipo de retorno de una función que calcula la media y la desviación típica de un array de `double`. De este modo, la función devuelve ambos valores agrupados en un único objeto, sin necesidad de crear una clase específica para ese propósito. El uso de genéricos permite que la misma clase `Par` pueda reutilizarse en cualquier otro contexto donde se necesite devolver dos valores relacionados, manteniendo siempre la seguridad de tipos.

```java
public class Par<T, U> {
    private final T primero;
    private final U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}
```

```java
public static Par<Double, Double> estadisticas(double[] datos) {
    double suma = 0;
    for (double d : datos) suma += d;
    double media = suma / datos.length;

    double sumaCuadrados = 0;
    for (double d : datos) sumaCuadrados += Math.pow(d - media, 2);
    double desviacion = Math.sqrt(sumaCuadrados / datos.length);

    return new Par<>(media, desviacion);
}
```

```java
Par<Double, Double> resultado = estadisticas(new double[]{1, 2, 3, 4});
System.out.println("Media = " + resultado.getPrimero());
System.out.println("Desviación típica = " + resultado.getSegundo());
```

---

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo

En Java, declarar parámetros de tipo a nivel de método permite que la operación sea genérica sin necesidad de parametrizar toda la clase. Un método como `seleccionaUno` puede definirse con un parámetro de tipo `<T>`, lo que garantiza que ambos argumentos son del mismo tipo y que el valor devuelto mantiene ese tipo sin necesidad de conversiones. Esto contrasta con la versión basada en `Object`, donde el compilador no puede asegurar que los dos objetos sean compatibles entre sí, y el programador debe realizar *downcasting* manual al recuperar el resultado, con el riesgo de errores en tiempo de ejecución. El método genérico, en cambio, conserva la seguridad de tipos y permite que el compilador detecte usos incorrectos.

Además, el método genérico evita que se mezclen tipos accidentalmente, algo que en C/C++ podría ocurrir al usar punteros genéricos como `void*`. En Java, el método `seleccionaUno(T a, T b)` impide pasar, por ejemplo, un `String` y un `Integer`, ya que el compilador exige que ambos coincidan con el mismo tipo `T`. Esto proporciona un diseño más robusto y coherente, especialmente útil en librerías y APIs donde la consistencia del tipo es fundamental. El resultado es un código más seguro, más claro y sin necesidad de conversiones explícitas.

---

### **Versión incorrecta (con `Object`, requiere downcasting y no fuerza tipos)**

```java
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}

// Uso problemático
String s = (String) seleccionaUno("hola", "adiós");  // funciona
String t = (String) seleccionaUno("hola", 123);      // compila, pero falla en ejecución
```

---

### **Versión correcta (método genérico, sin downcasting y con seguridad de tipos)**

```java
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}

// Uso seguro
String s = seleccionaUno("hola", "adiós");   // sin cast
Integer n = seleccionaUno(10, 20);           // sin cast
// seleccionaUno("hola", 10);                // error de compilación: tipos distintos
```

---



## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?



En Java es posible imponer restricciones sobre los parámetros de tipo mediante *bounds*, lo que permite exigir que un tipo genérico cumpla ciertas propiedades, como extender una clase concreta o implementar una interfaz. En el caso de valores numéricos, Java no permite expresar directamente “cualquier tipo que sea un número” en el sentido matemático, pero sí permite restringir el parámetro de tipo a subclases de `Number`, lo que garantiza la existencia de métodos como `doubleValue()`, `intValue()`, etc. Esto resulta útil para implementar operaciones matemáticas sin perder seguridad de tipos. La alternativa sin genéricos consiste en declarar directamente las coordenadas como `Number`, lo que permite almacenar cualquier tipo numérico, pero sin saber exactamente cuál se está usando en cada instancia.

Cuando se añade un parámetro de tipo `<T extends Number>`, el compilador puede verificar que todas las coordenadas de un `Punto<T>` son del mismo tipo numérico concreto, como `Integer`, `Double` o `Long`. Esto evita mezclas accidentales y permite que el código sea más expresivo y seguro. Sin embargo, debido al *type erasure*, toda la información de tipo genérico desaparece en tiempo de ejecución, de modo que `Punto<Integer>` y `Punto<Double>` se convierten en la misma clase `Punto` tras la compilación. El borrado de tipos implica que las restricciones se aplican únicamente en tiempo de compilación, pero no se conservan en el bytecode final.

---

## **Solución 1: Usar directamente `Number` (sin genéricos)**

```java
public class Punto {
    private final Number x;
    private final Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(Punto otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

---

## **Solución 2: Usar genéricos con restricción `<T extends Number>`**

```java
public class Punto<T extends Number> {
    private final T x;
    private final T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

---

## **¿Cuál es el tipo final tras la compilación?**

Debido al *type erasure*, la clase:

```java
Punto<T extends Number>
```

se convierte en:

```java
Punto
```

y todos los usos de `T` se sustituyen por `Number` en el bytecode. Es decir, **en tiempo de ejecución no existe `Punto<Integer>` ni `Punto<Double>`: solo existe `Punto`**, y las restricciones se aplican exclusivamente en tiempo de compilación.

---



## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?


Ambas soluciones permiten reutilizar la clase `Punto` para distintos tipos numéricos, pero difieren en la solidez del chequeo de tipos. La versión sin genéricos, basada únicamente en `Number`, permite crear un punto cuyas coordenadas sean de tipos distintos, por ejemplo `new Punto(3, 4.5)`, ya que el constructor acepta cualquier combinación de subclases de `Number`. Esto implica que el tipo devuelto por `getX()` y `getY()` es siempre `Number`, obligando a convertir manualmente si se desea recuperar el tipo concreto. En términos de diseño, esta solución es flexible pero menos segura, porque no garantiza homogeneidad entre las coordenadas.

La versión genérica `Punto<T extends Number>` refuerza el chequeo de tipos al exigir que ambas coordenadas sean del mismo tipo `T`. Esto impide crear un punto con una coordenada entera y otra real, ya que el compilador detecta la inconsistencia y rechaza la construcción del objeto. Como consecuencia, el método `getX()` devuelve exactamente el tipo `T`, lo que permite recuperar directamente un `Integer`, `Double`, `Long`, etc., sin necesidad de conversiones explícitas. Aunque en tiempo de ejecución ambos diseños se reducen a la misma clase debido al *type erasure*, la versión genérica proporciona una seguridad de tipos estricta en tiempo de compilación que evita errores sutiles y mejora la claridad del código.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting

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

### Respuesta

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo


En Java, aunque `String` es subtipo de `Object`, **no** se cumple que `List<String>` sea subtipo de `List<Object>`. Las clases genéricas en Java son **invariantes**, lo que significa que `List<A>` y `List<B>` son tipos completamente independientes, incluso si `A` es subtipo de `B`. Esta decisión se tomó para evitar errores de tipo en tiempo de compilación: si `List<String>` fuese subtipo de `List<Object>`, sería posible insertar un `Integer` en una lista que en realidad contiene solo `String`, rompiendo la seguridad del sistema de tipos. En cambio, los arrays en Java **sí son covariantes**, por lo que `String[]` es subtipo de `Object[]`. Esto permite asignar un `String[]` a una referencia `Object[]`, pero introduce un problema en tiempo de ejecución: si se intenta almacenar un objeto que no sea `String` dentro de ese array, se produce una excepción `ArrayStoreException`, evidenciando que la covarianza de arrays es insegura.

La diferencia entre ambos mecanismos permite introducir las nociones de **covarianza**, **contravarianza** e **invariancia**. Un tipo genérico es **covariante** respecto a su parámetro cuando `A` subtipo de `B` implica que `C<A>` es subtipo de `C<B>`; esto ocurre con los arrays (`String[]` es subtipo de `Object[]`). Es **contravariante** cuando la relación se invierte: `A` subtipo de `B` implica que `C<B>` es subtipo de `C<A>`. Finalmente, es **invariante** cuando no existe relación de subtipado entre `C<A>` y `C<B>` aunque `A` y `B` sí la tengan; este es el caso de los genéricos en Java (`List<String>` no es subtipo ni supertipo de `List<Object>`). La invariancia evita errores en tiempo de ejecución como los que sí pueden aparecer con arrays, reforzando la seguridad del sistema de tipos en el uso de colecciones genéricas.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`


Un *wildcard* (`?`) representa un tipo desconocido dentro de un parámetro genérico. Su utilidad radica en permitir cierto grado de flexibilidad en el subtipado, algo que los genéricos por defecto no permiten debido a su invariancia. Cuando se escribe `List<? extends T>`, se indica que la lista contiene elementos de un tipo desconocido que es subtipo de `T`; esto introduce **covarianza**, permitiendo leer elementos como `T` pero impidiendo añadir nuevos valores (salvo `null`). Por el contrario, `List<? super T>` indica que la lista contiene elementos de un tipo desconocido que es supertipo de `T`; esto introduce **contravarianza**, permitiendo añadir valores de tipo `T` pero limitando la lectura a `Object`. Esta distinción sigue el principio **PECS**: *Producer Extends, Consumer Super*.

El uso de `? extends` es adecuado cuando una estructura **produce** valores que deben ser leídos con seguridad, como en un método que suma números sin necesidad de modificarlos. En cambio, `? super` se utiliza cuando la estructura **consume** valores, como al insertar elementos en una colección. Esta separación evita errores de tipo y permite recuperar parte de la flexibilidad que sí tienen los arrays covariantes, pero sin los riesgos de tiempo de ejecución asociados a ellos. En definitiva, los wildcards permiten expresar relaciones de subtipado de forma controlada, manteniendo la seguridad del sistema de tipos.

---

## **(i) Ejemplo con `? extends`: sumar una lista de números**

```java
public static double sumarLista(List<? extends Number> lista) {
    double suma = 0;
    for (Number n : lista) {
        suma += n.doubleValue();
    }
    return suma;
}
```

Uso:

```java
double r1 = sumarLista(List.of(1, 2, 3));        // List<Integer>
double r2 = sumarLista(List.of(1.5, 2.5, 3.5));  // List<Double>
```

---

## **(ii) Ejemplo con `? super Integer`: añadir enteros a una lista**

```java
public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
    lista.add(30);
}
```

Uso:

```java
List<Number> ln = new ArrayList<>();
añadirEnteros(ln);   // válido: Number es supertipo de Integer

List<Object> lo = new ArrayList<>();
añadirEnteros(lo);   // válido: Object es supertipo de Integer
```

---


