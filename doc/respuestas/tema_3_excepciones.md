# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

En C, al no existir un mecanismo de excepciones, el error debe comunicarse de forma manual entre la función y quien la llama. Las dos opciones más habituales son: usar el **valor de retorno** como código de error, o usar un **parámetro de salida** (pasado por puntero) para indicar el estado.

**Opción 1: Valor de retorno como código de error.** La función devuelve un valor especial (por ejemplo, `-1.0`) cuando se produce un error, y el llamador comprueba ese valor. El inconveniente es que ese valor especial puede confundirse con un resultado legítimo en otros contextos.

```c
#include <stdio.h>
#include <math.h>

double raiz(double x) {
    if (x < 0) {
        return -1.0; // valor especial de error
    }
    return sqrt(x);
}

int main() {
    double resultado = raiz(-9.0);
    if (resultado == -1.0) {
        printf("Error: no se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }
    return 0;
}
```

**Opción 2: Parámetro de salida para el error.** La función recibe un puntero a un entero que actúa como indicador de error. Esto separa claramente el resultado del estado de error, pero obliga al llamador a declarar e inspeccionar esa variable adicional.

```c
#include <stdio.h>
#include <math.h>

double raiz(double x, int *error) {
    *error = 0;
    if (x < 0) {
        *error = 1; // señal de error
        return 0.0;
    }
    return sqrt(x);
}

int main() {
    int error;
    double resultado = raiz(-9.0, &error);
    if (error) {
        printf("Error: no se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }
    return 0;
}
```

Ambas opciones funcionan, pero imponen al programador la responsabilidad de acordarse siempre de comprobar el error tras cada llamada. Si en algún punto se olvida esa comprobación, el programa continúa silenciosamente con datos incorrectos, lo que puede provocar fallos difíciles de rastrear.

---

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una **excepción** es un mecanismo del lenguaje que permite indicar que ha ocurrido una situación anormal o errónea durante la ejecución de un programa, separando el flujo normal del código del flujo de tratamiento de errores. A diferencia de los valores de retorno especiales de C, la excepción se "lanza" y viaja automáticamente por la pila de llamadas hasta que alguien la gestiona.

Cuando un programador **implementa** una función, lanza una excepción en los casos en que no puede completar su trabajo de forma correcta (por ejemplo, recibir un argumento inválido o no encontrar un recurso). Cuando **llama** a una función, puede optar por capturar esa excepción y tratar el error en ese punto, o dejar que se siga propagando hacia capas superiores que tengan más contexto para gestionarla.

---

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java, la función `raiz` puede lanzar una excepción del tipo `IllegalArgumentException` cuando recibe un número negativo. El método `main` envuelve la llamada en un bloque `try-catch` para capturar ese error e informar al usuario, sin necesidad de comprobar ningún valor de retorno especial.

```java
public class Calculadora {

    public double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException(
                "No se puede calcular la raiz de un numero negativo: " + x);
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            double resultado = calc.raiz(-9.0);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error capturado: " + e.getMessage());
        }
    }
}
```

Obsérvese que, a diferencia del ejemplo en C, el método `raiz` no necesita ningún parámetro adicional ni devuelve un valor especial. El propio mecanismo del lenguaje se encarga de interrumpir la ejecución normal y trasladar la información del error hasta el bloque `catch`.

---

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

**Lanzar** una excepción (`throw`) es el acto de crear un objeto excepción y entregárselo a la máquina virtual para que interrumpa el flujo normal del método en ese punto. En el ejemplo, cuando `raiz` recibe `-9.0`, ejecuta `throw new IllegalArgumentException(...)` y deja de ejecutarse a partir de ahí: no llega a calcular `Math.sqrt`.

**Capturar** o **controlar** una excepción significa interceptarla en un bloque `catch`. En el ejemplo, el `main` tiene un `try-catch` que atrapa la `IllegalArgumentException` y ejecuta el código del `catch`, mostrando el mensaje de error. A partir de ese punto el programa continúa con normalidad después del bloque `catch`.

**Propagarse** quiere decir que, si un método no captura la excepción, ésta sube automáticamente al método que lo llamó, y así sucesivamente por la pila de llamadas. Imaginemos que `main` llamase a un método `calcularResultados` que a su vez llamase a `raiz`: si ninguno de los dos tiene `try-catch`, la excepción sube desde `raiz` → `calcularResultados` → `main`. Cada uno de esos métodos **se abandona inmediatamente** en el punto donde estaba: no se ejecutan las instrucciones que había después de la llamada problemática. Las funciones que no controlan la excepción **no se reanudan** en ningún momento; su ejecución quedó interrumpida definitivamente para esa llamada.

Si la excepción llega al método `main` sin ser capturada, la JVM la muestra por pantalla (con el mensaje y la traza de la pila) y termina el programa.

---

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

En C, si un error ocurre en una función profundamente anidada, cada función intermedia de la cadena de llamadas debe comprobar el valor de retorno o el parámetro de error y pasarlo hacia arriba manualmente. Esto contamina el código de cada nivel con comprobaciones repetitivas que no tienen que ver con la lógica principal del programa.

Con la propagación automática de Java, sólo el nivel que realmente sabe cómo tratar el error necesita un bloque `catch`. Las funciones intermedias se liberan de toda responsabilidad: si no pueden hacer nada útil con el error, simplemente no lo gestionan y la excepción sigue subiendo sola. Esto hace el código más limpio, más fácil de leer y menos propenso a que un programador olvide propagar el error.

Además, si en C se olvida comprobar el código de error, el programa continúa silenciosamente con un estado incorrecto. Con las excepciones, si ningún nivel la captura, la JVM termina el programa de forma controlada e informa del problema, en lugar de dejar que el error pase inadvertido.

---

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

Sí, en orientación a objetos las excepciones son objetos de clases que forman una jerarquía de herencia. En Java, todas derivan de `Throwable`, y las que usualmente maneja el programador descienden de `Exception`. El hecho de que sean objetos permite aplicar todos los principios de encapsulación: la excepción puede almacenar en sus atributos toda la información relevante sobre el error (mensaje descriptivo, código de error, estado del sistema en ese momento, etc.) y exponer métodos para acceder a ella.

Gracias a la jerarquía de clases, se puede capturar una excepción de forma más o menos genérica: capturar `Exception` atrapa cualquier excepción derivada, mientras que capturar `IllegalArgumentException` sólo atrapa ese tipo concreto. Esto da flexibilidad para tratar errores con la granularidad adecuada en cada nivel de la aplicación.

Efectivamente, se pueden crear excepciones personalizadas simplemente extendiendo `Exception` o `RuntimeException`. Por ejemplo, se podría definir una clase `NumeroNegativoException` con un atributo que almacene el valor problemático, lo que resulta mucho más expresivo e informativo que un simple código de error entero como en C.

```java
public class NumeroNegativoException extends RuntimeException {
    private final double valorNegativo;

    public NumeroNegativoException(double valor) {
        super("Valor negativo no permitido: " + valor);
        this.valorNegativo = valor;
    }

    public double getValorNegativo() {
        return valorNegativo;
    }
}
```

---

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

En C, cuando se detecta un error lo único que suele comunicarse es un número entero o un valor especial. El manejador recibe ese código pero no sabe dónde ocurrió exactamente el fallo ni por qué ruta llegó hasta él, lo que complica enormemente la depuración.

Todo objeto excepción en Java lleva incorporado, como mínimo, dos elementos esenciales: el **mensaje de error** (accesible con `getMessage()`), que describe en lenguaje natural qué salió mal, y el **stack trace** o traza de la pila (accesible con `printStackTrace()`), que registra automáticamente la secuencia exacta de llamadas de métodos que llevó hasta el punto donde se lanzó la excepción, con los números de línea correspondientes.

Esta traza es de un valor enorme para el diagnóstico: sin escribir ningún código adicional, el programador puede ver en qué clase, en qué método y en qué línea exacta se produjo el error, y cómo se llegó hasta allí. En C, para obtener información equivalente habría que añadir manualmente trazas de depuración en cada función, lo cual es costoso y fácilmente omisible.

---

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Sí, un bloque `try` puede ir seguido de múltiples bloques `catch`, cada uno capturando un tipo de excepción diferente. Esto permite tratar distintos tipos de error de forma diferenciada sin necesidad de anidar estructuras.

Sin embargo, **siempre se ejecuta como máximo un único bloque `catch`**: en cuanto la excepción lanzada coincide con el tipo declarado en un `catch`, ese bloque se ejecuta y los restantes se saltan completamente. Los bloques `catch` se evalúan en orden de arriba a abajo, por lo que se deben colocar primero los tipos más específicos y después los más generales; si se pusiese un `catch (Exception e)` al principio, capturaría cualquier excepción antes de que los bloques más específicos puedan actuar.

```java
try {
    double resultado = calc.raiz(-9.0);
} catch (IllegalArgumentException e) {
    System.out.println("Argumento invalido: " + e.getMessage());
} catch (ArithmeticException e) {
    System.out.println("Error aritmetico: " + e.getMessage());
} catch (Exception e) {
    System.out.println("Error generico: " + e.getMessage());
}
```

---

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

El bloque `finally` en Java es un bloque de código que **siempre se ejecuta** al salir de un `try`, independientemente de si se lanzó una excepción, si fue capturada o si el flujo terminó normalmente. Es el lugar ideal para colocar la liberación de recursos (cerrar ficheros, conexiones de base de datos, etc.) ya que garantiza que esa limpieza ocurre pase lo que pase.

**Ejemplo con `catch` y `finally`:** el `catch` trata el error y el `finally` libera el recurso en cualquier caso.

```java
public void leerFichero(String ruta) {
    FileReader fichero = null;
    try {
        fichero = new FileReader(ruta);
        // ... operaciones de lectura ...
    } catch (IOException e) {
        System.out.println("Error al leer el fichero: " + e.getMessage());
    } finally {
        if (fichero != null) {
            try { fichero.close(); } catch (IOException e) { /* ignorar */ }
        }
        System.out.println("Bloque finally ejecutado: recurso liberado.");
    }
}
```

**Ejemplo con `finally` sin `catch`:** la excepción no se captura aquí y se seguirá propagando, pero el `finally` se ejecuta igualmente antes de que suba.

```java
public void leerFichero(String ruta) throws IOException {
    FileReader fichero = new FileReader(ruta);
    try {
        // ... operaciones de lectura ...
    } finally {
        fichero.close(); // se ejecuta siempre, incluso si hay excepcion
        System.out.println("Fichero cerrado en finally.");
    }
}
```

---

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

Sí, como se ha visto en el ejemplo anterior, `finally` puede ir directamente acompañando a `try` sin ningún `catch`. La combinación `try-finally` es perfectamente válida y se usa precisamente cuando no se quiere capturar la excepción en ese nivel pero sí se necesita garantizar la limpieza de recursos.

El bloque `finally` se ejecuta **siempre**: tanto si el `try` termina normalmente, como si lanza una excepción (sea o no capturada por un `catch`). Esta garantía es incondicional.

Incluso si dentro del `try` (o del `catch`) hay una sentencia `return`, el bloque `finally` se ejecuta **antes** de que el método retorne efectivamente. La JVM "aparca" el valor de retorno, ejecuta el `finally` y sólo entonces devuelve el control al llamador. Hay una excepción muy particular a esta regla: si el propio bloque `finally` contiene un `return`, ese nuevo `return` sobreescribe al anterior, lo que puede ser una fuente de bugs confusos y por eso se considera mala práctica poner `return` dentro de `finally`.

---

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

Las **excepciones controladas** (*checked*) son aquellas que el compilador obliga a gestionar: si un método puede lanzarlas, el llamador **debe** o bien capturarlas con `try-catch` o bien declararlas en su firma con `throws`. Representan situaciones anómalas pero previsibles que escapan al control del programador (fallos de E/S, recursos externos, etc.). Heredan de `Exception` pero **no** de `RuntimeException`.

Las **excepciones no controladas** (*unchecked*) heredan de `RuntimeException` (o de `Error`). El compilador no obliga a gestionarlas: el programador puede ignorarlas si lo considera apropiado. Representan generalmente errores de programación (usar `null`, índice fuera de rango, argumento inválido) que deberían haberse evitado con código correcto, no con bloques `try-catch`.

Ejemplos de **excepciones controladas** estándar: `IOException`, `FileNotFoundException`, `SQLException`. Ejemplos de **no controladas**: `NullPointerException`, `ArrayIndexOutOfBoundsException`, `IllegalArgumentException`, `IllegalStateException`.

**Situaciones donde se prefiere una excepción controlada:**
- Abrir un fichero que puede no existir (`FileNotFoundException`).
- Conectarse a una base de datos cuya URL puede ser incorrecta o inaccesible (`SQLException`).
- Leer datos de red cuando la conexión puede interrumpirse (`IOException`).
- Parsear una fecha recibida de un formulario externo que puede tener formato incorrecto.

**Situaciones donde se prefiere una excepción no controlada:**
- Pasar un argumento `null` a un método que no lo admite (`NullPointerException` o `IllegalArgumentException`).
- Acceder a un índice inválido de un array, lo que indica un error lógico del programador.
- Llamar a un método en un objeto cuyo estado interno no es el esperado (`IllegalStateException`).
- Detectar una condición que nunca debería ocurrir si el código está bien escrito (violación de invariante interna).

---

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

La cláusula `throws` aparece en la **firma de un método** y declara que ese método puede lanzar (o dejar pasar) una o varias excepciones controladas sin capturarlas internamente. Es una forma de "delegar" la responsabilidad de gestión al método llamador.

```java
public void abrirFichero(String ruta) throws IOException {
    FileReader fichero = new FileReader(ruta); // puede lanzar IOException
    // ...
}
```

Ante una excepción controlada, el programador tiene dos opciones: capturarla con `try-catch` ahí mismo, o declararla con `throws` para que se propague. `throws` es la alternativa cuando el método en cuestión no tiene suficiente contexto para saber qué hacer con el error, o cuando se considera que quien llama al método es quien debe decidir cómo reaccionar. Así, el error "sube" hasta el nivel donde sí existe contexto suficiente para tratarlo de forma significativa.

Es importante destacar que `throws` es un contrato visible en la API del método: el compilador fuerza a cualquier llamador a decidir explícitamente si captura o propaga la excepción, lo que impide ignorarla accidentalmente.

---

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa manejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

En el siguiente ejemplo, el método `procesarFichero` declara `throws IOException`, por lo que cualquier `IOException` (incluyendo `FileNotFoundException`) se propagará al llamador. Sin embargo, el bloque `finally` garantiza que el recurso se cierre siempre, incluso cuando la excepción se está propagando hacia arriba.

```java
public class ProcesadorFichero {

    public void procesarFichero(String ruta) throws IOException {
        FileReader fichero = null;
        try {
            fichero = new FileReader(ruta); // lanza FileNotFoundException si no existe
            BufferedReader lector = new BufferedReader(fichero);
            String linea;
            while ((linea = lector.readLine()) != null) {
                System.out.println(linea);
            }
        } finally {
            // Se ejecuta siempre: tanto si hay excepcion como si no
            if (fichero != null) {
                fichero.close(); // puede lanzar IOException, pero raro
            }
        }
    }

    public static void main(String[] args) {
        ProcesadorFichero p = new ProcesadorFichero();
        try {
            p.procesarFichero("datos.txt");
        } catch (IOException e) {
            System.out.println("El fichero no pudo abrirse: " + e.getMessage());
        }
    }
}
```

Nótese que no hay `catch` dentro de `procesarFichero`: el método simplemente deja pasar la excepción, pero el `finally` asegura la limpieza. Es el `main` quien finalmente decide cómo informar al usuario del problema.

---

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Sí, técnicamente es posible declarar en `throws` excepciones no controladas (`RuntimeException` y sus subclases), pero el compilador **no lo exige** ni lo exige al llamador tampoco. Declarar una `RuntimeException` en `throws` no obliga a nadie a capturarla.

El sentido de hacerlo es puramente **documental**: sirve para advertir explícitamente a quien vaya a usar el método de que existe la posibilidad de que se lance esa excepción en ciertas condiciones. Por ejemplo, `public double raiz(double x) throws IllegalArgumentException` comunica en la propia firma que si se pasa un valor inválido habrá una excepción, aunque el llamador no esté obligado a capturarla.

El llamador podría optar libremente por poner un `try-catch` para esa excepción no controlada si en su contexto tiene sentido recuperarse de ese error (por ejemplo, validar la entrada del usuario y volver a pedirla). Pero también puede ignorarla si confía en que nunca pasará un valor inválido. En resumen, `throws` con excepciones no controladas es una anotación de intención, no un contrato impuesto por el compilador.

---

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Se recomienda usar **excepciones controladas** cuando el error es una condición externa, previsible y recuperable, que puede ocurrir aunque el código esté perfectamente escrito: un fichero que no existe, una conexión de red que falla, una base de datos inaccesible. En estos casos tiene sentido obligar al programador a que planifique qué hacer si ocurre ese error, porque es razonable que el programa continúe de alguna forma.

Las **excepciones no controladas** son preferibles cuando el error indica un fallo en la lógica del propio programa: pasar un argumento `null` donde no se admite, índices incorrectos, estados internos inconsistentes. Estos errores deberían prevenirse con código correcto, no gestionarse en tiempo de ejecución. Forzar al llamador a capturarlos generaría bloques `try-catch` por todas partes para errores que nunca deberían ocurrir.

No todos los lenguajes ofrecen ambas opciones. C++, Python, C#, Kotlin y JavaScript, entre otros, sólo tienen excepciones no controladas (el compilador nunca obliga a capturarlas). Java es de los pocos lenguajes con una distinción formal entre controladas y no controladas. Cuando sólo existe una opción, la más habitual es la excepción **no controlada**, ya que la experiencia de la industria ha mostrado que las excepciones controladas, aunque útiles en ciertos contextos, pueden volverse engorrosas cuando se abusa de ellas y llevan a código lleno de capturas vacías o que simplemente relanza la excepción sin añadir valor.

---

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, tiene pleno sentido lanzar excepciones dentro de un `catch`. Una razón habitual es capturar una excepción de bajo nivel y transformarla en otra excepción de más alto nivel que tenga más sentido semántico para el llamador (esto se conoce como "envolver" o "encadenar" excepciones). Otra razón es registrar el error en un log antes de relanzarlo.

**Ejemplo 1: lanzar una excepción diferente desde el `catch`.**

```java
public double raiz(double x) {
    try {
        if (x < 0) throw new IllegalArgumentException("Negativo: " + x);
        return Math.sqrt(x);
    } catch (IllegalArgumentException e) {
        // Se registra y se lanza una excepción de dominio propio
        System.err.println("Log: error en raiz - " + e.getMessage());
        throw new CalculadoraException("Operacion invalida en raiz", e);
    }
}
```

También se puede **relanzar la misma excepción capturada** usando `throw e` dentro del `catch`. Esto tiene sentido cuando se quiere hacer algo antes de que la excepción continúe propagándose (como registrar información, limpiar estado parcial, o simplemente añadir contexto) pero sin absorber el error.

**Ejemplo 2: relanzar la misma excepción.**

```java
public double raiz(double x) throws IllegalArgumentException {
    try {
        if (x < 0) throw new IllegalArgumentException("Negativo: " + x);
        return Math.sqrt(x);
    } catch (IllegalArgumentException e) {
        System.err.println("Log de error antes de propagar: " + e.getMessage());
        throw e; // se relanza la misma excepcion
    }
}
```

---

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

El concepto de **causa** (*cause*) permite encadenar excepciones: una excepción de alto nivel "envuelve" a una de bajo nivel, preservando la información original del fallo. Esto se hace pasando la excepción original como argumento al constructor de la nueva, o usando el método `initCause()`. La motivación es mantener la abstracción adecuada en cada capa: una capa de negocio no debería exponer detalles internos de `IOException` al llamador, pero sí conservar esa información para diagnóstico.

```java
// Excepcion personalizada de alto nivel
public class ServicioException extends Exception {
    public ServicioException(String mensaje, Throwable causa) {
        super(mensaje, causa); // la causa queda encadenada
    }
}

// Clase de servicio
public class ServicioDatos {
    public String cargarDatos(String ruta) throws ServicioException {
        try {
            FileReader fichero = new FileReader(ruta);
            // ... leer datos ...
            return "datos";
        } catch (IOException e) {
            // Se encapsula la IOException en una excepcion de dominio
            throw new ServicioException("No se pudieron cargar los datos desde: " + ruta, e);
        }
    }

    public static void main(String[] args) {
        ServicioDatos s = new ServicioDatos();
        try {
            s.cargarDatos("archivo_inexistente.txt");
        } catch (ServicioException e) {
            e.printStackTrace();
        }
    }
}
```

Cuando se llama a `e.printStackTrace()`, Java muestra en pantalla tanto la excepción de alto nivel como su causa, separadas por la etiqueta `Caused by:`. Se verían las dos trazas de pila completas, lo que permite saber tanto qué salió mal en términos de negocio como cuál fue el error técnico original que lo provocó. Esta información es inestimable para depurar aplicaciones con múltiples capas de abstracción.