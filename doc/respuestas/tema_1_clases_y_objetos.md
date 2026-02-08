

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

1. Abstracción
La abstracción consiste en centrarse en lo esencial y ocultar los detalles irrelevantes.
En Java, esto se refleja en cómo definimos clases que representan conceptos del mundo real sin necesidad de mostrar su complejidad interna.
Idea clave: Qué hace un objeto, no cómo lo hace.
Ejemplo mental:
Un coche tiene acelerar() y frenar(), pero no necesitas saber cómo funciona el motor para usarlo.

2. Encapsulamiento
El encapsulamiento consiste en proteger los datos de un objeto para evitar accesos indebidos.
En Java se logra usando modificadores como private, public, protected y métodos getters/setters. 
Se pueden cambiar detalles sin que las demás partes del programa se vean afectadas.
Idea clave: Controlar quién puede ver o modificar los datos.
Ejemplo mental:
Tu cuenta bancaria tiene un saldo, pero no puedes cambiarlo directamente; necesitas operaciones como ingresar() o retirar().

3. Herencia
Establecer jerarquías.
La herencia permite crear nuevas clases basadas en otras existentes, reutilizando código y comportamientos.
Idea clave: “Es un tipo de…”.
Ejemplo mental:
Un Perro es un tipo de Animal.
En Java: class Perro extends Animal.

4. Polimorfismo
El polimorfismo permite que un mismo método se comporte de forma distinta según el objeto que lo use.
Idea clave: Una misma acción, diferentes resultados.
Ejemplo mental:
El método hacerSonido() puede producir “guau” en un perro y “miau” en un gato.
En Java se ve en:
    • Sobrescritura de métodos (@Override) 
    • Sobrecarga de métodos (mismo nombre, diferentes parámetros) 


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

1. Java
Uno de los lenguajes más usados en universidades y empresas. Totalmente orientado a objetos (aunque permite tipos primitivos).
2. C++
Lenguaje multiparadigma que soporta programación orientada a objetos, muy usado en sistemas, videojuegos y aplicaciones de alto rendimiento.
3. Python
Lenguaje muy popular y flexible. Aunque es multiparadigma, su modelo de clases y objetos es muy potente y fácil de usar.
4. C#
Lenguaje de Microsoft, muy similar a Java en su sintaxis y fuertemente orientado a objetos. Muy usado en desarrollo de aplicaciones y videojuegos (Unity).


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La programación estructurada se basa en organizar el código siguiendo un flujo lógico mediante secuencia, selección y repetición. Su objetivo principal es evitar el desorden y los saltos incontrolados, haciendo que los programas sean más fáciles de leer y mantener. Este paradigma introdujo disciplina en la forma de construir algoritmos, pero no resolvía del todo el problema de manejar programas grandes.

La programación modular fue un paso más allá, proponiendo dividir el programa en partes independientes llamadas módulos o funciones. Cada módulo se encarga de una tarea concreta, lo que facilita la reutilización del código y reduce la complejidad. Además, introduce la idea de ocultar detalles internos mediante interfaces simples, anticipando conceptos que más tarde serían fundamentales en la Programación Orientada a Objetos.

En conjunto, ambos paradigmas prepararon el terreno para la POO: la programación estructurada aportó claridad en el control del flujo, mientras que la modular introdujo la división del programa en componentes reutilizables. La POO tomó estas ideas y las amplió, permitiendo representar entidades del mundo real mediante objetos que combinan datos y comportamiento.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

En programación orientada a objetos, un objeto se define a partir de tres elementos fundamentales que permiten describir tanto su estructura como su comportamiento. El primero de ellos son los atributos, que representan los datos o el estado interno del objeto. Estos atributos permiten almacenar información relevante, como por ejemplo el nombre, la edad o cualquier característica que describa a la entidad que se está modelando. En Java, suelen declararse como variables dentro de la clase, normalmente con visibilidad privada para favorecer el encapsulamiento.

El segundo elemento son los métodos, que describen las acciones o comportamientos que el objeto puede realizar. Los métodos permiten manipular los atributos, interactuar con otros objetos o ejecutar operaciones específicas. En Java, un método se define dentro de la clase y puede recibir parámetros, devolver valores o simplemente realizar una acción. Este componente es esencial para que los objetos no sean solo contenedores de datos, sino entidades activas dentro del programa.

El tercer elemento es la identidad, que permite distinguir un objeto de otro, incluso cuando ambos tienen los mismos atributos y métodos. La identidad se refiere al hecho de que cada objeto ocupa una posición única en memoria y se considera una instancia independiente. En Java, esta identidad se gestiona automáticamente al crear objetos mediante el operador new, lo que garantiza que cada instancia sea única aunque su contenido sea idéntico al de otra.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase puede entenderse como un modelo o plantilla que describe cómo serán los objetos que se creen a partir de ella. Define qué atributos tendrán esos objetos y qué métodos podrán ejecutar. En lenguajes como Java, la clase actúa como una especie de “molde” que organiza datos y comportamientos de forma coherente, permitiendo representar entidades del mundo real dentro del programa. Por tanto, la clase no es un objeto en sí misma, sino la definición teórica de cómo deben ser esos objetos.

Un objeto, en cambio, es una entidad concreta creada a partir de una clase. Mientras que la clase es solo una descripción, el objeto es algo real dentro del programa, con su propio estado almacenado en memoria. Por ejemplo, la clase Coche describe qué es un coche, pero cada coche concreto creado con new Coche() es un objeto distinto, con sus propios valores para atributos como color o velocidad. Esta diferencia es fundamental para entender la orientación a objetos.

El término instancia se utiliza para referirse a un objeto concreto creado a partir de una clase. Instanciar una clase significa crear un objeto real que sigue la estructura definida por esa clase. En Java, este proceso se realiza mediante el operador new, que reserva memoria y devuelve una referencia al nuevo objeto. Así, “objeto” e “instancia” suelen usarse como sinónimos, aunque “instancia” enfatiza el acto de creación.

No todos los lenguajes orientados a objetos manejan necesariamente el concepto de clase. Algunos lenguajes, como Java o C++, son basados en clases, mientras que otros, como JavaScript, utilizan un modelo basado en prototipos, donde los objetos se crean a partir de otros objetos sin necesidad de una clase formal. Esto demuestra que la orientación a objetos puede implementarse de distintas maneras según el lenguaje, aunque la mayoría de los lenguajes modernos sí utilizan clases como estructura principal.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En la mayoría de lenguajes orientados a objetos, los objetos se almacenan en memoria dinámica, es decir, en una zona que se reserva durante la ejecución del programa. En Java, esta zona se conoce como heap, y es donde se guardan todas las instancias creadas mediante el operador new. La referencia al objeto, en cambio, suele almacenarse en la pila (stack), lo que permite acceder al objeto sin necesidad de copiarlo. Este modelo difiere del de C, donde el programador decide explícitamente si un dato va en la pila o en el heap mediante funciones como malloc.

No todos los lenguajes gestionan la memoria de la misma forma. En C++, por ejemplo, los objetos pueden crearse tanto en la pila como en el heap, dependiendo de cómo se declaren, y el programador es responsable de liberar la memoria cuando ya no se necesita. En cambio, lenguajes como Java o Python utilizan exclusivamente memoria dinámica para los objetos y delegan la gestión de su ciclo de vida en el propio lenguaje. Esto hace que el comportamiento sea más uniforme, pero también implica que el programador tiene menos control directo sobre la memoria.

La recolección de basura (garbage collection) es un mecanismo automático que se encarga de liberar la memoria ocupada por objetos que ya no están en uso. En Java, este proceso se ejecuta en segundo plano y detecta cuándo un objeto ha dejado de ser accesible desde cualquier parte del programa. Una vez identificado, el recolector libera su espacio en el heap, evitando fugas de memoria y reduciendo la carga del programador. Este enfoque contrasta con lenguajes como C++, donde la liberación debe hacerse manualmente mediante delete.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un método es una unidad de comportamiento definida dentro de una clase y representa una acción que los objetos de esa clase pueden realizar. En Java, un método puede recibir parámetros, devolver un valor o simplemente ejecutar una operación. Para alguien que viene de C, puede verse como una función, pero asociada directamente a un tipo de objeto. Esto permite que los datos (atributos) y las operaciones que actúan sobre ellos estén agrupados dentro de la misma estructura, lo que facilita la organización y el diseño del programa.

La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre dentro de una misma clase, siempre que tengan parámetros distintos en número o tipo. Este mecanismo permite ofrecer diferentes formas de realizar una misma acción, adaptándose a las necesidades del programa sin tener que inventar nombres nuevos para cada variante. 

En Java, la sobrecarga se resuelve en tiempo de compilación, ya que el compilador determina qué versión del método debe ejecutarse según los argumentos utilizados.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

public class EjemploUso {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + distancia);
    }
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

En un programa escrito en Java, el punto de entrada es siempre el método main, cuya firma estándar es public static void main(String[] args). Este método actúa como la puerta de inicio desde la que la máquina virtual de Java comienza a ejecutar el programa. A diferencia de C o C++, donde main puede tener distintas variantes, en Java la firma debe respetarse exactamente para que el programa pueda iniciarse correctamente.

La palabra clave static indica que un método o atributo pertenece a la clase y no a un objeto concreto. Esto significa que puede utilizarse sin necesidad de crear una instancia. En el caso del método main, se declara como estático porque la JVM necesita ejecutarlo antes de que exista ningún objeto. Sin embargo, static no se limita al método main; también puede emplearse para definir métodos utilitarios, constantes o atributos compartidos entre todas las instancias de una clase.

Cuando static se combina con la palabra clave final, se obtiene un elemento que es compartido por la clase y cuyo valor no puede modificarse. Esta combinación se utiliza habitualmente para definir constantes, como static final double PI = 3.14159;. De este modo, se garantiza que el valor es único, accesible sin crear objetos y no puede alterarse accidentalmente durante la ejecución del programa.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar y ejecutar un programa en Java desde la línea de comandos se utilizan dos herramientas básicas: javac y java. El comando javac se encarga de compilar el archivo .java, mientras que java ejecuta el programa ya compilado. Por ejemplo, si se tiene un archivo llamado Hola.java, primero se compila con javac Hola.java, lo que genera un archivo Hola.class. Después, se ejecuta con java Hola, sin incluir la extensión. Este proceso recuerda al de C/C++, pero con la diferencia de que Java no genera un ejecutable nativo, sino un archivo intermedio.

Java se considera un lenguaje compilado e interpretado a la vez. El código fuente se compila a un formato intermedio llamado bytecode, que no es código máquina real, sino un conjunto de instrucciones portables. Este bytecode se almacena en los archivos .class generados por el compilador. A diferencia de C/C++, donde el resultado es un binario específico para un sistema operativo, el bytecode de Java puede ejecutarse en cualquier plataforma que disponga de una Máquina Virtual de Java.

La Máquina Virtual de Java (JVM) es el componente encargado de interpretar y ejecutar el bytecode. Actúa como una capa intermedia entre el programa y el sistema operativo, lo que permite que el mismo archivo .class funcione en Windows, Linux o macOS sin necesidad de recompilar. La JVM también gestiona aspectos como la memoria, la seguridad y la recolección de basura, lo que simplifica el trabajo del programador y reduce errores comunes.

El bytecode es, por tanto, el formato intermedio que hace posible la portabilidad de Java. Los archivos .class contienen estas instrucciones y son el resultado directo de la compilación. Cuando se ejecuta un programa, la JVM lee estos archivos, los interpreta o los optimiza mediante compilación Just-In-Time (JIT) y finalmente los ejecuta. Este modelo híbrido permite combinar portabilidad con un rendimiento razonablemente alto, manteniendo al mismo tiempo un entorno controlado y seguro para la ejecución del código.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

En el código de la clase Punto, la palabra clave new se utiliza para crear un objeto en memoria dinámica. En Java, todos los objetos se crean en el heap, y new se encarga de reservar ese espacio y devolver una referencia al objeto recién creado. 

Un constructor es un método especial dentro de una clase cuya función es inicializar los atributos del objeto en el momento de su creación. Se llama automáticamente cuando se usa new y tiene el mismo nombre que la clase. A diferencia de un método normal, un constructor no devuelve ningún tipo, ni siquiera void. Su propósito es garantizar que el objeto comience su vida en un estado válido, evitando que sus atributos queden sin inicializar o con valores incoherentes.

Ejemplo:

class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}



## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia this es un identificador especial que se utiliza dentro de una clase para referirse al objeto actual, es decir, a la instancia sobre la que se está ejecutando un método. Su función principal es distinguir entre los atributos del objeto y los parámetros o variables locales que puedan tener el mismo nombre. En lenguajes como Java, donde los métodos suelen recibir parámetros con nombres similares a los atributos, this permite evitar ambigüedades y deja claro que se está accediendo al estado interno del objeto.

No todos los lenguajes utilizan exactamente la palabra this, aunque la mayoría de los lenguajes orientados a objetos tienen un mecanismo equivalente. Por ejemplo, en Python se emplea self, mientras que en C++ también se usa this, pero como un puntero explícito. Aunque el nombre puede variar, la idea es la misma: proporcionar una forma de acceder al objeto actual desde dentro de sus propios métodos. Este concepto resulta fundamental para comprender cómo se relacionan los métodos con los datos que manipulan.
Ejemplo:

class Punto {
    int x;
    int y;

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

class Punto {
    int x;
    int y;

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    double distanciaA(Punto otro) {
        int dx = this.x - otro.x;
        int dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

Cuando un objeto de tipo Punto se pasa como parámetro a un método en Java, lo que se pasa es una copia de la referencia, no una copia del objeto. Esto significa que dentro del método se recibe una referencia que apunta al mismo objeto que existe fuera. Por tanto, si dentro del método se modifica algún atributo del punto recibido, esos cambios afectan directamente al objeto original. Este comportamiento es distinto del paso por referencia real de C++, pero en la práctica produce un efecto similar cuando se trabaja con objetos.

Sin embargo, si en lugar de un objeto se pasa un tipo primitivo, como un int, el comportamiento es diferente. Los tipos primitivos en Java se pasan por valor, lo que implica que el método recibe una copia independiente del dato. Si dentro del método se modifica ese entero, el cambio no afecta al valor original que existe fuera del método. Esto se asemeja al paso por valor tradicional de C, donde las funciones no pueden alterar directamente las variables primitivas externas a menos que se utilicen punteros.

Esta diferencia entre objetos y tipos primitivos es fundamental para entender cómo funciona la memoria en Java. Los objetos siempre viven en el heap y se accede a ellos mediante referencias, mientras que los tipos primitivos se almacenan directamente en la pila. Por ello, modificar un objeto dentro de un método afecta al original, pero modificar un primitivo no tiene efecto fuera del ámbito del método.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

El método toString() en Java es un método especial definido en la clase base Object, de la cual heredan todas las clases del lenguaje. Su propósito es devolver una representación en forma de cadena del objeto, normalmente útil para depuración, impresión por consola o registro de información. Cuando no se sobrescribe, la implementación por defecto muestra el nombre de la clase y una referencia interna poco útil. Por ello, es habitual redefinirlo para que muestre información significativa sobre el estado del objeto.

Ejemplo:

class Punto {
    int x;
    int y;

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


Una clase puede recordar superficialmente a un struct de C, ya que ambos permiten agrupar varios datos bajo un mismo tipo. En ambos casos, se pueden crear variables que contienen esos datos agrupados, lo que facilita representar entidades con varias características. Esta similitud hace que, al empezar con Java o con POO, resulte tentador pensar que una clase es simplemente un struct “mejorado”.

Sin embargo, al struct de C le falta un elemento esencial para comportarse como una clase: la capacidad de combinar datos y comportamiento. En una clase, además de atributos, se pueden definir métodos que operan sobre esos datos, lo que permite encapsular la lógica directamente dentro del tipo. En un struct de C solo se almacenan datos; cualquier función que opere sobre ellos debe definirse fuera, sin una relación formal con el propio tipo.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

Para emular una clase como Punto en C, se puede utilizar un struct para almacenar los datos y una función externa que opere sobre él. En este enfoque, el struct contiene únicamente los atributos x e y, mientras que la lógica para calcular la distancia se implementa como una función que recibe un puntero al struct. Este diseño reproduce parcialmente la idea de asociar comportamiento a datos, aunque en C no existe una relación formal entre ambos elementos. El programador debe mantener esa asociación de manera disciplinada.

El equivalente a un método como calculaDistanciaAOrigen sería una función que recibe un Punto* y calcula la distancia usando sus campos. En este contexto, el concepto de this desaparece, ya que en C no existe una referencia implícita al objeto actual. En su lugar, el programador debe pasar explícitamente un puntero al struct, que actúa como sustituto manual de this. Esto hace evidente que, en C, la responsabilidad de gestionar la relación entre datos y funciones recae completamente en el programador.
