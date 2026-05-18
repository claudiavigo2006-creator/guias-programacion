
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.
```c
#include <stdio.h>
#include <math.h>

// Estructura Punto
typedef struct {
    float x;
    float y;
} Punto;

// Estructura Linea
// Una línea está compuesta por dos puntos
typedef struct {
    Punto inicio;
    Punto fin;
} Linea;

// Función para calcular la distancia entre dos puntos
float distancia(Punto p1, Punto p2) {
    return sqrt(
        pow(p2.x - p1.x, 2) +
        pow(p2.y - p1.y, 2)
    );
}

// Función para calcular la longitud de una línea
float longitudLinea(Linea l) {
    return distancia(l.inicio, l.fin);
}

int main() {

    Punto a = {1.0, 2.0};
    Punto b = {4.0, 6.0};

    // Crear línea con dos puntos
    Linea linea1 = {a, b};

    printf("Distancia entre puntos: %.2f\n", distancia(a, b));
    printf("Longitud de la linea: %.2f\n", longitudLinea(linea1));

    return 0;
}
```

### Explicación
- `Punto` representa un punto con coordenadas `x` e `y`.
- `Linea` representa una composición, porque una línea tiene dos puntos (`inicio` y `fin`).
- La función `distancia()` calcula la distancia entre dos puntos.
- La función `longitudLinea()` reutiliza la función anterior para obtener la longitud de la línea.



## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  
```java
// Clase Punto
public class Punto {

    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método para calcular distancia a otro punto
    public double distancia(Punto otro) {
        return Math.sqrt(
            Math.pow(otro.x - this.x, 2) +
            Math.pow(otro.y - this.y, 2)
        );
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }
}
```

```java
// Clase Linea
public class Linea {

    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    // Método para calcular la longitud de la línea
    public double longitud() {
        return inicio.distancia(fin);
    }

    public Punto getInicio() {
        return inicio;
    }

    public Punto getFin() {
        return fin;
    }
}
```

```java
// Main de prueba
public class Main {

    public static void main(String[] args) {

        Punto p1 = new Punto(1, 2);
        Punto p2 = new Punto(4, 6);

        Linea linea = new Linea(p1, p2);

        System.out.println("Distancia entre puntos: " + p1.distancia(p2));
        System.out.println("Longitud de la linea: " + linea.longitud());
    }
}
```

### Explicación
- `Linea` está compuesta por dos objetos `Punto`.
- Los atributos son `private` para ocultar la información.
- Los atributos también son `final`, haciendo que los objetos sean inmutables.
- Una vez creado un `Punto`, sus coordenadas no pueden cambiar.
- Una vez creada una `Linea`, no se pueden cambiar los puntos que la forman.
- El método `distancia()` calcula la distancia entre dos puntos.
- El método `longitud()` reutiliza el método anterior para calcular la longitud de la línea.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.
La multiplicidad en una composición indica cuántos objetos de una clase pueden relacionarse con objetos de otra clase.

En el ejemplo anterior, una Linea está formada por dos objetos Punto, por lo que la multiplicidad de Linea a Punto es 2, ya que cada línea tiene exactamente dos puntos.

En cambio, un Punto puede pertenecer a ninguna, una o muchas líneas diferentes. Por eso, la multiplicidad de Punto a Linea es 0..* (“cero o muchas”).


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La composición fuerte ocurre cuando el objeto contenido depende totalmente del objeto que lo contiene. Esto significa que su ciclo de vida está ligado al del objeto principal: si el objeto principal desaparece, los objetos que contiene también desaparecen. A este tipo de relación se le suele llamar simplemente composición.

Por ejemplo, si una Casa contiene Habitaciones, normalmente las habitaciones no tienen sentido sin la casa.

La composición débil ocurre cuando los objetos contenidos pueden existir independientemente del objeto que los relaciona. Aunque estén asociados, cada objeto tiene su propio ciclo de vida. A esta relación se le suele llamar asociación o agregación.

Por ejemplo, un Departamento puede tener Profesores, pero un profesor puede seguir existiendo aunque el departamento desaparezca o cambie.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase usa otra únicamente como parámetro de métodos, valor de retorno, variable local o creando objetos con `new` dentro de un método, no hablamos de composición, sino de **dependencia**.

La dependencia significa que una clase necesita usar otra temporalmente para realizar alguna operación, pero no la almacena como parte permanente de su estado.

Por ejemplo:

```java
public class Calculadora {

    public void imprimir(Punto p) {
        System.out.println(p.getX());
    }
}
```

Aquí `Calculadora` depende de `Punto`, porque lo utiliza en un método, pero no contiene objetos `Punto` como atributos.

La diferencia principal es:

* **Composición** → la clase tiene objetos de otra clase como atributos.
* **Dependencia** → la clase solo usa temporalmente objetos de otra clase.



## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

Composición fuerte

En una composición fuerte, los puntos dependen totalmente de la línea. La propia clase Linea crea los objetos Punto, por lo que si la línea desaparece, los puntos también.

public class Punto {

    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        return Math.sqrt(
            Math.pow(otro.x - this.x, 2) +
            Math.pow(otro.y - this.y, 2)
        );
    }
}
public class Linea {

    private final Punto inicio;
    private final Punto fin;

    public Linea(double x1, double y1,
                 double x2, double y2) {

        // Los puntos se crean dentro de la línea
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
Composición débil (agregación)

En una composición débil, los puntos existen independientemente de la línea. Primero se crean los puntos y después se pasan a Linea.

public class Punto {

    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        return Math.sqrt(
            Math.pow(otro.x - this.x, 2) +
            Math.pow(otro.y - this.y, 2)
        );
    }
}
public class Linea {

    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
public class Main {

    public static void main(String[] args) {

        Punto p1 = new Punto(1, 2);
        Punto p2 = new Punto(4, 6);

        Linea linea = new Linea(p1, p2);

        System.out.println(linea.longitud());
    }
}

En este caso, aunque la Linea desaparezca, los objetos Punto siguen existiendo.

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java no destruimos objetos manualmente como ocurre en otros lenguajes como C++. La memoria se libera automáticamente mediante el **Garbage Collector (GC)**.

En la composición fuerte, los objetos contenidos suelen dejar de existir cuando desaparece el objeto contenedor porque ya no quedan referencias hacia ellos.

Por ejemplo:

```java id="16mf1r"
Linea l = new Linea(1, 2, 4, 6);
```

La línea crea internamente sus puntos:

```java id="hh5kdm"
this.inicio = new Punto(x1, y1);
this.fin = new Punto(x2, y2);
```

Esos puntos solo son accesibles desde `Linea`. Si el objeto `Linea` deja de tener referencias:

```java id="r2w87m"
l = null;
```

entonces también se pierden las únicas referencias a los `Punto`. En ese momento, el recolector de basura detecta que esos objetos ya no pueden usarse y libera su memoria automáticamente.

Por eso no vemos un `destroy()` ni un `delete` explícito: en Java la destrucción de objetos la realiza automáticamente el Garbage Collector cuando los objetos quedan inaccesibles.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

Ejemplo de composición débil: Departamento y Profesor

En este ejemplo, los profesores existen independientemente del departamento. Por eso es una composición débil o agregación.

public class Profesor {

    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede estar vacío");
        }

        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    @Override
    public String toString() {
        return nombre;
    }
}
public class Departamento {

    private static final int MAX_PROFESORES = 50;

    private final String nombre;
    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(String nombre, Profesor director) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede estar vacío");
        }

        if (director == null) {
            throw new IllegalArgumentException("Debe haber un director desde el inicio");
        }

        this.nombre = nombre;
        this.profesores = new Profesor[MAX_PROFESORES];
        this.numProfesores = 0;

        añadirProfesor(director);
        this.director = director;
    }

    public void añadirProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("El profesor no puede ser null");
        }

        if (numProfesores == MAX_PROFESORES) {
            throw new IllegalStateException("No caben más profesores");
        }

        profesores[numProfesores] = profesor;
        numProfesores++;
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición no válida");
        }

        if (profesores[posicion] == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }

        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }

        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición no válida");
        }

        return profesores[posicion];
    }

    public Profesor getDirector() {
        return director;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }

        if (!contieneProfesor(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento");
        }

        this.director = nuevoDirector;
    }

    private boolean contieneProfesor(Profesor profesor) {
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == profesor) {
                return true;
            }
        }

        return false;
    }
}
public class Main {

    public static void main(String[] args) {

        Profesor p1 = new Profesor("Ana");
        Profesor p2 = new Profesor("Luis");
        Profesor p3 = new Profesor("Marta");

        Departamento dep = new Departamento("Informática", p1);

        dep.añadirProfesor(p2);
        dep.añadirProfesor(p3);

        System.out.println("Director inicial: " + dep.getDirector());

        dep.cambiarDirector(p2);

        System.out.println("Nuevo director: " + dep.getDirector());

        dep.eliminarProfesor(2);

        System.out.println("Número de profesores: " + dep.getNumProfesores());
    }
}
Explicación

Departamento tiene una lista de profesores y también tiene un director. Ambas relaciones son de composición débil, porque los profesores no son creados dentro del departamento, sino que se crean fuera y luego se añaden.

La invariante principal es que el director siempre debe formar parte de la lista de profesores. Por eso:

El constructor obliga a recibir un director desde el principio.
Ese director se añade automáticamente a la lista.
No se puede cambiar el director por un profesor que no esté en el departamento.
No se puede eliminar al profesor que actualmente es director.

Aunque internamente se usa un array Profesor[], no se devuelve directamente el array, para no romper la encapsulación. En su lugar, se usan métodos como getNumProfesores() y getProfesor(posicion).

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

Ejemplo usando List
import java.util.ArrayList;
import java.util.List;

public class Departamento {

    private final String nombre;
    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(String nombre, Profesor director) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede estar vacío");
        }

        if (director == null) {
            throw new IllegalArgumentException("Debe haber un director desde el inicio");
        }

        this.nombre = nombre;
        this.profesores = new ArrayList<>();

        this.profesores.add(director);
        this.director = director;
    }

    public void añadirProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("El profesor no puede ser null");
        }

        profesores.add(profesor);
    }

    public void eliminarProfesor(int posicion) {
        Profesor profesor = profesores.get(posicion);

        if (profesor == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }

        profesores.remove(posicion);
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int posicion) {
        return profesores.get(posicion);
    }

    public Profesor getDirector() {
        return director;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }

        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento");
        }

        this.director = nuevoDirector;
    }

    public List<Profesor> getProfesores() {
        return List.copyOf(profesores);
    }
}

La clase Profesor sería igual que antes.

public class Profesor {

    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede estar vacío");
        }

        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    @Override
    public String toString() {
        return nombre;
    }
}
¿Qué parte del código nos ahorramos?

Con List nos ahorramos gestionar manualmente el array:

No necesitamos MAX_PROFESORES.
No necesitamos numProfesores.
No necesitamos desplazar elementos al eliminar.
No necesitamos comprobar si el array está lleno.
Podemos usar directamente add(), remove(), get(), size() y contains().
Problema de devolver la lista interna

Si hacemos esto:

public List<Profesor> getProfesores() {
    return profesores;
}

estaríamos rompiendo la encapsulación, porque desde fuera podrían modificar la lista interna:

dep.getProfesores().clear();

Eso podría dejar el departamento sin profesores y sin director válido.

La solución es devolver una copia no modificable:

public List<Profesor> getProfesores() {
    return List.copyOf(profesores);
}

Así quien recibe la lista puede consultarla, pero no puede modificar la lista interna del departamento.


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

Una composición recursiva ocurre cuando una clase contiene objetos de su misma clase. En este caso, una Persona tiene una madre, que también es una Persona.

public class Persona {

    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {

        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede estar vacío");
        }

        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }

    @Override
    public String toString() {
        return nombre;
    }
}
public class Main {

    public static void main(String[] args) {

        // Abuela
        Persona abuela = new Persona("Carmen", null);

        // Madre
        Persona madre = new Persona("Laura", abuela);

        // Nieto
        Persona hijo = new Persona("Pablo", madre);

        System.out.println("Nieto: " + hijo.getNombre());
        System.out.println("Madre: " + hijo.getMadre().getNombre());
        System.out.println("Abuela: " +
                hijo.getMadre().getMadre().getNombre());
    }
}

La clase es inmutable porque:

Los atributos son private final.
No existen setters.
Una vez creada la persona, no puede cambiar ni su nombre ni su madre.

Otros ejemplos clásicos de composiciones recursivas son:

Directorios que contienen otros directorios.
Árboles binarios, donde cada nodo tiene otros nodos hijos.
Excepciones en Java con causas (Throwable cause).
Empleados que tienen un supervisor, que también es un empleado.
Comentarios de redes sociales con respuestas anidadas.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?
Las relaciones de composición bidireccionales son aquellas en las que ambas clases se conocen mutuamente.

En una relación normal entre Departamento y Profesor, el departamento conoce a sus profesores porque guarda una lista de ellos. Sin embargo, los profesores no saben a qué departamento pertenecen.

Para que la relación sea bidireccional, también habría que guardar una referencia al Departamento dentro de Profesor.

Por ejemplo:

public class Profesor {

    private final String nombre;
    private Departamento departamento;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public Departamento getDepartamento() {
        return departamento;
    }

    public void setDepartamento(Departamento departamento) {
        this.departamento = departamento;
    }
}

Y al añadir un profesor al departamento, habría que actualizar ambas partes de la relación:

public void añadirProfesor(Profesor profesor) {

    profesores.add(profesor);

    profesor.setDepartamento(this);
}

De esta forma:

El Departamento conoce a sus profesores.
Cada Profesor conoce el departamento al que pertenece.

El principal problema de las relaciones bidireccionales es mantener la consistencia. Por ejemplo, si eliminamos un profesor del departamento, también habría que poner su departamento a null para que ambas partes sigan sincronizadas.

