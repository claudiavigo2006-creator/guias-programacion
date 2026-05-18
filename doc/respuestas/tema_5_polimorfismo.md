
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo permite tratar objetos de distintos tipos mediante referencias de un tipo común, haciendo que cada objeto responda de forma diferente al mismo método.

Sirve para escribir código más flexible y extensible.

La sobreescritura ocurre cuando una subclase redefine un método heredado para darle un comportamiento distinto.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.


La ligadura dinámica consiste en decidir en tiempo de ejecución qué versión real de un método debe ejecutarse.

Está directamente relacionada con el polimorfismo, porque aunque la referencia sea del supertipo, Java ejecuta el método del objeto real.

Comparación
Java: el polimorfismo es automático para métodos no final, static ni private.
C++: hay que indicar explícitamente virtual.
Python: todo es dinámico y polimórfico por defecto.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

public class Soldado {

    protected final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}
public class Artillero extends Soldado {

    public Artillero(String nombre) {
        super(nombre);
    }
}
public class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void saludar() {
        System.out.println("Soy el zapador " + nombre);
    }
}
public class Main {

    public static void main(String[] args) {

        Soldado[] ejercito = {
            new Artillero("Luis"),
            new Zapador("Ana")
        };

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Sí. Puede hacerse usando super.

public class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void saludar() {

        super.saludar();

        System.out.println("ZAPADOR A SUS ORDENES");
    }
}

La palabra clave usada es:

super

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al sobreescribir:

los parámetros deben ser exactamente iguales,
el retorno debe ser igual o compatible.
Diferencia
Overriding → redefinir un método heredado.
Overloading → varios métodos con el mismo nombre pero distintos parámetros.
@Override

Sirve para indicar que realmente queremos sobreescribir un método.

Es recomendable porque el compilador detecta errores automáticamente

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Sí. En Java se empieza a usar polimorfismo prácticamente desde el principio, muchas veces incluso sin darse cuenta.

Cuando sobreescribes métodos como:

* `toString()`
* `equals()`
* `hashCode()`

ya estás usando polimorfismo.

Por ejemplo:

```java id="n0d7f0"
public class Persona {

    private final String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    @Override
    public String toString() {
        return "Persona: " + nombre;
    }
}
```

```java id="l4e2na"
Persona p = new Persona("Ana");

System.out.println(p);
```

aunque escribimos:

```java id="7pbo8x"
System.out.println(p);
```

internamente Java ejecuta:

```java id="q3kt80"
p.toString()
```

y gracias al polimorfismo se llama a la versión concreta redefinida en `Persona`, no a la de `Object`.

Lo mismo ocurre con `equals()`:

```java id="z4k5l0"
@Override
public boolean equals(Object o) {
    ...
}
```

Cuando Java compara objetos, se ejecuta dinámicamente la versión específica del objeto real.

Por tanto, sí: sobreescribir métodos heredados ya es usar polimorfismo y ligadura dinámica, incluso aunque todavía no se estén usando arrays de supertipos o clases abstractas.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una clase abstracta es una clase pensada para servir de base a otras clases, pero que no puede instanciarse directamente.

Se utiliza cuando queremos definir comportamiento común para varias subclases, pero dejando algunos métodos sin implementar para que cada subtipo los defina a su manera.

Un método abstracto es un método que no tiene implementación. Solo se declara, y obliga a las subclases a implementarlo.

No se pueden crear objetos de una clase abstracta.

Por ejemplo, esto daría error:

```java id="g61g42"
Soldado s = new Soldado("Luis");
```

porque `Soldado` será abstracta.

Ejemplo:

```java id="r8g1nk"
public abstract class Soldado {

    protected final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }

    // Método abstracto
    public abstract void atacar();
}
```

```java id="skn40i"
public class Artillero extends Soldado {

    public Artillero(String nombre) {
        super(nombre);
    }

    @Override
    public void atacar() {
        System.out.println(nombre + " dispara cohetes");
    }
}
```

```java id="k7q4j7"
public class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void atacar() {
        System.out.println(nombre + " coloca minas");
    }
}
```

```java id="f6f8qf"
public class Main {

    public static void main(String[] args) {

        Soldado[] ejercito = {
            new Artillero("Luis"),
            new Zapador("Ana")
        };

        for (Soldado s : ejercito) {
            s.saludar();
            s.atacar();
        }
    }
}
```

La palabra clave `abstract` debe ponerse:

* en la clase, porque contiene métodos abstractos,
* y en el método abstracto, porque no tiene implementación.

Por eso escribimos:

```java id="rk2k9q"
public abstract class Soldado
```

y:

```java id="s6snkk"
public abstract void atacar();
```


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave `final` en Java impide que algo pueda modificarse o extenderse.

Aplicada a métodos:

* un método `final` no puede ser sobreescrito por las subclases.

Ejemplo:

```java id="b0k7p6"
public class Soldado {

    public final void identificarse() {
        System.out.println("Soy un soldado");
    }
}
```

```java id="mfhc6y"
public class Zapador extends Soldado {

    // ERROR: no puede sobreescribirse
    /*
    @Override
    public void identificarse() {
    }
    */
}
```

Aplicada a clases:

* una clase `final` no puede heredarse.

Ejemplo:

```java id="pw1w1g"
public final class CuentaBancaria {
}
```

```java id="zl8w55"
// ERROR
/*
public class CuentaPremium
        extends CuentaBancaria {
}
*/
```

### Relación con el polimorfismo

El polimorfismo se basa en la posibilidad de sobreescribir métodos y redefinir comportamiento en las subclases.

Por eso:

* un método `final` limita el polimorfismo, porque impide la sobreescritura,
* y una clase `final` elimina completamente la posibilidad de herencia y polimorfismo sobre ella.

### Ejemplos reales en la API de Java

Algunas clases `final` muy conocidas son:

```java id="f8tngq"
String
```

```java id="k7x6ls"
Integer
```

```java id="e58h7o"
Math
```

Por ejemplo, `String` es `final` para evitar que se altere su comportamiento interno y garantizar seguridad e inmutabilidad.



## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Las interfaces en Java son mecanismos para definir comportamientos que una clase debe implementar.

Una interfaz especifica qué métodos debe tener una clase, pero normalmente no cómo se implementan.

Por ejemplo:

```java id="n3aqzl"
public interface Volador {

    void volar();
}
```

Cualquier clase que implemente esa interfaz está obligada a implementar el método `volar()`.

```java id="n5g6wu"
public class Pajaro implements Volador {

    @Override
    public void volar() {
        System.out.println("El pájaro vuela");
    }
}
```

Las interfaces se parecen a las clases abstractas porque ambas pueden definir métodos que las subclases deben implementar.

Sin embargo, las interfaces están más orientadas a definir capacidades o comportamientos comunes, mientras que las clases abstractas suelen usarse para compartir estado y código entre clases relacionadas.

Diferencias principales:

* Una clase abstracta puede tener atributos de instancia y constructores.
* Una interfaz normalmente define comportamiento, no estado.
* Una clase solo puede heredar de una clase abstracta.
* Una clase puede implementar varias interfaces.

Por ejemplo:

```java id="mcn23g"
public interface Volador {
    void volar();
}
```

```java id="lr6ax8"
public interface Nadador {
    void nadar();
}
```

```java id="srlw7m"
public class Pato
        implements Volador, Nadador {

    @Override
    public void volar() {
        System.out.println("El pato vuela");
    }

    @Override
    public void nadar() {
        System.out.println("El pato nada");
    }
}
```

Por tanto, sí: una clase puede implementar más de una interfaz, lo que permite una forma de “herencia múltiple” de comportamiento en Java.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

```java
public abstract class Punto {

    public abstract double calcularDistanciaA(Punto otro);
}
```

```java
public class Punto2D extends Punto {

    private final double x;
    private final double y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {

        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("El punto debe ser 2D");
        }

        Punto2D p = (Punto2D) otro;

        return Math.sqrt(
            Math.pow(p.x - this.x, 2) +
            Math.pow(p.y - this.y, 2)
        );
    }
}
```

```java
public class Punto3D extends Punto {

    private final double x;
    private final double y;
    private final double z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {

        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("El punto debe ser 3D");
        }

        Punto3D p = (Punto3D) otro;

        return Math.sqrt(
            Math.pow(p.x - this.x, 2) +
            Math.pow(p.y - this.y, 2) +
            Math.pow(p.z - this.z, 2)
        );
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

    public double longitud() {
        return inicio.calcularDistanciaA(fin);
    }
}
```

```java
public class Main {

    public static void main(String[] args) {

        Punto p1 = new Punto2D(1, 2);
        Punto p2 = new Punto2D(4, 6);

        Linea linea2D = new Linea(p1, p2);

        System.out.println("Longitud 2D: " + linea2D.longitud());

        Punto p3 = new Punto3D(1, 2, 3);
        Punto p4 = new Punto3D(4, 6, 8);

        Linea linea3D = new Linea(p3, p4);

        System.out.println("Longitud 3D: " + linea3D.longitud());
    }
}
```

Aquí `Linea` trabaja con referencias de tipo `Punto`, sin saber si son puntos 2D o 3D. Gracias al polimorfismo, cuando llama a:

```java
inicio.calcularDistanciaA(fin);
```

Java ejecuta automáticamente la versión correcta según el objeto real: `Punto2D` o `Punto3D`.

Además, dentro de cada método se usa `instanceof` para comprobar que el otro punto es del mismo subtipo, y después se hace *downcasting* para poder acceder a sus coordenadas concretas.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La herencia de interfaces en Java consiste en que una interfaz puede extender otra interfaz usando la palabra clave `extends`.

La interfaz hija hereda todos los métodos de la interfaz padre y además puede añadir nuevos métodos.

Sí, en Java existe herencia múltiple de interfaces. Una interfaz puede extender varias interfaces al mismo tiempo.

Ejemplo:

```java
public interface Fichero {

    String leer();
}
```

La interfaz `Fichero` obliga a implementar un método para leer el contenido del fichero y devolverlo como `String`.

Ahora creamos una interfaz más específica:

```java
public interface FicheroEscribible
        extends Fichero {

    void escribir(String contenido);

    void eliminar();
}
```

`FicheroEscribible` hereda el método:

```java
String leer();
```

de `Fichero` y además añade:

* `escribir`
* `eliminar`

Una clase concreta podría implementar esa interfaz:

```java
public class FicheroTexto
        implements FicheroEscribible {

    private String contenido = "";

    @Override
    public String leer() {
        return contenido;
    }

    @Override
    public void escribir(String contenido) {
        this.contenido = contenido;
    }

    @Override
    public void eliminar() {
        contenido = "";
        System.out.println("Fichero eliminado");
    }
}
```

Ejemplo de uso:

```java
public class Main {

    public static void main(String[] args) {

        FicheroEscribible fichero =
                new FicheroTexto();

        fichero.escribir("Hola mundo");

        System.out.println(fichero.leer());

        fichero.eliminar();
    }
}
```

Además, Java permite herencia múltiple de interfaces:

```java
public interface A {
}

public interface B {
}

public interface C extends A, B {
}
```

Por eso, aunque Java no permite herencia múltiple de clases, sí permite herencia múltiple de interfaces.
