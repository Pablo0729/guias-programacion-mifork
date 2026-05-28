# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

A **puntero a función** en C es una variable que almacena la **dirección de una función**, del mismo modo que un puntero normal almacena la dirección de una variable. Esto permite tratar funciones como datos: pasarlas como parámetros, almacenarlas en estructuras o seleccionarlas dinámicamente en tiempo de ejecución. Para alguien que viene de C/C++ sin orientación a objetos, puede verse como una forma primitiva de “polimorfismo”: se puede invocar una función distinta según el puntero que se asigne, sin necesidad de `switch` ni `if` encadenados.

En el siguiente ejemplo se define una función que recibe una cadena y devuelve una nueva cadena en mayúsculas. Después se declara un puntero a función llamado `aMayusculas`, se le asigna la dirección de la función y finalmente se invoca a través del puntero. Se usa `malloc` para crear una copia modificable de la cadena, ya que las literales son de solo lectura.

```c
#include <stdio.h>
#include <ctype.h>
#include <stdlib.h>
#include <string.h>

char* convertirMayus(const char* s) {
    size_t n = strlen(s);
    char* res = malloc(n + 1);
    for (size_t i = 0; i < n; i++)
        res[i] = toupper((unsigned char)s[i]);
    res[n] = '\0';
    return res;
}

int main() {
    // Definición del puntero a función
    char* (*aMayusculas)(const char*) = convertirMayus;

    // Uso del puntero
    char* salida = aMayusculas("hola mundo");
    printf("%s\n", salida);c

    free(salida);
    return 0;
}
```



### CLASE

Las funciones son "ciudadanos de primera clase" Una funcion es un tipo más:
    Se puede asignar a una variable_____
    Se puede pasar como parámetro
    Se puede devolver una función como retorno de otra.

-Clousure
-Expresiones Labda--No tienen nombre
-En lenguajes con comprobación estática de tipos: ¿Qué tipo tienen?

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una **función lambda** es una función anónima, es decir, una función que no necesita nombre y que puede definirse “en línea” allí donde se necesite. Suelen emplearse para representar operaciones simples que se pasan como parámetro, se almacenan en variables o se aplican sobre colecciones. En lenguajes modernos, las lambdas permiten un estilo más declarativo y funcional, evitando la necesidad de declarar funciones auxiliares completas cuando solo se requiere una operación puntual. Conceptualmente, cumplen un papel similar al de los punteros a función en C, pero con sintaxis más compacta y con comprobación de tipos más segura.

En JavaScript, las funciones lambda (o *arrow functions*) son muy comunes y se tratan como valores de primera clase. En Java, las lambdas se introdujeron en Java 8 y permiten asignar comportamientos a interfaces funcionales como `Function<T,R>`. En ambos casos, se puede replicar el ejemplo anterior creando una lambda que convierta una cadena a mayúsculas y asignándola a una variable local llamada `aMayusculas`.

### Ejemplo en **JavaScript**

```js
// Función lambda (arrow function) que convierte a mayúsculas
const aMayusculas = s => s.toUpperCase();

// Uso
console.log(aMayusculas("hola mundo"));
```

### Ejemplo en **Java**

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {

        // Lambda que convierte una cadena a mayúsculas
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        // Uso
        String salida = aMayusculas.apply("hola mundo");
        System.out.println(salida);
    }
}
```



## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El **paradigma funcional** es un estilo de programación que se basa en la idea de que las funciones son transformaciones puras de datos: reciben valores, producen otros nuevos y no modifican el estado externo. Se centra en la inmutabilidad, la ausencia de efectos secundarios y el uso de funciones como unidades fundamentales de composición. En lugar de describir *cómo* realizar una tarea paso a paso (como en el paradigma imperativo), se describe *qué* transformación debe aplicarse a los datos. Esto permite razonar de forma más matemática sobre el código y facilita la paralelización, ya que las funciones puras no dependen de estados compartidos.

Lenguajes orientados a objetos como Java 8 se consideran **multi‑paradigma** porque, aunque su base es la orientación a objetos, incorporan elementos funcionales como lambdas, referencias a métodos y operaciones sobre colecciones basadas en funciones (`map`, `filter`, `reduce`). Esto permite combinar ambos estilos según convenga: se puede seguir diseñando clases y jerarquías, pero también expresar transformaciones de datos de forma declarativa. Cuando se dice que las funciones son **“ciudadanos de primera clase”**, se indica que pueden tratarse como cualquier otro valor: almacenarse en variables, pasarse como parámetros, devolverse como resultado o incluirse en estructuras de datos. Este principio es esencial para el paradigma funcional y es lo que habilita patrones como composición de funciones o programación declarativa.


## 4. Explica la sintaxis básica de una función lambda en Java.

En Java, una **función lambda** es una forma compacta de expresar una implementación de una *interfaz funcional*, es decir, una interfaz con un único método abstracto. La sintaxis básica sigue el patrón `parámetros -> expresión` o `parámetros -> { bloque }`. Cuando el cuerpo es una única expresión, no es necesario usar llaves ni `return`; cuando el cuerpo contiene varias sentencias, se emplean llaves y la palabra clave `return` si se devuelve un valor. El tipo de los parámetros suele inferirse automáticamente, lo que reduce la verbosidad respecto a declarar clases anónimas.

La forma general puede resumirse así:

```java
(TipoParametros parametros) -> expresion

(TipoParametros parametros) -> {
    // varias sentencias
    return resultado;
}
```

Por ejemplo, una lambda que convierte una cadena a mayúsculas puede escribirse como:

```java
Function<String, String> aMayusculas = s -> s.toUpperCase();
```

Esta sintaxis permite tratar el comportamiento como un valor, almacenarlo en variables, pasarlo como argumento o devolverlo desde métodos, integrando el estilo funcional dentro del modelo orientado a objetos de Java.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

En programación funcional y en lenguajes con funciones de primera clase, es habitual **pasar funciones como parámetros** para que un método pueda aplicar una transformación sin conocer su implementación concreta. Esto permite desacoplar la lógica del procesamiento de la operación concreta que se desea realizar. En JavaScript y Java, este patrón se expresa de forma natural gracias a las funciones lambda y a las interfaces funcionales. El método `transformar` recibe un `String` y una función transformadora, y simplemente delega en ella la operación, igual que antes se hacía con punteros a función en C.

A continuación se muestran las versiones equivalentes en JavaScript y Java. En ambos casos, la variable local `aMayusculas` contiene la función lambda que convierte la cadena a mayúsculas, y se pasa como argumento a `transformar`, que la invoca desde dentro. Esto ilustra cómo las funciones pueden tratarse como valores y componerse dinámicamente.

---

### **JavaScript**

```js
// Función transformar que recibe una cadena y una función transformadora
function transformar(cadena, funcion) {
    return funcion(cadena);
}

// Lambda que convierte a mayúsculas
const aMayusculas = s => s.toUpperCase();

// Uso
console.log(transformar("hola mundo", aMayusculas));
```

---

### **Java**

```java
import java.util.function.Function;

public class Main {

    // Método transformar que recibe una cadena y una función transformadora
    public static String transformar(String s, Function<String, String> funcion) {
        return funcion.apply(s);
    }

    public static void main(String[] args) {

        // Lambda que convierte a mayúsculas
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        // Uso
        String salida = transformar("hola mundo", aMayusculas);
        System.out.println(salida);
    }
}
```




## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

En este caso se desea **invocar `transformar` pasando una función lambda creada en el mismo momento de la llamada**, sin almacenarla previamente en una variable como `aMayusculas`. Esto es una práctica habitual en programación funcional: se define la transformación justo donde se necesita, lo que evita declarar funciones auxiliares cuando solo se van a usar una vez. La lambda de ejemplo invertirá la cadena, por lo que su cuerpo consistirá en recorrer la cadena al revés o usar utilidades ya existentes del lenguaje.

A continuación se muestran las versiones equivalentes en JavaScript y Java. En ambos casos, `transformar` recibe la cadena y la lambda, y la invoca desde dentro. La función de inversión se define directamente en la llamada, lo que demuestra que las funciones son valores que pueden crearse y pasarse dinámicamente.

---

### **JavaScript**

```js
function transformar(cadena, funcion) {
    return funcion(cadena);
}

// Llamada con una lambda definida en el momento
console.log(
    transformar("hola mundo", s => s.split("").reverse().join(""))
);
```

---

### **Java**

```java
import java.util.function.Function;

public class Main {

    public static String transformar(String s, Function<String, String> funcion) {
        return funcion.apply(s);
    }

    public static void main(String[] args) {

        // Llamada con una lambda definida directamente
        String salida = transformar("hola mundo",
                s -> new StringBuilder(s).reverse().toString());

        System.out.println(salida);
    }
}
```




## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

En el contexto de las funciones lambda, un **closure** (o *cierre*) es la capacidad de una función para **capturar y utilizar variables que estaban en su ámbito léxico en el momento en que fue definida**, incluso si se ejecuta más tarde y en otro contexto. En otras palabras, la lambda “recuerda” el entorno donde nació. En Java, esto se permite siempre que las variables capturadas sean *efectivamente finales*, es decir, que no cambien después de su inicialización. Este mecanismo permite crear funciones parametrizadas por valores externos sin necesidad de almacenarlos dentro de un objeto explícito.

En el siguiente ejemplo se modifica el caso anterior para que la función lambda no solo transforme la cadena, sino que además **concatene** otra cadena almacenada en una variable local definida fuera de la lambda. Esa variable es capturada por el closure y utilizada cuando se ejecuta la función. El método `transformar` sigue recibiendo la función como parámetro y aplicándola internamente.

---

### **Java — Ejemplo de closure con lambda**

```java
import java.util.function.Function;

public class Main {

    public static String transformar(String s, Function<String, String> funcion) {
        return funcion.apply(s);
    }

    public static void main(String[] args) {

        // Variable local que será capturada por la lambda (closure)
        String sufijo = "!!!";

        // Lambda que concatena el sufijo capturado
        Function<String, String> añadirSufijo =
                s -> s + sufijo;

        // Uso
        String salida = transformar("hola mundo", añadirSufijo);  ////añadirSufijo == (s -> s + sufijo)
        System.out.println(salida);  // hola mundo!!!
    }
}
```


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

La diferencia fundamental entre una **función lambda** y un **puntero a función en C** está en el nivel de abstracción y en el modelo de tipos que ofrece cada lenguaje. En C, un puntero a función es simplemente una **dirección de memoria** donde empieza el código de una función; no captura variables externas, no tiene un entorno asociado y su tipo es puramente una firma estática (parámetros y tipo de retorno). Esto lo hace muy flexible pero también más peligroso: no hay comprobaciones adicionales más allá de la compatibilidad de tipos, y no existe un mecanismo integrado para asociar datos al comportamiento. En esencia, un puntero a función es una referencia desnuda a código ejecutable.

En cambio, una lambda en lenguajes modernos como Java es un **objeto funcional** que puede incluir no solo el comportamiento, sino también el **entorno léxico donde fue definida** (closure). Esto permite capturar variables locales, integrarse con el sistema de tipos genéricos, participar en interfaces funcionales y beneficiarse de comprobaciones de tipo mucho más estrictas. Además, las lambdas no son direcciones de memoria sin más, sino instancias de un tipo funcional bien definido, lo que permite componerlas, almacenarlas y manipularlas con mayor seguridad. Por tanto, aunque ambas permiten tratar funciones como valores, las lambdas proporcionan un nivel de abstracción más alto y seguro que los punteros a función tradicionales de C.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

En este ejercicio se busca **devolver funciones** y observar cómo una lambda puede capturar variables externas mediante un *closure*. La idea es que `crearDescuento(porcentaje)` reciba un porcentaje y devuelva una función que, al aplicarse sobre una cantidad, reste ese porcentaje. La clave está en que la lambda devuelta **captura** el valor de `porcentaje`, que pertenece al ámbito donde fue creada, y lo conserva incluso después de que `crearDescuento` haya terminado su ejecución. Esto demuestra que la función resultante no solo contiene código, sino también el entorno léxico necesario para operar.

En el ejemplo siguiente se crean dos funciones de descuento distintas, cada una con su propio porcentaje capturado. Al aplicarlas sobre una cantidad, cada lambda utiliza el valor de `porcentaje` que quedó fijado en el momento de su creación. Este comportamiento es precisamente lo que define un *closure*: la función devuelta mantiene acceso a variables externas que estaban en su ámbito original, aunque ya no existan en el contexto de ejecución actual.

---

### **Java — Funciones que devuelven funciones (closures)**

```java
import java.util.function.Function;

public class Main {

    // Crea una función descuento capturando el porcentaje
    public static Function<Double, Double> crearDescuento(double porcentaje) {

        // La lambda captura 'porcentaje' (closure)
        return cantidad -> cantidad - (cantidad * porcentaje / 100.0);
    }

    public static void main(String[] args) {

        // Crear dos funciones descuento distintas
        Function<Double, Double> descuento10 = crearDescuento(10.0);
        Function<Double, Double> descuento25 = crearDescuento(25.0);

        // Aplicarlas a una cantidad
        double precio = 200.0;

        System.out.println(descuento10.apply(precio));  // 180.0
        System.out.println(descuento25.apply(precio));  // 150.0
    }
}
```




## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una **interfaz funcional** en Java es un tipo especial de interfaz que declara **exactamente un único método abstracto**. Ese método representa el “comportamiento” que podrá implementarse mediante una función lambda. Dado que Java es un lenguaje con comprobación estática de tipos, cada lambda debe corresponder a un tipo concreto, y ese tipo es precisamente la interfaz funcional. Esto permite que el compilador verifique que la lambda tiene la firma correcta (parámetros y tipo de retorno) y que se usa en un contexto válido. Además, las interfaces funcionales pueden contener métodos `default` o `static`, siempre que no añadan más métodos abstractos.

Los requisitos formales son muy simples: debe haber **un único método abstracto** y, opcionalmente, se puede anotar con `@FunctionalInterface` para que el compilador garantice que se cumple la condición. Esta anotación no es obligatoria, pero sí recomendable para evitar errores accidentales al añadir métodos adicionales. Interfaces como `Runnable`, `Callable`, `Comparator` o las de `java.util.function` (`Function`, `Predicate`, `Consumer`, etc.) son ejemplos típicos. Gracias a este modelo, Java puede integrar lambdas sin romper su sistema de tipos ni su filosofía orientada a objetos.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Una **interfaz funcional** en Java es aquella que declara exactamente un único método abstracto, lo que permite representarla mediante una expresión lambda. En este caso, se desea modelar explícitamente el tipo de una operación que recibe una cadena y devuelve otra, igual que haría `Function<String,String>`, pero con un nombre más expresivo y adaptado al dominio del ejercicio. Para alguien que viene de C/C++ sin orientación a objetos, puede verse como la formalización de un “puntero a función”, pero integrado en el sistema de tipos de Java y con verificación en tiempo de compilación.

La interfaz `Transformador` define un único método `transformar`, que toma un `String` y devuelve otro `String`. La anotación `@FunctionalInterface` no es obligatoria, pero permite que el compilador garantice que la interfaz sigue siendo funcional. Esta interfaz puede usarse como tipo de una variable que apunte a una lambda, o como parámetro de un método que reciba una transformación. Su definición es la siguiente:

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String s);
}
```


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.


Una versión genérica de la interfaz `Transformador` permite abstraer no solo el tipo de entrada, sino también el tipo de salida, siguiendo el mismo patrón que `Function<T, R>` de la librería estándar. Esto resulta útil cuando la operación no es simplemente una transformación entre cadenas, sino una conversión más general entre tipos arbitrarios. Desde la perspectiva de alguien que viene de C/C++, puede verse como parametrizar el “tipo de función” para que el compilador garantice que la entrada y la salida son coherentes, evitando conversiones inseguras o dependientes de `void*`.

Al parametrizar la interfaz con `<T, R>`, se obtiene un transformador capaz de convertir un valor de tipo `T` en otro de tipo `R`. Un ejemplo típico es la conversión de un `Double` en un `Integer` mediante redondeo, que puede implementarse con una expresión lambda. La interfaz y un ejemplo de uso quedarían así:

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}

// Ejemplo: redondear un Double a Integer
Transformador<Double, Integer> redondeador = d -> (int) Math.round(d);

Integer resultado = redondeador.transformar(3.7);  // resultado = 4
```


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

En Java existe un conjunto amplio de **interfaces funcionales predefinidas** que cubren los patrones más habituales de uso de funciones como valores. Estas interfaces permiten expresar operaciones de consumo, transformación, predicado, suministro de valores y combinaciones más complejas, sin necesidad de definir una interfaz propia cada vez. Para alguien que viene de C/C++, puede verse como disponer de una “biblioteca estándar de tipos de funciones”, pero con verificación estática de tipos y totalmente integrada en el modelo orientado a objetos de Java. Su existencia evita duplicar código y facilita el uso de lambdas y *streams*.

Estas interfaces se agrupan por categorías según su propósito:  
- **Consumer**: `Consumer<T>`, `BiConsumer<T,U>` — reciben valores y no devuelven nada.  
- **Supplier**: `Supplier<T>` — no reciben nada y devuelven un valor.  
- **Predicate**: `Predicate<T>`, `BiPredicate<T,U>` — reciben valores y devuelven un booleano.  
- **Function**: `Function<T,R>`, `BiFunction<T,U,R>`, `UnaryOperator<T>`, `BinaryOperator<T>` — transforman valores.  
- **Operadores primitivos**: `IntPredicate`, `DoubleUnaryOperator`, `LongBinaryOperator`, etc., optimizados para evitar *autoboxing*.  

Gracias a este conjunto, Java cubre prácticamente todas las combinaciones típicas de funciones de uno o dos argumentos, con o sin retorno, y con versiones especializadas para tipos primitivos.


### Clase
Function <E,S>  S apply , (E e);
BiFunction <E1, E2, S> S apply (E1 e1, E2 e2)


Predicate<T>
Runnable


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

En Java, el método `forEach` permite recorrer una colección aplicando una función a cada elemento, sustituyendo así al bucle `for` tradicional. Desde una perspectiva funcional, este enfoque evita gestionar índices manualmente y delega la operación en una función que se ejecuta para cada elemento. Esto hace que el código sea más declarativo: en lugar de indicar *cómo* recorrer la lista, se expresa *qué* debe hacerse con cada elemento. Además, el uso de lambdas permite definir comportamientos de forma compacta y expresiva, lo que resulta especialmente útil cuando se aplican condiciones simples como comprobar si un número es positivo.

En el caso de una lista de `Integer`, puede emplearse `forEach` junto con una expresión lambda que verifique si el valor es mayor que cero y, en tal caso, muestre un mensaje. Esta aproximación mantiene el estilo funcional al evitar estructuras de control explícitas dentro del recorrido. Un ejemplo sería:

```java
List<Integer> lista = List.of(3, -1, 0, 7, -5);

lista.forEach(n -> {
    if (n > 0) {
        System.out.println(n + " es positivo");
    }
});
```

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

En la firma de forEach se utiliza Consumer<? super T> porque el método debe aceptar cualquier consumidor capaz de procesar elementos del tipo T o de un supertipo suyo. Esta elección sigue el principio PECS (Producer Extends, Consumer Super), que indica que cuando una estructura consume valores (como ocurre al aplicar una operación sobre cada elemento), debe permitirse un tipo más general para maximizar la flexibilidad. De este modo, si T es Integer, un Consumer<Number> o un Consumer<Object> también pueden emplearse sin restricciones, ya que ambos son capaces de recibir un Integer. Esto no sería posible si la firma fuera estrictamente Consumer<T>, lo que limitaría innecesariamente el uso del método.

El mismo principio se aplica al mejorar el método transformar, que recibe una función para convertir elementos de tipo T en elementos de tipo R. En este caso, la función produce valores de tipo R, por lo que debe permitirse que el tipo devuelto sea un subtipo mediante ? extends R, y a la vez debe aceptarse que la función pueda recibir un supertipo de T mediante ? super T. La firma correcta sería: Function<? super T, ? extends R>. Esto permite, por ejemplo, transformar una lista de Integer usando una función que acepte Number como entrada y devuelva un subtipo concreto de R, manteniendo la compatibilidad y la expresividad propias del estilo funcional en Java.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

En JavaScript y Java, una referencia a método permite tratar un método como un valor, almacenarlo en una variable y ejecutarlo más tarde. En JavaScript esto es natural porque las funciones son ciudadanos de primera clase, mientras que en Java se consigue mediante referencias a métodos introducidas en Java 8. En ambos casos, el método conserva su contexto: en JavaScript, el valor de `this` depende de cómo se invoque; en Java, la referencia queda ligada al objeto concreto si se usa la sintaxis `objeto::método`.

A continuación se muestran ambos ejemplos con una clase `Persona` y su método `saludar`, creando una instancia, obteniendo la referencia al método y ejecutándola después.

---

### **JavaScript**

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const p = new Persona("Ana");

// Referencia al método
const refSaludar = p.saludar.bind(p);

// Invocación
refSaludar();
```

---

### **Java**

```java
class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Principal {
    public static void main(String[] args) {
        Persona p = new Persona("Ana");

        // Referencia al método de instancia
        Runnable refSaludar = p::saludar;

        // Invocación
        refSaludar.run();
    }
}
```
## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

En Java existen cuatro tipos principales de referencias a método, cada una asociada a un uso distinto dentro del estilo funcional introducido en Java 8. La primera es la **referencia a método estático**, que se emplea cuando la operación no depende de una instancia concreta; la segunda es la **referencia a constructor**, útil para crear objetos de forma funcional; la tercera es la **referencia a método de instancia de un objeto concreto**, que fija el método a una instancia ya existente; y la cuarta es la **referencia a método de instancia sobre cualquier instancia**, donde el método se aplicará al objeto recibido como argumento. Estas formas permiten expresar comportamientos de manera más declarativa y compacta, evitando lambdas innecesarias.

Los siguientes ejemplos ilustran cada caso de forma clara y directa:

```java
import java.util.function.*;

class Persona {
    private String nombre;

    public Persona() {
        this.nombre = "Sin nombre";
    }

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }

    public String getNombre() {
        return nombre;
    }
}

public class Principal {
    public static void main(String[] args) {

        // 1. Referencia a método estático
        Function<String, Integer> refParseInt = Integer::parseInt;

        // 2. Referencia a constructor
        Supplier<Persona> refConstructor = Persona::new;

        // 3. Referencia a método de instancia de una instancia concreta
        Persona p = new Persona("Ana");
        Runnable refSaludar = p::saludar;

        // 4. Referencia a método de instancia sobre cualquier instancia
        Function<Persona, String> refGetNombre = Persona::getNombre;

        // Invocaciones
        System.out.println(refParseInt.apply("123"));
        System.out.println(refConstructor.get().getNombre());
        refSaludar.run();
        System.out.println(refGetNombre.apply(p));
    }
}
```

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

En Java, la ordenación mediante `Collections.sort` permite definir criterios complejos usando expresiones lambda, lo que encaja bien con el estilo funcional. Cuando se comparan objetos como `Persona`, es habitual combinar varios criterios: primero por edad y, en caso de empate, por orden alfabético del nombre. La versión “manual” del comparador consiste en implementar explícitamente la lógica de comparación dentro de la lambda, devolviendo un valor negativo, cero o positivo según corresponda. Esta aproximación es directa y recuerda al estilo clásico de C/C++, pero expresada de forma más concisa gracias a las lambdas.

La segunda versión emplea la clase `Comparator`, que proporciona métodos estáticos y encadenables para construir comparadores de forma declarativa. Con `Comparator.comparing` se define el primer criterio (la edad) y con `thenComparing` se añade el criterio secundario (el nombre). Esta forma es más expresiva, reduce errores y facilita la lectura del código, especialmente cuando se combinan varios criterios. A continuación se muestran ambas versiones:

---

### **Versión 1: Comparador manual**

```java
Collections.sort(lista, (p1, p2) -> {
    int cmpEdad = Integer.compare(p1.getEdad(), p2.getEdad());
    if (cmpEdad != 0) return cmpEdad;
    return p1.getNombre().compareTo(p2.getNombre());
});
```

---

### **Versión 2: Usando `Comparator`**

```java
Collections.sort(
    lista,
    Comparator.comparing(Persona::getEdad)
              .thenComparing(Persona::getNombre)
);
```
