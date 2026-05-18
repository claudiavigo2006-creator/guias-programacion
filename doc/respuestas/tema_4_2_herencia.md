
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.
La herencia en orientación a objetos representa una relación de tipo “A es-un B”. Esto significa que una clase más específica hereda características de otra más general.

Por ejemplo:

* Un `Artillero` es-un `Soldado`.
* Un `Zapador` es-un `Soldado`.

La herencia tiene dos implicaciones principales:

1. Compatibilidad de tipos
   Un objeto de una subclase puede utilizarse como si fuese de la superclase. Por eso, un `Artillero` y un `Zapador` pueden almacenarse en un array de `Soldado`.

2. Herencia de estado y comportamiento
   Las subclases heredan atributos y métodos de la superclase. Así, `Artillero` y `Zapador` heredan el atributo `nombre` y el método `saludar()` de `Soldado`.

```java id="8qvbmu"
public class Soldado {

    private final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}
```

```java id="jlwm53"
public class Artillero extends Soldado {

    private final int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }

    public int getNumCohetes() {
        return numCohetes;
    }

    public void dispararCohete() {
        System.out.println(getNombre() + " dispara un cohete");
    }
}
```

```java id="r2ce5x"
public class Zapador extends Soldado {

    private final int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }

    public void ponerMina() {
        System.out.println(getNombre() + " coloca una mina");
    }
}
```

```java id="k63uc0"
public class Main {

    public static void main(String[] args) {

        Soldado[] ejercito = new Soldado[4];

        ejercito[0] = new Artillero("Luis", 5);
        ejercito[1] = new Zapador("Ana", 3);
        ejercito[2] = new Artillero("Carlos", 8);
        ejercito[3] = new Zapador("Marta", 2);

        for (int i = 0; i < ejercito.length; i++) {
            ejercito[i].saludar();
        }
    }
}
```

En el array aparecen objetos de distintos tipos, pero todos pueden tratarse como `Soldado` gracias a la compatibilidad de tipos producida por la herencia.



## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Al crear un objeto de una subclase, se ejecutan los constructores de toda la jerarquía de herencia, desde la clase más general hasta la más específica.

Por ejemplo:

```java id="w9b9d2"
Artillero a = new Artillero("Luis", 5);
```

primero se ejecuta el constructor de `Soldado` y después el de `Artillero`.

El orden es:

1. Constructor de la superclase (`Soldado`)
2. Constructor de la subclase (`Artillero`)

Esto ocurre porque antes de construir la parte específica del objeto, Java necesita construir primero la parte heredada.

La palabra `super` dentro de un constructor sirve para llamar al constructor de la clase padre.

Por ejemplo:

```java id="1g2zto"
public Artillero(String nombre, int numCohetes) {
    super(nombre);
    this.numCohetes = numCohetes;
}
```

Aquí:

```java
super(nombre);
```

llama al constructor de `Soldado` para inicializar el atributo `nombre`.

Si no escribimos ningún `super(...)`, Java intenta llamar automáticamente al constructor sin parámetros de la superclase:

```java
super();
```

Pero si la clase base no tiene un constructor vacío visible, entonces estamos obligados a llamar explícitamente a `super(...)` con los parámetros adecuados.

Por ejemplo, en `Soldado`:

```java id="mngj5z"
public Soldado(String nombre) {
    this.nombre = nombre;
}
```

como no existe un constructor vacío, `Artillero` debe llamar obligatoriamente a:

```java
super(nombre);
```

Si no lo hacemos, el código no compila.


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Sí. Los atributos privados de la superclase forman parte de los objetos de la subclase en memoria, porque un objeto de la subclase contiene también toda la parte heredada de la superclase.

Por ejemplo, un objeto `Artillero` contiene:

* la parte correspondiente a `Soldado` (como el atributo `nombre`)
* y su propia parte específica (como `numCohetes`)

Aunque el atributo `nombre` exista físicamente dentro del objeto `Artillero`, al ser `private` no puede accederse directamente desde el código de la subclase.

Por ejemplo:

```java id="f7pv9g"
public class Soldado {

    private final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java id="m9wp79"
public class Artillero extends Soldado {

    private final int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }

    public void disparar() {

        // ERROR: nombre es private en Soldado
        // System.out.println(nombre);

        // Correcto:
        System.out.println(getNombre() + " dispara un cohete");
    }
}
```

Aquí `nombre` sí existe dentro del objeto `Artillero`, porque fue heredado de `Soldado`, pero la subclase no puede acceder directamente a él al ser privado.

La única forma de usarlo es mediante métodos públicos o protegidos de la superclase, como `getNombre()`.


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.
La compatibilidad de tipos permite que el código sea fácilmente extensible. Esto significa que podemos crear nuevas subclases sin tener que modificar el código que ya trabaja con la superclase.

Por ejemplo, podemos añadir un nuevo tipo de soldado llamado `Medico`:

```java id="b9p0ru"
public class Medico extends Soldado {

    private final int numBotiquines;

    public Medico(String nombre, int numBotiquines) {
        super(nombre);
        this.numBotiquines = numBotiquines;
    }

    public int getNumBotiquines() {
        return numBotiquines;
    }

    public void curar() {
        System.out.println(getNombre() + " cura a un compañero");
    }
}
```

Ahora podemos meter objetos `Medico` en el mismo array de `Soldado`:

```java id="a61cgb"
public class Main {

    public static void main(String[] args) {

        Soldado[] ejercito = new Soldado[5];

        ejercito[0] = new Artillero("Luis", 5);
        ejercito[1] = new Zapador("Ana", 3);
        ejercito[2] = new Artillero("Carlos", 8);
        ejercito[3] = new Zapador("Marta", 2);

        // Nuevo tipo de soldado
        ejercito[4] = new Medico("Pedro", 4);

        // Este código no cambia
        for (int i = 0; i < ejercito.length; i++) {
            ejercito[i].saludar();
        }
    }
}
```

El bucle que pide saludar a todos los soldados no necesita modificarse, porque todos los subtipos son compatibles con `Soldado` y heredan el método `saludar()`.

Esto hace que el código sea más reutilizable y extensible, ya que podemos añadir nuevos tipos sin reescribir el código existente.



## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Sí. En Java puedes tener una referencia del supertipo apuntando a un objeto real de un subtipo:

```java
Soldado s = new Artillero("Luis", 5);
```

Eso es válido porque un `Artillero` **es un** `Soldado`.

Pero con una referencia de tipo `Soldado` solo puedes llamar directamente a los métodos definidos en `Soldado`, como `saludar()` o `getNombre()`. No puedes llamar directamente a métodos específicos de `Artillero`, como `getNumCohetes()`.

```java
s.saludar();          // Correcto
// s.getNumCohetes(); // Error, porque s es de tipo Soldado
```

El **upcasting** consiste en convertir un subtipo a un supertipo. Es automático y seguro:

```java
Soldado s = new Artillero("Luis", 5);
```

El **downcasting** consiste en convertir una referencia del supertipo a una referencia del subtipo. No siempre es seguro, por eso conviene comprobar antes con `instanceof`.

```java
Artillero a = (Artillero) s;
```

`instanceof` sirve para comprobar si el objeto real al que apunta una referencia pertenece a una clase concreta.

Ejemplo completo:

```java
public class Main {

    public static void main(String[] args) {

        Soldado[] ejercito = new Soldado[4];

        ejercito[0] = new Artillero("Luis", 5);
        ejercito[1] = new Zapador("Ana", 3);
        ejercito[2] = new Artillero("Carlos", 8);
        ejercito[3] = new Zapador("Marta", 2);

        for (int i = 0; i < ejercito.length; i++) {

            ejercito[i].saludar();

            if (ejercito[i] instanceof Artillero) {
                Artillero artillero = (Artillero) ejercito[i];

                System.out.println(
                    "Número de cohetes: " + artillero.getNumCohetes()
                );
            }
        }
    }
}
```

En resumen: puedes guardar `Artillero` y `Zapador` en referencias de tipo `Soldado`, pero para usar métodos específicos del subtipo necesitas hacer **downcasting**, normalmente comprobando antes con `instanceof`.



## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso **protegido** (`protected`) significa que un atributo o método:

* puede usarse dentro de la propia clase,
* puede usarse desde las subclases,
* y también desde clases del mismo paquete.

Se utiliza cuando queremos permitir que las clases hijas accedan directamente a ciertos elementos heredados, pero sin hacerlos completamente públicos.

En Java se implementa usando la palabra clave:

```java
protected
```

Ejemplo con `Soldado` y `Zapador`:

```java id="kdf9cf"
public class Soldado {

    // Ahora el atributo es protegido
    protected final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}
```

```java id="c7aj9n"
public class Zapador extends Soldado {

    private final int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }

    public void ponerMina() {

        // Puede acceder directamente a nombre
        // porque es protected
        System.out.println(nombre + " coloca una mina");
    }
}
```

Gracias a `protected`, `Zapador` puede acceder directamente al atributo `nombre` heredado de `Soldado`, algo que no sería posible si fuese `private`.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En muchos lenguajes orientados a objetos existe una clase base común de la que heredan todos los objetos, aunque no ocurre necesariamente en todos los lenguajes.

En Java sí ocurre. Todas las clases heredan, directa o indirectamente, de la clase:

```java
Object
```

Aunque no lo escribamos explícitamente, Java añade automáticamente:

```java
extends Object
```

Por ejemplo:

```java
public class Soldado {
}
```

realmente es equivalente a:

```java
public class Soldado extends Object {
}
```

Gracias a esto, todos los objetos en Java comparten ciertos métodos heredados de `Object`, como:

* `toString()`
* `equals()`
* `hashCode()`
* `getClass()`

Por ejemplo:

```java
Soldado s = new Soldado();

System.out.println(s.toString());
```

funciona porque `toString()` está definido en `Object`.

No todos los lenguajes orientados a objetos funcionan exactamente igual. Algunos tienen jerarquías distintas o permiten tipos que no heredan de una única clase base común. Pero en Java toda clase es siempre un subtipo de `Object`.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?


La herencia múltiple consiste en que una clase pueda heredar simultáneamente de varias clases distintas.

Por ejemplo, imaginemos:

```text
Clase C hereda de A y de B al mismo tiempo
```

Eso permitiría reutilizar atributos y métodos de varias superclases a la vez.

El problema es que puede generar conflictos y ambigüedades. Por ejemplo, si las dos superclases tienen un método con el mismo nombre, no estaría claro cuál debe heredarse.

Por ese motivo, Java no permite herencia múltiple de clases.

Esto no es válido en Java:

```java
// ERROR
public class C extends A, B {
}
```

En Java una clase solo puede heredar de una única clase.

Sin embargo, Java sí permite implementar múltiples interfaces:

```java
public class C implements A, B {
}
```

Por eso suele decirse que Java no tiene herencia múltiple de clases, pero sí admite una forma limitada de herencia múltiple mediante interfaces.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

Una excepción no controlada en Java es aquella que hereda de `RuntimeException`. No obliga a usar `try-catch` ni `throws`.

En este ejemplo, la excepción `UsuarioNoEncontradoException`:

* hereda de `RuntimeException`,
* contiene un objeto `Usuario`,
* y además permite guardar una causa subyacente mediante otro constructor.

```java id="m5cx9z"
public class Usuario {

    private final String nombre;

    public Usuario(String nombre) {
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
```

```java id="o0b73f"
public class UsuarioNoEncontradoException
        extends RuntimeException {

    private final Usuario usuario;

    // Constructor sin causa
    public UsuarioNoEncontradoException(Usuario usuario) {

        super("Usuario no encontrado: " + usuario);

        this.usuario = usuario;
    }

    // Constructor con causa
    public UsuarioNoEncontradoException(
            Usuario usuario,
            Throwable causa) {

        super("Usuario no encontrado: " + usuario,
              causa);

        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

```java id="xjlwm9"
public class Main {

    public static void main(String[] args) {

        Usuario u = new Usuario("Claudia");

        try {

            // Simular causa original
            Exception causa =
                    new Exception("Error en base de datos");

            throw new UsuarioNoEncontradoException(u,
                                                   causa);

        } catch (UsuarioNoEncontradoException e) {

            System.out.println(e.getMessage());

            System.out.println(
                    "Usuario problemático: "
                    + e.getUsuario().getNombre()
            );

            System.out.println(
                    "Causa original: "
                    + e.getCause().getMessage()
            );
        }
    }
}
```

Aquí la excepción está compuesta con un `Usuario`, porque guarda internamente el objeto que provocó el problema. Además, usando `Throwable cause`, puede encadenar excepciones y conservar el error original.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

No se recomienda usar herencia únicamente para reutilizar código porque la herencia crea una relación muy fuerte entre clases. Cuando una clase hereda de otra, no solo reutiliza código: también establece una relación conceptual de tipo “A es-un B”.

Por ejemplo:

* Un `Artillero` sí es un `Soldado`, por lo que la herencia tiene sentido.
* Pero si una clase solo necesita usar ciertas funcionalidades de otra, quizá no exista realmente esa relación “es-un”.

El problema de usar herencia solo para reutilizar código es que:

* aumenta el acoplamiento entre clases,
* hace el diseño más rígido,
* dificulta cambios futuros,
* y puede provocar jerarquías artificiales o incorrectas.

Además, la subclase hereda todo el comportamiento y estado de la superclase, incluso partes que quizá no necesita.

Por eso normalmente se prefiere la composición para reutilizar código. Con composición una clase contiene otra y reutiliza sus funcionalidades sin convertirse en un subtipo suyo.

Por ejemplo, en lugar de:

```java
class Coche extends Motor
```

que conceptualmente es incorrecto (“un coche no es un motor”),

es mejor:

```java
class Coche {
    private Motor motor;
}
```

porque:

* un coche tiene-un motor,
* no es-un motor.

La composición suele ser más flexible porque permite cambiar componentes internos sin modificar la jerarquía de herencia y reduce la dependencia entre clases.

Por eso existe la recomendación clásica:

> “Favor composition over inheritance”
> (“Preferir composición antes que herencia”).


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

Se dice que se debe favorecer la composición frente a la herencia porque la composición produce diseños más flexibles, reutilizables y menos acoplados.

La herencia crea una relación muy fuerte entre clases:

* la subclase depende internamente de la superclase,
* hereda todo su comportamiento,
* y cualquier cambio en la superclase puede afectar a todas las subclases.

Además, la herencia solo debería usarse cuando realmente exista una relación conceptual “A es-un B”.

En cambio, la composición se basa en relaciones “A tiene-un B”, donde una clase reutiliza otra sin convertirse en un subtipo suyo.

Por ejemplo:

```java id="q9dr4y"
class Coche {
    private Motor motor;
}
```

es mejor diseño que:

```java id="xh7mho"
class Coche extends Motor
```

porque un coche tiene un motor, pero no es un motor.

La composición tiene varias ventajas:

* menor acoplamiento,
* mayor encapsulación,
* más facilidad para cambiar implementaciones,
* más reutilización,
* y evita jerarquías de herencia artificiales o demasiado rígidas.

Además, con composición podemos cambiar componentes en tiempo de ejecución o combinar comportamientos de forma más flexible.

Por eso, la herencia se suele reservar para relaciones claras de especialización (“es-un”), mientras que la composición se utiliza como mecanismo general de reutilización y diseño.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?
Cuando se dice que la herencia “rompe la encapsulación” se refiere a que las subclases dependen de detalles internos de implementación de la superclase.

La encapsulación busca ocultar cómo funciona internamente una clase y mostrar solo una interfaz pública. Sin embargo, con la herencia, la subclase muchas veces necesita conocer cómo está implementada la clase padre para poder extenderla correctamente.

Por ejemplo, una subclase:

* hereda métodos y atributos,
* puede depender de métodos `protected`,
* puede sobrescribir métodos,
* y puede verse afectada por cambios internos de la superclase.

Eso provoca un acoplamiento fuerte entre ambas clases.

Imagina:

```java id="j1jewr"
class Lista {
    protected int tamaño;
}
```

y una subclase:

```java id="vlt68k"
class MiLista extends Lista {

    public void mostrar() {
        System.out.println(tamaño);
    }
}
```

La subclase depende directamente de cómo la superclase almacena su estado interno (`tamaño`). Si la implementación interna cambia, la subclase puede romperse.

Con composición esto no ocurre tanto, porque la clase usuaria solo conoce la interfaz pública del objeto contenido, no sus detalles internos.

Por eso se dice que la herencia debilita o rompe parcialmente la encapsulación: las subclases quedan muy ligadas a la implementación interna de la superclase y no solo a su interfaz pública.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Opción 1: reutilización mediante herencia

Aquí `Estudiante` y `Trabajador` heredan de `Persona`, porque ambos comparten nombre y DNI.

```java id="nuk28d"
public class Persona {

    private final String dni;
    private final String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java id="snaj8r"
public class Estudiante extends Persona {

    private final String carrera;

    public Estudiante(String dni,
                      String nombre,
                      String carrera) {

        super(dni, nombre);

        this.carrera = carrera;
    }

    public String getCarrera() {
        return carrera;
    }
}
```

```java id="cngn4x"
public class Trabajador extends Persona {

    private final String empresa;

    public Trabajador(String dni,
                      String nombre,
                      String empresa) {

        super(dni, nombre);

        this.empresa = empresa;
    }

    public String getEmpresa() {
        return empresa;
    }
}
```

---

### Opción 2: reutilización mediante composición

Aquí los datos comunes se encapsulan en una clase `DatosPersonales` y luego `Estudiante` y `Trabajador` contienen un objeto de ese tipo.

```java id="jlwm6s"
public class DatosPersonales {

    private final String dni;
    private final String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java id="mnn73x"
public class Estudiante {

    private final DatosPersonales datos;
    private final String carrera;

    public Estudiante(DatosPersonales datos,
                      String carrera) {

        this.datos = datos;
        this.carrera = carrera;
    }

    public DatosPersonales getDatos() {
        return datos;
    }

    public String getCarrera() {
        return carrera;
    }
}
```

```java id="2mjlwm"
public class Trabajador {

    private final DatosPersonales datos;
    private final String empresa;

    public Trabajador(DatosPersonales datos,
                      String empresa) {

        this.datos = datos;
        this.empresa = empresa;
    }

    public DatosPersonales getDatos() {
        return datos;
    }

    public String getEmpresa() {
        return empresa;
    }
}
```

En la versión con herencia, `Estudiante` y `Trabajador` son subtipos de `Persona`.

En la versión con composición, `Estudiante` y `Trabajador` no son `DatosPersonales`, sino que tienen unos `DatosPersonales`.
