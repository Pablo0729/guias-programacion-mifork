<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El **polimorfismo** es la capacidad de que un mismo mensaje (una llamada a un método) pueda producir comportamientos distintos según el tipo real del objeto que lo recibe. En Java, esto se apoya en la herencia: una referencia del supertipo puede apuntar a objetos de distintos subtipos, y cada uno responderá de manera específica. Su utilidad principal es permitir escribir código general y extensible, que funcione con cualquier subtipo sin necesidad de modificarlo cuando se añadan nuevas clases. Esto contrasta con C/C++, donde sin POO habría que recurrir a punteros a funciones o estructuras manuales para simular este comportamiento.

La **sobreescritura de métodos** (*method overriding*) consiste en que una subclase redefine un método heredado de la superclase, manteniendo la misma firma pero proporcionando una implementación distinta. Gracias a ello, cuando se invoca ese método a través de una referencia del supertipo, Java selecciona dinámicamente la versión correspondiente al tipo real del objeto. Esta resolución en tiempo de ejecución es la base del polimorfismo dinámico y permite que cada subtipo adapte o especialice el comportamiento heredado sin romper la compatibilidad con el código que opera sobre el supertipo.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La **ligadura dinámica** (o *enlace tardío*) consiste en que la elección del método que realmente se ejecuta no se decide en tiempo de compilación, sino **en tiempo de ejecución**, según el tipo **real** del objeto. Esto es esencial para el polimorfismo dinámico: cuando se llama a un método a través de una referencia del supertipo, el lenguaje determina en tiempo de ejecución qué implementación concreta debe usarse. Sin este mecanismo, el polimorfismo sería imposible, porque todas las llamadas quedarían fijadas estáticamente al tipo de la referencia, no al del objeto.

En **Java**, la ligadura dinámica es automática para todos los métodos no `static`, no `final` y no `private`. El programador no tiene que indicar nada: el lenguaje está diseñado para que el polimorfismo funcione siempre que haya herencia y sobreescritura. En **C++**, en cambio, la ligadura por defecto es **estática**, y solo se activa la ligadura dinámica cuando el método se declara como `virtual`. Esto implica que el programador debe decidir explícitamente qué métodos participan en el polimorfismo. En **Python**, la ligadura es siempre dinámica: no existe declaración de tipos ni palabras clave especiales; cualquier método puede ser redefinido y el lenguaje resuelve la llamada en tiempo de ejecución según el objeto real, lo que hace que el polimorfismo sea natural y omnipresente en su modelo de objetos.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

A continuación se muestra un ejemplo sencillo que ilustra la sobreescritura y el polimorfismo. La clase `Soldado` define un método `saludar`, y `Zapador` lo **sobrescribe** sustituyendo completamente su comportamiento. `Artillero` hereda el método sin modificarlo. Al recorrer un array de `Soldado` con objetos de ambos tipos, la llamada `saludar()` se resuelve dinámicamente según el tipo **real** del objeto, no según el tipo de la referencia.

```java
class Soldado {
    public void saludar() {
        System.out.println("Soldado: ¡A sus órdenes!");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador: Preparado para abrir camino.");
    }
}

class Artillero extends Soldado {
    // No sobrescribe: hereda el saludo de Soldado
}

public class Main {
    public static void main(String[] args) {
        Soldado[] escuadra = {
            new Zapador(),
            new Artillero(),
            new Zapador()
        };

        for (Soldado s : escuadra) {
            s.saludar(); // Polimorfismo: se ejecuta según el objeto real
        }
    }
}
```

Este ejemplo muestra cómo el polimorfismo permite tratar a todos los objetos como `Soldado` sin perder el comportamiento específico de cada subtipo. El código que recorre el array no necesita saber si cada elemento es un `Zapador` o un `Artillero`; simplemente invoca `saludar()`, y la **ligadura dinámica** se encarga de seleccionar la implementación adecuada en tiempo de ejecución. Esto hace que el diseño sea más extensible y flexible, ya que pueden añadirse nuevos tipos de soldados sin modificar el código que opera sobre la superclase.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Sí, cuando una subclase **sobrescribe** un método, puede igualmente **invocar la versión original** definida en la superclase y, a partir de ella, añadir comportamiento adicional. Esto permite extender la funcionalidad sin reemplazarla por completo. En Java, esta invocación se realiza mediante la palabra clave **`super`**, que permite acceder a métodos (y constructores) heredados tal como estaban definidos en la clase base. Es una técnica habitual cuando se desea conservar parte del comportamiento original y complementarlo con lógica específica del subtipo.

En el ejemplo siguiente, `Zapador` sobrescribe `saludar()`, pero primero llama a `super.saludar()` para ejecutar el saludo estándar del soldado. Después añade su propio mensaje. Al recorrer un array de `Soldado`, el polimorfismo garantiza que cada objeto ejecuta la versión adecuada del método, mostrando cómo la subclase puede ampliar el comportamiento heredado sin perder compatibilidad con el código que opera sobre el supertipo.

```java
class Soldado {
    public void saludar() {
        System.out.println("Soldado: ¡A sus órdenes!");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // Invoca al método base
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}

class Artillero extends Soldado {
    // Hereda el saludo tal cual
}

public class Main {
    public static void main(String[] args) {
        Soldado[] escuadra = {
            new Zapador(),
            new Artillero(),
            new Zapador()
        };

        for (Soldado s : escuadra) {
            s.saludar();
        }
    }
}
```

La palabra clave utilizada para invocar al método de la clase base es **`super`**.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al sobrescribir un método en Java, deben mantenerse **exactamente los mismos tipos de parámetros** y el **mismo nombre**; de lo contrario, no se considera sobreescritura, sino un método distinto. Respecto al tipo de retorno, Java permite un retorno **covariante**, es decir, puede devolverse un subtipo más específico que el declarado en la superclase, pero nunca un tipo incompatible. Estas restricciones garantizan que el método sobrescrito siga siendo utilizable a través de referencias del supertipo sin romper la compatibilidad del sistema de tipos.

La **sobreescritura** (*overriding*) implica redefinir en una subclase un método heredado, manteniendo la misma firma, para cambiar o extender su comportamiento. La **sobrecarga** (*overloading*), en cambio, consiste en definir varios métodos con el mismo nombre pero **distintos parámetros**, coexistiendo todos ellos en la misma clase. La anotación **`@Override`** sirve para indicar explícitamente que un método pretende sobrescribir otro; el compilador verifica que la firma coincide con la de la superclase. Es recomendable usarla siempre porque evita errores sutiles, como escribir mal el nombre del método o equivocarse en los parámetros, que de otro modo pasarían inadvertidos y romperían el polimorfismo.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Sí, en Java se usa **polimorfismo desde el primer momento**, incluso aunque no se mencione explícitamente. Cada vez que una clase sobrescribe un método heredado —como `toString()`, `equals()` o `hashCode()`— ya se está participando en el mecanismo polimórfico del lenguaje. Cuando un objeto se trata mediante una referencia de tipo `Object` (o de cualquier superclase) y se invoca uno de esos métodos, la versión que realmente se ejecuta es la de la **clase real del objeto**, no la de la referencia. Esa selección dinámica del método es precisamente polimorfismo, aunque al principio no se perciba como tal.

Sobrescribir `toString()` o `equals()` es, por tanto, un ejemplo clásico de polimorfismo en acción. Aunque se trabaje con referencias de tipo `Object`, Java invoca la implementación concreta definida en la clase del objeto, gracias a la ligadura dinámica. Esto permite que cualquier clase personalice su comportamiento sin romper la compatibilidad con el resto del sistema. Por eso, incluso en ejercicios básicos, ya se está utilizando polimorfismo sin necesidad de introducir jerarquías complejas ni ejemplos explícitos de herencia especializada.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una **clase abstracta** es una clase que no puede instanciarse directamente y que sirve como plantilla común para sus subclases. Puede contener métodos normales (con implementación) y métodos **abstractos**, que son métodos declarados sin cuerpo y que obligan a cada subclase concreta a proporcionar su propia implementación. Un **método abstracto**, por tanto, define *qué* debe existir, pero no *cómo* debe hacerse, delegando esa responsabilidad en las clases hijas. Debido a esta naturaleza incompleta, una clase abstracta **no puede tener instancias**, aunque sí puede usarse como tipo de referencia para aplicar polimorfismo.

En el ejemplo solicitado, `Soldado` se convierte en clase abstracta porque define un método `atacar()` sin implementación. Cada subtipo (`Zapador`, `Artillero`) debe implementar su propia versión de ese método. La palabra clave `abstract` debe colocarse tanto en la **declaración de la clase** como en la **declaración del método** que no tiene cuerpo. El método `saludar()` sigue siendo normal y puede heredarse o sobrescribirse según convenga. El polimorfismo permite tratar a todos los soldados como `Soldado` y, al invocar `atacar()`, se ejecuta la versión correspondiente al tipo real del objeto.

```java
abstract class Soldado {
    public void saludar() {
        System.out.println("Soldado: ¡A sus órdenes!");
    }

    public abstract void atacar(); // Método abstracto
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Zapador: Colocando explosivos.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Artillero: Disparando el cañón.");
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] escuadra = {
            new Zapador(),
            new Artillero()
        };

        for (Soldado s : escuadra) {
            s.saludar();
            s.atacar(); // Polimorfismo: cada uno ataca a su manera
        }
    }
}
```

En resumen: `abstract` se coloca **en la clase** y **en el método sin cuerpo**, y una clase abstracta **no puede instanciarse**.


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave **`final`** impide que un método pueda ser sobrescrito y que una clase pueda ser heredada, lo que bloquea cualquier extensión polimórfica a partir de ese punto. Esto afecta directamente al polimorfismo porque, si un método es `final`, ya no puede redefinirse en subclases y, si una clase es `final`, no puede participar en jerarquías de herencia. En la API estándar de Java existen varias clases `final`, siendo **`String`** el ejemplo más conocido: no puede heredarse y muchos de sus métodos tampoco pueden modificarse, garantizando seguridad, inmutabilidad y comportamiento consistente.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

En Java, una **interfaz** es un tipo que declara un conjunto de métodos sin implementación (salvo los `default` y `static` modernos) y que actúa como un contrato que las clases se comprometen a cumplir. Se parecen a las clases abstractas en que no pueden instanciarse y pueden contener métodos sin cuerpo, pero son más flexibles porque **no contienen estado mutable heredable** y permiten una forma de “herencia múltiple de comportamiento”. Una clase puede **implementar tantas interfaces como necesite**, lo que permite combinar capacidades diversas sin las restricciones de la herencia simple de clases.

Aquí tienes un ejemplo sencillo de **interfaces** aplicado al contexto que ya vienes usando con soldados. La interfaz define un *contrato* y las clases lo implementan libremente, mostrando cómo una clase puede implementar varias interfaces sin conflicto:

```java
interface Atacante {
    void atacar();
}

interface Saludable {
    void saludar();
}

class Soldado implements Atacante, Saludable {
    @Override
    public void saludar() {
        System.out.println("Soldado: ¡A sus órdenes!");
    }

    @Override
    public void atacar() {
        System.out.println("Soldado: Atacando con arma estándar.");
    }
}

class Zapador implements Atacante, Saludable {
    @Override
    public void saludar() {
        System.out.println("Zapador: Preparado para abrir camino.");
    }

    @Override
    public void atacar() {
        System.out.println("Zapador: Colocando explosivos.");
    }
}

public class Main {
    public static void main(String[] args) {
        Saludable[] escuadra = {
            new Soldado(),
            new Zapador()
        };

        for (Saludable s : escuadra) {
            s.saludar();
        }

        Atacante[] unidadAtaque = {
            new Soldado(),
            new Zapador()
        };

        for (Atacante a : unidadAtaque) {
            a.atacar();
        }
    }
}
```

Este ejemplo muestra que una interfaz actúa como un contrato y que una clase puede implementar tantas como necesite, permitiendo combinar comportamientos sin herencia múltiple de clases.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

Aquí tienes un diseño completo que combina **clase abstracta**, **polimorfismo**, **`instanceof`**, **downcasting** y una clase `Linea` capaz de trabajar con puntos sin conocer su dimensión. El método `calcularDistanciaA` se declara abstracto en `Punto`, y cada subtipo implementa su propia fórmula. Para garantizar que solo se calculen distancias entre puntos del mismo tipo, se usa `instanceof` y downcasting seguro.

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}
```

### Puntos en 2D
```java
class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Se esperaba un Punto2D");
        }
        Punto2D p = (Punto2D) otro; // Downcasting seguro
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

### Puntos en 3D
```java
class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Se esperaba un Punto3D");
        }
        Punto3D p = (Punto3D) otro; // Downcasting seguro
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        double dz = this.z - p.z;
        return Math.sqrt(dx*dx + dy*dy + dz*dz);
    }
}
```

### Clase `Linea` independiente de la dimensión
```java
class Linea {
    private Punto a, b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b); // Polimorfismo puro
    }
}
```

### Ejemplo de uso
```java
public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto2D(0, 0);
        Punto p2 = new Punto2D(3, 4);

        Linea l2D = new Linea(p1, p2);
        System.out.println("Longitud 2D: " + l2D.longitud());

        Punto q1 = new Punto3D(0, 0, 0);
        Punto q2 = new Punto3D(1, 2, 2);

        Linea l3D = new Linea(q1, q2);
        System.out.println("Longitud 3D: " + l3D.longitud());
    }
}
```

---

Este diseño permite que `Linea` funcione sin conocer si los puntos son 2D o 3D, delegando toda la lógica en el polimorfismo. Cada subtipo implementa su propia distancia y se protege contra combinaciones incompatibles mediante `instanceof` y downcasting seguro.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La **herencia de interfaces** consiste en que una interfaz puede **extender** a otra, heredando todos sus métodos abstractos (y también sus métodos `default` o `static`, si los hubiera). A diferencia de las clases, las interfaces **sí permiten herencia múltiple**, es decir, una interfaz puede extender a varias interfaces simultáneamente sin generar conflictos, porque no contienen estado mutable ni implementación obligatoria. Esto permite construir jerarquías de capacidades y contratos de forma muy flexible. Aquí tienes el ejemplo solicitado:

```java
interface Fichero {
    String leerComoString();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}
```

Este diseño permite que cualquier clase que implemente `FicheroEscribible` herede automáticamente el contrato de `Fichero`, garantizando que podrá leerse como `String` y, además, escribirse y eliminarse.
