<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

En Programación orientada a objetos (POO), la herencia es un mecanismo que permite a una clase (subclase) heredar características (atributos y métodos) de otra clase (superclase).
- **Compatibilidad de tipo:** La herencia permite tratar a un objeto de una subclase como si fuera un objeto de la superclase. Esto implica que dondequiera que se espere un objeto de la superclase, también se puede usar un objeto de cualquiera de sus subclase.
- **Herencia de estado y comportamiento:** La subclase hereda tanto los atributos como los métodos de la superclase. Esto significa que la subclase puede acceder a los atributos y métodos de la superclase y también puede agregar nuevos atributos y métodos propios.
```java
// Clase base Soldado
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("¡Hola, soy " + nombre + "!");
    }
}

// Subclase Artillero
class Artillero extends Soldado {
    private int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }

    public int getNumCohetes() {
        return numCohetes;
    }

    public void dispararCohete() {
        System.out.println("¡Disparando cohete!");
    }
}

// Subclase Zapador
class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }

    public void ponerMina() {
        System.out.println("¡Poniendo mina!");
    }
}

public class Main {
    public static void main(String[] args) {
        // Crear un array de Soldados
        Soldado[] soldados = new Soldado[3];
        soldados[0] = new Soldado("Soldado1");
        soldados[1] = new Artillero("Artillero1", 5);
        soldados[2] = new Zapador("Zapador1", 10);

        // Recorrer el array y hacer que todos saluden
        for (Soldado soldado : soldados) {
            soldado.saludar();
        }
    }
}
```
En la clase `Soldado` es la superclase que tiene un campo privado `nombre` y un método `saludar()`. Las clases `Artillero` y `Zapador` son subclases de `Soldado` y tienen campos adicionales (`numCohetes` y `numMinas`, respectivamente) y métodos especializados (`dispararCohete()` y `ponerMina()`). Luego, en el `main`, se crea un array de `Soldado`, y se puede colocar tanto instancias de `Soldado`, `Artillero` o `Zapador`, gracias a la compatibilidad de tipos. Luego, al recorrer el array y llamar al método `saludar()`, cada soldado (independientemente de ser Artillero, Zapador o Soldado genérico) saludará correctamente.

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

El orden en que se ejecutan los constructores es el siguiente: primero, se ejecuta el constructor de la superclase (`Soldado`) antes del constructor de la subclase (`Artillero` o `Zapador`). Esto se debe a que el constructor de la subclase implícitamente invoca al constructor de la superclase.
La palabra clave `super` dentro de un constructor se refiere a la clase padre (superclase) del objeto actual. Se utiliza para invocar al constructor de la clase base desde la subclase. En el ejemplo proporcionado, `super(nombre)` dentro de los constructores de `Artillero` y `Zapador` se utiliza para llamar al constructor de la clase base (`Soldado`) y pasarle el parámetro `nombre`.
Si la clase base no tiene un constructor sin parámetros y quieres crear instancias de las subclases, debes llamar explícitamente a un constructor de la superclase en cada constructor de las subclases, utilizando `super(...)`. No es necesario llamar a `super` si el constructor de la superclase sin parámetros está disponible y es accesible.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

**Sí, los atributos privados de la superclase forman parte de la instancia de la subclase en memoria.** Cuando se crea un objeto de una subclase, se reserva memoria para **todos** los atributos de la superclase, aunque sean privados.
Sin embargo, aunque estén en memoria, **no se pueden acceder directamente desde el código de la subclase** porque el modificador `private` impide el acceso desde fuera de la propia clase donde se declaran.
```java
class Soldado {
    private String nombre;  // Atributo privado
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("¡Hola, soy " + nombre + "!");
    }
}

class Artillero extends Soldado {
    private int numCohetes;
    
    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }
    
    // ERROR: No se puede acceder directamente a 'nombre'
    public void mostrarNombreIncorrecto() {
        // System.out.println(nombre);  // ❌ Error de compilación
    }
    
    // Forma correcta: usar métodos públicos de la superclase
    public void mostrarNombreCorrecto() {
        System.out.println("Mi nombre es: " + getNombre());  // ✓ Si hay getter
    }
}
```

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La compatibilidad a nivel de tipos implica una gran extensibilidad del código, ya que permite agregar nuevas subclases sin modificar el código existente que trabaja con la superclase. Esto es fundamental en términos de diseño orientado a objetos, ya que facilita la incorporación de nuevas funcionalidades y la expansión del sistema sin afectar el código existente.
Vamos a añadir un nuevo tipo de soldado llamado `Francotirador` que hereda de la clase `Soldado` y luego demostraremos que el código existente para pedir el saludo a todos los soldados no necesita ser modificado.
```java
// Nueva subclase Francotirador
class Francotirador extends Soldado {
    public Francotirador(String nombre) {
        super(nombre);
    }

    // Método específico para francotiradores
    public void apuntar() {
        System.out.println("¡Apuntando con precisión!");
    }
}

public class Main {
    public static void main(String[] args) {
        // Crear un array de Soldados que incluye un nuevo tipo de soldado (Francotirador)
        Soldado[] soldados = new Soldado[4];
        soldados[0] = new Soldado("Soldado1");
        soldados[1] = new Artillero("Artillero1", 5);
        soldados[2] = new Zapador("Zapador1", 10);
        soldados[3] = new Francotirador("Francotirador1"); // Nuevo tipo de soldado

        // Recorrer el array y hacer que todos saluden
        for (Soldado soldado : soldados) {
            soldado.saludar();
        }
    }
}
```
A pesar de esta adición, el código existente en la clase `Main` para pedir que todos los soldados saluden sigue siendo válido y no necesita ser modificado. Esto demuestra la extensibilidad del código: podemos agregar nuevas clases sin afectar el código existente que trabaja con la superclase `Soldado`.

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

En java se puede tener una referencia del supertipo que apunte a objetos reales de un subtipo. Este se conoce como upcasting. Se puede invocar con la referencia de supertipo a método público del subtipo, pero no se podía acceder a los métodos específicos del subtipo sin hacer downcasting explicito. El downcasting es la conservación de una referencia de un supertipo a un subtipo `instanceof` es un operador utilizado para verificar si un objeto es una instancia de una clase particular.

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] soldados = new Soldado[2];
        soldados[0] = new Soldado("Soldado1");
        soldados[1] = new Artillero("Artillero1", 5);

        for (Soldado soldado : soldados) {
            soldado.saludar();
            // Verificar si el soldado es un Artillero
            if (soldado instanceof Artillero) {
                // Downcasting: Convertir la referencia de Soldado a Artillero
                Artillero artillero = (Artillero) soldado;
                // Acceder al método específico de Artillero
                System.out.println("Número de cohetes: " + artillero.getNumCohetes());
            }
        }
    }
}
```
En este ejemplo, se recorre el array de `Soldado`, y si el objeto es un `Artillero` (verificado mediante `instanceof`), se realiza un downcasting de la referencia de `Soldado` a `Artillero` y se accede al método `getNumCohetes()` para imprimir el número de cohetes.

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso protegido significa que los miembros (atributos y métodos) de una clase son accesible dentro de la misma clase, de las clase derivadas (subclases) y de otra clases en el mismo paquete. En java, se implementa utilizando el modificador de acceso `protected` (`public` < `protected` < `private`).
Que ilustra cómo se implementa el acceso protegido en Java:
```java
// Clase base
public class Vehiculo {
    protected String marca; // Atributo protegido

    protected void mostrarMarca() { // Método protegido
        System.out.println("Marca del vehículo: " + marca);
    }
}

// Subclase que hereda de Vehiculo
public class Coche extends Vehiculo {
    public Coche(String marca) {
        this.marca = marca; // Se puede acceder al atributo protegido desde la subclase
    }

    public void mostrarDetalles() {
        mostrarMarca(); // Se puede llamar al método protegido desde la subclase
    }
}
```
La clase `Vehiculo` tiene un atributo protegido `marca` y un método protegido `mostrarMarca()`. La clase `Coche` hereda de `Vehiculo`, por lo que puede acceder tanto al atributo como al método protegido. Esto se ilustra en el constructor de `Coche`, donde se establece el valor de `marca`, y en el método `mostrarDetalles()`, donde se llama al método `mostrarMarca()`.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En la mayoría de los lenguajes orientados a objetos, incluido Java, hay una clase base implícita para todos los objetos. Esta clase base se conoce como la "clase base para todos los objetos" o "clase raíz". En Java, esta clase base se llama `Object`.
La clase `Object` en Java se encuentra en el paquete `java.lang` y es la superclase de todas las clases en Java. Esto significa que todas las clases de Java, ya sean directamente definidas por el usuario o proporcionadas por el propio lenguaje, son subclases directas o indirectas de la clase `Object`.
La clase `Object` proporciona métodos básicos que son heredados por todas las clases de Java, como `equals()`, `hashCode()`, `toString()`, `getClass()`, `wait()`, `notify()`, entre otros. Estos métodos son esenciales para el funcionamiento y la interoperabilidad de las clases en Java.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La herencia múltiple es un concepto en el que una clase puede heredar atributos y métodos de mas de una clase base. En Java, no admite herencia múltiple de clases lo cual significa que una clase no puede heredar de múltiples clases base directamente, sin embargo, Java admite herencia múltiple a través de interfaces, lo que permite que una clase implemente múltiples interfaces.
Por ejemplo:
```java
interface A {
    void metodoA();
}

interface B {
    void metodoB();
}

class MiClase implements A, B {
    public void metodoA() {
        System.out.println("Implementación de metodoA");
    }

    public void metodoB() {
        System.out.println("Implementación de metodoB");
    }
}
```

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

En Java, las excepciones personalizadas son clases que extienden la clase `Exception` o alguna de sus subclases, como `RuntimeException`. Aquí tienes un ejemplo de cómo crear una excepción personalizada llamada `UsuarioNoEncontradoException` que está compuesta con un objeto `Usuario` y que permite incluir una causa subyacente:
```java
public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;
    
    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }
    
    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }
    
    public Usuario getUsuario() {
        return usuario;
    }
}
```
En este ejemplo, `UsuarioNoEncontradoException` es una excepción personalizada que está compuesta con un objeto `Usuario`. Se proporcionan dos constructores: uno que acepta solo el objeto `Usuario` y otro que acepta tanto el objeto `Usuario` como una causa subyacente. Ambos constructores llaman al constructor de la superclase (`RuntimeException`) pasando un mensaje apropiado y, en el caso del segundo constructor, también pasan la causa subyacente.
```java
public class Ejemplo {
    public Usuario buscarUsuario(String nombreUsuario) {
        // Lógica para buscar el usuario
        Usuario usuario = null;
        if (usuario == null) {
            throw new UsuarioNoEncontradoException(new Usuario(nombreUsuario));
        }
        return usuario;
    }
}
```
En este ejemplo, si el usuario no es encontrado, se lanza una instancia de `UsuarioNoEncontradoException` con el objeto `Usuario` correspondiente. Esto proporciona información útil sobre el usuario que causó el problema. Si se tiene información adicional sobre la causa del error, se puede utilizar el segundo constructor para proporcionar una causa subyacente.

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

La herencia crea un **acoplamiento fuerte** (vínculo rígido) entre la superclase y la subclase. Si usamos herencia solo por "comodidad" para no escribir de nuevo un método, estamos forzando una relación jerárquica que puede no tener sentido lógico.
- **Rigidez:** Si la superclase cambia en el futuro, todas las subclases se ven afectadas obligatoriamente, incluso si ese cambio no les beneficia.
- **Jerarquías forzadas:** Podrías terminar con una clase `Pato` heredando de `Avión` solo porque ambos "vuelan", lo cual rompe la lógica del dominio y complica el mantenimiento.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

La frase _"Favor composition over inheritance"_ es un principio de diseño fundamental (propuesto en el libro _Design Patterns_).
- **Flexibilidad (Caja Negra):** En la composición, la relación es **"tiene-un"**. Puedes cambiar el comportamiento en tiempo de ejecución simplemente cambiando el objeto compuesto.
- **Menos acoplamiento:** La clase que contiene a la otra solo conoce su interfaz, no sus detalles internos.
- **Multiplicidad:** Una clase solo puede heredar de una (en Java), pero puede estar compuesta por **múltiples** objetos, permitiendo combinar funcionalidades de diversas fuentes.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

## 12. La herencia "rompe la encapsulación"

Se dice esto porque una subclase suele depender de los detalles de implementación de su superclase.
- **Dependencia interna:** Si el autor de la superclase decide cambiar cómo un método interno funciona (aunque mantenga la firma), puede romper accidentalmente el comportamiento de la subclase que dependía de esa lógica interna.
- **Exposición innecesaria:** Al heredar, la subclase expone todos los métodos públicos de la superclase, aunque no los necesite, violando el principio de que un objeto solo debe mostrar lo que es estrictamente necesario.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Opción A: Herencia (Relación "Es-Un")
Aquí, `Estudiante` **es una** `Persona`.
```java
class Persona {
    protected String dni;
    protected String nombre;
}

class Estudiante extends Persona {
    private String universidad;
    // Hereda dni y nombre automáticamente
}
```
### Opción B: Composición (Relación "Tiene-Un")
Aquí, `Estudiante` **tiene** `DatosPersonales`. Es más flexible porque podrías reutilizar `DatosPersonales` en otras clases que no sean necesariamente "Personas" en una jerarquía (como un `Cliente` o un `Avalista`).
```java
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    // Getters...
}

class Estudiante {
    private DatosPersonales datos; // Composición
    private String universidad;

    public Estudiante(DatosPersonales datos, String universidad) {
        this.datos = datos;
        this.universidad = universidad;
    }
}

class Trabajador {
    private DatosPersonales datos; // Composición
    private double salario;

    public Trabajador(DatosPersonales datos, double salario) {
        this.datos = datos;
        this.salario = salario;
    }
}
```
**Nota sobre la implementación:** En el modelo de composición, si quieres acceder al nombre desde el `Estudiante`, debes hacerlo a través del objeto `datos` (ej. `datos.getNombre()`), lo cual mantiene las responsabilidades separadas y bien organizadas.
