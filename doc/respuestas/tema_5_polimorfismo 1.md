<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo en la programación orientada a objetos permite a objetos de diferentes clases responder de manera distinta al mismo mensaje. Esto implica que diferentes clases pueden tener métodos con el mismo nombre pero con funcionalidades diferentes según el contexto. Esto facilita escribir código genérico y reutilizable, al poder tratar objetos de diferentes clases de forma uniforme si implementan la misma interfaz o heredan de la misma clase base. La sobreescritura de métodos en el polimorfismo permite a una subclase proporcionar una implementación específica de un método definido en su superclase, adaptándolo a sus propias necesidades.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La ligadura dinámica, también conocida como enlace tardío, es un proceso en el que la implementación específica de un método se determina en tiempo de ejecución en lugar de en tiempo de compilación. Esto significa que cuando se llama a un método de un objeto, el sistema determina qué versión específica de ese método debe ejecutarse según el tipo real del objeto en ese momento. La relación con el polimorfismo es estrecha, ya que la ligadura dinámica permite que el polimorfismo funcione correctamente. En C++, se logra utilizando punteros a objetos y métodos virtuales, mientras que en Java, todos los métodos son virtualmente enlazados por defecto. En Python, la ligadura dinámica es inherente al lenguaje y ocurre automáticamente en tiempo de ejecución, sin necesidad de declaraciones especiales. En resumen, la ligadura dinámica es esencial para el funcionamiento del polimorfismo y su implementación varía dependiendo del lenguaje de programación utilizado.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

en Java que ilustra el polimorfismo con un array de objetos Soldado que incluye tanto objetos de la clase Zapador como de la clase Artillero:
```java
// Definición de la clase Soldado
class Soldado {
    public void saludar() {
        System.out.println("¡Soldado reportándose para el deber!");
    }
}

// Definición de la subclase Zapador que sobreescribe el método saludar
class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("¡Zapador listo para la acción!");
    }
}

// Definición de la subclase Artillero
class Artillero extends Soldado {
    // No se sobreescribe el método saludar, utilizará el método de la clase base Soldado
}

// Clase principal para probar el polimorfismo
public class Main {
    public static void main(String[] args) {
        // Crear un array de Soldados con objetos de las clases Zapador y Artillero
        Soldado[] ejercito = new Soldado[2];
        ejercito[0] = new Zapador();
        ejercito[1] = new Artillero();
        
        // Recorrer el array y llamar al método saludar de cada Soldado
        for (Soldado soldado : ejercito) {
            soldado.saludar(); // Se invoca el método saludar, que mostrará el mensaje correspondiente de acuerdo al tipo de soldado
        }
    }
}
```
Este código creará un array de objetos Soldado que contiene un objeto Zapador y un objeto Artillero. Luego, recorre el array y llama al método saludar para cada objeto. Aunque todos los objetos son de tipo Soldado, la ligadura dinámica asegura que se ejecute la versión correcta del método saludar según el tipo real de cada objeto (Zapador o Artillero).

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Para invocar al método de la clase base desde una subclase en Java, se utiliza la palabra clave `super`. Aquí tienes el ejemplo modificado para que la subclase Zapador llame al método base para saludar de forma normal y luego añada "ZAPADOR A SUS ORDENES":
```java
// Definición de la subclase Zapador que sobreescribe el método saludar
class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // Llama al método saludar de la clase base Soldado
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```
Con esta modificación, cuando un objeto Zapador llame al método `saludar()`, primero se ejecutará el método `saludar()` de la clase base `Soldado`, que muestra el mensaje "¡Soldado reportándose para el deber!". Luego, el método de la clase Zapador añadirá "ZAPADOR A SUS ORDENES", resultando en la salida completa "¡Soldado reportándose para el deber! ZAPADOR A SUS ORDENES".

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Es fundamental no confundir estos dos conceptos, ya que operan en momentos distintos del ciclo de vida del software.
Para que una sobreescritura sea válida, el método en la subclase debe cumplir con lo siguiente:
1. **Misma firma:** El nombre del método y la lista de parámetros (tipo, orden y cantidad) deben ser **exactamente iguales**.
2. **Tipo de retorno:** Debe ser el mismo o un **subtipo** del original (esto se conoce como _retorno covariante_).
3. **Visibilidad:** No puede ser más restrictiva que el método original. Si el padre es `public`, el hijo debe ser `public`. Si el padre es `protected`, el hijo puede ser `protected` o `public`, pero nunca `private`.
4. **Excepciones:** El método sobreescrito no puede lanzar excepciones nuevas o más generales que las declaradas en el método de la superclase.
Las diferencias clave son:

|**Característica**|**Sobreescritura (Overriding)**|**Sobrecarga (Overloading)**|
|---|---|---|
|**Relación de clases**|Ocurre entre una **superclase y una subclase**.|Ocurre en la **misma clase**.|
|**Parámetros**|Deben ser **idénticos**.|Deben ser **diferentes** (en tipo o cantidad).|
|**Ligadura**|Dinámica (en tiempo de ejecución).|Estática (en tiempo de compilación).|
|**Objetivo**|Redefinir un comportamiento existente.|Ofrecer varias formas de hacer algo similar con distintos datos.|
La anotación `@Override` es una instrucción para el **compilador**. No cambia el funcionamiento del código, pero es una "red de seguridad" fundamental.
Informa al compilador que tu intención es sobreescribir un método de la clase padre. Si por error cometes un fallo tipográfico (ej. escribes `saluudar()` en vez de `saludar()`) o te equivocas en un parámetro, el compilador lanzará un **error inmediato**.
El porque es recomendable usarlo siempre es:
1. **Evita errores silenciosos:** Sin la anotación, si te equivocas en la firma, Java creerá que estás creando un método nuevo (sobrecarga) en lugar de sobreescribir el del padre. Tu polimorfismo fallará y será muy difícil de depurar.
2. **Documentación clara:** Al leer el código, cualquier programador sabe instantáneamente que ese método proviene de una jerarquía superior.
3. **Mantenimiento:** Si en el futuro alguien cambia la firma del método en la clase base, todas las subclases que usen `@Override` darán error de compilación, avisándote de que debes actualizar el código en toda la cadena.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

El polimorfismo se encuentra comúnmente al sobrescribir métodos como `toString()` o `equals()` en Java. Al sobrescribir `toString()` en una clase específica para proporcionar una representación significativa del objeto, se utiliza el polimorfismo, ya que se ejecutará la versión específica del método definida para esa clase. Del mismo modo, al sobrescribir `equals()` para definir un comportamiento personalizado de comparación de igualdad entre objetos, se hace uso del polimorfismo, ya que la implementación puede diferir entre clases y el método llamado dependerá del tipo de objeto en tiempo de ejecución. En resumen, sobrescribir métodos como `toString()` o `equals()` en Java implica el uso de polimorfismo, donde la implementación específica del método se determina en tiempo de ejecución según el tipo real del objeto.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una clase abstracta en Java sirve como plantilla para otras clases, con métodos concretos y abstractos. Los métodos abstractos no tienen implementación en la clase abstracta, pero deben ser implementados por las subclases. No se pueden crear instancias directas de una clase abstracta, únicamente se utilizan para definir comportamientos comunes y obligar a las subclases a implementar métodos específicos. La palabra clave `abstract` se coloca antes de `class` en la definición de la clase abstracta y antes del tipo de retorno en la firma de un método abstracto. 
Añade un método abstracto `atacar()` y utiliza las clases Zapador y Artillero para proporcionar implementaciones concretas para este método:
```java
// Definición de la clase abstracta Soldado
abstract class Soldado {
    // Método abstracto atacar que debe ser implementado por las subclases
    public abstract void atacar();
}

// Definición de la subclase Zapador
class Zapador extends Soldado {
    // Implementación del método atacar específica para el Zapador
    @Override
    public void atacar() {
        System.out.println("¡Zapador desplegando explosivos!");
    }
}

// Definición de la subclase Artillero
class Artillero extends Soldado {
    // Implementación del método atacar específica para el Artillero
    @Override
    public void atacar() {
        System.out.println("¡Artillero disparando cañón!");
    }
}

// Clase principal para probar las clases Soldado, Zapador y Artillero
public class Main {
    public static void main(String[] args) {
        // Crear instancias de Zapador y Artillero
        Zapador zapador = new Zapador();
        Artillero artillero = new Artillero();
        
        // Llamar al método atacar de cada soldado
        zapador.atacar();
        artillero.atacar();
    }
}
```
En este ejemplo, la clase Soldado es abstracta y define el método `atacar()` como abstracto. Las subclases Zapador y Artillero implementan este método, proporcionando una implementación específica para cada tipo de soldado.

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

###### 1. El efecto de `final` en Clases y Métodos
- **En una Clase:** Cuando declaras una clase como `final`, **no puede tener subclases**. Se impide la herencia. Esto se usa habitualmente por motivos de seguridad o diseño, para asegurar que el comportamiento de la clase no sea alterado por terceros.
    - _Sintaxis:_ `public final class MiClase { ... }`
- **En un Método:** Un método marcado como `final` **no puede ser sobreescrito** (`override`) por ninguna subclase. La subclase heredará el método y podrá usarlo, pero estará obligada a usar la implementación exacta de la superclase.
    - _Sintaxis:_ `public final void miMetodo() { ... }`
###### 2. Relación con el Polimorfismo
El polimorfismo se basa en la capacidad de las subclases de redefinir comportamientos (sobreescritura) y en la creación de jerarquías de tipos (herencia). Por tanto, `final` es el **antagonista del polimorfismo**:
1. **Corta la jerarquía:** Al hacer una clase `final`, eliminas la posibilidad de que existan subtipos de esa clase. No podrás hacer _Upcasting_ desde una clase hija porque, sencillamente, no puede haber hijas.
2. **Elimina la Ligadura Dinámica:** En un método `final`, el compilador sabe exactamente qué código se va a ejecutar (enlace estático), ya que no existe ninguna versión alternativa del método en el árbol de herencia. Esto puede ofrecer una ligera mejora de rendimiento (inlining), aunque hoy en día los JIT compilers lo hacen de forma automática.
###### 3. Ejemplos en la API estándar de Java
Existen varias clases fundamentales en Java que son `final` por diseño, principalmente para garantizar la inmutabilidad y la seguridad del sistema:
- **`java.lang.String`**: Es el ejemplo más famoso. Si `String` no fuera `final`, alguien podría crear una subclase `MiString` que fingiera ser una cadena normal pero que enviara los datos (como contraseñas) a un servidor externo. Al ser `final`, Java garantiza que un `String` siempre se comporta como un `String`.
- **`java.lang.Integer`**, **`Double`**, etc. (Clases Wrapper): Todas las clases que envuelven tipos primitivos son `final`.
- **`java.lang.Math`**: Es una clase de utilidad que no tiene sentido heredar, por lo que está cerrada a la extensión.
- **`java.util.Scanner`**: También está marcada como `final` para evitar alteraciones en la lógica de lectura de entrada.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

En Java, una interfaz es una colección de métodos abstractos y constantes usados para definir métodos que una clase debe implementar, sin implementaciones de métodos. A diferencia de clases abstractas, interfaces solo tienen métodos abstractos y constantes. Son similares a clases abstractas en que definen un contrato, pero difieren:  
- Implementación de métodos: Interfaces solo tienen métodos abstractos, mientras clases abstractas pueden tener métodos concretos.  
- Herencia: Una clase puede extender una clase abstracta o implementar múltiples interfaces. Así, una clase puede cumplir múltiples contratos a través de varias interfaces. En Java, una clase puede implementar más de una interfaz, permitiendo herencia múltiple sin problemas.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

Vamos a definir las clases `Punto` y `Linea` siguiendo las especificaciones mencionadas. Utilizaremos la técnica de downcasting para verificar y calcular la distancia entre puntos 2D y 3D de manera adecuada. Luego, implementaremos la clase `Linea` que acepta objetos de tipo `Punto` y puede calcular su longitud sin conocer su tipo específico. Aquí tienes el código:
```java
// Definición de la clase abstracta Punto
abstract class Punto {
    // Método abstracto para calcular la distancia a otro punto
    public abstract double calcularDistanciaA(Punto otro);
}

// Definición de la subclase Punto2D
class Punto2D extends Punto {
    private double x;
    private double y;
    
    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    @Override
    public double calcularDistanciaA(Punto otro) {
        // Verificar si el otro punto es compatible
        if (otro instanceof Punto2D) {
            Punto2D punto2D = (Punto2D) otro; // Downcasting
            // Calcular distancia entre puntos 2D
            return Math.sqrt(Math.pow(x - punto2D.x, 2) + Math.pow(y - punto2D.y, 2));
        } else {
            throw new IllegalArgumentException("El punto recibido no es de tipo Punto2D");
        }
    }
}

// Definición de la subclase Punto3D
class Punto3D extends Punto {
    private double x;
    private double y;
    private double z;
    
    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }
    
    @Override
    public double calcularDistanciaA(Punto otro) {
        // Verificar si el otro punto es compatible
        if (otro instanceof Punto3D) {
            Punto3D punto3D = (Punto3D) otro; // Downcasting
            // Calcular distancia entre puntos 3D
            return Math.sqrt(Math.pow(x - punto3D.x, 2) + Math.pow(y - punto3D.y, 2) + Math.pow(z - punto3D.z, 2));
        } else {
            throw new IllegalArgumentException("El punto recibido no es de tipo Punto3D");
        }
    }
}

// Definición de la clase Linea
class Linea {
    private Punto puntoInicial;
    private Punto puntoFinal;
    
    public Linea(Punto puntoInicial, Punto puntoFinal) {
        this.puntoInicial = puntoInicial;
        this.puntoFinal = puntoFinal;
    }
    
    public double calcularLongitud() {
        // Calcular la longitud de la línea utilizando el método calcularDistanciaA de Punto
        return puntoInicial.calcularDistanciaA(puntoFinal);
    }
}

// Clase principal para probar el código
public class Main {
    public static void main(String[] args) {
        // Crear puntos 2D y 3D
        Punto punto2D = new Punto2D(0, 0);
        Punto punto3D = new Punto3D(0, 0, 0);
        
        // Crear línea utilizando puntos de distintas dimensiones
        Linea linea = new Linea(punto2D, punto3D);
        
        // Calcular la longitud de la línea
        double longitud = linea.calcularLongitud();
        System.out.println("Longitud de la línea: " + longitud);
    }
}
```
En este ejemplo, la clase `Punto` es abstracta y define un método abstracto `calcularDistanciaA(Punto otro)`. Las subclases `Punto2D` y `Punto3D` implementan este método de manera específica para calcular la distancia entre puntos 2D y 3D, respectivamente. La clase `Linea` acepta objetos de tipo `Punto` y puede calcular su longitud utilizando el método `calcularDistanciaA()` sin necesidad de conocer el tipo específico de los puntos.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La herencia de interfaces en Java es un concepto que permite que una interfaz adquiera los métodos definidos en otra interfaz. Cuando una interfaz hereda de otra, la subinterfaz (la que está extendiendo) incluye todos los métodos declarados en la interfaz base.
A diferencia de la herencia de clases, donde una clase solo puede heredar de una sola clase base, en Java una interfaz puede extender múltiples interfaces. Esto significa que una interfaz puede heredar métodos de múltiples interfaces, lo que proporciona una forma de lograr un tipo de herencia múltiple en Java.
Aquí tienes un ejemplo que demuestra la herencia de interfaces en Java:
```java
// Definición de la interfaz Fichero que define un método para leer su contenido como String
interface Fichero {
    String leerContenido();
}

// Definición de la interfaz FicheroEscribible que extiende la interfaz Fichero
interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}

// Implementación de la interfaz FicheroEscribible
class FicheroImpl implements FicheroEscribible {
    private String contenido;
    
    @Override
    public String leerContenido() {
        return contenido;
    }
    
    @Override
    public void escribirContenido(String contenido) {
        this.contenido = contenido;
    }
    
    @Override
    public void eliminar() {
        this.contenido = null;
    }
}

// Clase principal para probar las interfaces de Fichero y FicheroEscribible
public class Main {
    public static void main(String[] args) {
        FicheroEscribible fichero = new FicheroImpl();
        fichero.escribirContenido("Hola, este es el contenido del fichero.");
        System.out.println("Contenido del fichero: " + fichero.leerContenido());
        fichero.eliminar();
        System.out.println("Contenido del fichero después de eliminar: " + fichero.leerContenido());
    }
}
```
En este ejemplo, la interfaz `FicheroEscribible` extiende la interfaz `Fichero`, lo que significa que hereda el método `leerContenido()` de la interfaz base y añade los métodos `escribirContenido()` y `eliminar()`. La clase `FicheroImpl` implementa la interfaz `FicheroEscribible`, proporcionando implementaciones para todos los métodos heredados y añadidos.