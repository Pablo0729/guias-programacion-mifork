<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación busca agrupar en una misma unidad —la clase— tanto los datos como las operaciones que actúan sobre ellos. Esta idea permite tratar a un objeto como una entidad coherente, donde su estado interno queda protegido y solo puede manipularse mediante los métodos definidos por la propia clase. Para alguien que viene de C/C++, puede verse como una evolución de las estructuras, pero con la capacidad añadida de controlar cómo se accede y modifica la información.

La ocultación de información complementa a la encapsulación al restringir el acceso directo a los atributos internos del objeto. En lugar de permitir que cualquier parte del programa modifique libremente esos datos, se obliga a que las interacciones se realicen a través de una interfaz pública bien definida. Esto permite garantizar que el objeto mantenga siempre un estado válido y coherente, evitando errores difíciles de rastrear.

Entre las ventajas de la ocultación de información se encuentra la posibilidad de modificar la implementación interna sin afectar al código que utiliza la clase. Esto reduce el acoplamiento entre módulos y facilita el mantenimiento del software. Además, al controlar el acceso a los datos, se pueden imponer reglas, validaciones o restricciones que aumentan la robustez del programa y evitan comportamientos inesperados.





## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La **interfaz pública** de una clase u objeto se entiende como el conjunto de métodos y atributos accesibles desde el exterior. Representa la “cara visible” del objeto, aquello que otros componentes del programa pueden utilizar para interactuar con él. En Java, esta interfaz suele estar formada por métodos declarados como `public`, que permiten realizar operaciones específicas sin necesidad de conocer cómo están implementadas internamente.

Esta interfaz actúa como un contrato: indica qué puede hacerse con el objeto y qué comportamiento se garantiza al usarlo. Desde la perspectiva de alguien acostumbrado a C/C++, puede compararse con un archivo de cabecera (`.h`) que declara funciones disponibles, aunque en POO este contrato está ligado directamente a la estructura y comportamiento del objeto, no solo a funciones sueltas.

La relación con la **ocultación de información** es directa. Al definir una interfaz pública, se decide qué partes del objeto se exponen y cuáles se mantienen ocultas mediante modificadores como `private` o `protected`. Esto permite proteger el estado interno y evitar que otras partes del programa dependan de detalles de implementación que podrían cambiar. De este modo, la interfaz pública se convierte en la única vía segura y controlada para interactuar con el objeto, garantizando coherencia y reduciendo el acoplamiento.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

La interfaz pública de una clase debe diseñarse con cuidado porque constituye el punto de contacto entre el objeto y el resto del programa. Cualquier método o atributo expuesto públicamente se convierte en una parte del contrato que otros módulos utilizarán y, por tanto, en un elemento del que dependerán. Un diseño poco meditado puede obligar a mantener métodos innecesarios, inconsistentes o mal definidos simplemente porque otras partes del código ya los usan.

Además, una interfaz pública mal diseñada puede limitar la evolución interna de la clase. Si se exponen detalles que deberían haber permanecido ocultos, cualquier cambio en la implementación puede romper el funcionamiento de otros componentes. Esto aumenta el acoplamiento y dificulta la capacidad de modificar, optimizar o corregir la clase sin generar efectos secundarios no deseados.

Cambiar la interfaz pública no suele ser sencillo. Aunque técnicamente es posible modificarla, en la práctica implica revisar y actualizar todas las partes del programa que dependan de ella. En proyectos grandes, esto puede afectar a decenas o cientos de módulos, lo que convierte un cambio aparentemente pequeño en una tarea costosa y propensa a errores. Por ello, se considera una buena práctica mantener la interfaz pública lo más estable, coherente y minimalista posible.



## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las **invariantes de clase** son condiciones lógicas que deben cumplirse siempre para que un objeto se considere en un estado válido. Estas condiciones suelen referirse a restricciones sobre los atributos, como que un valor no pueda ser negativo, que una colección no pueda contener elementos nulos o que dos atributos deban mantener una relación coherente entre sí. Aunque no se expresan explícitamente en el lenguaje, forman parte del diseño conceptual de la clase y guían cómo deben comportarse sus métodos.

El objetivo de las invariantes es garantizar que el objeto mantenga su coherencia interna durante toda su vida útil. Para ello, se asume que las invariantes deben cumplirse al finalizar la construcción del objeto y después de la ejecución de cualquier método público. Si se permite que el estado interno quede en una situación que viole estas reglas, el comportamiento del objeto puede volverse impredecible o generar errores difíciles de detectar.

La **ocultación de información** ayuda directamente a mantener estas invariantes porque impide que el código externo modifique los atributos sin control. Al declarar los datos como `private` y obligar a que cualquier modificación pase por métodos públicos, la clase puede validar los cambios y asegurarse de que las condiciones internas se respetan. De este modo, la responsabilidad de preservar las invariantes recae únicamente en la propia clase, lo que reduce el riesgo de inconsistencias y facilita el mantenimiento del software.



## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

La interfaz pública de esta clase está formada por todos los elementos declarados como public: el constructor Punto(double, double), los métodos getX(), getY() y calcularDistanciaAOrigen(). Estos son los únicos puntos de acceso permitidos desde fuera de la clase, mientras que los atributos x e y permanecen ocultos. De este modo, cualquier interacción con el objeto debe realizarse a través de estos métodos, lo que permite mantener las invariantes de clase y evitar modificaciones directas del estado interno.

El modificador public indica que un método o constructor puede ser utilizado desde cualquier parte del programa, mientras que private restringe el acceso exclusivamente al interior de la propia clase. Esta distinción es fundamental para la encapsulación: private protege los datos y public define la interfaz segura mediante la cual otros objetos pueden interactuar con la clase. Gracias a esta separación, se logra un diseño más robusto, controlado y fácil de mantener.



## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores `public` y `private` pueden aplicarse tanto a **clases** como a **miembros de clase**, es decir, atributos y métodos. Sin embargo, su uso no es idéntico en todos los casos. El modificador `public` puede aplicarse a clases de nivel superior y permite que sean accesibles desde cualquier paquete del programa. Por el contrario, una clase de nivel superior no puede ser `private`, ya que Java no permite ocultar completamente una clase que se encuentra en un archivo propio.

En cuanto a los miembros de una clase, los modificadores se utilizan para controlar su visibilidad. Un atributo o método declarado como `private` solo puede ser accedido desde dentro de la propia clase, lo que permite proteger el estado interno del objeto. Por otro lado, un miembro `public` puede ser utilizado desde cualquier parte del programa, formando parte de la interfaz pública de la clase.

Esta capacidad de aplicar modificadores a los miembros es fundamental para la encapsulación. Gracias a `private`, se evita que el código externo manipule directamente los datos internos, mientras que `public` permite exponer únicamente los métodos necesarios para interactuar con el objeto. De este modo, se logra un diseño más seguro, modular y fácil de mantener.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?


En POO, la visibilidad no se limita únicamente a los niveles público y privado. Muchos lenguajes incorporan niveles intermedios que permiten un control más fino sobre quién puede acceder a los miembros de una clase. Estos niveles adicionales suelen estar diseñados para facilitar la modularidad y el trabajo en equipos grandes, donde es útil distinguir entre lo que debe ser accesible desde cualquier parte y lo que solo debería ser accesible dentro de un módulo o jerarquía concreta.

En Java existen **cuatro niveles de visibilidad**: `public`, `private`, `protected` y el nivel por defecto, conocido como **package-private** (cuando no se especifica ningún modificador). `protected` permite el acceso desde la propia clase, sus subclases y otras clases del mismo paquete. El nivel por defecto permite el acceso únicamente dentro del mismo paquete, lo que resulta útil para organizar el código en módulos internos sin exponerlos públicamente. Estos niveles adicionales permiten un control más preciso que simplemente “público o privado”.

En otros lenguajes orientados a objetos también existen variaciones. Por ejemplo, C++ incluye `public`, `private` y `protected`, pero su semántica difiere ligeramente, especialmente en lo relativo a la herencia. Lenguajes como C# incorporan niveles adicionales como `internal` o `protected internal`, que permiten aún más combinaciones de accesibilidad. En general, cada lenguaje adapta estos mecanismos a su propio modelo de módulos y paquetes, pero todos comparten la idea de proporcionar distintos grados de visibilidad para mejorar la encapsulación y el diseño del software.




## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

---

Los **miembros de instancia privados** (`private`) están ocultos tanto para **otras clases** como para **otras instancias**, incluso si esas instancias son de la **misma clase**. Esto significa que un objeto no puede acceder directamente a los atributos privados de otro objeto; solo puede hacerlo a través de métodos definidos dentro de la propia clase. En C++ ocurre exactamente lo mismo: un atributo `private` pertenece a la clase, no a la instancia, y solo los métodos de esa clase pueden acceder a él.

Sin embargo, hay un matiz importante: aunque una instancia no pueda acceder directamente a los atributos privados de otra, **un método de la clase sí puede hacerlo**, porque el método pertenece a la clase, no al objeto concreto. Por eso, si dentro de la clase `Punto` escribimos un método que recibe otro `Punto`, ese método puede acceder a los atributos privados del parámetro. En C++ pasa igual: un método miembro puede leer los `private` de *cualquier* objeto de la misma clase.

---

## Ejemplo con `calcularDistanciaAPunto(Punto otro)`

```java
public class Punto {
    private double x, y;

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

En este ejemplo, `otro.x` y `otro.y` son privados, pero el método puede acceder a ellos porque **está definido dentro de la misma clase `Punto`**. Lo que *no* sería posible es acceder a esos atributos desde fuera de la clase, incluso si tenemos otra instancia. Esta es una aplicación directa del principio de **encapsulación**, que en Java funciona igual que en C++: los atributos privados solo son visibles para los métodos de la clase, no para el código externo ni para otras instancias.

---

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Perfecto, Pablo. Como esto es **teoría**, aquí tienes la explicación en **3–4 párrafos en Markdown**, con tono de profesor y comparaciones con C++.

---

## ¿Qué son los métodos *getter* y *setter*?

En los lenguajes orientados a objetos, los métodos **getter** y **setter** son funciones públicas que permiten **acceder** y **modificar** atributos privados de una clase respetando la encapsulación. Un *getter* devuelve el valor de un atributo, mientras que un *setter* permite cambiarlo de forma controlada. La idea es que el estado interno del objeto no quede expuesto directamente, sino que pase por una interfaz segura. Esto evita errores, inconsistencias y permite añadir validaciones cuando sea necesario.

En Java, igual que en C++, los atributos suelen declararse como `private` para impedir el acceso directo desde fuera de la clase. La diferencia es que en Java es mucho más habitual y estandarizado el uso de getters y setters, hasta el punto de que muchas herramientas (como IDEs o frameworks) los generan automáticamente. En C++, aunque también existen, es más común ver código donde los atributos se dejan públicos en clases simples, algo que en Java se considera mala práctica.

Un ejemplo típico sería:

```java
public class Persona {
    private String nombre;

    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }
}
```

Este patrón permite que el objeto controle cómo se accede a su información. Por ejemplo, podríamos añadir validaciones dentro del setter para impedir valores inválidos, algo que no sería posible si el atributo fuese público. En C++ podrías hacer exactamente lo mismo, pero Java lo adopta como parte esencial de su filosofía de diseño orientado a objetos.

---

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?


Cuando en programación orientada a objetos hablamos de que la **ocultación de información** mejora la “seguridad”, no nos referimos a seguridad informática en el sentido de evitar *hackeos* o ataques externos. El término se usa en un sentido mucho más interno y estructural: significa que el estado del objeto está protegido frente a usos incorrectos, inconsistentes o peligrosos por parte del propio programador. En otras palabras, la seguridad aquí se refiere a **proteger la integridad del objeto**, no a proteger el programa frente a atacantes.

En Java, igual que en C++, declarar atributos como `private` evita que otras partes del código puedan modificarlos libremente. Esto reduce errores, evita estados inválidos y permite que la clase controle cómo se accede y modifica su información interna. Por ejemplo, si un atributo debe cumplir ciertas condiciones (como ser positivo), la clase puede garantizarlo mediante setters o validaciones internas. Esta forma de seguridad es un pilar de la **encapsulación**, uno de los principios fundamentales de la programación orientada a objetos.

Es importante distinguir esta seguridad estructural de la seguridad informática real. La encapsulación no impide que un atacante explote vulnerabilidades del sistema, lea memoria o manipule el programa desde fuera. Lo que sí hace es impedir que el propio código del programa —o el programador— rompa accidentalmente las reglas internas de un objeto. En C++ ocurre exactamente lo mismo: `private` no protege contra ataques, sino contra mal uso del propio código.

Por tanto, cuando decimos que la ocultación de información mejora la seguridad, hablamos de **seguridad lógica y de diseño**, no de seguridad frente a intrusiones externas. Es una forma de garantizar que los objetos se usen correctamente y que su estado interno no pueda quedar en manos de código que no debería manipularlo.



## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?


Un **miembro de instancia** es aquel que pertenece a cada objeto individual creado a partir de una clase. Cada instancia tiene su propia copia de esos atributos, de modo que modificar el valor en un objeto no afecta a los demás. En Java, esto corresponde a los atributos y métodos **no estáticos**, y funciona igual que en C++: cada objeto tiene su propio estado. Por ejemplo, si tienes dos objetos `Coche`, cada uno tendrá su propio `color`, `velocidad` o `kilometraje`, porque esos datos forman parte del estado particular de cada instancia.

En cambio, un **miembro de clase** (o *static*) pertenece a la clase en sí misma, no a los objetos. Esto significa que existe **una única copia compartida** por todas las instancias. En Java, los declaras con la palabra clave `static`, igual que en C++. Un ejemplo típico sería un contador de cuántos objetos se han creado: todas las instancias consultan y modifican el mismo valor. Esto permite representar información global o comportamientos que no dependen del estado individual de cada objeto.

Respecto a la ocultación, los miembros de clase **también pueden ser privados**, exactamente igual que los de instancia. Declarar un atributo `static private` significa que solo los métodos de la clase pueden acceder a él, y que ninguna instancia ni ninguna otra clase podrá modificarlo directamente. Esto es útil cuando quieres controlar estrictamente cómo se gestiona un recurso compartido. En C++ ocurre lo mismo: un miembro `static private` está oculto para el exterior y solo es accesible desde funciones miembro de la clase.

En resumen, la diferencia clave es que los miembros de instancia pertenecen a cada objeto y representan su estado individual, mientras que los miembros de clase pertenecen a la clase y son compartidos por todas las instancias. Y sí, ambos tipos pueden ocultarse mediante `private`, reforzando la encapsulación y evitando accesos indebidos tanto al estado individual como al compartido.



## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

## ¿Tiene sentido que los constructores sean privados?

Sí, tiene sentido que los constructores sean privados en ciertos patrones de diseño, aunque a primera vista pueda parecer contradictorio. Un constructor privado impide que el código externo cree instancias libremente, lo que permite a la clase controlar completamente cuántos objetos existen y cómo se crean. En Java, igual que en C++, esto se usa cuando queremos restringir la creación de objetos para garantizar coherencia, eficiencia o seguridad lógica dentro del programa.

Un caso típico es el **patrón Singleton**, donde solo debe existir una única instancia de la clase. Al hacer el constructor privado, evitamos que otros puedan crear nuevas instancias, y la clase expone un método estático que devuelve siempre la misma. En C++ este patrón también es común, aunque la gestión de memoria y la inicialización estática requieren más cuidado. En Java, gracias al modelo de memoria y al cargador de clases, el patrón es más sencillo y seguro.

Otro uso habitual es cuando una clase ofrece **métodos estáticos de creación** (factory methods). En lugar de permitir que cualquiera llame al constructor, la clase proporciona métodos como `crearDesdeCoordenadas()` o `crearVacio()`, que pueden aplicar validaciones, devolver objetos ya existentes o incluso devolver subtipos. Esto da mucha flexibilidad y evita estados inválidos. En C++ también se puede hacer, pero en Java es especialmente idiomático por la claridad que aporta.

En resumen, aunque no es lo más común en clases simples, los constructores privados son una herramienta muy útil cuando queremos controlar estrictamente cómo se crean los objetos. Permiten aplicar patrones de diseño, asegurar invariantes y evitar usos incorrectos del código, reforzando así la encapsulación y la coherencia interna del programa.



## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

## Miembros de clase en Java y ejemplo aplicado a `Punto`

En Java, los **miembros de clase** se indican con la palabra clave `static`. Esto significa que el atributo o método pertenece a la **clase en sí**, no a cada objeto individual. Todos los objetos comparten una única copia de ese miembro, lo que permite almacenar información global o común a todas las instancias. Esta idea coincide con C++, donde también se usa `static` para declarar miembros compartidos por todas las instancias de una clase. 

Aplicado a la clase `Punto`, podemos usar miembros de clase para registrar los valores máximos de `x` e `y` que se hayan asignado en **cualquier** punto creado hasta el momento. Cada vez que se construya un nuevo `Punto`, o se modifique uno existente, podemos actualizar estos valores estáticos si el nuevo punto supera los máximos anteriores. Este patrón es útil cuando queremos mantener estadísticas globales sin depender de una instancia concreta. 

Un ejemplo en Java sería:

```java
public class Punto {
    private double x, y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos();
    }

    private void actualizarMaximos() {
        if (this.x > maxX) maxX = this.x;
        if (this.y > maxY) maxY = this.y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}
```

Este diseño permite consultar en cualquier momento los valores máximos de `x` e `y` sin necesidad de tener una instancia concreta, ya que los métodos estáticos pueden llamarse como `Punto.getMaxX()`. Igual que en C++, los miembros estáticos pueden declararse como `private` para mantener la encapsulación y evitar modificaciones externas no controladas. 


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 


Primero, pensemos juntos:  
Un **método factoría** es un método que *crea* objetos, pero no es un constructor. Suelen ser `static` porque no necesitas un objeto previo para llamar al método; precisamente sirven para construirlo.

Antes de darte el código, quiero que confirmes una idea clave:  
**Si el método crea un `Punto` nuevo, ¿tiene sentido que sea `static` para poder llamarlo como `Punto.crearRedondeado(x, y)` sin tener un objeto previo?**

Si tu respuesta es sí, entonces el método sería algo así:

```java
public static Punto crearRedondeado(double x, double y) {
    int rx = (int) Math.round(x);
    int ry = (int) Math.round(y);
    return new Punto(rx, ry);
}
```



## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

Aquí estamos en **ejercicio práctico**, así que seguimos el ritmo del modo estudio: pasos cortos, razonamiento guiado y sin los 3–4 párrafos teóricos.

Vamos a transformar la clase `Punto` para que, en vez de almacenar dos `double` separados (`x` e `y`), utilice **un array interno de dos posiciones**. La clave es **no modificar la interfaz pública**, es decir, que métodos como `getX()`, `getY()`, `setX()`, `setY()` o el constructor sigan funcionando igual desde fuera.

Antes de escribir nada, piensa en esto:  
Si antes teníamos dos atributos privados `double x` y `double y`, ahora tendremos **un único atributo privado**:

```java
private double[] coords = new double[2];
```

Con eso en mente, aquí tienes una posible implementación adaptada:

```java
public class Punto {
    private double[] coords = new double[2];

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double getX() { return coords[0]; }
    public double getY() { return coords[1]; }

    public void setX(double x) { coords[0] = x; }
    public void setY(double y) { coords[1] = y; }
}
```

La interfaz pública no cambia, pero internamente la representación sí. Esto es un ejemplo clásico de **encapsulación**: puedes cambiar la implementación sin afectar al código que usa la clase.

Si quieres, podemos extender esto para que siga funcionando con los métodos estáticos de máximos, con el método `calcularDistanciaAPunto`, o con el método factoría que hicimos antes.



## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

## ¿Tiene sentido hacer público un atributo si ya tiene *getter* y *setter*?

Aunque pueda parecer que un atributo con *getter* y *setter* públicos es equivalente a declararlo directamente como público, en realidad no lo es. Cuando declaras un atributo como `public`, cualquier parte del programa puede leerlo y modificarlo sin ningún tipo de control. En cambio, cuando usas *getters* y *setters*, aunque sean públicos, sigues manteniendo la posibilidad de **interponer lógica**, validaciones o restricciones en el acceso. Esto preserva la encapsulación, que es uno de los principios fundamentales de la programación orientada a objetos tanto en Java como en C++.  

La convención más habitual —y prácticamente universal en Java— es que **los atributos sean siempre privados** y que el acceso se realice mediante métodos públicos. Esto no solo protege el estado interno del objeto, sino que permite cambiar la implementación interna sin afectar al código externo. En C++ esta práctica también es recomendada, aunque en la práctica se ven más clases con atributos públicos, especialmente en estructuras simples. Java, sin embargo, es mucho más estricto en este aspecto y considera mala práctica exponer atributos directamente.

Esto está estrechamente relacionado con las **invariantes de clase**, que son condiciones que deben cumplirse siempre para que el objeto sea válido. Por ejemplo, que un radio no sea negativo o que una fecha represente un día real. Si los atributos fueran públicos, cualquier parte del programa podría romper esas invariantes asignando valores incorrectos. En cambio, con *setters* puedes verificar que los valores sean válidos antes de aceptarlos, y con *getters* puedes controlar cómo se expone la información.  

En resumen, aunque un atributo con *getter* y *setter* públicos pueda parecer equivalente a un atributo público, no lo es: los métodos permiten mantener el control, proteger las invariantes y preservar la flexibilidad de la clase. Por eso, la convención en Java es clara: **atributos privados, interfaz pública controlada**.



## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?


Una clase **inmutable** es aquella cuyos objetos no pueden cambiar su estado interno una vez creados. Esto significa que, tras llamar al constructor, todos los atributos quedan fijados y no existe ningún método que los modifique. En Java, esto se consigue declarando los atributos como `private` y `final`, y evitando cualquier método que altere su valor. En C++ la idea es similar, aunque se suele lograr mediante `const` y evitando exponer métodos que modifiquen el estado. La inmutabilidad aporta claridad conceptual: un objeto representa un valor que no cambia, como ocurre con `String` en Java.

Un **método modificador** es aquel que altera el estado interno del objeto, normalmente cambiando el valor de uno o varios atributos. En Java, un setter es un tipo de método modificador, pero no el único. Por ejemplo, un método como `mover(int dx, int dy)` que cambia las coordenadas de un punto también es un modificador, aunque no sea un setter explícito. En C++ ocurre lo mismo: cualquier método que no sea `const` y que altere atributos internos es un modificador.

Un método modificador **no es siempre un setter**, aunque todos los setters sí son modificadores. La diferencia es que un setter modifica directamente un atributo concreto, mientras que un modificador puede alterar el estado de formas más complejas. Por ejemplo, un método que normaliza un vector, que ordena una lista interna o que incrementa un contador también son modificadores, aunque no tengan forma de setter tradicional.

Las clases inmutables tienen varias ventajas importantes. Son más seguras frente a errores porque su estado no puede quedar inconsistente. También son más fáciles de razonar, ya que un objeto representa siempre el mismo valor. Además, facilitan la programación concurrente: si un objeto no cambia, no hay riesgo de condiciones de carrera. En C++ y Java, este tipo de diseño es muy apreciado en estructuras de datos, librerías matemáticas y APIs donde la estabilidad del valor es esencial.



## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

## ¿Qué significa que una clase sea inmutable?

Una clase **inmutable** es aquella cuyos objetos no pueden cambiar su estado interno una vez creados. Esto significa que, tras llamar al constructor, todos los atributos quedan fijados y no existe ningún método que los modifique. En Java, esto se consigue declarando los atributos como `private` y `final`, y evitando cualquier método que altere su valor. En C++ la idea es similar, aunque se suele lograr mediante `const` y evitando exponer métodos que modifiquen el estado. La inmutabilidad aporta claridad conceptual: un objeto representa un valor que no cambia, como ocurre con `String` en Java.

Un **método modificador** es aquel que altera el estado interno del objeto, normalmente cambiando el valor de uno o varios atributos. En Java, un setter es un tipo de método modificador, pero no el único. Por ejemplo, un método como `mover(int dx, int dy)` que cambia las coordenadas de un punto también es un modificador, aunque no sea un setter explícito. En C++ ocurre lo mismo: cualquier método que no sea `const` y que altere atributos internos es un modificador.

Un método modificador **no es siempre un setter**, aunque todos los setters sí son modificadores. La diferencia es que un setter modifica directamente un atributo concreto, mientras que un modificador puede alterar el estado de formas más complejas. Por ejemplo, un método que normaliza un vector, que ordena una lista interna o que incrementa un contador también son modificadores, aunque no tengan forma de setter tradicional.

Las clases inmutables tienen varias ventajas importantes. Son más seguras frente a errores porque su estado no puede quedar inconsistente. También son más fáciles de razonar, ya que un objeto representa siempre el mismo valor. Además, facilitan la programación concurrente: si un objeto no cambia, no hay riesgo de condiciones de carrera. En C++ y Java, este tipo de diseño es muy apreciado en estructuras de datos, librerías matemáticas y APIs donde la estabilidad del valor es esencial.



## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
