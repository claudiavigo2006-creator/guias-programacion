
# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Encapsulación: Agrupar datos y métodos en una clase y controlar su acceso.
Herencia: Permite crear nuevas clases a partir de otras (reutilización de código)
Polimorfismo: Un mismo método puede comportarse de distintas formas
Abstracción:Mostrar solo lo importante y ocultar los detalles internos

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Java C++ Python C#

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

P.Estructurada:  La programación estructurada organiza el programa mediante estructuras de control como condicionales, bucles y funciones, evitando el uso de saltos desordenados.
P.Modular: La programación modular consiste en dividir el programa en módulos o funciones independientes, facilitando su comprensión, reutilización y mantenimiento.



## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Atributos (estado)/ Métodos(comportamiento)/ Identidad(referencia única)

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Clase: Plantilla o modelo que define los atributos y métodos de un tipo de objeto
Objeto: instancia concreta creada a partir de una clase
Instancia: sinónimo de objeto
No todos los lenguajes OO usan clases.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En Java, los objetos se almacenan en el heap, mientras que las referencias se almacenan en el stack.
La gestión de memoria es automática.
La recolección de basura es un mecanismo de la JVM que elimina automáticamente los objetos que ya no tienen referencias.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un método es una función definida dentro de una clase que describe el comportamiento de sus objetos.
La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre en una misma clase, pero con distinta lista de parámetros.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

class Punto {
double x;
double y;
calculaDistaciaAOrigen(){
return Math.sqrt(x*x+y*y);
}
}
public class main{
public static void main(String[]args){
Punto p =new Punto();
p.x=3;
p.y=4;,
System.out.println(p.calclaDistanciaAOrigen());
}
}



## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada de un programa en Java es el método public static void main(String[] args), que es el primero que ejecuta la JVM.
static indica que un atributo o método pertenece a la clase y no a los objetos, por lo que es compartido por todos y puede usarse sin crear instancias.Y no solo se emplea en el método main, sino también en variables y otros métodos.
Cuando se combina con final, se utiliza para definir constantes que no pueden modificarse.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

genera un archivo .class. Para ejecutarlo se usa java Programa.
Java no es especialmente complicado, aunque es un lenguaje más estricto.
La máquina virtual (JVM) es el sistema que ejecuta el programa Java, permitiendo que sea independiente del sistema operativo.
El bytecode es un código intermedio generado al compilar, que se almacena en los ficheros .class y es ejecutado por la JVM.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

new es la palabra clave que se utiliza para crear un objeto en memoria.
Un constructor es un método especial de una clase que se encarga de inicializar los atributos del objeto al crearlo.
Por ejemplo, en una clase Empleado, el constructor puede recibir el DNI, nombre y apellidos y asignarlos a los atributos mediante this.

class Empleado {
String dni;
String nombre;
String apellidos;

Empleado(String dni, String nombre, String apellidos) {
this.dni = dni;
this.nombre = nombre;
this.apellidos = apellidos;
}
}



## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

this es una referencia al objeto actual, que permite acceder a sus atributos y métodos.
Se utiliza especialmente para diferenciar entre los atributos del objeto y los parámetros de un método o constructor.
No todos los lenguajes utilizan el mismo nombre, aunque muchos emplean this.

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado
double distancia(Punto p) {
return Math.sqrt((x - p.x)*(x - p.x) + (y - p.y)*(y - p.y));
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, los parámetros se pasan por valor.
En el caso de los objetos, se pasa el valor de la referencia, por lo que si se modifican los atributos del objeto dentro del método, dichos cambios sí se reflejan fuera. Sin embargo, si se modifica la referencia en sí, no afecta al objeto original.
En el caso de tipos primitivos como int, se pasa una copia del valor, por lo que cualquier modificación dentro del método no afecta al valor original fuera del mismo.



## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

El método toString() en Java devuelve una representación en forma de cadena de texto de un objeto. Se utiliza, por ejemplo, al imprimir un objeto con System.out.println().
Este tipo de método existe en otros lenguajes, aunque puede tener distintos nombres.
En una clase Punto, podría devolver las coordenadas del punto en formato texto.


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Una clase es similar a un struct en C en el sentido de que ambos agrupan datos. Sin embargo, un struct solo contiene datos, mientras que una clase también incluye métodos y permite encapsulación, herencia y polimorfismo. Por tanto, a un struct le falta comportamiento y mecanismos propios de la programación orientada a objetos. 
En C, las variables de tipo struct son simplemente estructuras de datos y no instancias en el sentido de la POO, ya que carecen de métodos y comportamiento.



## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

En C se puede emular una clase utilizando un struct para los datos y funciones externas para el comportamiento. Para calcular la distancia al origen, se define una función que recibe un struct Punto como parámetro. El concepto de this no existe en C, por lo que el objeto debe pasarse explícitamente como argumento a la función.
