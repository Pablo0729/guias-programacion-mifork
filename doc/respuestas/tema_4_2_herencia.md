

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

La **herencia** en orientación a objetos se entiende como una relación jerárquica donde un tipo más específico *extiende* a otro más general. La frase “A es‑un B” expresa precisamente esta relación: si `Artillero` es‑un `Soldado`, entonces todo artillero puede ser tratado como soldado. Esta idea no existe en C procedural, pero sí se aproxima a cómo una estructura puede contener otra; sin embargo, en Java la herencia añade reglas formales que permiten reutilizar código y establecer jerarquías conceptuales claras. La clase base define atributos y métodos comunes, mientras que las subclases amplían o especializan ese comportamiento.

La primera implicación es la **compatibilidad de tipos**. Si `Artillero` y `Zapador` heredan de `Soldado`, entonces cualquier instancia de estas subclases puede almacenarse en una variable de tipo `Soldado`. Esto permite trabajar con colecciones heterogéneas donde todos los elementos comparten una interfaz común, aunque su comportamiento concreto pueda diferir. Esta compatibilidad es fundamental para el polimorfismo, ya que permite invocar métodos definidos en la superclase sin conocer el subtipo exacto.

La segunda implicación es la **herencia de estado y comportamiento**. Las subclases reciben automáticamente los atributos y métodos no privados de la superclase, y también pueden acceder a los privados mediante getters o métodos protegidos. En este caso, tanto `Artillero` como `Zapador` heredan el atributo `nombre` y el método `saludar()`, lo que evita duplicar código. Cada subtipo añade su propio estado específico (`cohetes` o `minas`) y métodos de acceso, manteniendo la coherencia con la estructura general de un soldado.

El siguiente ejemplo muestra estas ideas de forma sencilla. Se define una clase base `Soldado` con un nombre y un saludo, y dos subclases que añaden capacidades específicas. Después, se crea un array de `Soldado` que contiene instancias de distintos tipos, demostrando la compatibilidad de tipos y la reutilización del comportamiento heredado.

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] escuadra = {
            new Artillero("Marco", 5),
            new Zapador("Luis", 3),
            new Artillero("Ana", 2)
        };

        for (Soldado s : escuadra) {
            s.saludar(); // Todos pueden saludar porque heredan el método
        }
    }
}
```
**CLASE**

Composición --> "tienes un/ tiene varios"

    VS

Herencia --> es un.



1. COMPATIBILIDAD DE TIPOS
    Soldado s = new Artillero ("Pepe");

2. Herencia de estado (--> atributos) y compartimento (--> métodos)


IMAGEN

```java
private class Artillero extends Soldado{

private int numCohetes;
public Artillero(int numCohetes){

super("nombre")
this.numCohetes = numCohetes

}


public class Zapador


}


public class PruebaHerencia{
    private static void pasarRevista (Soldado[] soldados){
        for(Soldado soldado: soldados){
        soldado.saludar();
        }

    }
}


public static void main (String[] args){


    Soldado[] soldadosVarios = new Soldado [3];

    soldadosVarios[0] = new Artillero("Juan");
    soldadosVarios[1] = new Artillero("Ana");
    soldadosVarios[2] = new Zapador("Pepe");

}

```



## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre?

Al crear un objeto de una subclase en Java, se ejecutan **dos constructores como mínimo**: primero el de la **superclase** y después el de la **subclase**. Este orden es obligatorio porque la parte “soldado” del objeto debe inicializarse antes que la parte “artillero” o “zapador”. En términos de memoria, puede imaginarse como construir primero el bloque base común y luego añadir encima la especialización. Aunque en C no existe este mecanismo, puede compararse con inicializar primero una estructura incluida antes de inicializar los campos propios.

La palabra clave `super` dentro de un constructor sirve para **invocar explícitamente el constructor de la superclase**. Esto permite pasarle parámetros y garantizar que el estado heredado queda correctamente inicializado. Si no se escribe `super(...)`, Java inserta automáticamente una llamada a `super()` sin argumentos. Esta llamada implícita solo funciona si la superclase tiene un constructor sin parámetros y además es accesible.

Si la clase base **no tiene visible** el constructor sin parámetros (por ejemplo, solo define `Soldado(String nombre)`), entonces la llamada a `super(...)` debe hacerse **siempre** y de forma explícita. De lo contrario, el compilador producirá un error porque intentará insertar `super()` y no encontrará un constructor adecuado. Por tanto, en subclases como `Artillero` o `Zapador` es obligatorio llamar a `super(nombre)` para que la parte heredada del objeto quede correctamente construida.

En el ejemplo anterior, cuando se crea un `new Artillero("Marco", 5)`, primero se ejecuta el constructor de `Soldado(String nombre)` y después el de `Artillero(String nombre, int cohetes)`. Este orden garantiza que el atributo `nombre` queda inicializado antes de que la subclase añada su propio estado (`cohetes` o `minas`).


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

En una instancia de una subclase, **todos los atributos heredados existen físicamente en memoria**, incluidos los que son privados en la superclase. Esto significa que un `Artillero` contiene internamente un “bloque Soldado” completo, con su atributo privado `nombre`, igual que si se hubiera creado un `Soldado` normal. La subclase no elimina ni sustituye esos campos; simplemente añade los suyos propios encima. Desde el punto de vista de la memoria, el objeto es una composición vertical: primero la parte de `Soldado`, luego la parte de `Artillero` o `Zapador`.

Sin embargo, que un atributo privado exista en memoria **no implica** que pueda usarse directamente desde el código de la subclase. La privacidad es una restricción del lenguaje, no de la memoria. El atributo `nombre` está presente dentro de cada `Artillero`, pero el código de `Artillero` no puede escribir `this.nombre` porque la visibilidad `private` lo impide. La única forma de acceder a ese estado heredado es a través de métodos públicos o protegidos definidos en la superclase, como el constructor o un posible getter. Esto garantiza encapsulación y evita que las subclases rompan invariantes internas.

Aplicado al ejemplo, un `Artillero` contiene en memoria el atributo `nombre` heredado de `Soldado`, pero no puede manipularlo directamente. El constructor de `Artillero` llama a `super(nombre)` para que sea la propia clase `Soldado` quien inicialice ese campo privado. Si se necesitara leerlo, debería existir un método público en `Soldado`, como `getNombre()`. De este modo, la subclase hereda el estado y el comportamiento, pero no la capacidad de vulnerar la encapsulación.

En resumen, la herencia transmite estructura y comportamiento, pero **no rompe las reglas de acceso**. La presencia física del atributo privado en la instancia no otorga permiso para usarlo desde la subclase; solo confirma que la parte “soldado” del objeto está realmente ahí, integrada en cada instancia de `Artillero` o `Zapador`.


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.


La compatibilidad de tipos propia de la herencia permite que el código sea **extensible** sin necesidad de modificar las partes que trabajan con el tipo general. Si todas las subclases son tratables como `Soldado`, entonces cualquier algoritmo que opere sobre soldados seguirá funcionando aunque aparezcan nuevos tipos especializados. Esta propiedad es clave en diseño orientado a objetos porque permite añadir comportamientos sin alterar el código existente, reduciendo el acoplamiento y evitando que cada ampliación obligue a reescribir estructuras ya probadas.

En términos prácticos, esto significa que el bucle que recorre un array de `Soldado` y llama a `saludar()` no necesita conocer la existencia de nuevos subtipos. El contrato definido por la superclase garantiza que todos los soldados, presentes o futuros, podrán ejecutar ese método. Así, el sistema puede crecer añadiendo nuevas clases sin romper el código que ya funciona. Esta capacidad de extender sin modificar es una de las bases del principio *Open/Closed* en diseño de software.

Para ilustrarlo, puede añadirse un nuevo tipo de soldado, por ejemplo un `Francotirador`, que hereda de `Soldado` y añade su propio estado. No es necesario cambiar el código que recorre la escuadra: el nuevo tipo se integra de forma natural gracias a la compatibilidad de tipos. El comportamiento común (saludar) se mantiene, y el específico queda encapsulado en la nueva subclase.

```java
class Francotirador extends Soldado {
    private int balas;

    public Francotirador(String nombre, int balas) {
        super(nombre);
        this.balas = balas;
    }

    public int getBalas() {
        return balas;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] escuadra = {
            new Artillero("Marco", 5),
            new Zapador("Luis", 3),
            new Francotirador("Clara", 10)
        };

        for (Soldado s : escuadra) {
            s.saludar(); // No se modifica: funciona con cualquier nuevo tipo de Soldado
        }
    }
}
```

El código que pide el saludo permanece idéntico, demostrando que la herencia permite ampliar el sistema sin alterar las partes ya establecidas.


***



## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Una referencia del **supertipo** puede apuntar sin problema a un objeto real de cualquier **subtipo**. Esto es precisamente la base del polimorfismo: si `Artillero` y `Zapador` son `Soldado`, entonces una variable de tipo `Soldado` puede contener indistintamente cualquiera de ellos. Esta compatibilidad no funciona al revés: una referencia de subtipo no puede apuntar a un objeto del supertipo sin conversión explícita, porque no todos los soldados son artilleros o zapadores. Este mecanismo permite escribir código general que opere sobre el tipo base sin conocer los detalles concretos de cada subtipo.

Sin embargo, una referencia del supertipo **no puede invocar métodos que solo existen en el subtipo**, aunque el objeto real sí los tenga. Si se tiene un `Soldado s = new Artillero("Ana", 4);`, la referencia `s` solo permite llamar a los métodos definidos en `Soldado`. Para acceder a métodos específicos del subtipo, como `getCohetes()`, es necesario realizar un **downcasting**, es decir, convertir la referencia del supertipo al subtipo real. El proceso inverso, convertir un subtipo a su supertipo, se denomina **upcasting** y siempre es seguro, por lo que Java lo hace de forma implícita.

El operador `instanceof` permite comprobar en tiempo de ejecución si un objeto referenciado realmente pertenece a un subtipo concreto. Esta comprobación es esencial antes de hacer un downcasting, ya que evita errores de tipo. Si `s instanceof Artillero` es verdadero, entonces puede realizarse el casting y acceder a los métodos propios del artillero. Este patrón es habitual cuando se recorren colecciones heterogéneas de objetos que comparten una superclase común.

El siguiente ejemplo muestra un recorrido por un array de `Soldado`, donde se detecta si cada elemento es un `Artillero` y, en ese caso, se imprime su número de cohetes:

```java
for (Soldado s : escuadra) {
    s.saludar(); // Método común heredado

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // Downcasting seguro
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```

Este patrón combina compatibilidad de tipos, polimorfismo y comprobación dinámica del subtipo, permitiendo trabajar con jerarquías de clases de forma flexible y segura.



## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso **protegido** (`protected`) permite que un atributo o método sea accesible desde la propia clase, desde cualquier subclase y también desde otras clases del mismo paquete. Se sitúa a medio camino entre `private` y `public`: mantiene la ocultación frente a código externo, pero permite que las subclases trabajen directamente con ciertos elementos heredados. En herencia, este modificador resulta útil cuando la superclase necesita exponer parte de su estado interno a las subclases sin romper completamente la encapsulación.

En Java, el acceso protegido se implementa mediante la palabra clave `protected` delante del atributo o método. Esto permite que una subclase pueda leer o modificar ese elemento sin necesidad de getters o setters, siempre que se considere seguro para la integridad del objeto. A diferencia de `private`, que impide cualquier acceso directo desde la subclase, `protected` habilita una relación más estrecha entre superclase y subclases, facilitando la especialización del comportamiento heredado.

Aplicado al ejemplo, si se desea que el `Zapador` pueda usar el nombre del soldado en su método de poner minas, basta con declarar el atributo `nombre` como protegido en la clase `Soldado`. De este modo, el zapador puede acceder directamente a ese campo sin violar las reglas de visibilidad. Esta decisión debe tomarse con criterio: se expone parte del estado interno, pero se mantiene dentro del ámbito de la jerarquía de herencia.

El siguiente ejemplo muestra cómo implementar este acceso protegido y cómo el `Zapador` puede utilizar el nombre heredado:

```java
class Soldado {
    protected String nombre; // Ahora es accesible desde subclases

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy " + nombre);
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMina() {
        System.out.println(nombre + " ha puesto una mina"); // Acceso permitido
    }
}
```

Con este cambio, el `Zapador` puede utilizar el nombre directamente, demostrando cómo `protected` permite compartir información relevante dentro de la jerarquía sin exponerla públicamente.



## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En los lenguajes orientados a objetos, algunos establecen una **clase base universal** de la que heredan automáticamente todas las demás clases. Esta clase raíz proporciona un conjunto mínimo de operaciones comunes que todos los objetos comparten, lo que facilita la integración del lenguaje, la reflexión y ciertos mecanismos internos. Sin embargo, no todos los lenguajes adoptan este enfoque: algunos permiten jerarquías sin una raíz común o incluso múltiples raíces según el diseño del sistema de tipos.

En Java, **todas las clases heredan implícitamente de `java.lang.Object`**, salvo que se indique explícitamente otra superclase. Esto significa que cualquier clase definida por el programador, incluso si no se especifica `extends`, posee automáticamente métodos como `toString()`, `equals()`, `hashCode()` o `getClass()`. Esta herencia implícita unifica el comportamiento básico de todos los objetos del lenguaje y permite que estructuras como colecciones, comparaciones o mecanismos de reflexión funcionen de manera uniforme.

No ocurre así en todos los lenguajes. Por ejemplo, en C++ no existe una clase base universal: una clase solo hereda de otra si se indica explícitamente, y es perfectamente válido crear jerarquías sin raíz común. Otros lenguajes, como C#, Python o Ruby, sí siguen el modelo de una clase base universal (`object` en C#, `object` en Python, `Object` en Ruby). La decisión depende del diseño del lenguaje y de cómo se conciba la uniformidad del sistema de tipos.

En el caso concreto de Java, esta clase raíz única simplifica la interoperabilidad entre componentes y garantiza que cualquier objeto, independientemente de su jerarquía, pueda ser tratado de forma homogénea. Gracias a ello, estructuras como `ArrayList<Object>` pueden almacenar cualquier tipo de objeto, y mecanismos como la comparación o la conversión a cadena están siempre disponibles sin necesidad de redefinirlos desde cero.



## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La **herencia múltiple** consiste en permitir que una clase tenga **más de una superclase directa**, heredando simultáneamente atributos y métodos de todas ellas. Este modelo existe en algunos lenguajes como C++, donde una clase puede extender varias clases base. La idea ofrece flexibilidad, pero también introduce problemas de ambigüedad, como el conocido *problema del diamante*, en el que un atributo heredado desde dos rutas distintas genera conflictos sobre cuál versión debe usarse. Por ello, muchos lenguajes modernos han optado por evitarla o limitarla para mantener la claridad del modelo de herencia.

En Java **no existe herencia múltiple de clases**. Una clase solo puede extender a una única superclase. Esta decisión se tomó para evitar ambigüedades y simplificar el sistema de tipos. Sin embargo, Java sí permite **herencia múltiple de comportamiento** mediante **interfaces**, ya que una clase puede implementar tantas interfaces como necesite. Las interfaces no contienen estado mutable heredable, por lo que no generan los conflictos típicos de la herencia múltiple clásica.

Este enfoque híbrido permite que Java mantenga un modelo de herencia simple y seguro, sin renunciar a la capacidad de combinar comportamientos diversos. Las interfaces actúan como contratos que las clases pueden adoptar, mientras que la herencia de clases se reserva para la relación jerárquica principal. Así, se obtiene extensibilidad sin comprometer la coherencia del diseño.

En resumen, Java evita la herencia múltiple de clases para impedir problemas de ambigüedad, pero ofrece una alternativa segura mediante interfaces. Esto permite que una clase pueda “parecer” de muchos tipos distintos sin heredar múltiples estructuras internas, manteniendo un modelo de herencia claro y predecible.



## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

En los lenguajes orientados a objetos, una excepción personalizada no es más que una clase que **hereda** de otra excepción del lenguaje. En Java, todas las excepciones son objetos y forman una jerarquía que parte de `Throwable`. Para crear una excepción *no controlada* basta con heredar de `RuntimeException`, ya que este subtipo no obliga al programador a capturarla ni declararla con `throws`. Esto permite modelar errores lógicos o situaciones inesperadas sin imponer manejo obligatorio. Además, una excepción puede contener otros objetos como parte de su estado interno, lo que facilita transportar información relevante sobre el error.

En este caso, se desea una excepción `UsuarioNoEncontradoException` que incluya un objeto `Usuario` para indicar cuál provocó el problema. Esta composición permite que el código que capture la excepción pueda inspeccionar el usuario implicado y actuar en consecuencia. Para ello, basta con añadir un atributo privado y un getter. La excepción debe ofrecer al menos dos constructores: uno que reciba el usuario y un mensaje, y otro que además permita incluir la causa subyacente mediante un parámetro `Throwable`. Esta sobrecarga es habitual en Java y facilita encadenar errores.

La excepción personalizada sigue el mismo patrón que cualquier clase, pero aprovechando la herencia para integrarse en la jerarquía de excepciones del lenguaje. Al heredar de `RuntimeException`, se convierte automáticamente en una excepción no controlada. El uso de composición con `Usuario` no afecta al comportamiento de la excepción, pero sí mejora la capacidad de diagnóstico. De este modo, el diseño combina herencia para el tipo de error y composición para transportar información contextual.

Un ejemplo completo podría ser el siguiente:

```java
class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario, String mensaje) {
        super(mensaje);
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, String mensaje, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

Con esta implementación, cualquier parte del programa puede lanzar la excepción incluyendo el usuario problemático, y el código que la capture podrá consultar tanto el mensaje como la causa y el propio objeto `Usuario`.



## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

La herencia no debe emplearse únicamente como mecanismo de **reutilización de código** porque introduce una relación jerárquica fuerte entre clases que quizá no refleje correctamente el dominio del problema. Cuando se hereda, se afirma que existe una relación conceptual del tipo “A es‑un B”, lo cual implica compatibilidad de tipos, propagación de comportamiento y dependencia estructural. Si esa relación no es verdadera y solo se busca aprovechar métodos ya escritos, la jerarquía resultante se vuelve artificial y frágil. Esto conduce a diseños rígidos donde cambios en la superclase afectan a todas las subclases, incluso cuando no deberían, generando acoplamiento innecesario.

Además, la herencia fija en tiempo de compilación la estructura del objeto, lo que limita la flexibilidad. Si se usa para reutilizar código sin que exista una relación conceptual clara, se obliga a las subclases a heredar atributos y métodos que no necesitan, violando el principio de responsabilidad única. Esto puede provocar que las subclases tengan comportamientos incoherentes o que deban sobrescribir métodos solo para neutralizar funcionalidades heredadas. En estos casos, la reutilización obtenida es superficial y el coste en complejidad estructural es elevado.

La **composición**, en cambio, permite reutilizar código sin imponer una jerarquía conceptual. En lugar de decir “A es‑un B”, se expresa “A tiene‑un B”, lo que evita dependencias rígidas y permite intercambiar componentes fácilmente. La composición favorece diseños más modulares, donde cada clase delega tareas a objetos internos sin heredar su estructura completa. Esto reduce el acoplamiento y facilita la evolución del sistema, ya que los cambios en un componente no afectan a toda una jerarquía.

Por estas razones, la herencia debe emplearse cuando exista una relación conceptual genuina y estable entre superclase y subclase, mientras que la composición es preferible cuando el objetivo principal es **reutilizar funcionalidad** sin imponer una jerarquía innecesaria. Esta distinción conduce a diseños más claros, mantenibles y coherentes con los principios fundamentales de la orientación a objetos.



## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

La recomendación de *“favorecer la composición frente a la herencia”* surge de la necesidad de construir sistemas más flexibles, mantenibles y con menor acoplamiento. La herencia establece una relación rígida y permanente entre superclase y subclase: si una clase hereda de otra, queda ligada a su estructura interna, a sus decisiones de diseño y a cualquier cambio futuro. Esto puede generar problemas cuando la jerarquía no refleja una relación conceptual auténtica del tipo “A es‑un B”, ya que la subclase hereda comportamientos que quizá no necesita o que incluso debe anular, lo que conduce a diseños frágiles y difíciles de extender sin romper código existente.

La composición, en cambio, permite construir clases complejas a partir de otras más simples mediante relaciones del tipo “A tiene‑un B”. Este enfoque evita imponer jerarquías innecesarias y reduce el acoplamiento, ya que los objetos colaboran entre sí sin depender de la estructura interna de los demás. Además, la composición permite intercambiar componentes en tiempo de ejecución, lo que ofrece una flexibilidad que la herencia no puede proporcionar. De este modo, se pueden modificar comportamientos sin alterar jerarquías completas, manteniendo el sistema más estable y adaptable.

Otro motivo para favorecer la composición es que evita los problemas derivados de la herencia profunda, como la propagación no deseada de métodos, la dificultad para mantener invariantes o la aparición de efectos secundarios inesperados. La composición encapsula mejor el comportamiento, ya que cada clase delega tareas a sus componentes sin heredar su estructura completa. Esto facilita cumplir principios de diseño como responsabilidad única, bajo acoplamiento y alta cohesión, que son esenciales para sistemas robustos.

En conjunto, la composición ofrece una forma más controlada y modular de reutilizar código, mientras que la herencia debe reservarse para relaciones conceptuales claras y estables. Por ello, los principios de diseño modernos recomiendan emplear herencia solo cuando sea semánticamente correcta y utilizar composición como mecanismo preferente para extender funcionalidad sin comprometer la arquitectura del sistema.



## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

La afirmación de que la **herencia rompe la encapsulación** se refiere a que una subclase queda expuesta a detalles internos de la superclase que, idealmente, deberían permanecer ocultos. Aunque los atributos privados siguen siendo inaccesibles, la subclase depende de la estructura, las decisiones de diseño y los métodos internos de la superclase. Esto significa que cualquier cambio en la implementación de la superclase puede afectar de forma inesperada a todas sus subclases, incluso si estas no deberían verse afectadas conceptualmente. La encapsulación se debilita porque la subclase no trata a la superclase como una “caja negra”, sino como algo cuyo funcionamiento interno condiciona su propio comportamiento.

Además, la herencia obliga a que la subclase herede *todo* el comportamiento público y protegido de la superclase, incluso aquel que no necesita o que no encaja con su responsabilidad. Esto puede llevar a sobrescribir métodos solo para evitar comportamientos heredados, lo cual es una señal de que la encapsulación se ha visto comprometida. La subclase queda acoplada a decisiones internas de la superclase, y ese acoplamiento hace que la jerarquía sea frágil: un cambio en la superclase puede romper subclases que no estaban preparadas para ese cambio.

En contraste, la **composición** mantiene la encapsulación porque un objeto utiliza a otro sin depender de su estructura interna. La clase compuesta solo conoce la interfaz pública del componente, no su implementación. Esto permite sustituir componentes, modificar su comportamiento interno o extenderlos sin afectar a las clases que los usan. La relación es más flexible y respeta mejor la separación entre interfaz e implementación, manteniendo la encapsulación intacta.

Por tanto, se dice que la herencia rompe la encapsulación porque introduce un acoplamiento estructural fuerte entre superclase y subclase, mientras que la composición preserva la independencia entre componentes. Esta es una de las razones por las que se recomienda emplear herencia solo cuando exista una relación conceptual clara y estable, y preferir composición cuando el objetivo sea extender o reutilizar comportamiento sin comprometer la arquitectura.



## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

La herencia y la composición permiten modelar relaciones entre clases, pero lo hacen desde perspectivas conceptuales distintas. Cuando se usa **herencia**, se afirma que existe una relación del tipo “A es‑una B”. En este caso, `Estudiante` y `Trabajador` serían dos tipos específicos de `Persona`, y ambos heredarían directamente los atributos comunes como el DNI y el nombre. Esta solución funciona bien cuando la relación conceptual es clara y estable, y cuando las subclases realmente representan especializaciones naturales de la superclase. La reutilización de código es directa, pero se introduce una jerarquía rígida que puede volverse problemática si aparecen nuevas necesidades que no encajan bien en la estructura heredada.

La **composición**, en cambio, expresa una relación del tipo “A tiene‑un B”. En lugar de heredar los datos comunes, `Estudiante` y `Trabajador` contienen internamente un objeto `DatosPersonales`. Este enfoque evita imponer una jerarquía innecesaria y permite mayor flexibilidad: si en el futuro se desea modificar cómo se gestionan los datos personales, basta con cambiar la clase `DatosPersonales` sin afectar a las clases que la utilizan. Además, la composición favorece un diseño más modular y reduce el acoplamiento entre clases, ya que cada una delega responsabilidades en componentes internos sin heredar su estructura completa.

A continuación se muestran ambas alternativas. La primera emplea herencia con una superclase `Persona`, mientras que la segunda utiliza composición con una clase `DatosPersonales`. Ambas modelan el mismo dominio, pero desde enfoques conceptuales distintos. La comparación permite apreciar cómo la herencia fija una jerarquía, mientras que la composición ofrece mayor libertad estructural.

### ✔ Alternativa 1: Herencia (`Persona` como superclase)

```java
class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
```

### ✔ Alternativa 2: Composición (`DatosPersonales` como componente interno)

```java
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}
```

Ambas soluciones son válidas, pero cada una responde a una filosofía distinta: la herencia modela especialización jerárquica, mientras que la composición modela colaboración entre objetos. Esta distinción es clave para elegir la estructura más adecuada según el dominio y la evolución prevista del sistema.

