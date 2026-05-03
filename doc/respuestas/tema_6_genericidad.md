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
LO ANTERIOR ES LO DE CLASE. A PARTIR DE AHORA ES LA RESPUESTA DE LA IA

### Respuesta

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`

### Respuesta

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo

### Respuesta

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

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

### Respuesta

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`

### Respuesta
