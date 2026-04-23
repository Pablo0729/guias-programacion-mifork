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

A continuación se muestra un ejemplo en C que ilustra la composición mediante `struct`, donde una **línea** está formada por **dos puntos**, y cada punto contiene sus coordenadas `x` e `y`. Este tipo de diseño refleja la idea de que una estructura mayor puede construirse a partir de otras más simples, manteniendo una relación “tiene‑un”. Además, se incluyen funciones auxiliares para calcular la distancia entre dos puntos y la longitud de la línea, lo que permite separar claramente los datos de las operaciones que se realizan sobre ellos.

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

double distancia(Punto a, Punto b) {
    double dx = b.x - a.x;
    double dy = b.y - a.y;
    return sqrt(dx*dx + dy*dy);
}

double longitudLinea(Linea l) {
    return distancia(l.p1, l.p2);
}

int main() {
    Punto a = {0.0, 0.0};
    Punto b = {3.0, 4.0};
    Linea l = {a, b};

    printf("Distancia entre puntos: %.2f\n", distancia(a, b));
    printf("Longitud de la línea: %.2f\n", longitudLinea(l));

    return 0;
}
```



**Clase:**


Este ejemplo muestra cómo la composición permite construir estructuras más complejas sin necesidad de mecanismos orientados a objetos. La estructura `Linea` contiene directamente dos `Punto`, y las funciones externas operan sobre ellos sin asociarse formalmente a las estructuras. La modularidad se consigue mediante la separación entre definición de datos y funciones que los manipulan, lo que facilita la reutilización y el mantenimiento del código.

En C++ podría lograrse el mismo diseño, pero normalmente se preferiría encapsular los datos dentro de clases y definir métodos miembro como `distancia()` o `longitud()`. Además, se podrían usar constructores para inicializar los puntos y la línea de forma más segura, y aplicar control de acceso mediante `private` o `public`. En C, en cambio, la composición se limita a la inclusión directa de estructuras dentro de otras, y el control de invariantes depende exclusivamente del programador.



## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

En Java, la composición se expresa creando clases cuyos objetos contienen a otros objetos, garantizando así relaciones del tipo “una línea tiene dos puntos”. Para ello, se define una clase `Punto` cuyos atributos son privados e inmutables, de modo que una vez creado un punto no puedan alterarse sus coordenadas. Esta inmutabilidad se consigue declarando los campos como `private final` y proporcionando únicamente métodos de lectura. Además, se implementa un método que calcula la distancia a otro punto aplicando la fórmula euclidiana, encapsulando así el comportamiento dentro de la propia clase.

La clase `Linea` se construye siguiendo el mismo principio: contiene dos puntos privados y finales, que se inicializan en el constructor y no pueden cambiarse posteriormente. De este modo, la línea queda completamente definida en el momento de su creación, y su longitud se obtiene delegando en el método de distancia de `Punto`. Esta estructura refleja una composición fuerte: la línea depende de sus puntos y no tiene sentido sin ellos, y además controla su integridad evitando modificaciones externas.

```java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}

public final class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }

    public Punto getP1() { return p1; }
    public Punto getP2() { return p2; }
}
```

Este diseño aprovecha la ocultación de información para asegurar que ni los puntos ni la línea puedan modificarse desde fuera, algo que en C requeriría disciplina manual y no podría garantizarse plenamente. En Java, la combinación de campos privados, palabras clave como `final` y la ausencia de métodos modificadores permite construir objetos robustos y coherentes. En C++ podría lograrse un enfoque similar mediante clases, constructores y miembros constantes, pero Java simplifica la gestión de memoria y elimina la posibilidad de manipular directamente los objetos, reforzando así la seguridad del modelo de composición.



## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La **multiplicidad** describe cuántas instancias de una clase pueden estar asociadas a una instancia de otra dentro de una relación de composición. En composición, esta relación suele ser fuerte: el objeto compuesto no existe sin sus componentes, y además controla completamente su ciclo de vida. La multiplicidad permite expresar si un objeto contiene exactamente uno, varios o un número variable de otros objetos, y es fundamental para entender la estructura interna de un modelo orientado a objetos.

En el ejemplo de `Linea` y `Punto`, una línea está formada **exactamente por dos puntos**, sin posibilidad de que sean más ni menos, ya que la definición geométrica lo exige. Por tanto, la multiplicidad desde `Linea` hacia `Punto` es **2**, lo que suele expresarse como `Linea → Punto: 2`. Esto indica que cada línea contiene dos puntos concretos y que estos puntos forman parte esencial de su identidad.

En la dirección contraria, un punto puede pertenecer a **cero o una línea**, dependiendo de si se usa de forma aislada o como parte de una línea concreta. Como no se impone que un punto deba estar siempre asociado a una línea, la multiplicidad desde `Punto` hacia `Linea` es `0..1`. Esto refleja que un punto puede existir por sí mismo sin necesidad de formar parte de ninguna línea, lo que respeta la independencia conceptual del punto como entidad geométrica.

En C++ esta relación se representaría de forma similar mediante clases que contienen objetos miembro, pero la multiplicidad no se expresa explícitamente: se deduce del número de atributos y de cómo se gestionan. Java mantiene la misma idea, pero la combinación de encapsulación estricta e inmutabilidad permite modelar la composición de forma más segura y menos propensa a errores que en C++, donde la gestión manual de memoria y referencias puede complicar la integridad de la relación.




**CLASE:**


1 línea se relaciona con dos puntos y como máximo con dos spuntos
1 punto se relaciona como mínimo con 0 líneas y como máximo con muchas líneas.



## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La **composición fuerte** describe una relación en la que el objeto compuesto es dueño exclusivo de los objetos que contiene, controlando completamente su creación y su destrucción. En este tipo de relación, los componentes no tienen sentido fuera del todo: su ciclo de vida está estrictamente ligado al del objeto que los contiene. Cuando el objeto compuesto deja de existir, sus componentes también desaparecen, porque forman parte esencial de su estructura interna y no se comparten con otros objetos.

La **composición débil**, por el contrario, establece una relación en la que el objeto compuesto utiliza o referencia a otros objetos, pero no es responsable de su existencia. Los componentes pueden existir independientemente, pueden ser compartidos por varios objetos y su ciclo de vida no depende del objeto que los usa. Este tipo de relación se conoce habitualmente como **asociación** o **agregación**, y se caracteriza por una menor dependencia estructural entre las partes.

La consecuencia principal en términos de ciclo de vida es que, en composición fuerte, el objeto compuesto determina completamente la vida de sus componentes, mientras que en composición débil los objetos referenciados pueden sobrevivir más allá del objeto que los utiliza. Esto permite modelar relaciones más flexibles, donde los objetos pueden ser reutilizados o gestionados de forma independiente, algo habitual en estructuras de datos o modelos conceptuales complejos.

En C++ estas distinciones también existen, aunque no se expresan de forma tan explícita: la composición fuerte suele representarse mediante objetos miembro por valor, mientras que la composición débil se modela mediante punteros o referencias. Java mantiene la misma idea conceptual, pero al gestionar automáticamente la memoria y no permitir objetos por valor dentro de otros objetos, la diferencia se expresa sobre todo en si un objeto crea y controla a sus componentes o simplemente mantiene referencias a ellos.


**CLASE:**

Conjuntos fuertes vs débil:

FUERTE: -> El contenedor (P e.j. Linea) es el que crea los objetos que contiene (P e.j. Punto) y estos no ven más allá del contenedor.
        -> El ciclo de vida del contenido está vinculado al contenedor.

DÉBIL: -> El contenedor y el contenido tienen ciclos de vida independientes (P. e.j. los objetos Punto pueden vivir sin estar en objeto Linea).


Se representan graficamente con rombo claro(clase débil) y robo oscuro (clase fuerte).

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase utiliza a otra únicamente **como parámetro**, **como valor de retorno**, **como variable local** o creando objetos puntuales con `new` dentro de un método, no se considera que exista composición, sino una relación de **dependencia**. En este caso, la clase no “posee” ni controla el ciclo de vida de esos objetos, sino que simplemente los usa de forma temporal para realizar una operación concreta. La dependencia indica que un método necesita otro tipo para funcionar, pero no implica que el objeto dependiente forme parte estructural del estado interno de la clase.

La clave está en que, en dependencia, los objetos utilizados **no viven dentro del objeto que los usa**. No se almacenan como atributos, no forman parte de su identidad y pueden existir antes, después o completamente al margen de él. La relación es momentánea y funcional: un método recibe un objeto, lo procesa y lo descarta, o crea uno para un cálculo puntual sin que pase a ser parte del estado interno de la instancia.

En cambio, la **composición** implica que los objetos utilizados se guardan como **atributos privados**, forman parte del estado permanente de la clase y su ciclo de vida queda ligado al del objeto que los contiene. Esto no ocurre cuando solo se usan como parámetros o variables locales, por lo que en esos casos no puede hablarse de composición, sino únicamente de dependencia.

En C++ la distinción es similar: pasar objetos por parámetro o crearlos dentro de funciones implica dependencia, mientras que declararlos como miembros internos de una clase implica composición. La diferencia es que en C++ puede haber objetos miembro por valor, lo que refuerza aún más la composición fuerte, mientras que en Java todo se maneja mediante referencias, aunque el concepto lógico de dependencia frente a composición se mantiene igual.



**CLASE:**

```java

class Punto{


    public String toString(){

        StringBuilder sb = new StringBUilder();
    }
}

class OperadorFichero{

public static String leerFichero(Punto p)

}



```


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

A continuación se muestran dos formas de modelar la relación entre `Linea` y `Punto`: una como **composición fuerte**, donde los puntos pertenecen exclusivamente a la línea y no existen fuera de ella, y otra como **composición débil**, donde la línea solo mantiene referencias a puntos creados externamente. Ambas aproximaciones permiten comprender cómo cambia el ciclo de vida de los objetos según el tipo de relación establecida.

---

## 🧱 Composición fuerte (los puntos “viven y mueren” con la línea)

En una composición fuerte, la clase `Linea` **crea y gestiona internamente** sus propios `Punto`. Esto implica que los puntos no existen fuera de la línea y que su ciclo de vida está completamente ligado al de ella. Este enfoque se utiliza cuando los elementos internos no tienen sentido por sí mismos o cuando se desea un control total sobre su creación y modificación. Desde la perspectiva de C/C++, sería equivalente a tener estructuras anidadas o miembros que no se comparten con otros objetos.

```java
class Punto {
    private int x;
    private int y;

    public Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Linea {
    private Punto inicio;
    private Punto fin;

    public Linea(int x1, int y1, int x2, int y2) {
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }
}
```

En este diseño, la línea controla completamente la creación de los puntos. No se permite que vengan del exterior, lo que garantiza que nadie más pueda modificarlos o reutilizarlos. Cuando la línea deja de existir, también lo hacen sus puntos, lo que refleja una relación de dependencia total. Este tipo de composición es útil cuando se busca encapsulación estricta y coherencia interna del objeto compuesto.

---

## 🔗 Composición débil (la línea solo referencia puntos externos)

En una composición débil, la clase `Linea` **no crea los puntos**, sino que recibe referencias a objetos `Punto` que existen independientemente. Esto permite que los puntos sean compartidos, reutilizados o modificados desde fuera, lo que otorga mayor flexibilidad. En términos de C/C++, se asemeja a trabajar con punteros o referencias a estructuras creadas en otro lugar.

```java
class Punto {
    private int x;
    private int y;

    public Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Linea {
    private Punto inicio;
    private Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }
}
```

En este caso, la línea no controla el ciclo de vida de los puntos. Si otro objeto modifica un punto, la línea verá ese cambio, lo que puede ser deseable o no según el diseño. Esta relación es adecuada cuando los puntos representan entidades independientes que pueden existir sin pertenecer exclusivamente a una línea, como ocurre en sistemas gráficos o modelos geométricos más complejos.

**CLASE:**


```java

public Linea{
    private Punto p1;
    private Punto p2;

    public Linea(double x1, double x2, double y1, double y2){
this.p1= new Punto(x1 , y1);
this.p2= new Punto(x2 , y2);

    }
    public double getX1(){
        return this.p1.get();
    }
}


```



## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java, incluso en una composición fuerte, el contenedor no destruye explícitamente los objetos que contiene porque el lenguaje utiliza **recolección automática de basura (garbage collector)**. Esto implica que los objetos no se eliminan mediante instrucciones directas del programador, sino que el sistema detecta cuándo ya no existen referencias accesibles hacia ellos y libera su memoria de forma automática. Por tanto, aunque `Linea` deje de usarse, no se observa ninguna llamada explícita a un destructor como ocurriría en C++, porque Java no ofrece ese mecanismo.

Cuando una instancia de `Linea` desaparece —por ejemplo, porque sale de ámbito o se asigna otra referencia sobre ella— también desaparecen las referencias internas a sus `Punto`. En ese momento, los objetos `Punto` quedan inaccesibles desde cualquier parte del programa, lo que los convierte en candidatos para ser eliminados por el recolector de basura. Este proceso no ocurre de inmediato, sino cuando la máquina virtual decide que es un buen momento para optimizar memoria, lo que explica por qué no se ve ninguna acción explícita en el código.

La composición fuerte se refleja, por tanto, en el **ciclo de vida lógico** de los objetos, no en instrucciones de destrucción. Los puntos dependen completamente de la línea porque solo existen dentro de ella y no se exponen referencias externas. Sin embargo, la eliminación física de esos objetos sigue siendo responsabilidad del sistema de ejecución de Java, no del programador. Esta diferencia es especialmente notable para quien proviene de C/C++, donde la gestión manual de memoria es habitual.

Este modelo simplifica el diseño de clases y reduce errores comunes como fugas de memoria o dobles liberaciones. Aun así, exige comprender que la destrucción no es inmediata ni controlada directamente, sino que depende del recolector de basura y de la ausencia total de referencias vivas hacia los objetos implicados.



**CLASE:**
En java , la vida de un Punto termina cuando es inaccesible, y en el ejemplo ocurre cuando Linea deja de serlo a su vez. Por tanto, cuando Linea "Es Basura" también lo serán sus puntos y serán eliminados de memoria por el recolector de basura.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.





**CLASE:**


```java
public class Departamento{


    //composición débil 1:
    //1 departamento tiene como mínimo 0 y como máximo muchos Profesor****
    //1 profesor como mínimo 0 y como máximo muchos Departamento.

    private Profesor[] profesores = new Profesor[50];
    private int numProfesor=0;


    //composición débil 2:
    //1 departamento tiene como mínimo 1 y como máximo 1 Profesor****
    //1 profesor como mínimo 0 y como máximo muchos Departamento.



    private Profesor director;

    public DepartamentoProfesor director(){
//0. si el director es null lanzamos IAE
//1. añadimos el director al conjunto de profesores.
//2.Establecemos ese profesor como director.


    }

    public int getNumProfesores() { return this.numProfesores;}

    public Proesor getProfesor(int pos){
        //0. validamos pos, y si no valida lanzamos IAE.
        return this.profesores(pos);
    }

    public void addProfesor(Profesor p){
//0. Si ya no hay mas sitio, lanzar AIOBE (ArrayIndexOutofBoundsException)


    }

    public deleteProfesor(int pos){
//0. Si no está en el rango correcto (0-numProfesores), lanzar IAE.
//1. Si está en pos pero es el profesor director, lanzar IAE.


    }

    public changeProfesor(Profesor nuevoProfesor){
//0.si nuevo profesor es null, lanzar IAE.
//1. Si nuevo director no lo encuentro, lanzar IAE, diciendo que hay que meterlo en el departamento primero.

    }

    public getDirector(){
        return this.director;
    }
}





```


### Respuesta


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?



**CLASE:**
```java
public class Departamento{


    //composición débil 1:
    //1 departamento tiene como mínimo 0 y como máximo muchos Profesor****
    //1 profesor como mínimo 0 y como máximo muchos Departamento.

    private Profesor[] profesores = new Profesor[50];
    private int numProfesor=0;


    //composición débil 2:
    //1 departamento tiene como mínimo 1 y como máximo 1 Profesor****
    //1 profesor como mínimo 0 y como máximo muchos Departamento.



    private Profesor director;

    public DepartamentoProfesor director(){
//0. si el director es null lanzamos IAE
//1. añadimos el director al conjunto de profesores.
//2.Establecemos ese profesor como director.


    }

    public int getNumProfesores() { return this.numProfesores;}

    public Proesor getProfesor(int pos){
        //0. validamos pos, y si no valida lanzamos IAE.
        return this.profesores(pos);
    }

    public void addProfesor(Profesor p){
//0. Si ya no hay mas sitio, lanzar AIOBE (ArrayIndexOutofBoundsException)


    }

    public deleteProfesor(int pos){
//0. Si no está en el rango correcto (0-numProfesores), lanzar IAE.
//1. Si está en pos pero es el profesor director, lanzar IAE.
//Eliminar el elemento pos en el array. (bucle de copia del siguiente y establecer a null en la última posición usada.)


    }

    public changeProfesor(Profesor nuevoProfesor){
//0.si nuevo profesor es null, lanzar IAE.
//1. Si nuevo director no lo encuentro, lanzar IAE, diciendo que hay que meterlo en el departamento primero.

    }

    public getDirector(){
        return this.director;
    }
    
    
    
    
    
    

//ACCESO A LA LISTA INTERNA(CUIDADO INVARIANTE DE CLASE)

public List<Profesor> getProfesores(){
    //NOOOOOOOOOOOOO!!! return this.profesores;
    
    //solucion clasica, con copia (penalización pequeña en rendimiento)
    return Collections.unmodificableList(this.profesores);
}

//Qué problema hay -> La lista es mutable, perdemos el control sobre la invariante de clase.

//Si insisto en que me gusta la interfaz, pero quiero garantizar que solo se use para recorrerlos y modificar, puedo:

//1. Devolver una copia de la lista.
//2. Devolver un envoltorio no mutable.
}





```



COMENTARIO:
Con List<Profesor>...
1.No cambia la interfaz pública.
2Es más fácil implementar métodos, delegando en métodos de List.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.


//FALTA RESPONDER


**CLASE**
IMAGEN: P4_1 p10;








## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?


//FALTA RESPONDER


**CLASE**
Las bidireccionamientos exigen programar cuidadosamente para mantener la consistencia. Por ejemplo: si añado un profesor al departamento debo activarlos la ---- desde Profesor.

