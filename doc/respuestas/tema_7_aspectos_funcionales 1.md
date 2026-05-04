<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Un puntero a una función en C es una variable que almacena la dirección de memoria de una función en lugar de un dato. Esto permite que la función sea tratada como cualquier otro tipo de dato, como enteros o caracteres, y puede ser pasada como argumento a otras funciones, devuelta por funciones o almacenada en estructuras de datos.
En C que define una función llamada `convertirAMayusculas` que toma una cadena de caracteres como parámetro y devuelve la cadena en mayúsculas. Luego, se declara un puntero a función llamado `aMayusculas` que apunta a `convertirAMayusculas`, y finalmente, se invoca la función a través del puntero:
```c
#include <stdio.h>
#include <ctype.h> // Para la función toupper

// Función para convertir una cadena a mayúsculas
char* convertirAMayusculas(char *cadena) {
    int i = 0;
    while (cadena[i] != '\0') {
        cadena[i] = toupper(cadena[i]);
        i++;
    }
    return cadena;
}

int main() {
    char cadena[] = "Hola, mundo!";
    char *(*aMayusculas)(char*) = &convertirAMayusculas; // Declaración del puntero a función

    // Invocación de la función a través del puntero
    printf("Cadena original: %s\n", cadena);
    printf("Cadena en mayúsculas: %s\n", aMayusculas(cadena));

    return 0;
}
```
En `aMayusculas` es un puntero a una función que toma un parámetro `char*` y devuelve `char*`. Se inicializa apuntando a la función `convertirAMayusculas`. Luego, se invoca la función `convertirAMayusculas` utilizando el puntero `aMayusculas`.

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima que se puede definir en línea sin la necesidad de declararla formalmente. Estas funciones son útiles cuando se necesitan pequeñas piezas de código que se pueden pasar como argumentos a otras funciones o asignar a variables.
###### JavaScript (ES6+):
```javascript
// Definición de la función lambda en JavaScript
const convertirAMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

// Variable local que apunta a la función lambda
let aMayusculas = convertirAMayusculas;

// Invocación de la función a través de la variable
let cadenaOriginal = "Hola, mundo!";
console.log("Cadena original: " + cadenaOriginal);
console.log("Cadena en mayúsculas: " + aMayusculas(cadenaOriginal));
```
###### Java (con funciones lambda y `Function<String, String>`):
```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // Definición de la función lambda en Java
        Function<String, String> convertirAMayusculas = cadena -> cadena.toUpperCase();

        // Variable local que apunta a la función lambda
        Function<String, String> aMayusculas = convertirAMayusculas;

        // Invocación de la función a través de la variable
        String cadenaOriginal = "Hola, mundo!";
        System.out.println("Cadena original: " + cadenaOriginal);
        System.out.println("Cadena en mayúsculas: " + aMayusculas.apply(cadenaOriginal));
    }
}
```
En ambos, se define una función lambda que toma una cadena como parámetro y devuelve la misma cadena en mayúsculas. Luego, se asigna esta función a una variable local llamada `aMayusculas`, y se invoca la función a través de esta variable.

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El paradigma funcional se basa en tratar a las funciones como elementos principales, enfocándose en funciones puras sin efectos secundarios, inmutabilidad de datos y evaluación perezosa. Algunos lenguajes orientados a objetos, como Java 8, incorporan características del paradigma funcional sin abandonar su orientación a objetos, como expresiones lambda y el paquete `java.util.function`. En este contexto, las funciones son consideradas "ciudadanos de primera clase", lo que significa que pueden ser manejadas de manera flexible y poderosa, asignadas a variables, pasadas como argumentos, devueltas como resultados y almacenadas en estructuras de datos. Esto facilita la programación funcional en lenguajes como Haskell, así como en lenguajes multi-paradigma como JavaScript, Python y Java a partir de Java 8.

## 4. Explica la sintaxis básica de una función lambda en Java.

En Java, las funciones lambda se introdujeron en la versión 8 como una forma de proporcionar soporte para programación funcional. La sintaxis básica de una función lambda en Java es la siguiente:
```java
(parametros) -> expresion
```
Donde:
- `parametros`: Son los parámetros de la función lambda. Pueden ser cero o más parámetros separados por comas. Si hay cero o un parámetro, los paréntesis son opcionales. Si no hay parámetros, se utilizan paréntesis vacíos `()`.
- `->`: Es el operador de flecha lambda, que separa los parámetros de la expresión lambda.
- `expresion`: Es el cuerpo de la función lambda, que puede ser una expresión o un bloque de código.
Aquí hay algunos ejemplos de funciones lambda en Java:
1. Función lambda sin parámetros y sin retorno:
```java
() -> System.out.println("Hola, mundo!");
```
2. Función lambda con un parámetro y con retorno:
```java
(num) -> num * num;
```
3. Función lambda con múltiples parámetros y con retorno:
```java
(x, y) -> x + y;
```
4. Función lambda con un bloque de código:
```java
(x, y) -> {
    int suma = x + y;
    System.out.println("La suma es: " + suma);
    return suma;
};
```
Se pueden utilizar en contextos como la definición de interfaces funcionales, la programación funcional con streams, y más, lo que proporciona una forma concisa y expresiva de trabajar con funciones en Java.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

En Java y JavaScript para recibir una función como parámetro en un método llamado "transformar" y luego llamarla desde dentro:
###### Java:
```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // Función lambda para convertir a mayúsculas
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        // Invocación del método transformar
        String cadenaOriginal = "Hola, mundo!";
        String resultado = transformar(cadenaOriginal, aMayusculas);
        System.out.println("Resultado: " + resultado);
    }

    // Método que recibe una cadena y una función transformadora
    public static String transformar(String cadena, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(cadena);
    }
}
```
###### JavaScript:
```javascript
// Función lambda para convertir a mayúsculas
const aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

// Invocación del método transformar
let cadenaOriginal = "Hola, mundo!";
let resultado = transformar(cadenaOriginal, aMayusculas);
console.log("Resultado: " + resultado);

// Método que recibe una cadena y una función transformadora
function transformar(cadena, funcionTransformadora) {
    return funcionTransformadora(cadena);
}
```
En ambos casos, hemos creado una función llamada "transformar" que toma dos parámetros: una cadena y una función transformadora. Dentro del método "transformar", se llama a la función transformadora pasándole la cadena como argumento. Luego, en el método principal, invocamos el método "transformar" pasándole la cadena original y la función de transformación como argumentos. Esto nos permite modularizar la lógica de transformación y hacerla más reutilizable.

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

Cómo invocar la función `transformar` con una nueva función lambda que invierta la cadena, definida directamente en la llamada a `transformar`:
#### Java:
```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // Invocación del método transformar con una función lambda para invertir la cadena
        String cadenaOriginal = "Hola, mundo!";
        String resultado = transformar(cadenaOriginal, cadena -> {
            StringBuilder builder = new StringBuilder(cadena);
            return builder.reverse().toString();
        });
        System.out.println("Resultado: " + resultado);
    }

    // Método que recibe una cadena y una función transformadora
    public static String transformar(String cadena, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(cadena);
    }
}
```
#### JavaScript:
```javascript
// Invocación del método transformar con una función lambda para invertir la cadena
let cadenaOriginal = "Hola, mundo!";
let resultado = transformar(cadenaOriginal, cadena => cadena.split("").reverse().join(""));
console.log("Resultado: " + resultado);

// Método que recibe una cadena y una función transformadora
function transformar(cadena, funcionTransformadora) {
    return funcionTransformadora(cadena);
}
```
En ambos casos, hemos definido la función lambda directamente en la llamada a `transformar`. La función lambda en Java invierte la cadena utilizando un `StringBuilder`, mientras que en JavaScript, dividimos la cadena en un array de caracteres, la invertimos y luego la volvemos a unir.

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

En el contexto de las funciones lambda, un cierre o "closure" se refiere a la capacidad de la función lambda para capturar y acceder a variables locales definidas en el ámbito en el que se encuentra. Esto significa que una función lambda puede acceder y utilizar variables locales externas incluso después de que el ámbito en el que fueron definidas haya desaparecido.
```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        String sufijo = "!";
        
        // Definición de una función lambda que concatena un sufijo a una cadena
        Function<String, String> agregarSufijo = (cadena) -> cadena + sufijo;

        // Invocación del método transformar con la función lambda
        String cadenaOriginal = "Hola, mundo";
        String resultado = transformar(cadenaOriginal, agregarSufijo);
        System.out.println("Resultado: " + resultado);
    }

    // Método que recibe una cadena y una función transformadora
    public static String transformar(String cadena, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(cadena);
    }
}
```
La función lambda `agregarSufijo` captura la variable `sufijo`, que es una variable local definida fuera de la función lambda. La función lambda puede acceder y utilizar esta variable local incluso después de que el método `main` haya terminado de ejecutarse.

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

Las funciones lambda y los punteros a funciones comparten la capacidad de representar funciones en lenguajes de programación, pero difieren en varios aspectos:  
- Sintaxis y legibilidad: Las funciones lambda son más concisas y legibles que los punteros a funciones en C.  
- Contexto de captura de variables: Las funciones lambda pueden capturar variables locales del ámbito en el que se definen, a diferencia de los punteros a funciones en C.  
- Tipado estático y dinámico: Las funciones lambda en lenguajes modernos están integradas en el sistema de tipos estático, mientras que los punteros a funciones en C requieren conversiones explícitas de tipos.  
- Características adicionales: Las funciones lambda ofrecen características como evaluación perezosa, funciones de orden superior y cierres, haciéndolas más versátiles y poderosas que los punteros a funciones en C.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

 cómo se implementa:
```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // Creación de dos funciones descuento
        Function<Double, Double> descuento10 = crearDescuento(10); // Descuento del 10%
        Function<Double, Double> descuento20 = crearDescuento(20); // Descuento del 20%

        // Aplicación de los descuentos
        double cantidad = 100;
        System.out.println("Cantidad original: $" + cantidad);
        System.out.println("Descuento del 10%: $" + descuento10.apply(cantidad));
        System.out.println("Descuento del 20%: $" + descuento20.apply(cantidad));
    }

    // Función que crea una función descuento
    public static Function<Double, Double> crearDescuento(double porcentajeDescuento) {
        return cantidad -> cantidad * (1 - porcentajeDescuento / 100);
    }
}
```
 La función `crearDescuento` devuelve una función que aplica el descuento especificado a una cantidad dada. La función lambda creada dentro de `crearDescuento` captura la variable `porcentajeDescuento`, lo que permite que la función devuelta acceda y utilice ese valor incluso después de que `crearDescuento` haya terminado de ejecutarse. Esto demuestra el concepto de cierre o "closure" en acción, donde la función lambda retiene acceso al ámbito en el que fue creada.

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una interfaz funcional en Java contiene exactamente un método abstracto y se usa con funciones lambda para programación funcional. Los requisitos para ser considerada funcional son: 
1) tener un método abstracto 
2) poder tener métodos default o estáticos 
3) opcionalmente llevar la anotación `@FunctionalInterface`
```java
@FunctionalInterface
public interface MiInterfazFuncional {
    // Método abstracto
    int operacion(int a, int b);

    // Método default
    default void metodoDefault() {
        System.out.println("Este es un método default en la interfaz funcional.");
    }

    // Método estático
    static void metodoEstatico() {
        System.out.println("Este es un método estático en la interfaz funcional.");
    }
}
```
Esta anotación ayuda a garantizar que la interfaz cumple con el requisito de un solo método abstracto y generará un error de compilación si no se cumple. Los métodos default y estáticos no afectan la condición de funcionalidad de la interfaz, siempre y cuando haya solo un método abstracto.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String cadena);
}
```
En esta definición:
- La anotación `@FunctionalInterface` marca esta interfaz como funcional.
- La interfaz contiene un único método abstracto llamado `transformar`, que toma una cadena de texto como parámetro y devuelve otra cadena de texto como resultado.
Esta interfaz puede ser utilizada para representar cualquier función que tome una cadena de texto y la transforme en otra. Ahora, se pueden crear instancias de esta interfaz utilizando expresiones lambda que implementen el método `transformar`, lo que permite una programación más funcional en Java.

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

Para hacer la interfaz funcional más genérica y emplear generics, podemos modificarla para que acepte un tipo de entrada y un tipo de salida. Llamémosla `Transformador<T, R>`, donde `T` representa el tipo de entrada y `R` representa el tipo de salida. Aquí tienes cómo se podría definir en Java:
```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}
```
En esta definición:
- La anotación `@FunctionalInterface` marca la interfaz como funcional.
- La interfaz ahora toma dos parámetros de tipo genérico `T` y `R`, que representan el tipo de entrada y el tipo de salida, respectivamente.
- La interfaz contiene un único método abstracto llamado `transformar`, que toma un parámetro de tipo `T` como entrada y devuelve un resultado de tipo `R`.
Ahora, para ejemplificar esta interfaz, creemos un transformador que redondee un `Double` a un `Integer`. Aquí tienes cómo podríamos hacerlo:
```java
public class Main {
    public static void main(String[] args) {
        // Creación de un transformador para redondear un Double a un Integer
        Transformador<Double, Integer> redondear = numero -> (int) Math.round(numero);

        // Ejemplo de uso del transformador
        double numeroOriginal = 3.7;
        int numeroRedondeado = redondear.transformar(numeroOriginal);
        System.out.println("Número original: " + numeroOriginal);
        System.out.println("Número redondeado: " + numeroRedondeado);
    }
}
```
Hemos creado una instancia de la interfaz `Transformador<Double, Integer>` utilizando una expresión lambda que toma un `Double` como entrada y devuelve un `Integer`. La expresión lambda utiliza el método `Math.round` para redondear el número `Double` y luego lo convierte a `Integer`. Luego, hemos aplicado este transformador a un número `Double` de ejemplo y hemos mostrado el resultado.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

En Java, el paquete `java.util.function` proporciona varias interfaces funcionales predefinidas para representar funciones en diferentes contextos:  
- **`Function`**: toma un argumento de tipo `T` y devuelve un resultado de tipo `R`  
- **`Consumer`**: opera con un argumento de tipo `T` sin devolver resultado  
- **`Supplier`**: no toma argumentos y devuelve un resultado de tipo `T`  
- **`Predicate`**: toma un argumento de tipo `T` y devuelve un valor booleano  
- **`UnaryOperator`**: opera con un solo argumento de tipo `T` y devuelve un resultado de tipo `T`  
- **`BinaryOperator`**: opera con dos argumentos de tipo `T` y devuelve un resultado de tipo `T`  
Estas son solo algunas de las interfaces funcionales disponibles en Java. Consulta la documentación oficial para más detalles.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

 El método `forEach` en Java permite iterar sobre los elementos de una colección y aplicar una acción a cada elemento. Aquí tienes un ejemplo de cómo utilizar `forEach` para recorrer una lista de enteros y mostrar un mensaje si el entero es positivo:
```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        // Creación de una lista de enteros
        List<Integer> numeros = Arrays.asList(1, -2, 3, -4, 5, -6);

        // Utilización de forEach para mostrar un mensaje si el entero es positivo
        numeros.forEach(numero -> {
            if (numero > 0) {
                System.out.println("El número " + numero + " es positivo.");
            }
        });
    }
}
```
Creamos una lista de enteros utilizando `Arrays.asList`. Luego, utilizamos el método `forEach` para iterar sobre cada elemento de la lista. Dentro del cuerpo del `forEach`, comprobamos si el número es positivo (mayor que 0) y, en ese caso, mostramos un mensaje indicando que el número es positivo.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

Si la firma fuera simplemente `Consumer<T>`, el método solo aceptaría un consumidor diseñado **exactamente** para el tipo `T`. Sin embargo, gracias al uso de `? super T` (comodín de límite inferior), el método permite pasar un consumidor que trabaje con cualquier tipo **padre** de `T`.
**Ejemplo:** Si tienes una `List<Integer>`, el método `forEach` acepta:
- Un `Consumer<Integer>`.
- Pero también un `Consumer<Number>` o un `Consumer<Object>`. Esto tiene sentido: si una función sabe cómo procesar cualquier `Number`, por definición sabe cómo procesar un `Integer`.
**PECS** es un acrónimo mnemotécnico acuñado por Joshua Bloch para recordar cómo usar comodines (_wildcards_) en Java:
- **Producer Extends (`? extends T`):** Si una estructura de datos va a "producir" elementos para que tú los leas, usa `extends`. Esto permite que la estructura sea de `T` o de cualquier subclase.
- **Consumer Super (`? super T`):** Si una estructura de datos (o función) va a "consumir" (recibir) elementos que tú le pases, usa `super`. Esto permite que el consumidor acepte `T` o cualquier superclase.
 La firma de tu método era: `public static String transformar(String cadena, Function<String, String> funcionTransformadora)`
Sin embargo, para que sea verdaderamente profesional y flexible siguiendo el principio PECS, la firma de una función genérica de transformación debería ser
``` java
public static <T, R> R transformar(T entrada, Function<? super T, ? extends R> funcion) {
    return funcion.apply(entrada);
}
```
Explicación de la mejora:
1. **`? super T` (El consumidor):** La función `apply` va a _consumir_ la `entrada`. Por tanto, la función puede estar preparada para recibir el tipo `T` o algo más genérico (un padre).
2. **`? extends R` (El productor):** La función va a _producir_ un resultado que esperamos sea de tipo `R`. Por tanto, la función puede devolver `R` o cualquier cosa que sea una subclase de `R`.
**¿Por qué es mejor?** Imagina que quieres transformar un `Integer` en un `String`:
- Tienes una función que transforma cualquier `Number` en un `Object`.
- Sin PECS, Java daría error de tipos porque `Number` no es exactamente `Integer`.
- **Con PECS**, el código compila porque `Number` es un "super" de `Integer` y `String` es un "extends" de `Object`.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

En JavaScript y Java podemos obtener referencias a métodos de objetos o clases. Aquí tienes ejemplos en ambos lenguajes utilizando una clase `Persona` con un método `saludar`:
### JavaScript:
```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("¡Hola! Soy " + this.nombre);
    }
}

// Crear una instancia de Persona
let persona = new Persona("Juan");

// Obtener una referencia al método saludar
let referenciaSaludar = persona.saludar;

// Invocar saludar utilizando la referencia al método
referenciaSaludar.call(persona); // O también referenciaSaludar();
```
Creamos una clase `Persona` con un método `saludar`. Luego, creamos una instancia de `Persona` llamada `persona`. Después, obtenemos una referencia al método `saludar` y la almacenamos en la variable `referenciaSaludar`. Finalmente, invocamos el método `saludar` utilizando la referencia almacenada, utilizando `call` para establecer el contexto correcto.
### Java:
```java
public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("¡Hola! Soy " + this.nombre);
    }

    public static void main(String[] args) {
        // Crear una instancia de Persona
        Persona persona = new Persona("Juan");

        // Obtener una referencia al método saludar
        Runnable referenciaSaludar = persona::saludar;

        // Invocar saludar utilizando la referencia al método
        referenciaSaludar.run();
    }
}
```
Creamos una clase `Persona` con un método `saludar`. Luego, creamos una instancia de `Persona` llamada `persona`. Después, obtenemos una referencia al método `saludar` utilizando la notación `::` y la almacenamos en la interfaz funcional `Runnable`. Finalmente, invocamos el método `run` de `Runnable`, que ejecutará el método `saludar` de la instancia `persona`.

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

Los principales tipos de referencias a método:
1. **Referencia a método estático:** Se utiliza para hacer referencia a un método estático de una clase. Se puede hacer referencia a un método estático utilizando la sintaxis `Clase::metodoEstático`.
2. **Referencia a constructor:** Se utiliza para hacer referencia a un constructor de una clase. Se puede hacer referencia a un constructor utilizando la sintaxis `Clase::new`.
3. **Referencia a método de instancia de una instancia concreta:** Se utiliza para hacer referencia a un método de instancia de un objeto específico. Se puede hacer referencia a un método de instancia utilizando la sintaxis `instancia::metodoDeInstancia`.
4. **Referencia a método de instancia sobre cualquier instancia:** Se utiliza para hacer referencia a un método de instancia sobre cualquier instancia de una clase. Se puede hacer referencia a un método de instancia utilizando la sintaxis `Clase::metodoDeInstancia`.
###### Referencia a método estático:
```java
class Ejemplo {
    static void metodoEstatico() {
        System.out.println("¡Hola desde el método estático!");
    }
    public static void main(String[] args) {
        // Referencia a método estático
        Runnable referenciaMetodo = Ejemplo::metodoEstatico;
        // Invocación del método utilizando la referencia
        referenciaMetodo.run();
    }
}
```
###### Referencia a constructor:
```java
class Persona {
    private String nombre;
    Persona(String nombre) {
        this.nombre = nombre;
    }
    public static void main(String[] args) {
        // Referencia a constructor
        Function<String, Persona> referenciaConstructor = Persona::new;
        // Creación de una instancia utilizando la referencia al constructor
        Persona persona = referenciaConstructor.apply("Juan");
        // Uso de la instancia creada
        System.out.println("Nombre: " + persona.getNombre());
    }
}
```
###### Referencia a método de instancia de una instancia concreta:
```java
class Persona {
    private String nombre;
    Persona(String nombre) {
        this.nombre = nombre;
    }
    void saludar() {
        System.out.println("¡Hola! Soy " + this.nombre);
    }
    public static void main(String[] args) {
        // Creación de una instancia de Persona
        Persona persona = new Persona("Juan");
        // Referencia a método de instancia de una instancia concreta
        Runnable referenciaMetodo = persona::saludar;
        // Invocación del método utilizando la referencia
        referenciaMetodo.run();
    }
}
```
###### Referencia a método de instancia sobre cualquier instancia:
```java
class Persona {
    private String nombre;
    Persona(String nombre) {
        this.nombre = nombre;
    }
    void saludar() {
        System.out.println("¡Hola! Soy " + this.nombre);
    }
    public static void main(String[] args) {
        // Referencia a método de instancia sobre cualquier instancia
        Consumer<Persona> referenciaMetodo = Persona::saludar;
        // Creación de una instancia de Persona
        Persona persona = new Persona("Juan");
        // Invocación del método utilizando la referencia
        referenciaMetodo.accept(persona);
    }
}
```
Hemos mostrado cómo hacer referencia a un método estático, a un constructor, a un método de instancia de una instancia concreta y a un método de instancia sobre cualquier instancia de una clase.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

Hay dos versiones de ordenar una lista de `Persona`, primero con una función de comparación manual y luego utilizando `Comparator` con una expresión lambda para comparar las edades y los nombres alfabéticamente en caso de que tengan la misma edad:
#### Versión con función de comparación manual:
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        // Crear lista de personas
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Juan", 25));
        personas.add(new Persona("María", 30));
        personas.add(new Persona("Pedro", 25));
        personas.add(new Persona("Ana", 20));
        // Ordenar la lista de personas con comparación manual
        Collections.sort(personas, (p1, p2) -> {
            // Comparar por edad
            int comparacion = Integer.compare(p1.getEdad(), p2.getEdad());
            if (comparacion == 0) {
                // Si las edades son iguales, comparar por nombre
                comparacion = p1.getNombre().compareTo(p2.getNombre());
            }
            return comparacion;
        });
        // Mostrar lista ordenada
        System.out.println("Lista ordenada:");
        for (Persona persona : personas) {
            System.out.println(persona);
        }
    }
}
```

#### Versión con Comparator y expresión lambda:
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        // Crear lista de personas
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Juan", 25));
        personas.add(new Persona("María", 30));
        personas.add(new Persona("Pedro", 25));
        personas.add(new Persona("Ana", 20));
        // Ordenar la lista de personas con Comparator y expresión lambda
        Collections.sort(personas, Comparator.comparingInt(Persona::getEdad)
                                                .thenComparing(Persona::getNombre));
        // Mostrar lista ordenada
        System.out.println("Lista ordenada:");
        for (Persona persona : personas) {
            System.out.println(persona);
        }
    }
}
```
En ambos, creamos una lista de `Persona`, la ordenamos utilizando `Collections.sort()` y luego mostramos la lista ordenada. En la primera versión, definimos manualmente una función de comparación que compara las edades y los nombres si las edades son iguales. En la segunda versión, utilizamos `Comparator.comparingInt()` y `thenComparing()` para construir un comparador que compara las edades y luego los nombres si las edades son iguales. Ambas versiones producirán el mismo resultado ordenado.
