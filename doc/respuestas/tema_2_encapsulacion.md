# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

La **encapsulación** es el principio que agrupa datos y los métodos que operan sobre ellos dentro de una misma unidad (la clase), de forma que el objeto gestiona su propio estado interno. La **ocultación de información** va un paso más allá: consiste en restringir el acceso directo a los detalles internos de esa unidad, exponiendo solo aquello que sea estrictamente necesario para el uso externo.

Entre las ventajas de la ocultación de información se encuentran: menor acoplamiento entre clases (los cambios internos no afectan al código externo), mayor facilidad de mantenimiento, posibilidad de garantizar la consistencia interna del objeto (invariantes de clase), y una interfaz más clara y fácil de usar.

---

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La **interfaz pública** de una clase es el conjunto de métodos y atributos accesibles desde el exterior, es decir, todo aquello que otras clases pueden utilizar para interactuar con sus objetos. Constituye el "contrato" que la clase ofrece al resto del programa.

La relación con la ocultación de información es directa: todo lo que no forma parte de la interfaz pública queda oculto. Así, los detalles de implementación (cómo se almacenan los datos, qué algoritmos se usan internamente) permanecen invisibles para el exterior, que solo conoce qué puede hacer con el objeto, no cómo lo hace.

---

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

Cambiar la interfaz pública de una clase es costoso porque implica modificar todo el código que la utiliza. Si una clase es empleada por muchos otros módulos o por código de terceros, cualquier alteración en su interfaz puede romper el sistema de forma masiva.

Por ello, es fundamental diseñarla con cuidado desde el principio: debe ser lo más pequeña posible (exponer solo lo necesario), estable, y coherente con la responsabilidad de la clase. Una vez publicada y en uso, la interfaz pública debe considerarse prácticamente inmutable.

---

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Una **invariante de clase** es una condición que debe cumplirse siempre sobre el estado interno de un objeto, independientemente de qué operaciones se hayan realizado sobre él. Por ejemplo, en una clase `Fraccion`, el denominador nunca puede ser cero.

La ocultación de información ayuda a mantener las invariantes porque impide que el código externo modifique los atributos internos de forma arbitraria. Al forzar el acceso a través de métodos controlados, la propia clase puede validar cualquier cambio de estado y rechazar aquellos que violarían sus invariantes.

---

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}
```

La interfaz pública de `Punto` está formada por: el constructor `Punto(double, double)`, el método `calcularDistanciaAOrigen()`, y los métodos `getX()` y `getY()`.

`public` indica que el miembro es accesible desde cualquier otra clase. `private` indica que el miembro solo es accesible desde dentro de la propia clase; ninguna otra clase puede leerlo ni modificarlo directamente. Los atributos `x` e `y` son privados para proteger el estado interno del objeto.

---

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores `public` y `private` se pueden aplicar a **atributos**, **métodos** y **constructores** de una clase. También se puede aplicar `public` a la propia **clase** (aunque con restricciones según el fichero en que se encuentre).

No se puede aplicar `private` directamente a una clase de nivel superior (top-level), aunque sí a clases internas (nested classes). En la práctica, la mayor parte del uso de estos modificadores recae sobre atributos y métodos.

---

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

Sí, existen más niveles de visibilidad. En Java hay cuatro: `private` (solo la propia clase), *package-private* o sin modificador (accesible dentro del mismo paquete), `protected` (accesible desde el mismo paquete y desde subclases), y `public` (accesible desde cualquier lugar).

En otros lenguajes existen variantes similares. C++ tiene `private`, `protected` y `public`, y además el concepto de `friend`. C# añade `internal` (visible dentro del mismo ensamblado) y `protected internal`. Python, por convenio, usa un guión bajo (`_`) para indicar "uso interno" y doble guión bajo (`__`) para name mangling, aunque no existe una restricción de acceso real a nivel de lenguaje.

---

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

La respuesta correcta es **(a): los miembros privados están ocultos para otras clases**, no para otras instancias de la misma clase. En Java, la visibilidad `private` es a nivel de clase, no de instancia. Por tanto, un objeto de tipo `Punto` puede acceder a los atributos privados de *otro* objeto `Punto`.
```java
public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x; // Acceso a atributo privado de otra instancia
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

Dentro del método, `otro.x` y `otro.y` son accesibles aunque sean `private`, porque el código está escrito dentro de la clase `Punto`. Esto resulta útil en métodos que comparan o combinan dos objetos de la misma clase.

---

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Los **getters** son métodos que devuelven el valor de un atributo privado, permitiendo leerlo desde el exterior de forma controlada. Los **setters** son métodos que permiten modificar el valor de un atributo privado, pudiendo incluir validaciones antes de realizar el cambio.

Por convención en Java, se nombran `getNombreAtributo()` y `setNombreAtributo(valor)`. Para atributos booleanos, el getter suele llamarse `isNombreAtributo()`. Su uso permite mantener los atributos privados y controlar el acceso a ellos sin romper la encapsulación.

---

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No. En este contexto, "seguridad" no se refiere a seguridad informática frente a ataques externos, sino a **seguridad en la corrección del programa**. Se trata de garantizar que el estado interno de los objetos siempre sea válido y coherente, evitando que código externo coloque un objeto en un estado inconsistente que provoque errores difíciles de detectar.

Por ejemplo, si el denominador de una fracción fuera público, cualquier parte del código podría asignarlo a cero, causando una división por cero en algún punto posterior. La ocultación previene este tipo de errores al centralizar y controlar los cambios de estado.

---

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Un **miembro de instancia** pertenece a cada objeto individualmente: cada instancia tiene su propia copia del atributo. Un **miembro de clase** (declarado con `static` en Java) pertenece a la clase en sí, y es compartido por todas las instancias; solo existe una copia independientemente de cuántos objetos se creen.

Sí, los miembros de clase también se pueden ocultar aplicándoles el modificador `private`. De hecho, es habitual que los atributos estáticos sean privados y se acceda a ellos mediante métodos estáticos públicos, exactamente igual que con los miembros de instancia.

---

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí, tiene sentido en varios escenarios. El más común es el patrón **Singleton**, donde se desea que solo exista una instancia de la clase: el constructor privado impide que se creen instancias desde fuera, y se proporciona un método estático público que controla la creación.

Otro uso habitual es en combinación con **métodos factoría** (métodos estáticos que construyen y devuelven objetos), donde se quiere ofrecer distintas formas semánticas de crear un objeto sin exponer el constructor directamente.

---

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

Los miembros de clase se indican con la palabra clave `static`. A continuación se muestra la clase `Punto` ampliada con atributos estáticos para rastrear los máximos:
```java
public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    public double getX() { return x; }
    public double getY() { return y; }
}
```

`maxX` y `maxY` son atributos de clase: existen una sola vez y se actualizan cada vez que se crea un nuevo `Punto`. Se accede a ellos mediante métodos estáticos públicos, manteniendo la ocultación.

---

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
```java
public static Punto crearRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
```

Sí, se ha usado `static`. Un método factoría debe ser estático porque se invoca sobre la clase, no sobre una instancia existente (precisamente su objetivo es *crear* una nueva instancia). Se llamaría así: `Punto p = Punto.crearRedondeado(3.7, -1.2);`.

---

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

Gracias a la ocultación de información, se puede cambiar completamente la representación interna sin que el código externo se vea afectado, siempre que la interfaz pública permanezca igual:
```java
public class Punto {
    private double[] coordenadas; // índice 0 = x, índice 1 = y

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        coordenadas = new double[]{x, y};
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] + coordenadas[1] * coordenadas[1]);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coordenadas[0] - otro.coordenadas[0];
        double dy = this.coordenadas[1] - otro.coordenadas[1];
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() { return coordenadas[0]; }
    public double getY() { return coordenadas[1]; }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    public static Punto crearRedondeado(double x, double y) {
        return new Punto(Math.round(x), Math.round(y));
    }
}
```

La interfaz pública no ha cambiado en absoluto. Este es precisamente uno de los beneficios clave de la encapsulación.

---

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Aunque pueda parecer equivalente, no lo es. Al usar getter y setter, la clase mantiene el control: el setter puede validar el valor antes de asignarlo (por ejemplo, rechazar un radio negativo), y el getter puede devolver una copia en lugar del objeto original para evitar modificaciones externas. Con un atributo público, cualquier asignación directa se escapa completamente al control de la clase.

La convención mayoritaria en Java y en POO en general es que los atributos sean **privados**. Tiene relación directa con las invariantes de clase: si los atributos son privados, la clase puede garantizar que siempre se cumplen sus invariantes, ya que todo cambio de estado pasa por sus métodos.

---

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase es **inmutable** cuando sus objetos, una vez creados, no pueden cambiar de estado. Todos sus atributos se fijan en el constructor y no se modifican después. La clase `String` en Java es el ejemplo más conocido.

Un **método modificador** es cualquier método que altera el estado interno de un objeto. No todos los métodos modificadores son setters: por ejemplo, un método `mover(double dx, double dy)` en `Punto` modificaría las coordenadas sin ser un setter convencional.

Las clases inmutables tienen varias ventajas: son intrínsecamente seguras en entornos multihilo (no requieren sincronización), son más fáciles de razonar sobre ellas, pueden compartirse libremente sin riesgo de modificaciones inesperadas, y se prestan bien al uso como claves en mapas o elementos en conjuntos.

---

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No, no es recomendable incluir setters de forma indiscriminada. Añadir un setter a un atributo equivale a hacerlo mutable desde el exterior, lo cual puede comprometer las invariantes de clase y dificultar el razonamiento sobre el estado del objeto. Solo se deben añadir setters cuando la mutabilidad de ese atributo tenga sentido semánticamente.

La tendencia moderna en diseño orientado a objetos es favorecer la inmutabilidad siempre que sea posible, y añadir setters únicamente cuando el caso de uso lo exija claramente. Generar getters y setters automáticamente para todos los atributos (como hacen algunos IDEs por defecto) se considera una mala práctica si no se reflexiona sobre ello.

---

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase `String` en Java es **inmutable**. Cada vez que se concatenan dos cadenas con el operador `+`, no se modifica ninguna de las existentes; en su lugar se crea un nuevo objeto `String` que contiene el resultado. Las cadenas originales permanecen intactas.

Esto implica que concatenar cadenas en un bucle es muy ineficiente: en cada iteración se crea un nuevo objeto y el anterior queda como basura para el recolector. Para construir cadenas paso a paso de forma eficiente, se debe usar la clase `StringBuilder`, que sí es mutable y está diseñada específicamente para esta tarea:
```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append("fragmento").append(i);
}
String resultado = sb.toString();
```

---

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

En POO, los objetos tienen tanto **identidad** (son instancias distintas en memoria) como **contenido** (los valores de sus atributos). El operador `==` en Java compara identidades: devuelve `true` solo si las dos referencias apuntan exactamente al mismo objeto. Para comparar por contenido, se utiliza el método `equals()`.

Por defecto, `equals()` en Java (heredado de `Object`) hace exactamente lo mismo que `==`: compara identidades. Para que compare contenido, es necesario **sobreescribirlo** en la clase. Clases de la biblioteca estándar como `String`, `Integer` o `LocalDate` ya lo sobreescriben correctamente.

Por ello, dos cadenas en Java **nunca deben compararse con `==`**, sino siempre con `equals()`:
```java
String a = new String("hola");
String b = new String("hola");
System.out.println(a == b);       // false (distinta identidad)
System.out.println(a.equals(b));  // true  (mismo contenido)
```

---

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Las clases **wrapper** (o envoltorio) son clases que encapsulan un tipo primitivo dentro de un objeto. En Java, los tipos primitivos (`int`, `double`, `boolean`, etc.) no son objetos, pero a veces es necesario tratarlos como tales (por ejemplo, para almacenarlos en colecciones como `ArrayList`). Para ello existen clases como `Integer`, `Double`, `Boolean`, etc.

En Java, la conversión entre primitivo y wrapper es automática gracias al mecanismo de **autoboxing** y **unboxing**: `int i = 5; Integer obj = i;` funciona sin código adicional. Las ventajas incluyen poder usar primitivos en contextos que requieren objetos, acceso a métodos útiles (`Integer.parseInt`, `Double.MAX_VALUE`, etc.), y posibilidad de representar la ausencia de valor con `null`.

No todos los lenguajes necesitan wrappers. Lenguajes como Kotlin, Scala o Python tratan todo como objetos desde el principio (aunque internamente puedan usar primitivos por optimización), por lo que el programador no necesita distinguir entre ambos ni realizar conversiones explícitas.

---

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un **tipo enumerado** es un tipo de dato que solo puede tomar un conjunto fijo y predefinido de valores. En lugar de usar constantes enteras arbitrarias (como en C), se nombran explícitamente los posibles valores, lo que hace el código más legible y seguro.

En Java, un `enum` **es una clase**. Esto lo hace especialmente potente: cada valor del enumerado es una instancia de esa clase, y el tipo puede tener atributos privados, constructores privados y métodos públicos, igual que cualquier otra clase. Esto supone una ventaja importante en términos de encapsulación: se pueden asociar datos y comportamiento a cada valor de forma controlada, manteniendo ocultos los detalles internos.

---

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta
```java
public enum Mes {
    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    private final int numeroDeMes;
    private final int numeroDeDias;

    Mes(int numeroDeMes, int numeroDeDias) {
        this.numeroDeMes = numeroDeMes;
        this.numeroDeDias = numeroDeDias;
    }

    public int getNumeroDeMes() {
        return numeroDeMes;
    }

    public int getNumeroDeDias() {
        return numeroDeDias;
    }
}
```

El constructor es implícitamente privado en los enumerados de Java: no se puede instanciar un `Mes` desde fuera, solo se pueden usar las doce instancias predefinidas. Los atributos son `final` y privados, lo que garantiza que los datos de cada mes no pueden cambiar una vez creados.

---

## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

Se consideran las estaciones astronómicas aproximadas: primavera (meses 3-5), verano (6-8), otoño (9-11), invierno (12, 1, 2) para el hemisferio norte. En el hemisferio sur las estaciones se invierten.
```java
public boolean esDePrimavera(boolean esHemisferioNorte) {
    boolean primaveraEnNorte = numeroDeMes >= 3 && numeroDeMes <= 5;
    return esHemisferioNorte ? primaveraEnNorte : !primaveraEnNorte && !esDeVerano(true) && !esDeInvierno(true);
}
```

Una implementación más clara y mantenible extrae la lógica del hemisferio norte y la invierte para el sur:
```java
private boolean esPrimaveraEnNorte() {
    return numeroDeMes >= 3 && numeroDeMes <= 5;
}

private boolean esVeranoEnNorte() {
    return numeroDeMes >= 6 && numeroDeMes <= 8;
}

private boolean esOtonioEnNorte() {
    return numeroDeMes >= 9 && numeroDeMes <= 11;
}

private boolean esInviernoEnNorte() {
    return numeroDeMes == 12 || numeroDeMes <= 2;
}

public boolean esDePrimavera(boolean esHemisferioNorte) {
    return esHemisferioNorte ? esPrimaveraEnNorte() : esOtonioEnNorte();
}

public boolean esDeVerano(boolean esHemisferioNorte) {
    return esHemisferioNorte ? esVeranoEnNorte() : esInviernoEnNorte();
}

public boolean esDeOtoño(boolean esHemisferioNorte) {
    return esHemisferioNorte ? esOtonioEnNorte() : esPrimaveraEnNorte();
}

public boolean esDeInvierno(boolean esHemisferioNorte) {
    return esHemisferioNorte ? esInviernoEnNorte() : esVeranoEnNorte();
}
```

Los métodos auxiliares privados evitan repetir la lógica y hacen que el código sea más legible. La relación