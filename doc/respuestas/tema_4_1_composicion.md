# Tema 4.1. Composición

---

## 1. Composición en C con `struct`: línea de puntos

En C, la composición se expresa anidando `struct`s unos dentro de otros, siguiendo la relación "A tiene-un/tiene-varios B". En este caso, una `Linea` tiene dos `Punto`s, y cada `Punto` tiene dos coordenadas `x` e `y`.

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto inicio;
    Punto fin;
} Linea;

double distancia(Punto a, Punto b) {
    double dx = b.x - a.x;
    double dy = b.y - a.y;
    return sqrt(dx * dx + dy * dy);
}

double longitud(Linea l) {
    return distancia(l.inicio, l.fin);
}

int main() {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};
    Linea linea = {p1, p2};

    printf("Distancia entre puntos: %.2f\n", distancia(p1, p2));
    printf("Longitud de la linea: %.2f\n", longitud(linea));
    return 0;
}
```

La función `distancia` recibe dos `Punto`s y aplica el teorema de Pitágoras. La función `longitud` simplemente delega en `distancia` pasándole los dos extremos de la línea. Este enfoque es válido pero no garantiza ningún tipo de protección sobre los datos: cualquier parte del programa puede modificar directamente los campos `x`, `y`, `inicio` o `fin`.

---

## 2. Composición en Java: clases `Punto` y `Linea`

Al trasladar el ejemplo a Java con orientación a objetos, la composición se expresa haciendo que una clase contenga referencias a objetos de otra clase. La encapsulación permite declarar los campos como `private` y no ofrecer métodos `set`, lo que garantiza la inmutabilidad tanto de `Punto` como de `Linea`.

```java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

```java
public class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public Punto getInicio() { return inicio; }
    public Punto getFin()    { return fin; }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
```

El uso de `final` en los atributos impide que las referencias sean reasignadas tras la construcción del objeto. Dado que `Punto` tampoco expone métodos modificadores, el objeto es completamente inmutable. Esto supera a la versión en C, donde no había ningún mecanismo del lenguaje que protegiese los datos de modificaciones externas.

---

## 3. Multiplicidad en la composición

La **multiplicidad** indica cuántos objetos de un tipo pueden estar asociados a cuántos objetos de otro tipo en una relación. Se expresa habitualmente con notación `1`, `0..1`, `0..*`, `1..*`, o un número concreto como `2`.

En el ejemplo de `Linea` y `Punto`:

- **De `Linea` a `Punto`**: una `Linea` tiene exactamente **2** puntos (`2`). No puede tener más ni menos.
- **De `Punto` a `Linea`**: un `Punto` puede pertenecer a **0 o más** líneas (`0..*`), ya que un mismo punto podría ser extremo de ninguna, una o varias líneas distintas.

La multiplicidad es importante porque determina cómo se estructuran los atributos de la clase contenedora: si la multiplicidad es fija y pequeña (como `2`), se usan atributos individuales (`inicio`, `fin`); si es variable o potencialmente grande, se emplean colecciones.

---

## 4. Composición fuerte y composición débil

La **composición fuerte** (denominada habitualmente solo **"composición"**) es aquella en la que el objeto contenido no puede existir de forma independiente al contenedor: su ciclo de vida está completamente subordinado al del objeto que lo contiene. Cuando el contenedor desaparece, los objetos contenidos también desaparecen. Un ejemplo clásico es un `Pedido` que contiene `LineaDePedido`: si se elimina el pedido, sus líneas no tienen sentido por sí solas.

La **composición débil** (denominada habitualmente **"asociación" o "agregación"**) es aquella en la que los objetos contenidos tienen existencia propia e independiente del contenedor. Pueden pertenecer a varios contenedores a la vez, o seguir existiendo aunque el contenedor desaparezca. Un ejemplo sería un `Departamento` que agrupa `Profesor`es: si el departamento se disuelve, los profesores siguen existiendo.

La principal consecuencia práctica sobre el ciclo de vida es que, en la composición fuerte, los objetos contenidos son creados y destruidos por el propio contenedor, mientras que en la composición débil los objetos son creados externamente y simplemente "referenciados" por el contenedor.

---

## 5. Dependencia

Cuando una clase utiliza a otra únicamente como tipo de un parámetro en un método, como tipo de retorno, como variable local, o haciendo `new` dentro de un método puntualmente, no se habla de composición sino de **dependencia**. La diferencia fundamental es que en la dependencia no existe una relación de "posesión" o de ciclo de vida: el objeto dependiente no mantiene al otro como parte de su estado interno de forma duradera.

Por ejemplo, si `Linea` tuviese un método `trasladar(Punto desplazamiento)` que recibe un `Punto` como parámetro sin guardarlo como atributo, eso sería una dependencia. La dependencia es la relación más débil entre clases y simplemente refleja que una clase "conoce" o "usa" a otra de forma temporal y puntual.

---

## 6. `Linea` y `Punto`: composición fuerte vs. débil

### Composición fuerte

En la composición fuerte, `Linea` crea los `Punto`s internamente. Los puntos son parte inseparable de la línea y no existen fuera de ella.

```java
public class Linea {
    private final Punto inicio;
    private final Punto fin;

    // Linea crea sus propios Puntos: composición fuerte
    public Linea(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);
        this.fin    = new Punto(x2, y2);
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
```

```java
// Uso
Linea l = new Linea(0, 0, 3, 4);
```

### Composición débil (agregación)

En la composición débil, los `Punto`s son creados externamente y pasados a `Linea`. Pueden compartirse con otros objetos y su ciclo de vida no depende de `Linea`.

```java
public class Linea {
    private final Punto inicio;
    private final Punto fin;

    // Linea recibe Puntos externos: composición débil
    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin    = fin;
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
```

```java
// Uso
Punto p1 = new Punto(0, 0);
Punto p2 = new Punto(3, 4);
Linea l  = new Linea(p1, p2);
// p1 y p2 pueden usarse en otras líneas también
```

---

## 7. Destrucción de objetos en la composición fuerte en Java

En Java no existe destrucción explícita de objetos: no hay `delete` como en C++. La memoria es gestionada automáticamente por el **recolector de basura** (*garbage collector*). Cuando el objeto `Linea` deja de ser referenciado (por ejemplo, al salir de ámbito o al ser asignado a `null`), el recolector de basura detecta que los `Punto`s que contenía tampoco son accesibles desde ningún otro lugar (en la composición fuerte, nadie más los referencia) y los libera automáticamente.

Por tanto, la "destrucción" de los puntos no es algo que `Linea` deba hacer de forma explícita: simplemente, al desaparecer la única referencia que existía hacia ellos (la que tenía `Linea`), el recolector de basura se encarga de recuperar esa memoria en algún momento posterior. Esta es precisamente una de las diferencias fundamentales entre Java y C/C++: Java libera al programador de la gestión manual de la memoria.

---

## 8. `Departamento` con array de `Profesor`es y director

```java
public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() { return nombre; }

    @Override
    public String toString() { return nombre; }
}
```

```java
public class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(Profesor director) {
        this.profesores    = new Profesor[MAX_PROFESORES];
        this.numProfesores = 0;
        this.director      = director;
        añadirProfesor(director); // el director siempre está en la lista
    }

    public void añadirProfesor(Profesor p) {
        if (p == null)
            throw new IllegalArgumentException("El profesor no puede ser null.");
        if (numProfesores >= MAX_PROFESORES)
            throw new IllegalStateException("El departamento está lleno.");
        profesores[numProfesores++] = p;
    }

    public void eliminarProfesor(int pos) {
        validarPosicion(pos);
        if (profesores[pos] == director)
            throw new IllegalStateException(
                "No se puede eliminar al director. Cambia el director antes.");
        // Desplazar elementos
        for (int i = pos; i < numProfesores - 1; i++)
            profesores[i] = profesores[i + 1];
        profesores[--numProfesores] = null;
    }

    public int getNumProfesores() { return numProfesores; }

    public Profesor getProfesor(int pos) {
        validarPosicion(pos);
        return profesores[pos];
    }

    public Profesor getDirector() { return director; }

    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null)
            throw new IllegalArgumentException("El director no puede ser null.");
        if (!estaEnDepartamento(nuevoDirector))
            throw new IllegalArgumentException(
                "El nuevo director debe ser ya profesor del departamento.");
        this.director = nuevoDirector;
    }

    // --- Métodos privados auxiliares ---

    private void validarPosicion(int pos) {
        if (pos < 0 || pos >= numProfesores)
            throw new IndexOutOfBoundsException(
                "Posición inválida: " + pos);
    }

    private boolean estaEnDepartamento(Profesor p) {
        for (int i = 0; i < numProfesores; i++)
            if (profesores[i] == p) return true;
        return false;
    }
}
```

```java
// Main de ejemplo
public class Main {
    public static void main(String[] args) {
        Profesor ana   = new Profesor("Ana");
        Profesor luis  = new Profesor("Luis");
        Profesor marta = new Profesor("Marta");

        Departamento dept = new Departamento(ana); // Ana es directora y ya está en la lista
        dept.añadirProfesor(luis);
        dept.añadirProfesor(marta);

        System.out.println("Director: " + dept.getDirector());
        System.out.println("Profesores: " + dept.getNumProfesores());

        dept.setDirector(luis); // Cambiar director
        System.out.println("Nuevo director: " + dept.getDirector());

        dept.eliminarProfesor(0); // Elimina Ana (pos 0)
        System.out.println("Profesores tras eliminar: " + dept.getNumProfesores());
    }
}
```

El array es un detalle de implementación completamente oculto: el exterior solo sabe que puede añadir, eliminar y consultar profesores por posición y cantidad. Las invariantes (director siempre en la lista, director nunca null) se protegen lanzando excepciones.

---

## 9. Uso de `List` en lugar de arrays primitivos

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Departamento {

    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor director) {
        this.profesores = new ArrayList<>();
        this.director   = director;
        añadirProfesor(director);
    }

    public void añadirProfesor(Profesor p) {
        if (p == null)
            throw new IllegalArgumentException("El profesor no puede ser null.");
        profesores.add(p);
    }

    public void eliminarProfesor(int pos) {
        validarPosicion(pos);
        if (profesores.get(pos) == director)
            throw new IllegalStateException(
                "No se puede eliminar al director. Cambia el director antes.");
        profesores.remove(pos);
    }

    public int getNumProfesores() { return profesores.size(); }

    public Profesor getProfesor(int pos) {
        validarPosicion(pos);
        return profesores.get(pos);
    }

    public List<Profesor> getProfesores() {
        // Devolvemos una copia defensiva para no exponer la lista interna
        return Collections.unmodifiableList(profesores);
    }

    public Profesor getDirector() { return director; }

    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null)
            throw new IllegalArgumentException("El director no puede ser null.");
        if (!profesores.contains(nuevoDirector))
            throw new IllegalArgumentException(
                "El nuevo director debe ser ya profesor del departamento.");
        this.director = nuevoDirector;
    }

    private void validarPosicion(int pos) {
        if (pos < 0 || pos >= profesores.size())
            throw new IndexOutOfBoundsException("Posición inválida: " + pos);
    }
}
```

**Código ahorrado respecto a la versión con array:** se elimina por completo la gestión manual del tamaño máximo (`MAX_PROFESORES`), el desplazamiento de elementos al eliminar, la variable `numProfesores` y la inicialización de la posición liberada a `null`. `ArrayList` gestiona todo eso internamente.

**Problema de devolver la lista interna directamente:** si `getProfesores()` devolviera `profesores` tal cual, el código externo podría llamar a `lista.add(...)`, `lista.remove(...)` o `lista.clear()` y modificar el estado interno del departamento saltándose todas las validaciones. Esto rompería la encapsulación y podría violar las invariantes de clase. La solución más habitual es devolver `Collections.unmodifiableList(profesores)`, que crea una vista de solo lectura de la lista original: cualquier intento de modificarla lanza `UnsupportedOperationException`. Otra opción sería devolver `new ArrayList<>(profesores)`, una copia independiente, aunque en listas grandes tiene mayor coste de memoria.

---

## 10. Composición recursiva: `Persona` con madre

Una composición es **recursiva** cuando un objeto de un tipo contiene una referencia a otro objeto del mismo tipo. El caso de las excepciones encadenadas en Java (`getCause()` devuelve otra `Throwable`) es el ejemplo más conocido del propio lenguaje.

```java
public class Persona {
    private final String nombre;
    private final Persona madre; // puede ser null si no se conoce

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank())
            throw new IllegalArgumentException("El nombre no puede estar vacío.");
        this.nombre = nombre;
        this.madre  = madre;
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }

    @Override
    public String toString() {
        return nombre + (madre != null ? " (madre: " + madre.getNombre() + ")" : "");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Carmen", null);
        Persona madre  = new Persona("Lucía",  abuela);
        Persona hijo   = new Persona("Carlos", madre);

        System.out.println(hijo);
        System.out.println(hijo.getMadre());
        System.out.println(hijo.getMadre().getMadre());
    }
}
```

**Otros ejemplos clásicos de composición recursiva:**

- **Nodo de lista enlazada**: un `Nodo` contiene un valor y una referencia al siguiente `Nodo`.
- **Nodo de árbol binario**: un `Nodo` tiene referencias a su hijo izquierdo y derecho, ambos del mismo tipo `Nodo`.
- **Categoría con subcategorías**: una `Categoria` puede contener una lista de `Categoria`s hijas (patrón *Composite*).
- **Carpeta del sistema de archivos**: una `Carpeta` puede contener otras `Carpeta`s.
- **Expresión aritmética**: una `Expresion` puede estar formada por dos `Expresion`es y un operador.

---

## 11. Relaciones de composición bidireccionales

Una relación es **bidireccional** cuando ambas clases implicadas se conocen mutuamente: no solo A tiene una referencia a B, sino que B también tiene una referencia a A. En una relación unidireccional, solo uno de los dos extremos "conoce" al otro.

Para implementar la bidireccionalidad entre `Profesor` y `Departamento`, habría que añadir en la clase `Profesor` un atributo `departamento` de tipo `Departamento`, de modo que cada profesor sepa a qué departamento pertenece. El principal reto es mantener la **consistencia**: si se añade un profesor a un departamento, también hay que actualizar la referencia del profesor a su departamento, y viceversa al eliminarlo.

```java
public class Profesor {
    private final String nombre;
    private Departamento departamento; // referencia inversa

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() { return nombre; }

    public Departamento getDepartamento() { return departamento; }

    // Solo se llama desde Departamento para mantener consistencia
    void setDepartamento(Departamento d) {
        this.departamento = d;
    }
}
```

```java
// En Departamento, al añadir/eliminar un profesor, se actualiza la referencia inversa:
public void añadirProfesor(Profesor p) {
    if (p == null) throw new IllegalArgumentException("El profesor no puede ser null.");
    profesores.add(p);
    p.setDepartamento(this); // mantener consistencia
}

public void eliminarProfesor(int pos) {
    validarPosicion(pos);
    Profesor p = profesores.get(pos);
    if (p == director)
        throw new IllegalStateException("No se puede eliminar al director.");
    profesores.remove(pos);
    p.setDepartamento(null); // el profesor ya no pertenece al departamento
}
```

El método `setDepartamento` se declara con visibilidad de paquete (sin modificador de acceso) para que solo pueda ser llamado desde `Departamento` y no quede expuesto como parte de la API pública de `Profesor`. Las relaciones bidireccionales aumentan la complejidad del mantenimiento de la consistencia, por lo que deben usarse solo cuando realmente se necesita navegar la relación en ambas direcciones.