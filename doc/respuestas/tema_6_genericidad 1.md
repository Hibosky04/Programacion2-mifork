<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

Claro, puedo darte ejemplos en C++ y Java que utilicen `void*` y `Object` respectivamente para crear una estructura de datos que pueda contener cualquier tipo de dato utilizando un array primitivo.
##### Ejemplo en C++ utilizando `void*`:
```c++
#include <iostream>
using namespace std;

class AnyArray {
private:
    void** array;
    int size;

public:
    AnyArray(int size) : size(size) {
        array = new void*[size];
    }

    void set(int index, void* data) {
        if (index >= 0 && index < size) {
            array[index] = data;
        } else {
            cerr << "Index out of bounds" << endl;
        }
    }

    void* get(int index) {
        if (index >= 0 && index < size) {
            return array[index];
        } else {
            cerr << "Index out of bounds" << endl;
            return nullptr;
        }
    }

    ~AnyArray() {
        delete[] array;
    }
};

int main() {
    AnyArray array(5);
    int intValue = 10;
    double doubleValue = 3.14;
    const char* stringValue = "Hello";

    array.set(0, &intValue);
    array.set(1, &doubleValue);
    array.set(2, (void*)stringValue);

	cout << "Int value: " << *(int*)array.get(0) << endl;
    cout << "Double value: " << *(double*)array.get(1) << endl;
    cout << "String value: " << (const char*)array.get(2) << endl;

    return 0;
}
```
Este código crea una clase `AnyArray` que permite almacenar cualquier tipo de dato utilizando `void*`. Se puede usar para almacenar enteros, flotantes, punteros, etc.
##### Ejemplo en Java utilizando `Object`:
```java
public class AnyArray {
    private Object[] array;

    public AnyArray(int size) {
        array = new Object[size];
    }

    public void set(int index, Object data) {
        if (index >= 0 && index < array.length) {
            array[index] = data;
        } else {
            System.err.println("Index out of bounds");
        }
    }

    public Object get(int index) {
        if (index >= 0 && index < array.length) {
            return array[index];
        } else {
            System.err.println("Index out of bounds");
            return null;
        }
    }

    public static void main(String[] args) {
        AnyArray array = new AnyArray(5);
        int intValue = 10;
        double doubleValue = 3.14;
        String stringValue = "Hello";

        array.set(0, intValue);
        array.set(1, doubleValue);
        array.set(2, stringValue);

        System.out.println("Int value: " + array.get(0));
        System.out.println("Double value: " + array.get(1));
        System.out.println("String value: " + array.get(2));
    }
}
```
Este código Java define una clase `AnyArray` que utiliza un array de `Object` para almacenar cualquier tipo de dato. Se pueden almacenar enteros, flotantes, cadenas, objetos, etc.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La programación genérica es un paradigma que permite escribir algoritmos y estructuras de datos para trabajar con diferentes tipos de datos sin especificarlos previamente. En lugar de trabajar con tipos concretos, se utilizan parámetros de tipo que representan tipos específicos, lo que hace que el código sea reutilizable y flexible al adaptarse automáticamente a distintos tipos de datos. Ejemplos básicos en Java y C++ muestran cómo se pueden almacenar objetos de cualquier tipo usando `Object` o `void*`, respectivamente. Sin embargo, la verdadera programación genérica moderna se basa en utilizar clases o funciones que trabajan con tipos genéricos a través de parámetros de tipo en lenguajes como Java, C# y C++.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El uso de `void*` en C++ o `Object` en Java para estructuras de datos genéricas presenta problemas relacionados con el chequeo de tipos. Primero, hay una pérdida de información de tipo en tiempo de compilación, lo que impide al compilador detectar errores hasta la ejecución. Además, los castings inseguros necesarios para recuperar los datos pueden causar excepciones o comportamientos inesperados si no coinciden con el tipo esperado. La falta de verificación de tipo en tiempo de compilación permite almacenar cualquier tipo de objeto, lo que puede provocar errores difíciles de depurar. Finalmente, la ausencia de soporte para la inferencia de tipos aumenta la complejidad y el potencial de errores al requerir castings explícitos. En contraste, lenguajes como C++ con plantillas o Java con genéricos ofrecen una programación genérica nativa más segura y mantenible. Por lo tanto, se recomienda utilizar estas características en lugar de `void*` o `Object` para garantizar un código más robusto.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los "parámetros de tipo" son variables que representan tipos de datos en lenguajes de programación como Java, C++ o C#. Se definen entre corchetes angulares `< >` y permiten escribir código genérico reutilizable con diferentes tipos de datos. Al definir una clase o función con un parámetro de tipo, se puede utilizar en lugar de un tipo específico. Al utilizar esa clase o función, se especifica el tipo concreto que se utilizará en lugar del parámetro de tipo.
En Java, puedes definir una clase genérica `Lista` con un parámetro de tipo `T` que represente el tipo de elementos que contendrá la lista:
```java
public class Lista<T> {
    private T[] elementos;

    public Lista(int capacidad) {
        elementos = (T[]) new Object[capacidad];
    }

    public void agregar(T elemento) {
        // Agregar elemento a la lista
    }

    // Otros métodos de la lista
}
```
 `T` es un parámetro de tipo que representa el tipo de elementos que contendrá la lista. Luego, al utilizar la clase `Lista`, se especifica el tipo concreto que se utilizará:
```java
Lista<Integer> listaDeEnteros = new Lista<>(10);
Lista<String> listaDeStrings = new Lista<>(10);
```
En este caso, `listaDeEnteros` contendrá enteros, mientras que `listaDeStrings` contendrá cadenas.
Los parámetros de tipo proporcionan flexibilidad y reutilización de código al permitir que las clases y funciones sean genéricas y puedan trabajar con diferentes tipos de datos de manera segura y eficiente.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

##### En Java utilizando generics:
```java
import java.util.ArrayList;

public class ListaDinamica<T> {
    private ArrayList<T> elementos;

    public ListaDinamica() {
        elementos = new ArrayList<>();
    }

    public void agregar(T elemento) {
        elementos.add(elemento);
    }

    public void mostrarElementos() {
        for (T elemento : elementos) {
            System.out.println("Tipo del elemento: " + elemento.getClass().getName() + ", Valor: " + elemento);
        }
    }

    public static void main(String[] args) {
        ListaDinamica<String> lista = new ListaDinamica<>();
        lista.agregar("Hola");
        lista.agregar("Mundo");
        lista.mostrarElementos();
    }
}
```
##### En C++ utilizando templates:
```cpp
#include <iostream>
#include <vector>
#include <string>

template<typename T>
class ListaDinamica {
private:
    std::vector<T> elementos;

public:
    void agregar(T elemento) {
        elementos.push_back(elemento);
    }

    void mostrarElementos() {
        for (const auto& elemento : elementos) {
            std::cout << "Tipo del elemento: " << typeid(elemento).name() << ", Valor: " << elemento << std::endl;
        }
    }
};

int main() {
    ListaDinamica<std::string> lista;
    lista.agregar("Hola");
    lista.agregar("Mundo");
    lista.mostrarElementos();

    return 0;
}
```
En ambos, se define una clase `ListaDinamica` utilizando generics en Java y templates en C++. Esta clase puede almacenar elementos de cualquier tipo, pero se instancian con el tipo `String`. Luego, se agregan algunos elementos a la lista y se recorren para mostrar el tipo concreto de cada elemento.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

Cuando se instancian clases con parámetros de tipo en Java y C++, el compilador realiza acciones específicas para garantizar la seguridad de tipos y la eficiencia del código, aunque los enfoques difieren entre ambos lenguajes.  
En Java, se utiliza el Type Erasure, donde el compilador reemplaza los parámetros de tipo con Object o su límite superior, eliminando la información de tipo del código generado. Además, se utiliza el casting implícito para preservar la compatibilidad.  
En C++, se lleva a cabo la instanciación de plantillas, generando código específico para cada combinación de tipos utilizados, sin realizar borrado de tipo como en Java. Cada instancia de una plantilla es tratada como una entidad independiente por el compilador, conservando toda la información de tipo.  
En conclusión, Java usa el borrado de tipo para interoperabilidad, mientras que C++ realiza la instanciación de plantillas sin eliminar la información de tipo.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

 La clase `Par` en Java que permite alojar dos valores de tipos diferentes y un ejemplo de cómo se puede utilizar para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`:
```java
public class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }

    public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
        double suma = 0;
        for (double num : array) {
            suma += num;
        }
        double media = suma / array.length;

        double sumaCuadrados = 0;
        for (double num : array) {
            sumaCuadrados += Math.pow(num - media, 2);
        }
        double desviacion = Math.sqrt(sumaCuadrados / array.length);

        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] numeros = {1.0, 2.0, 3.0, 4.0, 5.0};

        Par<Double, Double> resultado = calcularMediaYDesviacion(numeros);
        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación Típica: " + resultado.getSegundo());
    }
}
```
La clase `Par` tiene dos parámetros de tipo `T` y `U`, que representan los tipos de los dos valores que puede contener. El constructor permite inicializar los valores de los dos tipos, y los métodos `getPrimero` y `getSegundo` devuelven estos valores respectivamente.
La función `calcularMediaYDesviacion` toma un array de `double`, calcula su media y desviación típica, y las devuelve en un objeto `Par` donde el primer valor es la media y el segundo valor es la desviación típica.
En el método `main`, se crea un array de `double`, se llama a `calcularMediaYDesviacion` para obtener la media y desviación típica, y luego se muestra el resultado utilizando los getters de `Par`.

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

Un método genérico `seleccionaUno` en Java que toma dos objetos del mismo tipo y devuelve aleatoriamente uno de ellos. Mostraré la diferencia entre definirlo con dos `Object` y definirlo con dos parámetros de tipo en términos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo:
###### Definición del método con dos `Object`:
```java
import java.util.Random;

public class MetodoGenerico {

    public static Object seleccionaUno(Object obj1, Object obj2) {
        //Random random = new Random();
	    //boolean seleccion = random.nextBoolean();
	    //return seleccion ? obj1 : obj2;
        return new Random().nextBoolean()? obj1 : obj2;
    }

    public static void main(String[] args) {
        String cadena1 = "Hola";
        String cadena2 = "Mundo";
        Object resultado = seleccionaUno(cadena1, cadena2);
        System.out.println("Resultado: " + resultado);

        // Ejemplo con tipos diferentes
        Integer numero1 = 10;
        Double numero2 = 3.14;
        resultado = seleccionaUno(numero1, numero2);
        System.out.println("Resultado: " + resultado);
    }
}
```
###### Definición del método con dos parámetros de tipo:
```java
import java.util.Random;

public class MetodoGenerico {

    public static <T> T seleccionaUnoConTipo(T obj1, T obj2) {
	    //Random random = new Random();
	    //boolean seleccion = random.nextBoolean();
	    //return seleccion ? obj1 : obj2;
        return new Random().nextBoolean()? obj1 : obj2;
    }

    public static void main(String[] args) {
        String cad1 = "Hola";
        String cad2 = "Mundo";
        String resultado = seleccionaUnoConTipo(cad1, cad2);
        System.out.println("Resultado: " + resultado);

        // Ejemplo con tipos diferentes (da error de compilación)
        /*
        Integer numero1 = 10;
        Double numero2 = 3.14;
        resultado = seleccionaUnoConTipo(numero1, numero2);
        System.out.println("Resultado: " + resultado);
        */
    }
}
```
En el segundo, el método `seleccionaUnoConTipo` utiliza dos parámetros de tipo `T`, lo que garantiza que los dos objetos pasados como argumentos sean del mismo tipo. Además, elimina la necesidad de downcasting, ya que el tipo de retorno y los parámetros están fuertemente tipados. Si intentamos usar tipos diferentes como en el ejemplo comentado en el segundo `main`, obtendremos un error de compilación, lo que demuestra cómo el uso de parámetros de tipo puede ayudar a garantizar la seguridad de tipos en tiempo de compilación.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Sí, en Java se pueden establecer restricciones en los parámetros de tipo utilizando "bounded type parameters" (parámetros de tipo acotados). Esto permite especificar que un parámetro de tipo debe ser una subclase de cierto tipo, implementar cierta interfaz o tener ciertas propiedades.
##### Solución 1: Coordenadas de tipo `Number`
```java
public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otroPunto) {
        double distanciaX = x.doubleValue() - otroPunto.getX().doubleValue();
        double distanciaY = y.doubleValue() - otroPunto.getY().doubleValue();
        return Math.sqrt(distanciaX * distanciaX + distanciaY * distanciaY);
    }

    public static void main(String[] args) {
        Punto punto1 = new Punto(1, 2);
        Punto punto2 = new Punto(4.5, 6.7);

        double distancia = punto1.calcularDistanciaA(punto2);
        System.out.println("Distancia entre los puntos: " + distancia);
    }
}
```
##### Solución 2: Uso de generics con restricción numérica
```java
public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otroPunto) {
        double distanciaX = x.doubleValue() - otroPunto.getX().doubleValue();
        double distanciaY = y.doubleValue() - otroPunto.getY().doubleValue();
        return Math.sqrt(distanciaX * distanciaX + distanciaY * distanciaY);
    }

    public static void main(String[] args) {
        Punto<Integer> punto1 = new Punto<>(1, 2);
        Punto<Double> punto2 = new Punto<>(4.5, 6.7);

        double distancia = punto1.calcularDistanciaA(punto2);
        System.out.println("Distancia entre los puntos: " + distancia);
    }
}
```
En la segunda solución, se utiliza un tipo genérico `T` que extiende `Number`, lo que garantiza que `T` será un tipo numérico. Esto permite al programador saber con certeza que las coordenadas son números, y también permite al compilador realizar un chequeo de tipos más estricto.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

En la solución sin el uso de generics, donde se utilizan objetos de tipo `Number` para representar las coordenadas `x` y `y`, es posible crear un punto con una coordenada de tipo entero y la otra coordenada de tipo `double`. Sin embargo, esto puede llevar a una pérdida de precisión si se realiza algún cálculo aritmético con estas coordenadas, ya que `Number` es una clase base para todos los tipos de números en Java y puede requerir downcasting.
En cambio, con la solución que utiliza generics con restricción numérica, se refuerza el chequeo de tipos y se permite una mayor flexibilidad y seguridad en la manipulación de los puntos. En esta solución, al crear un objeto `Punto`, se especifica el tipo de coordenadas que se utilizarán. Por lo tanto, es posible crear un punto con una coordenada de tipo entero y la otra coordenada de tipo `double`, siempre y cuando ambas sean del mismo tipo numérico.
###### Solución sin generics:
```java
Punto punto = new Punto(1, 3.5);
Number x = punto.getX(); // Devuelve un objeto Number
```
Aquí, la variable `x` contendrá un objeto `Number` que puede ser un `Integer` o un `Double`, dependiendo de cómo se haya creado el punto. Sin embargo, al trabajar con este objeto, podríamos necesitar hacer downcasting para obtener la precisión necesaria en las operaciones aritméticas.
###### Solución con generics:
```java
Punto<Integer> punto = new Punto<>(1, 3);
Integer x = punto.getX(); // Devuelve un Integer
```
En este caso, la variable `x` contendrá un valor de tipo `Integer`, ya que se especificó que el tipo de coordenadas del punto es `Integer`. Esto proporciona una mayor seguridad en el chequeo de tipos y evita la necesidad de downcasting, lo que hace que el código sea más claro y menos propenso a errores.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

Para asegurarnos de que la sobreescritura del método `calcularDistanciaA(Punto p)` siempre sea sobre un `Punto` del mismo tipo y evitar el uso de `instanceof` y el downcasting, podemos utilizar generics en la interfaz `Punto` y en las clases `Punto2D` y `Punto3D`. Esto nos permitirá especificar el tipo concreto de `Punto` con el que estamos trabajando y garantizar que cualquier implementación de `Punto` que sobrescriba el método `distanciaA` lo haga con un parámetro del mismo tipo. Aquí tienes cómo hacerlo:
```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2));
    }
}
```
La interfaz `Punto` ahora toma un parámetro de tipo `T` que extiende `Punto<T>`. Esto significa que cualquier implementación de `Punto` debe implementar el método `distanciaA` con un parámetro del mismo tipo.
En las clases `Punto2D` y `Punto3D`, se implementa la interfaz `Punto` con el mismo parámetro de tipo que la clase. Esto asegura que el método `distanciaA` en cada clase tome un parámetro del mismo tipo que la propia clase, evitando la necesidad de `instanceof` y el downcasting.

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

- **¿Es `List<String>` subtipo de `List<Object>`?** **NO**. Los tipos genéricos en Java son **invariantes**. Esto significa que aunque `String` sea un `Object`, una lista de strings no guarda ninguna relación jerárquica con una lista de objetos.
- **¿Es `String[]` subtipo de `Object[]`?** **SÍ**. Los arrays en Java son **covariantes**
###### ¿Por qué la respuesta es diferente?
Los arrays se diseñaron antes que los genéricos. Se permitió la covarianza para poder escribir métodos generales (como `Arrays.sort()`) antes de que existiera la genericidad. Sin embargo, esto crea un **problema de seguridad en tiempo de ejecución**:
```java
String[] palabras = new String[2];
Object[] objetos = palabras; // Permitido por covarianza
objetos[0] = 10; // ¡ERROR en ejecución! ArrayStoreException
```
El compilador cree que está bien meter un `Integer` en un `Object[]`, pero en memoria ese array sigue siendo de `String`. Al ejecutarlo, el programa explota. **Los genéricos son invariantes precisamente para evitar este error** y que el fallo ocurra en tiempo de compilación.
###### Definiciones:
1. **Invariante:** No hay relación. `List<A>` no es subtipo ni supertipo de `List<B>`.
2. **Covariante:** La relación se mantiene. Si $A \subset B$, entonces $G\langle A \rangle \subset G\langle B \rangle$. (Ejemplo: Arrays).
3. **Contravariante:** La relación se invierte. Si $A \subset B$, entonces $G\langle B \rangle \subset G\langle A \rangle$.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un **wildcard** (`?`) representa un tipo desconocido. Se usa para relajar la invarianza cuando necesitamos que un método sea más flexible.
###### Diferencias: `? extends T` vs `? super T`
Usamos la regla **PECS**: _Producer Extends, Consumer Super_.
###### (i) `? extends T` (Covarianza - Productor)
Se usa cuando quieres **leer** de una estructura. Promete que lo que salga será, al menos, de tipo `T`.
```java
// Acepta List<Integer>, List<Double>, etc.
public static double sumarLista(List<? extends Number> lista) {
    double suma = 0;
    for (Number n : lista) {
        suma += n.doubleValue(); // Podemos leer como Number
    }
    return suma;
}
```
###### (ii) `? super T` (Contravarianza - Consumidor)
Se usa cuando quieres **escribir** en una estructura. Garantiza que la lista puede alojar objetos de tipo `T`.
```java
// Acepta List<Integer>, List<Number>, List<Object>
public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
    // No podemos leer con seguridad como Integer, porque podría ser una lista de Object
}
```
