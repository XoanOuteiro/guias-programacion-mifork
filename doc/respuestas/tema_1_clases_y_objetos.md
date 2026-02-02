<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

```
Las cuatro características básicas de la programación orientada a objetos son abstracción, encapsulación, herencia y polimorfismo. Estas propiedades definen la forma en que se modelan los programas y permiten organizar el código de manera más cercana a los conceptos del mundo real, en contraste con la programación estructurada tradicional usada en C.

La abstracción consiste en identificar las características esenciales de un objeto y omitir los detalles irrelevantes. Se centra en el qué hace un objeto y no en cómo lo hace, permitiendo trabajar con conceptos de alto nivel. De este modo, se reduce la complejidad del sistema y se facilita su comprensión y mantenimiento.

La encapsulación implica agrupar los datos y las operaciones que actúan sobre ellos dentro de una misma entidad, llamada clase, y controlar el acceso a dichos datos (en el sentido de ocultarlos). Esto evita manipulaciones indebidas desde el exterior y fuerza a interactuar con el objeto mediante interfaces bien definidas. 

La herencia permite crear nuevas clases a partir de otras ya existentes, reutilizando su comportamiento y añadiendo o modificando funcionalidades, lo que reduce la duplicación de código.

El polimorfismo permite que diferentes objetos respondan de manera distinta a una misma operación, siempre que compartan una interfaz común. Esto posibilita escribir código más genérico y flexible, ya que una misma llamada puede tener comportamientos distintos según el tipo concreto del objeto que la recibe.
```

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

```
Algunos de los lenguajes de programación más populares que permiten la programación orientada a objetos son Java, C++, Python y C#. Todos ellos incorporan mecanismos para definir clases y objetos, así como soporte para encapsulación, herencia y polimorfismo, aunque su sintaxis y filosofía de diseño difieren entre sí.

Estos lenguajes se utilizan ampliamente en distintos ámbitos del desarrollo de software, como aplicaciones de escritorio, desarrollo web, sistemas empresariales y videojuegos. Su popularidad se debe, en parte, a que la programación orientada a objetos facilita la reutilización del código, el mantenimiento de programas grandes y el trabajo colaborativo en proyectos complejos.
```

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

```
La programación estructurada es un paradigma que propone organizar el código mediante estructuras de control bien definidas, evitando el uso de saltos incondicionales como goto. Se basa principalmente en tres construcciones fundamentales: secuencia, selección y repetición. Este enfoque favorece programas más claros, predecibles y fáciles de depurar, y es característico de lenguajes como C, donde el flujo del programa se controla mediante funciones y estructuras de control.

La programación modular va un paso más allá y consiste en dividir un programa en módulos independientes, cada uno encargado de una funcionalidad concreta. Cada módulo agrupa funciones y datos relacionados, reduciendo las dependencias entre distintas partes del programa. Este enfoque mejora la reutilización del código, facilita el mantenimiento y permite que distintas partes del sistema se desarrollen y prueben de forma separada, incluso por diferentes personas.
```

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

```
En la programación orientada a objetos, un objeto queda definido por tres elementos fundamentales: identidad, estado y comportamiento. Estos elementos permiten diferenciar un objeto de otro y describen tanto qué es como qué puede hacer dentro de un programa.

La identidad es aquello que distingue a un objeto de cualquier otro, incluso cuando dos objetos tienen el mismo estado. El estado está formado por los valores de sus atributos en un momento dado y representa la información que el objeto almacena. El comportamiento corresponde a las operaciones que el objeto puede realizar, normalmente implementadas mediante métodos, y define cómo el objeto actúa o responde ante determinadas acciones.
```

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

```
Una clase es una definición o plantilla que describe las características y comportamientos que tendrán los objetos de un determinado tipo. En ella se especifican los atributos (datos) y los métodos (operaciones) que comparten todos los objetos creados a partir de esa clase. Puede entenderse como un molde que aún no representa nada concreto en memoria.

Una clase no es lo mismo que un objeto. El objeto es una entidad concreta que existe en tiempo de ejecución y ocupa memoria, mientras que la clase es solo la descripción de cómo deben ser esos objetos. Cuando se crea un objeto a partir de una clase, se dice que ese objeto es una instancia de la clase, es decir, una realización concreta de dicha definición con su propio estado.

No todos los lenguajes orientados a objetos manejan el concepto de clase de la misma forma. Aunque lenguajes como Java, C++ o C# se basan claramente en clases, existen otros lenguajes orientados a objetos que utilizan enfoques distintos, como los basados en prototipos. En estos casos no existen clases como tal, pero sí se mantienen los principios fundamentales de la programación orientada a objetos.
```


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

```
En la mayoría de los lenguajes orientados a objetos, los objetos se almacenan en la memoria dinámica, también conocida como heap. Esta zona de memoria se utiliza para datos cuyo tiempo de vida no está ligado a un bloque concreto de ejecución, lo que permite que los objetos sigan existiendo mientras sean necesarios. Sin embargo, el modo exacto en que se gestionan estas asignaciones depende del lenguaje y de su modelo de ejecución.

No es igual en todos los lenguajes. En lenguajes como C++, un objeto puede almacenarse tanto en la pila (stack) como en el heap, dependiendo de cómo se cree. En cambio, en lenguajes como Java, los objetos se crean siempre en memoria dinámica y se accede a ellos mediante referencias, sin que el programador controle directamente su liberación.

La recolección de basura es un mecanismo automático de gestión de memoria que se encarga de liberar los objetos que ya no se utilizan en el programa. El sistema detecta qué objetos no son accesibles desde ninguna parte activa del código y recupera la memoria que ocupan. Este proceso simplifica el desarrollo y reduce errores comunes como fugas de memoria o accesos a zonas ya liberadas, aunque introduce una gestión menos directa de los recursos.
```

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

```
Un método es una función o procedimiento que pertenece a una clase y define un comportamiento que sus objetos pueden ejecutar. Los métodos operan sobre los atributos del objeto y permiten interactuar con él de forma controlada, garantizando que las operaciones se realicen de manera coherente con la estructura interna del objeto. Se puede pensar en ellos como acciones que el objeto sabe realizar.

La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre dentro de una clase, pero con diferentes listas de parámetros (tipo o número de argumentos). Esto permite ofrecer múltiples formas de realizar una operación similar, facilitando la legibilidad y flexibilidad del código. Por ejemplo, un método sumar podría recibir dos números enteros, dos números decimales o una lista de números, dependiendo de la sobrecarga implementada.
```

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

``` java
// Definición de la clase
class Punto {
    double x; // atributo con visibilidad por defecto
    double y; // atributo con visibilidad por defecto

    // Método que calcula la distancia al origen (0,0)
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

// Ejemplo de uso
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(); // creación de instancia
        p.x = 3;               // asignación de valores (gnorando encapsulacion (los atributos son publicos))
        p.y = 4;

        double distancia = p.calculaDistanciaAOrigen(); // uso del método
        System.out.println("Distancia al origen: " + distancia);
    }
}

```

```
En este ejemplo, la clase Punto define los atributos x e y y un método que devuelve la distancia al origen utilizando la fórmula de la raíz cuadrada de la suma de los cuadrados de las coordenadas. La instancia p se crea en el main, se asignan valores a sus atributos y se llama al método para obtener la distancia, que se imprime en consola.

Si se desea, se puede mostrar también una versión más “estilo Java moderno” con constructor, manteniendo la sencillez, para comparar con C++ y facilitar la transición desde tu experiencia previa.
```


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

```
En Java, el punto de entrada de cualquier programa es el método main. Este método tiene una firma específica: public static void main(String[] args). Cuando se ejecuta un programa, la máquina virtual de Java busca este método y comienza la ejecución desde allí. Sin main, el programa no sabe por dónde iniciar, aunque las clases puedan existir y definirse correctamente.

La palabra clave static indica que un miembro (atributo o método) pertenece a la clase en sí, y no a ninguna instancia concreta de esa clase. Esto significa que puede accederse sin crear un objeto de la clase, como ocurre con main. En este contexto, el método main es estático para que la JVM pueda ejecutarlo sin necesitar instanciar la clase que lo contiene.

static no se usa solo para main; puede aplicarse a cualquier método o atributo que deba existir a nivel de clase y compartirse entre todas las instancias. Por ejemplo, se pueden definir contadores o métodos utilitarios como static para que todos los objetos accedan a la misma información o función sin crear copias redundantes.

Cuando static se combina con final, se obtiene un miembro constante a nivel de clase. Esto significa que su valor no puede cambiar y sigue siendo compartido por todas las instancias. Es útil, por ejemplo, para definir constantes globales como public static final double PI = 3.1416;, evitando errores y asegurando que su valor permanezca fijo a lo largo del programa.
```

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

```
Para ejecutar un programa en Java desde línea de comandos, primero se compila el archivo fuente .java usando javac. Por ejemplo, si el archivo se llama Main.java, se escribe: javac Main.java

Esto genera un archivo .class con el byte-code, que es un código intermedio que la máquina virtual de Java puede interpretar. Para ejecutar el programa, se utiliza el comando java seguido del nombre de la clase que contiene el método main: java Main

Java es un lenguaje compilado e interpretado. Se compila a byte-code en lugar de código máquina nativo, lo que permite que el mismo archivo .class se ejecute en cualquier sistema operativo que tenga la máquina virtual de Java (JVM) instalada. La JVM interpreta o just-in-time compila ese byte-code para que funcione en la plataforma concreta, proporcionando portabilidad y seguridad.

El byte-code es una representación intermedia de las instrucciones del programa, optimizada para ser ejecutada por la JVM. Los ficheros .class contienen este byte-code; cada clase Java compilada genera su propio .class. Así, el proceso de compilación transforma el código legible por humanos (.java) en un formato que puede ser ejecutado de manera eficiente en cualquier sistema que tenga la JVM.
```

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

```
En Java, la palabra clave new se utiliza para crear una nueva instancia de una clase en memoria dinámica (heap). Al usar new, se reserva espacio para el objeto y se llama automáticamente a su constructor, que es un método especial encargado de inicializar los atributos del objeto cuando se crea.

Un constructor es un bloque de código dentro de una clase que se ejecuta al instanciarla. Su nombre coincide exactamente con el de la clase y no tiene tipo de retorno, ni siquiera void. Su función principal es inicializar los atributos del objeto y, opcionalmente, realizar operaciones de preparación necesarias para que el objeto quede listo para su uso.

Ejemplo de una clase Empleado con constructor:
```

``` java
class Empleado {
    String DNI;
    String nombre;
    String apellidos;

    // Constructor
    Empleado(String DNI, String nombre, String apellidos) {
        this.DNI = DNI;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

// Ejemplo de uso
public class Main {
    public static void main(String[] args) {
        Empleado e = new Empleado("12345678A", "Ana", "Pérez");
        System.out.println(e.nombre + " " + e.apellidos);
    }
}

```

```
En este ejemplo, al ejecutar new Empleado(...), se crea un objeto en memoria y el constructor inicializa sus atributos DNI, nombre y apellidos. El uso de this diferencia los atributos de los parámetros del constructor.
```

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

```
En Java, la referencia this se utiliza dentro de un objeto para referirse a sí mismo, es decir, a la instancia actual de la clase donde se encuentra el código. Permite acceder a los atributos y métodos del objeto de forma explícita, resolver ambigüedades entre variables locales y atributos de la clase, o pasar la propia instancia como argumento a otros métodos.

No todos los lenguajes orientados a objetos usan la misma palabra. Por ejemplo, en C++ también se usa this, mientras que en Python se emplea self. La idea es la misma: referirse al objeto actual, pero la sintaxis depende del lenguaje.

Ejemplo de uso de this en la clase Punto:
```

``` java
class Punto {
    double x;
    double y;

    // Constructor que usa 'this' para diferenciar atributos de parámetros
    Punto(double x, double y) {
        this.x = x; // 'this.x' se refiere al atributo, 'x' al parámetro
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}

// Uso
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println(p.calculaDistanciaAOrigen()); // 5.0
    }
}

```

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

``` java
class Punto {
    double x;
    double y;

    // Constructor
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Distancia al origen
    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Nuevo método: distancia a otro punto
    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

// Ejemplo de uso
public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto(3, 4);
        Punto p2 = new Punto(6, 8);

        System.out.println("Distancia al origen p1: " + p1.calculaDistanciaAOrigen());
        System.out.println("Distancia entre p1 y p2: " + p1.distanciaA(p2));
    }
}

```

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

```
En Java, cuando se pasa un objeto como un parámetro, como en el caso de Punto, en realidad se pasa una copia de la referencia al objeto, no el objeto completo. Esto significa que dentro del método se trabaja sobre la misma instancia del objeto en memoria. Por lo tanto, si se modifican los atributos del objeto pasado como parámetro, esos cambios sí afectan al objeto fuera del método, porque todos los accesos apuntan a la misma ubicación en memoria.

Por el contrario, cuando se pasa un tipo primitivo como int, double o boolean, Java pasa una copia del valor. Esto implica que cualquier modificación realizada dentro del método no afecta a la variable original fuera del método, ya que el método trabaja con una copia independiente de ese valor.

Ejemplo comparativo:
```

``` java
class Punto {
    double x, y;
    Punto(double x, double y) { this.x = x; this.y = y; }
}

public class Main {
    static void cambiaPunto(Punto p) {
        p.x = 10; // afecta al objeto original
    }

    static void cambiaEntero(int n) {
        n = 10; // no afecta a la variable original
    }

    public static void main(String[] args) {
        Punto p = new Punto(1, 2);
        int numero = 5;

        cambiaPunto(p);
        cambiaEntero(numero);

        System.out.println(p.x);    // 10 → el objeto se modificó
        System.out.println(numero); // 5  → el entero no cambió
    }
}

```

```
En resumen, en Java los objetos se pasan “por referencia a la copia”, permitiendo modificar su estado desde el método, mientras que los primitivos se pasan por valor, manteniendo la variable original intacta. Esto es un detalle clave para evitar confusiones al trabajar con métodos que reciben objetos como parámetros.
```


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

```
En Java, el método toString() es un método heredado de la clase base Object que se utiliza para obtener una representación en forma de cadena de texto de un objeto. Por defecto, devuelve el nombre de la clase seguido de un identificador hexadecimal de la instancia, pero suele sobrescribirse en cada clase para ofrecer información más significativa sobre el estado del objeto. Esto facilita la depuración, la impresión por consola y la conversión de objetos a cadenas.

No todos los lenguajes usan exactamente toString(), pero muchos cuentan con mecanismos similares. Por ejemplo, en Python se emplea __str__ o __repr__, y en C++ se puede definir un operador de salida (operator<<) para lograr un comportamiento equivalente al de toString() en Java. La idea general es la misma: proporcionar una forma legible de representar un objeto como texto.

Ejemplo de toString() en la clase Punto:
```

``` java
class Punto {
    double x, y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Sobrescribir toString para mostrar coordenadas
    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}

// Uso
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println(p);           // llama automáticamente a p.toString()
        System.out.println(p.toString()); // salida: (3.0, 4.0)
    }
}

```

```
En este ejemplo, se sobrescribe toString() para que devuelva las coordenadas del punto en formato (x, y). Así, cada vez que se imprime el objeto p, Java llama automáticamente a este método, mostrando una representación legible del objeto en lugar del valor predeterminado.
```

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta

```
Una clase en Java comparte algunas similitudes con un struct en C, en el sentido de que ambos agrupan datos bajo un mismo nombre, permitiendo organizar información relacionada en una sola unidad. En un struct, se definen campos o variables que representan el estado de la estructura, lo que recuerda a los atributos de una clase.

Sin embargo, un struct en C no tiene métodos ni comportamiento asociado, mientras que una clase puede contener tanto atributos como métodos que operan sobre esos datos. Además, las clases incluyen mecanismos de encapsulación, herencia y polimorfismo, que no existen en los structs. Esto permite que los objetos sean entidades con identidad, comportamiento propio y relaciones jerárquicas, mientras que un struct es simplemente un contenedor de datos.

Otro punto importante es que, en C, las variables de tipo struct no son instancias completas en el sentido orientado a objetos; no tienen constructores automáticos, no pueden tener referencias a sí mismas como this, ni existen mecanismos automáticos de gestión de memoria o inicialización más allá de lo que el programador implemente manualmente. Para que un struct se comporte como una clase, habría que añadir manualmente funciones asociadas, punteros a datos y lógica para simular métodos y constructores.
```

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

```
En C, que no tiene orientación a objetos, se puede emular una clase usando un struct para almacenar los datos y una función separada que reciba un puntero al struct como parámetro. El puntero cumple el mismo papel que this en Java, ya que permite que la función acceda a los campos de una instancia concreta.

Ejemplo de cómo emular la clase Punto en C:
```

``` c
#include <stdio.h>
#include <math.h>

// Definición del "struct Punto"
typedef struct {
    double x;
    double y;
} Punto;

// Función que calcula la distancia al origen, recibe un puntero al struct
double calculaDistanciaAOrigen(Punto *p) {
    return sqrt(p->x * p->x + p->y * p->y);
}

int main() {
    Punto p = {3, 4}; // "instancia" del struct
    printf("Distancia al origen: %f\n", calculaDistanciaAOrigen(&p));
    return 0;
}

```

```
En este ejemplo, el puntero p que se pasa a la función actúa como this, permitiendo acceder a los atributos x e y de la instancia específica. No existe sintaxis mágica como en Java; la relación entre datos y funciones debe establecerse explícitamente.

Lo que ha cambiado es que no hay métodos asociados al struct, ni this implícito. Cada vez que se quiere operar sobre un "objeto", se debe pasar su dirección a la función, y la función actúa sobre esa dirección. Esto muestra que la orientación a objetos es principalmente una forma de organizar datos y comportamientos juntos, algo que en C se hace manualmente mediante convenciones y punteros.
```