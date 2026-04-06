
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En Java, a diferencia de C, el control de errores se basa en excepciones, que permiten separar claramente el flujo normal del flujo anómalo. La primera forma de diseñar la función raiz consiste en lanzar una excepción explícita cuando el argumento es negativo. Esta opción es la más idiomática en Java y se parece al uso de throw en C++, aunque en Java el sistema de excepciones es más uniforme y obligatorio en ciertos casos. El método no devuelve ningún valor especial: simplemente interrumpe su ejecución y delega el manejo del error al código que lo invoque.



public static double raiz(double x) {
    if (x < 0) {
        throw new IllegalArgumentException("El número no puede ser negativo");
    }
    return Math.sqrt(x);
}

public static void main(String[] args) {
    try {
        double r = raiz(-9);
        System.out.println("Resultado: " + r);
    } catch (IllegalArgumentException e) {
        System.out.println("Error: " + e.getMessage());
    }
}


Una segunda opción consiste en no lanzar excepciones, sino devolver un tipo que indique si hubo error, como OptionalDouble. Esto recuerda a usar std::optional en C++, donde el valor puede o no existir. El método devuelve un contenedor vacío si el argumento es inválido, y el llamador debe comprobarlo. Esta estrategia evita el coste y la semántica de las excepciones cuando el error forma parte del flujo esperado y no de una situación excepcional.


import java.util.OptionalDouble;

public static OptionalDouble raiz(double x) {
    if (x < 0) {
        return OptionalDouble.empty();
    }
    return OptionalDouble.of(Math.sqrt(x));
}

public static void main(String[] args) {
    OptionalDouble r = raiz(-9);
    if (r.isPresent()) {
        System.out.println("Resultado: " + r.getAsDouble());
    } else {
        System.out.println("Error: número negativo");
    }
}



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una **excepción** es un objeto que representa una situación anómala durante la ejecución de un programa: algo que impide continuar el flujo normal, como un argumento inválido, un acceso fuera de rango o un fallo de E/S. En lugar de devolver un valor especial o un código de error, el programa *interrumpe* la ejecución normal y “salta” a un manejador de excepciones, lo que permite separar claramente el código que hace el trabajo del código que gestiona errores.

Un programador las usa al **implementar funciones** para señalar que la función no puede completar su tarea de forma correcta. Esto evita mezclar valores válidos con señales de error y hace que el problema sea explícito. Al **llamar funciones**, las excepciones permiten reaccionar solo cuando ocurre un error real, sin tener que comprobar manualmente cada retorno como en C. El código queda más limpio y el compilador ayuda a garantizar que los errores no se ignoran accidentalmente.

En comparación con C++, Java tiene un sistema más rígido: algunas excepciones son *checked*, lo que obliga al programador a capturarlas o declararlas con `throws`. C++ permite excepciones, pero no obliga a tratarlas, y en C directamente no existen, por lo que todo depende de convenciones manuales. ¿Quieres que pasemos ahora a ver la diferencia entre excepciones *checked* y *unchecked* en Java?



## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

public class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("El número no puede ser negativo");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double resultado = Calculadora.raiz(-9);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error al calcular la raíz: " + e.getMessage());
        }
    }
}

Explicación breve del diseño
- El método raiz lanza una excepción si recibe un número negativo. No devuelve valores especiales ni códigos de error como harías en C.
- El método main envuelve la llamada en un bloque try-catch, que captura la excepción y muestra un mensaje al usuario.
- Esto separa el flujo normal del flujo de error, algo que en C no es posible sin mezclar ambos caminos. En C++ podrías hacer algo similar con throw, pero Java obliga a un estilo más uniforme y seguro.





## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Qué significa *lanzar*, *capturar* y *propagar* una excepción

Lanzar una excepción significa **interrumpir el flujo normal del programa** creando un objeto que representa un error y enviándolo hacia arriba en la pila de llamadas. En Java se hace con `throw`. Cuando `Calculadora.raiz(-9)` ejecuta `throw new IllegalArgumentException(...)`, la función deja de ejecutarse inmediatamente: no continúa, no devuelve nada y no vuelve al punto donde estaba. Es como si la función “saltara” fuera de sí misma debido a un fallo que no puede resolver.

Capturar o controlar una excepción significa **interceptar ese salto anómalo** usando un bloque `try-catch`. El código dentro de `try` se ejecuta normalmente, pero si ocurre una excepción, Java busca un `catch` compatible y transfiere allí el control. En el ejemplo, el `main` captura la excepción lanzada por `raiz` y muestra un mensaje al usuario. Esto separa el código que calcula de aquel que decide qué hacer si algo falla, algo que en C no existe y en C++ es similar pero menos rígido.

La propagación ocurre cuando una función **no captura la excepción**, de modo que la excepción sube automáticamente a la función que la llamó. Si esa tampoco la captura, sigue subiendo por la pila. Cada función por la que pasa la excepción **se aborta por completo**: no ejecuta más instrucciones, no devuelve valores y no se reanuda después. En el ejemplo, si `main` no tuviera `try-catch`, la excepción seguiría subiendo hasta el sistema de ejecución de Java, que terminaría el programa mostrando un mensaje de error.

En C++ el mecanismo es parecido, aunque sin el sistema de *checked exceptions* de Java. En C, en cambio, nada de esto existe: no hay salto automático, no hay propagación y no hay pila que se desmonte por error; todo debe hacerse manualmente con códigos de retorno. ¿Quieres que veamos ahora cómo afecta la propagación a la liberación de recursos y por qué Java usa `finally` para garantizar limpieza?



## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La propagación natural de excepciones aporta una ventaja clave frente al modelo de C: **el error viaja automáticamente hacia arriba por la pila sin que cada función tenga que comprobar manualmente códigos de retorno**. En C, cada llamada debe verificar si la función anterior falló, lo que genera código repetitivo, propenso a olvidos y difícil de mantener. En Java (y también en C++), una función que no puede manejar el error simplemente lo deja subir, y el lenguaje garantiza que el flujo anómalo se transmite correctamente hasta encontrar un manejador adecuado.

Esta propagación también mejora la **claridad del código**. Las funciones pueden centrarse en su tarea principal sin mezclar lógica normal con lógica de error. En el ejemplo de la raíz cuadrada, `Calculadora.raiz` solo calcula o lanza la excepción; no necesita saber qué hará el llamador con ese error. En C, en cambio, la función tendría que devolver un valor especial y confiar en que el llamador lo compruebe, lo que introduce ambigüedad y obliga a documentar convenciones que el compilador no puede verificar.

Otra ventaja importante es la **seguridad y consistencia del programa**. Cuando una excepción se propaga, cada función por la que pasa se aborta limpiamente: no continúa en un estado inconsistente, no ejecuta código parcial y no retorna valores inválidos. En C, si olvidas comprobar un código de error, el programa sigue ejecutándose como si nada, lo que puede causar fallos silenciosos difíciles de depurar. La propagación automática evita este tipo de errores lógicos y hace que los fallos sean visibles y manejables.

Finalmente, la propagación permite que el **manejo del error se concentre en un único punto**, normalmente cercano al usuario o a la capa de control del programa. En el ejemplo de `main`, basta con un `try-catch` para capturar cualquier error que ocurra en funciones profundas como `raiz`. En C, tendrías que ir comprobando manualmente cada retorno en cada nivel. ¿Quieres que veamos ahora cómo se comporta la pila cuando hay bloques `finally` o recursos que deben cerrarse durante la propagación?



## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En orientación a objetos, una excepción suele ser un **objeto** que hereda de la clase base `Exception` o de alguna de sus subclases. Esto permite que la excepción no sea solo un mensaje suelto, sino una entidad con información encapsulada: tipo de error, mensaje, causa original, e incluso datos adicionales si el programador los incluye. Frente al enfoque de C, donde un error suele ser un simple código numérico, el hecho de que una excepción sea un objeto permite describir el problema con mucha más riqueza y precisión.

El uso de objetos también aporta ventajas claras en términos de **encapsulación**. Cada tipo de error puede representarse mediante su propia clase, lo que permite distinguirlos de forma estructurada. Por ejemplo, no es lo mismo un error de argumentos (`IllegalArgumentException`) que un error de acceso a un archivo (`IOException`). Esta separación por tipos permite que el código capture solo las excepciones que realmente sabe manejar, manteniendo el resto encapsuladas y propagándose hacia arriba. En C++ ocurre algo similar, ya que las excepciones también son objetos, pero en Java la jerarquía es más uniforme y está más integrada en el diseño del lenguaje.

Dado que las excepciones son objetos, es totalmente posible **crear excepciones personalizadas**. Esto se hace extendiendo `Exception` o `RuntimeException`, según si queremos que sea *checked* o *unchecked*. Esto permite modelar errores específicos de un dominio concreto, algo muy útil en aplicaciones grandes. Por ejemplo, una calculadora podría definir su propia excepción `NumeroNegativoException` para representar exactamente ese caso, en lugar de reutilizar una excepción genérica. En C++ también podrías crear tus propias clases de excepción, pero en C esto es imposible porque no existe el mecanismo de excepciones.

Un ejemplo sencillo en Java sería:

```java
public class NumeroNegativoException extends Exception {
    public NumeroNegativoException(String mensaje) {
        super(mensaje);
    }
}
```

Y su uso dentro de la clase `Calculadora`:

```java
public static double raiz(double x) throws NumeroNegativoException {
    if (x < 0) {
        throw new NumeroNegativoException("No se puede calcular la raíz de un número negativo");
    }
    return Math.sqrt(x);
}
```






## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

La información esencial que lleva cualquier **objeto excepción** en Java es un conjunto de datos encapsulados que permiten entender con precisión qué ha fallado y dónde. Esta información incluye, como mínimo, **el tipo concreto de la excepción**, que ya describe la naturaleza del error (por ejemplo, `IllegalArgumentException` frente a `IOException`), y un **mensaje descriptivo** que explica la causa del problema. Además, toda excepción contiene una **traza de pila (stack trace)**, que indica la secuencia exacta de llamadas que condujo al error, algo imposible de obtener automáticamente en C.

Esta encapsulación aporta una ventaja enorme respecto a C, donde un error suele representarse con un simple código numérico o un valor especial. En Java, el manejador recibe un objeto completo que describe el error con detalle, lo que facilita depurar, registrar y tomar decisiones informadas. El stack trace, en particular, permite localizar el origen exacto del fallo sin necesidad de imprimir manualmente mensajes en cada función, como suele hacerse en C. En C++ también existen objetos excepción con mensajes y trazas, pero Java estandariza este mecanismo y lo integra en toda la jerarquía del lenguaje.

En el ejemplo de la raíz cuadrada, si `Calculadora.raiz(-9)` lanza una `IllegalArgumentException`, el manejador en `main` recibe un objeto que contiene:  
- **El tipo**: `IllegalArgumentException`.  
- **El mensaje**: `"El número no puede ser negativo"`.  
- **La traza de pila**: indicando que el error ocurrió dentro de `raiz`, llamada desde `main`.  

Esta combinación permite al programador saber exactamente qué ocurrió, por qué ocurrió y en qué punto del programa ocurrió, sin necesidad de pasar manualmente información adicional entre funciones.



## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

 En Java es totalmente válido tener **varios bloques `catch`** detrás de un mismo `try`. Cada bloque se asocia a un **tipo concreto de excepción**, y esto permite reaccionar de forma diferente según el error que se haya producido. Esta idea encaja con la jerarquía de clases de excepciones: cada `catch` actúa como un filtro por tipo, igual que en C++ cuando capturas por clase derivada, pero con reglas más estrictas sobre el orden (los tipos más específicos deben ir antes que los más generales).

Solo **se ejecuta un único bloque `catch`**, concretamente el primero cuyo tipo sea compatible con la excepción lanzada. Una vez que un `catch` captura la excepción, los demás se ignoran por completo. Esto evita ambigüedades y mantiene un flujo claro: el error se maneja en un único punto. En C no existe nada parecido; tendrías que comprobar manualmente códigos de error y decidir qué hacer en cada caso, lo que hace el código más repetitivo y menos seguro.

Este mecanismo permite diseñar manejadores muy expresivos. Por ejemplo, podrías tener un `catch` para `NumeroNegativoException`, otro para `ArithmeticException` y un último para `Exception` como “cajón de sastre”. El orden importa: si pones primero `Exception`, capturaría todo y los demás nunca se ejecutarían. En C++ ocurre algo similar: un `catch(...)` o un `catch` de una clase base debe ir al final para no eclipsar a los específicos.

Un ejemplo ilustrativo sería:

```java
try {
    double r = Calculadora.raiz(-9);
    System.out.println("Resultado: " + r);
} catch (NumeroNegativoException e) {
    System.out.println("Error específico: " + e.getMessage());
} catch (IllegalArgumentException e) {
    System.out.println("Argumento no válido: " + e.getMessage());
} catch (Exception e) {
    System.out.println("Otro error inesperado.");
}
```

**Clase**
Si, se puede tener ás de uno:
    - Sólo se ejecuta uno.
    - Se va coomprobando por orden hasta encontrar el primero que encaja.
    - IMPORTANTE: se deben poner del más específico al más general porque sino, las catch para excepciones específicas no se ejecutarán.





## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

En Java, cuando una excepción rompe el flujo normal, el mecanismo que garantiza que cierto código *siempre* se ejecute —haya o no error— es el bloque `finally`. Su función es asegurar la liberación de recursos: cerrar ficheros, conexiones, liberar memoria nativa, desbloquear estructuras, etc. A diferencia de C, donde el programador debe recordar manualmente liberar recursos en cada camino posible, Java garantiza que el contenido de `finally` se ejecuta incluso si la excepción se propaga. En C++ existe algo parecido mediante RAII, pero en Java el patrón explícito es `try–catch–finally`.

El bloque `finally` se ejecuta tanto si hay `catch` como si no lo hay. Si existe un `catch`, primero se captura la excepción y luego se ejecuta `finally`. Si no existe `catch`, la excepción se propaga hacia arriba, pero *antes* de salir del bloque `try`, Java ejecuta igualmente el `finally`. Esto evita fugas de recursos y asegura que el programa no quede en un estado inconsistente. En C, en cambio, si olvidas liberar un recurso en un camino de error, el compilador no te ayuda y el error puede pasar desapercibido.

---

### Ejemplo con `catch` y `finally`

```java
public static void main(String[] args) {
    try {
        double r = Calculadora.raiz(-9);
        System.out.println("Resultado: " + r);
    } catch (IllegalArgumentException e) {
        System.out.println("Error: " + e.getMessage());
    } finally {
        System.out.println("Liberando recursos...");
    }
}
```

En este caso, se ejecuta el `catch` y **después** el `finally`.

---

### Ejemplo sin `catch`, solo con `finally`

```java
public static void main(String[] args) {
    try {
        double r = Calculadora.raiz(-9);
        System.out.println("Resultado: " + r);
    } finally {
        System.out.println("Liberando recursos antes de propagar la excepción...");
    }
}
```

Aquí no hay `catch`, así que la excepción se **propaga**, pero **el `finally` se ejecuta igualmente** antes de abandonar el método. Esto es algo que en C no existe: si una función devuelve un código de error y tú no lo compruebas, no hay ningún mecanismo automático que garantice la liberación de recursos.


**CLASE**
El finally se ejecuta siempre que se entre en el bloque try.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

En Java, **sí es posible usar un bloque `finally` sin `catch`**. La forma válida es `try { … } finally { … }`, y se utiliza cuando quieres garantizar que cierto código se ejecute siempre, incluso aunque no quieras capturar la excepción. En este caso, si ocurre una excepción dentro del `try`, el `finally` se ejecuta igualmente y, después, la excepción continúa propagándose hacia arriba.

El bloque `finally` **se ejecuta siempre**, tanto si hay excepción como si no la hay. Su propósito es asegurar la liberación de recursos, cierre de ficheros, desconexión de bases de datos, etc. Solo hay dos situaciones excepcionales en las que *podría* no ejecutarse: si la JVM se detiene abruptamente (por ejemplo, `System.exit()` o un apagado del sistema) o si ocurre un error muy grave como un `StackOverflowError` o `OutOfMemoryError`.

Si dentro del `try` hay un `return`, el comportamiento sigue siendo el mismo: **el `finally` se ejecuta antes de que el método devuelva el valor**. Incluso si el `return` está en un `catch`, o si hay varios returns, el `finally` siempre se ejecuta antes de abandonar el método. Esto puede generar efectos curiosos: si el `finally` también contiene un `return`, este *anula* el valor devuelto por el `try` o el `catch`, lo cual se considera una mala práctica porque dificulta la lectura y puede ocultar errores.

En resumen, `finally` puede existir sin `catch`, se ejecuta prácticamente siempre, y se ejecuta incluso cuando hay un `return` en el `try`. Si quieres, puedo mostrarte un ejemplo concreto con código para ver cómo se comportan los returns y las excepciones en cada caso.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

El bloque `finally` **sí puede ir sin `catch`**. Java permite dos estructuras válidas: `try–catch–finally` y `try–finally`. En ambos casos, el objetivo del `finally` es garantizar que cierto código se ejecute siempre, independientemente de cómo termine el bloque `try`. Esto lo diferencia de C, donde no existe un mecanismo automático equivalente y el programador debe recordar liberar recursos manualmente en cada camino posible.

El contenido del `finally` **se ejecuta siempre**, tanto si ocurre una excepción como si no ocurre. Incluso si dentro del `try` hay un `return`, un `break`, un `continue` o una excepción no capturada, Java ejecuta el `finally` antes de abandonar definitivamente el bloque. Esto es crucial para liberar recursos como ficheros, sockets o memoria nativa. En C++ este comportamiento se consigue mediante RAII, pero en Java el mecanismo explícito es el `finally`.

Cuando hay un `return` dentro del `try`, el flujo funciona así: Java evalúa el valor de retorno, luego ejecuta el `finally`, y **solo después** devuelve el valor. Si dentro del `finally` hubiera otro `return`, este sobrescribiría el anterior, lo cual se considera mala práctica porque hace el código difícil de entender. En C, en cambio, un `return` simplemente abandona la función sin ejecutar nada más, lo que puede provocar fugas de recursos si no se gestiona manualmente.

Un ejemplo con `catch`:

```java
try {
    return Calculadora.raiz(-9);
} catch (IllegalArgumentException e) {
    System.out.println("Error: " + e.getMessage());
    return -1;
} finally {
    System.out.println("Cerrando recursos...");
}
```

Y un ejemplo sin `catch`:

```java
try {
    return Calculadora.raiz(-9); // lanzará excepción
} finally {
    System.out.println("Liberando recursos antes de propagar la excepción...");
}
```




## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

`throws` es la forma que tiene un método en Java de **anunciar formalmente** que puede producir una excepción *checked* y que **no la va a capturar él mismo**. Es una parte del *contrato* del método: igual que especifica qué parámetros recibe y qué devuelve, también especifica qué errores excepcionales puede generar. Esto obliga a quien llame al método a decidir si quiere capturar esa excepción o volver a declararla con su propio `throws`. En C no existe nada parecido: una función puede fallar y el compilador no te obliga a tratar ese fallo; en C++ existe `throw` pero no hay un sistema obligatorio de declaración como en Java.

El uso de `throws` es una **alternativa a capturar una excepción controlada** porque permite que el método delegue la responsabilidad del manejo del error. Si un método lanza, por ejemplo, `IOException`, puede optar por no poner un bloque `try-catch` y simplemente declarar `throws IOException`. Esto hace que la excepción se *propague* hacia arriba, obligando al llamador a decidir qué hacer. Es útil cuando el método no tiene suficiente contexto para manejar el error de forma adecuada. En C++ podrías hacer algo similar dejando que la excepción suba, pero el compilador no te obliga a declararlo; en C, de nuevo, no existe esta posibilidad.

En el contexto de nuestro ejemplo de la raíz cuadrada, si definimos una excepción personalizada `NumeroNegativoException`, el método podría declararla así:

```java
public static double raiz(double x) throws NumeroNegativoException {
    if (x < 0) {
        throw new NumeroNegativoException("No se puede calcular la raíz de un número negativo");
    }
    return Math.sqrt(x);
}
```

Y el llamador tendría dos opciones: capturarla…

```java
try {
    double r = Calculadora.raiz(-9);
} catch (NumeroNegativoException e) {
    System.out.println("Error: " + e.getMessage());
}
```

…o volver a declararla con `throws` para que siga subiendo:

```java
public static void main(String[] args) throws NumeroNegativoException {
    double r = Calculadora.raiz(-9); // si falla, sube al sistema
}
```

Este mecanismo hace que el manejo de errores sea explícito, verificable por el compilador y más seguro que en C, donde un error puede ignorarse accidentalmente. 



## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

Un método que abre un fichero y **declara que no quiere manejar la excepción**, sino **propagarla hacia arriba**, debe incluir `throws` en la firma. Además, aunque no capture la excepción, puede usar un bloque `finally` para garantizar el cierre del recurso. Un ejemplo típico sería:

```java
public void leerFichero(String ruta) throws IOException {
    FileReader fr = null;
    try {
        fr = new FileReader(ruta);   // Puede lanzar FileNotFoundException (subtipo de IOException)
        int c = fr.read();           // Lectura de ejemplo
        System.out.println("Primer carácter: " + c);
    } finally {
        if (fr != null) {
            fr.close();              // El finally se ejecuta siempre
        }
    }
}
```

La firma `throws IOException` indica que el método **no maneja** la excepción y que será el código que llame a este método quien deba capturarla o volver a propagarla. Dentro del `try` se realiza la operación peligrosa (abrir el fichero), y el `finally` garantiza que el recurso se cierre incluso si ocurre una excepción o si el método sale antes. Esta estructura es habitual cuando se quiere delegar la responsabilidad del manejo de errores sin renunciar a la limpieza de recursos.

Este patrón es especialmente útil en métodos de bajo nivel que forman parte de una cadena de llamadas, donde la decisión de cómo reaccionar ante la ausencia del fichero corresponde a un nivel superior. En versiones modernas de Java, la alternativa más segura es `try-with-resources`, pero el ejemplo anterior ilustra exactamente el comportamiento solicitado: propagación de excepciones y uso obligatorio de `finally`.

Si quieres, puedo mostrarte también la versión equivalente con `try-with-resources` para comparar ambas aproximaciones.




## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Sí, en Java **podemos poner en `throws` excepciones no controladas**, como `RuntimeException` o cualquiera de sus subclases. No es obligatorio, porque las *unchecked exceptions* no requieren declaración explícita, pero el lenguaje lo permite. Incluirlas en `throws` tiene un significado semántico: el autor del método está avisando de forma explícita de que ese método puede lanzar una excepción de ejecución, aunque no esté obligado a declararlo.

El método llamador **no está obligado** a poner un `try-catch` cuando la excepción es no controlada. Las *unchecked* pueden propagarse libremente sin que el compilador exija capturarlas. Esto refleja su filosofía: representan errores de programación (índices fuera de rango, null pointers, argumentos inválidos) que normalmente no se “recuperan”, sino que indican un fallo lógico que debe corregirse.

El sentido de declarar una `RuntimeException` en `throws` es principalmente documental. Sirve para advertir al programador que usa ese método de que puede producirse una condición excepcional concreta, aunque no sea obligatorio capturarla. Es una forma de contrato: “este método puede lanzar esto; úsalo con cuidado”. Algunas APIs lo hacen para mejorar la claridad, especialmente en librerías grandes donde la trazabilidad de errores es importante.

En resumen, sí se puede declarar una excepción no controlada en `throws`, el llamador no está obligado a capturarla, y su utilidad es comunicar mejor el comportamiento del método.


**CLASE**

- Por poder podemos, pero el compilador na va a obligar al bloque try.catch.
- No es habitual
- A veces te ponen por documentación.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Las **excepciones controladas** se recomiendan cuando el problema forma parte del *flujo normal de error* de un programa y el llamador **puede y debe decidir qué hacer**. Casos típicos son fallos de E/S, ausencia de un fichero, pérdida de conexión o límites externos del sistema. Son situaciones previsibles, no errores de programación. Por eso `IOException`, `SQLException` o `ClassNotFoundException` obligan al programador a pensar explícitamente en cómo reaccionar: reintentar, informar al usuario, abortar la operación o registrar el fallo.

Las **excepciones no controladas**, como `IllegalArgumentException`, `NullPointerException` o `IndexOutOfBoundsException`, se usan cuando el error indica un **fallo del programador**, no una condición externa. Si un método recibe un argumento inválido, lo correcto es lanzar una excepción no controlada para señalar que el contrato del método se ha violado. No tiene sentido obligar al llamador a capturarla, porque la solución no es “manejarla”, sino **corregir el código**. Por eso las unchecked se propagan libremente y no requieren `throws`.

No todos los lenguajes distinguen entre excepciones controladas y no controladas. De hecho, **la mayoría no lo hace**. C, C++, Python, JavaScript, Ruby, Go, Swift o Kotlin solo tienen un tipo de excepción (o mecanismo equivalente), y todas se comportan como *no controladas*: el compilador no obliga a capturarlas. Esto refleja la tendencia moderna: se considera que las checked exceptions generan demasiado ruido, dificultan la evolución de APIs y fuerzan capturas artificiales. Por eso, en lenguajes donde solo existe una opción, la aproximación habitual es la de Java con las *unchecked*: excepciones que se propagan sin obligación de declararlas ni capturarlas.

En conjunto, la regla práctica es clara: **usa excepciones controladas para problemas externos y recuperables**, y **usa no controladas para errores de programación o violaciones de contrato**. Esta separación mantiene el código más claro, más seguro y más fácil de mantener.



## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Lanzar excepciones dentro de un `catch` **sí tiene sentido**, y es una práctica habitual cuando el código que captura la excepción **no puede resolverla por completo** o necesita **convertirla en otra excepción más adecuada**. En estos casos, el `catch` actúa como un filtro o adaptador: registra información, añade contexto o transforma el tipo de error, y luego lanza una nueva excepción. Esto es común en capas intermedias de una aplicación, donde cada nivel decide qué información añadir antes de propagar el fallo.

También es posible **relanzar la misma excepción capturada**, usando simplemente `throw e;`. Esto tiene sentido cuando el método quiere, por ejemplo, registrar el error, liberar recursos adicionales o realizar alguna acción compensatoria, pero sin ocultar la excepción original. Relanzarla mantiene el *stack trace* intacto y permite que niveles superiores tomen la decisión final. Es una técnica útil en arquitecturas por capas, donde cada nivel añade contexto sin perder la causa raíz.

---

### Ejemplo 1: lanzar una excepción distinta dentro del `catch`

```java
public void cargarConfiguracion() throws ConfigException {
    try {
        Files.readAllLines(Path.of("config.txt"));
    } catch (IOException e) {
        // Añadimos contexto y convertimos la excepción
        throw new ConfigException("Error cargando la configuración", e);
    }
}
```

Aquí el método no quiere exponer detalles de E/S al exterior, así que transforma la excepción en una más semántica para su dominio (`ConfigException`). El `catch` sirve para encapsular la causa original y mejorar la claridad de la API.

---

### Ejemplo 2: relanzar la misma excepción capturada

```java
public void procesar() throws IOException {
    try {
        realizarOperacion();
    } catch (IOException e) {
        System.err.println("Fallo durante el procesamiento: " + e.getMessage());
        throw e; // Se relanza la misma excepción
    }
}
```

Este patrón es útil cuando el método quiere registrar el error o realizar alguna acción adicional, pero no tiene capacidad para resolverlo. Relanzar la misma excepción mantiene el *stack trace* y evita ocultar información importante.

---

### Cuándo tiene sentido cada caso

- **Lanzar una excepción distinta**:  
  - Cuando quieres *traducir* una excepción técnica a una del dominio.  
  - Cuando quieres ocultar detalles internos de la implementación.  
  - Cuando necesitas añadir contexto significativo para el llamador.

- **Relanzar la misma excepción**:  
  - Cuando quieres registrar el error sin alterar su naturaleza.  
  - Cuando necesitas ejecutar lógica adicional antes de propagarlo.  
  - Cuando no quieres perder el *stack trace* original.

Ambas técnicas son parte esencial del diseño robusto de APIs y de la separación de responsabilidades entre capas.



**CLASE**


Si tiene sentido.
 
    - Reforzar la misma excepción --> 

```java
        try{
            }catch(NumberFormatException e){
                    throw e;

            }
```
    
    - Envolver en otra excepcion nueva (será causa) --> ver pregunta 17.
```java
     try{

     }catch(IOSException e){
        throw new RuntimeException( "Excepción de entrada y salida." ,e*)

     }
```
    -Lanzar otra excepcion totalmente nueva.

```java
    try{
        catch(IOSException e)
        throw new AplicationError("Error")
    }
```
    


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?




**CLASE**

Causa de excepcion.
    - Se ve cuando la excepcion se muestra por pantalla.
        "Excepcion externa (NetfluxException)
        ---
        ---
        ---

        Caused by exception interna (IOSException)
        ---
        ---
        ---
    
    -Se puede obtener con el método "getCase()"




```java



try{

}catch(IOSException e){
    throw new NetfluxException("error E/S", e);
}

```
