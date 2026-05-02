
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación y la ocultación de información buscan proteger los datos internos de un objeto, permitiendo acceder a ellos solo a través de métodos controlados.
Ventajas: 
mayor seguridad
evita errores
facilita el mantenimiento
permite cambiar la implementación sin afectar al resto del programa.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública es el conjunto de métodos y atributos accesibles desde fuera de la clase, es decir, lo que otros pueden usar del objeto.
Se relaciona con la ocultación de información porque la interfaz pública muestra únicamente los métodos necesarios para usar la clase, ocultando los detalles internos y evitando el acceso directo a los datos.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Porque define cómo se va a usar la clase desde fuera y cualquier error en su diseño afecta a todo el programa.
No es fácil cambiarla, ya que otros programas o partes del código pueden depender de ella.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clase son condiciones o reglas que siempre deben cumplirse en los objetos de una clase. La ocultación de información ayuda a mantenerlas porque impide modificar directamente los atributos y obliga a hacerlo mediante métodos que controlan que dichas condiciones se cumplan

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

class Punto{
    private double x;
    private double y;

    public Punto (double x,double y){
        this.x=x;
        this.y=y;
    }
    public double calcularDistanciaAOrigen(){
        return Math.sqrt(x*x+y*y);
    }
}
La interfaz pública de la clase es el constructor y el método calcularDistanciaAOrigen, ya que son los elementos accesibles desde fuera.
public significa que se puede acceder desde cualquier parte del programa, mientras que private indica que solo se puede acceder desde la propia clase.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

Se pueden aplicar a clases, atributos, métodos y constructores.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Sí, existen más niveles de visibilidad. En Java también existe protected y visibilidad por defecto (package). En otros lenguajes puede variar el número de niveles.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Están ocultos para otras clases, pero no para otras instancias de la misma clase
public double calcularDistanciaAPunto(Punto otro) {
    return Math.sqrt((x - otro.x)*(x - otro.x) + (y - otro.y)*(y - otro.y));
}

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Son métodos que permiten acceder y modificar los atributos de una clase de forma controlada.
Los getter se utilizan para obtener el valor de un atributo y los setter para modificarlo, normalmente cuando los atributos son privados.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No. Se refiere a evitar errores y accesos indebidos dentro del propio programa, protegiendo los datos y evitando que se modifiquen incorrectamente, no a la seguridad frente a ataques externos.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un miembro de instancia pertenece a cada objeto y cada uno tiene su propia copia, mientras que un miembro de clase (static) es compartido por todos los objetos de la clase.
Sí, los miembros de clase también se pueden ocultar utilizando modificadores como private.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Se hacen privados para controlar  la creación de objetos, evitando que se instancien directamente con new y obligando a utilizar métodos específicos que garanticen un uso correcto.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.
En Java, los miembros de clase se indican con la palabra clave static.
Estos miembros pertenecen a la clase y son compartidos por todos los objetos.
public class Punto {
private double x, y; // atributos de instancia

private static double maxX = Double.MIN_VALUE;
private static double maxY = Double.MIN_VALUE;

public Punto(double x, double y) {
this.x = x;
this.y = y;

if (x > maxX) maxX = x;
if (y > maxY) maxY = y;
}

public static double getMaxX() {
return maxX;
}

public static double getMaxY() {
return maxY;
}
}

Los atributos maxX y maxY son miembros de clase porque usan static almacenan los valores máximos de todos los puntos creados.



## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 
public static Punto crearPuntoRedondeado(double x, double y) {
    int xRed = (int) Math.round(x);
    int yRed = (int) Math.round(y);
    return new Punto(xRed, yRed);
}
Sí, he usado static porque es un método factoría, es decir, se llama desde la clase (Punto.crearPuntoRedondeado(...)) y no desde un objeto.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

class Punto {
    private double[] coord = new double[2];

    public Punto(double x, double y) {
        coord[0] = x;
        coord[1] = y;
    }

    public double calcularDistanciaAlOrigen() {
        return Math.sqrt(coord[0] * coord[0] + coord[1] * coord[1]);
    }
}

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

No es recomendable declarar un atributo como public, aunque tenga métodos getter y setter. Si el atributo es público, cualquier parte del programa puede modificarlo directamente sin ningún tipo de control, lo que impide validar los valores o garantizar el correcto estado del objeto.
La práctica habitual en programación orientada a objetos es declarar los atributos como private y acceder a ellos mediante métodos públicos (getters y setters). De este modo, la clase puede controlar cómo y cuándo se modifican sus datos.
Esto está directamente relacionado con las invariantes de clase, que son condiciones que deben cumplirse siempre en los objetos. Si los atributos son privados, la clase puede asegurarse de que esas condiciones no se rompan; en cambio, si son públicos, cualquier código externo podría modificarlos y violar dichas invariantes.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es inmutable cuando, una vez creado un objeto, su estado no puede cambiar. Es decir, sus atributos no se modifican después de la construcción del objeto.
Un método modificador es aquel que cambia el estado interno de un objeto, es decir, altera el valor de alguno de sus atributos.
Un método modificador no es siempre un setter. Un setter es un tipo concreto de método modificador cuya función es asignar un valor a un atributo, pero existen otros métodos modificadores que cambian el estado del objeto de formas más complejas.
Las clases inmutables tienen varias ventajas: son más seguras porque no pueden cambiar de forma inesperada, son más fáciles de razonar y depurar, y evitan problemas en entornos concurrentes, ya que no hay riesgo de modificaciones simultáneas.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No, no es recomendable incluir métodos setter siempre como convención.
Los setters solo deben añadirse cuando tiene sentido que el estado del objeto pueda modificarse. Incluirlos por defecto rompe la encapsulación, ya que permite cambiar los atributos sin control y puede hacer que el objeto pierda coherencia.
En muchos casos, es preferible no incluir setters y diseñar clases inmutables o con control estricto sobre sus cambios. De este modo, se protegen mejor las invariantes de clase y se evita que el objeto adopte estados inválidos.
En resumen, los setters no son obligatorios: se deben usar solo cuando sean necesarios y adecuados al diseño de la clase.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase String en Java es inmutable, es decir, una vez creada una cadena, su contenido no puede modificarse.
Cuando se concatenan dos cadenas, en realidad no se modifica ninguna de las originales, sino que se crea un nuevo objeto String con el resultado de la concatenación.
Si se realizan muchas concatenaciones de forma repetida, se crean muchos objetos nuevos, lo que es ineficiente en tiempo y memoria. En estos casos, es recomendable utilizar clases como StringBuilder (o StringBuffer), que permiten modificar la cadena sin crear nuevos objetos continuamente y son mucho más eficientes para construir textos largos paso a paso.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En programación orientada a objetos, los objetos se pueden comparar de dos formas: por identidad (si son exactamente el mismo objeto en memoria) o por contenido (si tienen los mismos valores en sus atributos).
En Java, el operador == compara por identidad, es decir, comprueba si dos referencias apuntan al mismo objeto.
El método equals se utiliza para comparar objetos por contenido. Sin embargo, por defecto (en la clase Object), el método equals se comporta igual que ==, es decir, compara por identidad. Por eso, muchas clases lo sobrescriben (override) para comparar el contenido de los objetos.
En el caso de las cadenas (String), se debe usar el método equals para compararlas, ya que este método está sobrescrito para comparar el contenido de las cadenas. Usar == con String puede dar resultados incorrectos, porque solo compara si son el mismo objeto, no si tienen el mismo texto.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper son clases que envuelven (representan como objetos) a los tipos primitivos. En Java, por ejemplo, int tiene como wrapper a Integer, double a Double, etc.
Este proceso consiste en convertir un valor primitivo en un objeto (boxing) o un objeto en un primitivo (unboxing). En Java puede hacerse de forma explícita, pero también existe el autoboxing y auto-unboxing, que lo realizan automáticamente.
Ejemplo:

Intenger num=5;
ib¡nt valor =num;

Las ventajas de las clases wrapper son que permiten tratar valores primitivos como objetos, lo cual es necesario para usar estructuras como colecciones (ArrayList, HashMap), y además ofrecen métodos útiles para trabajar con esos valores.
No todos los lenguajes orientados a objetos tienen tipos primitivos separados de los objetos. Algunos lenguajes (como Python) tratan todo como objetos, por lo que no necesitan clases wrapper como tal.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

En programación orientada a objetos, un tipo de dato enumerado (enum) es un tipo que define un conjunto finito y fijo de valores posibles. Se utiliza cuando una variable solo puede tomar uno de esos valores (por ejemplo, días de la semana, estados, tipos, etc.).
En Java, un enumerado sí es una clase especial. Internamente es una clase que hereda de Enum, y puede tener atributos, métodos e incluso constructores.
En términos de encapsulación, los enumerados en Java ofrecen varias ventajas:
Restringen los valores posibles, evitando estados inválidos.
Permiten agrupar en un solo tipo valores relacionados, mejorando la claridad del código.
Pueden incluir comportamiento propio (métodos), manteniendo la lógica asociada a esos valores dentro del propio tipo.
En resumen, los enum mejoran la seguridad, la claridad y la encapsulación del programa al limitar y organizar los valores permitidos.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

enum Mes {
    ENERO(31), FEBRERO(28), MARZO(31), ABRIL(30),
    MAYO(31), JUNIO(30), JULIO(31), AGOSTO(31),
    SEPTIEMBRE(30), OCTUBRE(31), NOVIEMBRE(30), DICIEMBRE(31);

    private int dias;

    Mes(int dias) {
        this.dias = dias;
    }

    public int getDias() {
        return dias;
    }

    public int getNumeroMes() {
        return this.ordinal() + 1;
    }
}

## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

public boolean esDePrimavera(boolean norte) {
    int m = this.ordinal() + 1;
    return norte ? (m >= 3 && m <= 5) : (m >= 9 && m <= 11);
}

public boolean esDeVerano(boolean norte) {
    int m = this.ordinal() + 1;
    return norte ? (m >= 6 && m <= 8) : (m == 12 || m <= 2);
}

public boolean esDeOtono(boolean norte) {
    int m = this.ordinal() + 1;
    return norte ? (m >= 9 && m <= 11) : (m >= 3 && m <= 5);
}

public boolean esDeInvierno(boolean norte) {
    int m = this.ordinal() + 1;
    return norte ? (m == 12 || m <= 2) : (m >= 6 && m <= 8);
}