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

Perfecto, Pablo. A partir de ahora redactaré las respuestas con el nivel de profundidad adecuado para alguien que viene de C/C++ sin POO y que ya conoce clases y objetos en Java. También usaré un estilo impersonal y cada respuesta tendrá entre **2 y 4 párrafos**, como pediste.

Aquí va la respuesta a tu primera pregunta del cuestionario:

---

## **1. En Programación Orientada a Objetos (POO), ¿qué buscan la encapsulación y la ocultación de información?**

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

### Respuesta


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


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
