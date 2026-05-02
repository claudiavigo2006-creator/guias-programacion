
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C no existen las excepciones, por lo que el control de errores debe diseñarse manualmente. Si queremos implementar una función raíz cuadrada que solo acepte números positivos y detectar el error desde fuera, existen varias alternativas.
Una primera opción consiste en devolver un valor especial que indique error. Por ejemplo, si la función recibe un número negativo, puede devolver -1. El problema de este enfoque es que ese valor podría ser válido en otros contextos, por lo que no es una solución robusta.

#include <math.h>

float raiz(float x) {
    if (x < 0) {
        return -1; // indica error
    }
    return sqrt(x);
}
Otra opción más correcta consiste en separar el resultado del estado de la operación. Para ello, la función puede devolver un código de éxito o error, y el resultado se pasa por referencia.

#include <math.h>

int raiz(float x, float *resultado) {
    if (x < 0) {
        return 0; // error
    }
    *resultado = sqrt(x);
    return 1; // éxito
}

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un mecanismo que permite indicar que se ha producido un error o una situación anómala durante la ejecución de un programa. Su principal objetivo es separar el flujo normal del programa del tratamiento de errores, permitiendo escribir código más limpio, estructurado y mantenible.
Los programadores utilizan excepciones tanto al implementar funciones, para indicar que algo ha fallado, como al llamarlas, para poder gestionar esos errores de forma controlada.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, el mismo ejemplo se implementa utilizando excepciones. En este caso, si el número es negativo, se lanza una excepción, y el error se controla desde el método main.
class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo no permitido");
        }
        return Math.sqrt(x);
    }
}

public class Main {
    public static void main(String[] args) {
        try {
            double resultado = Calculadora.raiz(-4);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar una excepción significa crear un objeto excepción y enviarlo al sistema mediante la palabra clave throw. Esto interrumpe inmediatamente la ejecución normal del método.
Capturar o controlar una excepción consiste en interceptarla mediante un bloque catch para poder gestionar el error y evitar que el programa termine abruptamente.
Cuando una excepción no es capturada en el método donde se produce, se dice que se propaga. Esto significa que sube por la pila de llamadas buscando un método que la capture. Durante este proceso, los métodos que no la controlan finalizan su ejecución y no continúan después del punto donde se produjo el error.
En el ejemplo de la raíz, si el método raiz lanza la excepción y no la captura, esta se propaga hasta main, donde finalmente se gestiona.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

El uso de excepciones en Java presenta varias ventajas frente al control manual de errores en C. En primer lugar, permite evitar la repetición constante de comprobaciones de error. Además, la propagación automática simplifica el código, ya que no es necesario comprobar cada llamada. Por último, mejora la claridad del programa al separar la lógica principal del tratamiento de errores.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En Java, las excepciones son objetos. Esto permite encapsular información relevante como el mensaje de error, el tipo de excepción o la causa. Gracias a esto, se pueden crear excepciones personalizadas adaptadas a las necesidades del programa.
class MiExcepcion extends Exception{
    public MiEcpecion(String mensaje){
        super(mensaje);
    }
}

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Una excepción contiene información muy útil, como un mensaje descriptivo, el tipo de error, la traza de ejecución (stack trace) y, en muchos casos, la causa del error. Esta información es fundamental para diagnosticar problemas.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

En Java se pueden definir varios bloques catch para manejar distintos tipos de excepciones. Sin embargo, solo se ejecuta uno: el primero que coincida con el tipo de excepción lanzada.



## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

El bloque finally se utiliza para garantizar que cierto código se ejecuta siempre, independientemente de si ocurre una excepción o no. Es especialmente útil para liberar recursos como ficheros.

try {
    int x = 5 / 0;
} catch (ArithmeticException e) {
    System.out.println("Error");
} finally {
    System.out.println("Siempre se ejecuta");
}
O sin catch:

try {
    int x = 5 / 0;
} finally {
    System.out.println("Siempre se ejecuta");
}

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí.
El bloque finally puede ir sin catch. En ese caso, si ocurre una excepción, no se captura ahí, pero el finally se ejecuta antes de que la excepción se propague.
El finally se ejecuta siempre, tanto si hay excepción como si no.
Además, incluso si hay un return dentro del try, el finally se ejecuta antes de devolver el valor.
Finally garantiza que ese código se ejecute pase lo que pase.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

Las excepciones controladas (checked) son aquellas que el compilador obliga a tratar o declarar, como IOException. Las no controladas (unchecked), que heredan de RuntimeException, no requieren tratamiento obligatorio, como NullPointerException.
Se suelen usar excepciones controladas en situaciones recuperables, como errores de entrada/salida, y no controladas en errores de programación, como argumentos inválidos.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La palabra clave throws se utiliza en la firma de un método para indicar que este no maneja una excepción, sino que la deja propagarse hacia el método llamador. Es una alternativa a capturar la excepción dentro del propio método.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

import java.io.*;

public class Ejemplo {

    public static void leer() throws IOException {
        FileReader fr = null;

        try {
            fr = new FileReader("archivo.txt");
            System.out.println("Leyendo archivo...");
        } finally {
            if (fr != null) {
                fr.close();
            }
        }
    }
}

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Aunque es posible declarar excepciones no controladas en throws, no es obligatorio hacerlo. El método llamador tampoco está obligado a capturarlas. En estos casos, su uso es más bien informativo.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Las excepciones controladas se utilizan cuando el error es previsible y el programa puede recuperarse, como en el caso de lectura de ficheros. En cambio, las no controladas se emplean cuando el error indica un fallo de programación, como pasar argumentos incorrectos.
En muchos lenguajes modernos solo existen excepciones no controladas, ya que simplifican el desarrollo.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Es posible lanzar nuevas excepciones dentro de un bloque catch, por ejemplo para traducir un error de bajo nivel a uno de mayor nivel.
También se puede relanzar la misma excepción capturada. Esto es útil cuando se quiere realizar alguna acción adicional, como registrar el error, antes de que continúe propagándose.


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Una excepción puede encapsular otra como causa. Esto permite mantener la información del error original mientras se lanza una excepción más adecuada al contexto.

try{
    new FileReader("noexiste.txt");
}catch(IOException e){
    throw new RuntimeException("Error al proceso el archivo",e)
}
Cuando se imprime la excepción, aparece tanto el mensaje principal como la causa original, lo que facilita enormemente la depuración.

