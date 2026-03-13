<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

En C que implementa una línea compuesta por dos puntos y funciones para calcular la distancia entre puntos y la longitud de la línea:
```c
#include <stdio.h>
#include <math.h>

// Definición de la estructura Punto
struct Punto {
    double x;
    double y;
};

// Definición de la estructura Linea compuesta por dos puntos
struct Linea {
    struct Punto punto1;
    struct Punto punto2;
};

// Función para calcular la distancia entre dos puntos
double distanciaEntrePuntos(struct Punto punto1, struct Punto punto2) {
    return sqrt(pow(punto2.x - punto1.x, 2) + pow(punto2.y - punto1.y, 2));
}

// Función para calcular la longitud de una línea
double longitudDeLinea(struct Linea linea) {
    return distanciaEntrePuntos(linea.punto1, linea.punto2);
}

int main() {
    // Crear dos puntos
    struct Punto puntoA = {3.0, 4.0};
    struct Punto puntoB = {6.0, 8.0};

    // Crear una línea con los dos puntos
    struct Linea lineaAB = {puntoA, puntoB};

    // Calcular la distancia entre los dos puntos
    double distancia = distanciaEntrePuntos(puntoA, puntoB);
    printf("La distancia entre los puntos es: %.2f\n", distancia);

    // Calcular la longitud de la línea
    double longitud = longitudDeLinea(lineaAB);
    printf("La longitud de la línea es: %.2f\n", longitud);

    return 0;
}
```

Este código define dos estructuras, `Punto` y `Linea`. La estructura `Punto` tiene dos campos `x` e `y` para almacenar las coordenadas del punto en el plano. La estructura `Linea` contiene dos campos de tipo `Punto`, lo que permite que represente una línea compuesta por dos puntos. Las funciones `distanciaEntrePuntos()` y `longitudDeLinea()` calculan la distancia entre dos puntos y la longitud de una línea, respectivamente. Luego, en la función `main()`, se crean dos puntos y se calcula la distancia entre ellos y la longitud de la línea que forman.

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

En Java utilizando orientación a objetos con las clases `Punto` y `Linea`, donde se garantiza la inmutabilidad de los puntos y la línea:
```java
import java.lang.Math;

// Clase Punto
class Punto {
    private final double x;
    private final double y;

    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método para calcular la distancia a otro punto
    public double calcularDistancia(Punto otroPunto) {
        double dx = this.x - otroPunto.x;
        double dy = this.y - otroPunto.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    // Getters para obtener las coordenadas del punto
    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }
}

// Clase Linea
class Linea {
    private final Punto punto1;
    private final Punto punto2;

    // Constructor
    public Linea(Punto punto1, Punto punto2) {
        this.punto1 = punto1;
        this.punto2 = punto2;
    }

    // Método para calcular la longitud de la línea
    public double calcularLongitud() {
        return punto1.calcularDistancia(punto2);
    }
}

public class Main {
    public static void main(String[] args) {
        // Crear dos puntos
        Punto puntoA = new Punto(3.0, 4.0);
        Punto puntoB = new Punto(6.0, 8.0);

        // Crear una línea con los dos puntos
        Linea lineaAB = new Linea(puntoA, puntoB);

        // Calcular la distancia entre los dos puntos
        double distancia = puntoA.calcularDistancia(puntoB);
        System.out.println("La distancia entre los puntos es: " + distancia);

        // Calcular la longitud de la línea
        double longitud = lineaAB.calcularLongitud();
        System.out.println("La longitud de la línea es: " + longitud);
    }
}

```
En este código, la clase `Punto` tiene dos campos `x` e `y` que son finales (usando `final`), lo que significa que una vez inicializados en el constructor, no pueden cambiar. La clase `Linea` también tiene dos campos `punto1` y `punto2`, los cuales también son finales, asegurando así que una vez creada una línea, sus puntos no pueden ser modificados. Las clases `Punto` y `Linea` tienen métodos para calcular la distancia entre puntos y la longitud de la línea, respectivamente. Esto proporciona una forma más segura y encapsulada de trabajar con puntos y líneas en comparación con el ejemplo en C.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

En el contexto de la composición en la programación orientada a objetos, la multiplicidad se refiere a la cantidad de instancias de una clase que pueden estar asociadas con instancias de otra clase. En otras palabras, describe cuántos objetos de una clase están relacionados con uno o varios objetos de otra clase.
En el ejemplo anterior, la multiplicidad entre la clase `Linea` y la clase `Punto` es de dos puntos. Esto se debe a que una línea está compuesta por exactamente dos puntos. Cada instancia de la clase `Linea` está asociada con exactamente dos instancias de la clase `Punto`, uno para el punto inicial de la línea (`punto1`) y otro para el punto final de la línea (`punto2`). Por lo tanto, la multiplicidad entre `Linea` y `Punto` es de "2 a 1". Esto significa que cada instancia de `Linea` tiene dos instancias de `Punto`.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La composición (en general), los atributos de A tiene el tipo B, la relación es relativamente duradera ya que va ha haber atributos o arrays por ahí y hay dos tipos: 
- Débil: El ciclo de vida de B no está  vinculados al de A, los objetos contenidos pueden existir independientemente del objeto contenedor. No hay una dependencia directa en el ciclo de vida. En la composición débil, los objetos contenidos pueden ser creados y destruidos independientemente del objeto contenedor. A menudo se hace referencia  a la composición débil como "asociación" o "agregación".
- Fuerte: El ciclo de vida de B está vinculados al de A, implica que los objetos contenidos no pueden existir sin el objeto contenedor. Esto significa que el ciclo de vida de los objetos contenidos está directamente vinculados al ciclo de vida del objeto contador. La composición fuerte se refiere comúnmente como "composición" propiamente dicha.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

La **dependencia** ocurre cuando una clase utiliza los servicios de otra, recibiéndola o devolviéndola como parámetro en métodos, creando instancias o utilizándola como variables locales. Esta relación no implica que una clase sea parte integral de la otra ni que compartan estrechamente su ciclo de vida. La clase dependiente usa otra clase para su implementación o para realizar operaciones específicas, pero puede existir y funcionar de manera independiente. La dependencia es más flexible y débil que la composición, dado que no establece una conexión estrecha entre las clases. En resumen, implica un uso de una clase por parte de otra sin una fuerte vinculación.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

###### Composición Fuerte (Composición Propiamente Dichas):
En este caso, la clase `Linea` controlará el ciclo de vida de los objetos de la clase `Punto`. Esto significa que cuando una instancia de `Linea` se destruya, también se destruirán automáticamente los puntos asociados.
```java
class Linea {
    private final Punto punto1;
    private final Punto punto2;

    // Constructor
    public Linea(double x1, double y1, double x2, double y2) {
        this.punto1 = new Punto(x1, y1);
        this.punto2 = new Punto(x2, y2);
    }

    // Métodos para obtener los puntos de la línea
    public Punto getPunto1() {
        return punto1;
    }

    public Punto getPunto2() {
        return punto2;
    }
}

public class Main {
    public static void main(String[] args) {
        // Crear una línea
        Linea lineaAB = new Linea(3.0, 4.0, 6.0, 8.0);

        // Obtener los puntos de la línea
        Punto puntoA = lineaAB.getPunto1();
        Punto puntoB = lineaAB.getPunto2();

        // Operar con los puntos si es necesario...
    }
}

```

###### Composición Débil (Agregación o Asociación):
En este caso, la clase `Linea` simplemente "tiene" puntos, pero no controla su ciclo de vida. Los puntos pueden existir independientemente de la línea y podrían ser compartidos por múltiples líneas u otras clases.
```java
class Linea {
    private final Punto punto1;
    private final Punto punto2;

    // Constructor
    public Linea(Punto punto1, Punto punto2) {
        this.punto1 = punto1;
        this.punto2 = punto2;
    }

    // Métodos para obtener los puntos de la línea
    public Punto getPunto1() {
        return punto1;
    }

    public Punto getPunto2() {
        return punto2;
    }
}

public class Main {
    public static void main(String[] args) {
        // Crear dos puntos
        Punto puntoA = new Punto(3.0, 4.0);
        Punto puntoB = new Punto(6.0, 8.0);

        // Crear una línea con los puntos
        Linea lineaAB = new Linea(puntoA, puntoB);

        // Operar con los puntos y la línea si es necesario...
    }
}

```

En la composición fuerte, la línea crea y controla los puntos, mientras que en la composición débil, los puntos son creados independientemente y luego se pasan como argumentos al constructor de la línea.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java, en una composición fuerte, como en el caso de una `Linea` que contiene dos objetos `Punto`, los objetos `Punto` son destruidos por el recolector de basura cuando ya no son accesibles y no tienen referencias que los mantengan en memoria. Cuando una instancia de `Linea` se vuelve inaccesible, los objetos `Punto` se convierten en candidatos para ser recolectados. La `Linea` no destruye explícitamente los objetos `Punto`, simplemente pierde la referencia a ellos. Una vez que ya no hay referencias a `Punto`, estos se vuelven elegibles para la recolección, aunque el momento de recolección es determinado por el recolector y no por el programador. Por lo tanto, no es necesario destruir los objetos explícitamente, simplificando la gestión de memoria y reduciendo errores en la liberación de memoria.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

Aquí tienes la implementación en Java que cumple con los requisitos descritos:
```java 
public class Departamento {
    private Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    // Constructor
    public Departamento(Profesor director) {
        this.profesores = new Profesor[50];
        this.numProfesores = 0;
        this.director = director;
        this.profesores[numProfesores++] = director; // Añadir director como primer profesor
    }

    // Método para añadir un profesor al departamento
    public void añadirProfesor(Profesor nuevoProfesor) throws Exception {
        if (numProfesores >= 50) {
            throw new Exception("El departamento ya tiene el máximo de profesores permitidos.");
        }

        profesores[numProfesores++] = nuevoProfesor;
    }

    // Método para eliminar un profesor por posición
    public void eliminarProfesor(int posicion) throws Exception {
        if (posicion < 1 || posicion >= numProfesores) {
            throw new Exception("Posición inválida.");
        }

        // Si el profesor a eliminar es el director, lanzar excepción
        if (profesores[posicion] == director) {
            throw new Exception("No se puede eliminar al director del departamento.");
        }

        // Desplazar los profesores hacia la izquierda para eliminar al profesor
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        numProfesores--;
    }

    // Método para cambiar el director del departamento
    public void cambiarDirector(Profesor nuevoDirector) throws Exception {
        if (nuevoDirector == null) {
            throw new Exception("El nuevo director no puede ser nulo.");
        }

        if (numProfesores >= 50) {
            throw new Exception("El departamento ya tiene el máximo de profesores permitidos.");
        }

        if (!esProfesorDelDepartamento(nuevoDirector)) {
            throw new Exception("El nuevo director debe ser un profesor del departamento.");
        }

        director = nuevoDirector;
    }

    // Método privado para verificar si un profesor pertenece al departamento
    private boolean esProfesorDelDepartamento(Profesor profesor) {
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == profesor) {
                return true;
            }
        }
        return false;
    }

    // Método para obtener el número de profesores en el departamento
    public int getNumProfesores() {
        return numProfesores;
    }

    // Método para obtener un profesor por posición
    public Profesor getProfesor(int posicion) throws Exception {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new Exception("Posición inválida.");
        }

        return profesores[posicion];
    }

    // Otros métodos de la clase Profesor según sea necesario...
}

class Profesor {
    // Atributos, constructores y métodos de la clase Profesor
}

```

En la clase `Departamento` representa un departamento universitario que contiene un conjunto de profesores. Se utiliza un array de tamaño fijo para almacenar los profesores, permitiendo añadir y eliminar profesores manteniendo la encapsulación. La clase `Departamento` tiene un atributo para el director del departamento, que también es un profesor perteneciente al mismo. Se implementan métodos para añadir, eliminar y cambiar al director, y se lanzan excepciones si se viola alguna de las invariantes de clase.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

El código modificado usando `List` en lugar de arrays primitivos:
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Departamento {
    private List<Profesor> profesores;
    private Profesor director;
    private static final int MAX_PROFESORES = 50;

    // Constructor
    public Departamento(Profesor director) {
        this.profesores = new ArrayList<>();
        this.director = director;
        this.profesores.add(director); // Añadir director como primer profesor
    }

    // Método para añadir un profesor al departamento
    public void añadirProfesor(Profesor nuevoProfesor) throws Exception {
        if (profesores.size() >= MAX_PROFESORES) {
            throw new Exception("El departamento ya tiene el máximo de profesores permitidos.");
        }
        profesores.add(nuevoProfesor);
    }

    // Método para eliminar un profesor por posición
    public void eliminarProfesor(int posicion) throws Exception {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new Exception("Posición inválida.");
        }

        // Si el profesor a eliminar es el director, lanzar excepción
        if (profesores.get(posicion) == director) {
            throw new Exception("No se puede eliminar al director del departamento.");
        }

        profesores.remove(posicion);
    }

    // Método para cambiar el director del departamento
    public void cambiarDirector(Profesor nuevoDirector) throws Exception {
        if (nuevoDirector == null) {
            throw new Exception("El nuevo director no puede ser nulo.");
        }

        if (!profesores.contains(nuevoDirector)) {
            throw new Exception("El nuevo director debe ser un profesor del departamento.");
        }

        director = nuevoDirector;
    }

    // Método para obtener el número de profesores en el departamento
    public int getNumProfesores() {
        return profesores.size();
    }

    // Método para obtener un profesor por posición
    public Profesor getProfesor(int posicion) throws Exception {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new Exception("Posición inválida.");
        }
        return profesores.get(posicion);
    }

    // Método que devuelve una copia defensiva de la lista de profesores
    public List<Profesor> getProfesores() {
        return new ArrayList<>(profesores); // Copia defensiva
    }

    // Método que devuelve la lista de profesores como no modificable
    public List<Profesor> getProfesoresNoModificable() {
        return Collections.unmodifiableList(profesores);
    }
}

class Profesor {
    // Atributos, constructores y métodos de la clase Profesor
}
```

Nos ahorramos:
1. **Gestión manual del array**: Me he ahorrado tener que llevar un contador `numProfesores` y verificar manualmente los límites del array.
2. **Desplazamiento manual de elementos**: En el método `eliminarProfesor`, ya no necesito el bucle para desplazar elementos hacia la izquierda - `ArrayList.remove()` lo hace automáticamente.
3. **Inicialización del array**: No necesito crear el array con tamaño fijo inicial.
4. **Método `esProfesorDelDepartamento`**: Lo he eliminado porque `List.contains()` proporciona directamente esta funcionalidad.

El problema de devolver la lista interna  es si existiera un método que devolviera todos los profesores a la vez (por ejemplo, `getProfesores()`), **devolver directamente la lista interna (`return profesores;`) tendría el problema de que el código cliente podría modificar la lista interna del departamento**, violando el encapsulamiento.
Por ejemplo, alguien podría hacer:
```java
departamento.getProfesores().remove(0); // ¡Esto modificaría la lista interna!
```


Para solucionar el problema he incluido dos soluciones en el código:
1. **Copia defensiva**: `return new ArrayList<>(profesores);` - Devuelve una copia de la lista, protegiendo la original.
2. **Vista no modificable**: `return Collections.unmodifiableList(profesores);` - Devuelve una vista de la lista que lanza excepción si se intenta modificar.
La primera opción es más segura pero tiene overhead de copia. La segunda opción es más eficiente pero lanza excepción en lugar de permitir modificaciones. La elección depende de los requisitos específicos.


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

Para ilustrar una composición recursiva en Java, implementaré una clase `Persona` que es inmutable y tiene una referencia a su madre, que también es una instancia de `Persona`. 
```java
public class Persona {
    private final String nombre;
    private final Persona madre;

    // Constructor
    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    // Método para obtener el nombre de la persona
    public String getNombre() {
        return nombre;
    }

    // Método para obtener la madre de la persona
    public Persona getMadre() {
        return madre;
    }

    // Método para verificar si la persona tiene madre
    public boolean tieneMadre() {
        return madre != null;
    }

    // Método para representar la información de la persona
    @Override
    public String toString() {
        return "Nombre: " + nombre + ", Madre: " + (madre != null ? madre.getNombre() : "Desconocida");
    }
}

```
Ahora, podemos utilizar esta clase para crear una familia de personas desde el nieto hasta la abuela en el método `main`:
```java
public class Main {
    public static void main(String[] args) {
        // Crear la abuela
        Persona abuela = new Persona("Abuela", null);

        // Crear la madre
        Persona madre = new Persona("Madre", abuela);

        // Crear el hijo (nieto de la abuela)
        Persona hijo = new Persona("Hijo", madre);

        // Imprimir información de la familia
        System.out.println("Abuela: " + abuela);
        System.out.println("Madre: " + madre);
        System.out.println("Hijo: " + hijo);
    }
}

```
En este ejemplo, cada instancia de `Persona` tiene una referencia a su madre, lo que constituye una composición recursiva. El método `toString()` nos permite visualizar la información de cada persona, incluyendo el nombre y el nombre de su madre.
Otros ejemplos clásicos de composiciones recursivas incluyen:
1. **Estructuras de datos recursivas**: Por ejemplo, árboles binarios, listas enlazadas y grafos pueden ser implementados utilizando composiciones recursivas.
2. **Composiciones en modelos de objetos**: Por ejemplo, un árbol genealógico, como el que hemos representado con la clase `Persona`, es un ejemplo común de composición recursiva en el contexto de modelos de objetos.
3. **Estructuras de documentos**: Por ejemplo, un documento puede contener secciones, que a su vez pueden contener subsecciones, y así sucesivamente, formando una composición recursiva de estructuras dentro del documento.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Las relaciones de composición bidireccionales son un tipo de asociación en la programación orientada a objetos donde **dos clases están relacionadas de tal manera que cada una tiene una referencia a la otra**, y además existe una relación de composición (todo-parte) donde el ciclo de vida de las partes está ligado al todo.
En una composición:
- Si el objeto contenedor (todo) se destruye, los objetos contenidos (partes) también se destruyen
- Las partes no pueden pertenecer a más de un todo simultáneamente
- La relación es más fuerte que la agregación
Para implementar una relación bidireccional entre `Profesor` y `Departamento`, necesitaríamos que `Profesor` también tenga una referencia a su `Departamento`. 
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Departamento {
    private List<Profesor> profesores;
    private Profesor director;
    private static final int MAX_PROFESORES = 50;

    // Constructor
    public Departamento(Profesor director) throws Exception {
        this.profesores = new ArrayList<>();
        this.director = null; // Temporalmente null
        
        // Establecer el director usando el método setDirector
        // que maneja correctamente la bidireccionalidad
        setDirector(director);
    }

    // Método para añadir un profesor al departamento
    public void añadirProfesor(Profesor nuevoProfesor) throws Exception {
        if (profesores.size() >= MAX_PROFESORES) {
            throw new Exception("El departamento ya tiene el máximo de profesores permitidos.");
        }
        
        // Establecer la relación bidireccional
        nuevoProfesor.setDepartamento(this);
        profesores.add(nuevoProfesor);
    }

    // Método para eliminar un profesor por posición
    public void eliminarProfesor(int posicion) throws Exception {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new Exception("Posición inválida.");
        }

        Profesor profesorAEliminar = profesores.get(posicion);
        
        // Si el profesor a eliminar es el director, lanzar excepción
        if (profesorAEliminar == director) {
            throw new Exception("No se puede eliminar al director del departamento.");
        }

        // Romper la relación bidireccional
        profesorAEliminar.setDepartamento(null);
        profesores.remove(posicion);
    }

    // Método privado para establecer el director
    private void setDirector(Profesor nuevoDirector) throws Exception {
        if (nuevoDirector == null) {
            throw new Exception("El nuevo director no puede ser nulo.");
        }

        // Si el director anterior existe, actualizar su estado
        if (this.director != null) {
            this.director.setEsDirector(false);
        }

        // Si el nuevo director no está en el departamento, añadirlo
        if (!profesores.contains(nuevoDirector)) {
            nuevoDirector.setDepartamento(this);
            profesores.add(nuevoDirector);
        }

        this.director = nuevoDirector;
        nuevoDirector.setEsDirector(true);
    }

    // Método para cambiar el director del departamento
    public void cambiarDirector(Profesor nuevoDirector) throws Exception {
        setDirector(nuevoDirector);
    }

    // Método para obtener el número de profesores
    public int getNumProfesores() {
        return profesores.size();
    }

    // Método para obtener un profesor por posición
    public Profesor getProfesor(int posicion) throws Exception {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new Exception("Posición inválida.");
        }
        return profesores.get(posicion);
    }

    // Método que devuelve una copia defensiva
    public List<Profesor> getProfesores() {
        return new ArrayList<>(profesores);
    }
    
    // Getter para el director
    public Profesor getDirector() {
        return director;
    }
}

class Profesor {
    private String nombre;
    private String id;
    private Departamento departamento;  // Referencia al departamento (bidireccionalidad)
    private boolean esDirector;

    public Profesor(String nombre, String id) {
        this.nombre = nombre;
        this.id = id;
        this.departamento = null;
        this.esDirector = false;
    }

    // Getter y setter para departamento con manejo de bidireccionalidad
    public void setDepartamento(Departamento nuevoDepartamento) throws Exception {
        // Si ya pertenece a un departamento y es diferente, eliminar la relación anterior
        if (this.departamento != null && this.departamento != nuevoDepartamento) {
            Departamento antiguoDepartamento = this.departamento;
            this.departamento = null;  // Evitar recursión infinita
            antiguoDepartamento.eliminarProfesor(this); // Método auxiliar necesario
        }
        
        this.departamento = nuevoDepartamento;
        
        // Si el nuevo departamento no nulo no contiene a este profesor, añadirlo
        if (nuevoDepartamento != null && !nuevoDepartamento.getProfesores().contains(this)) {
            nuevoDepartamento.añadirProfesor(this);
        }
    }
    
    // Método auxiliar para eliminar profesor (necesario en Departamento)
    public void eliminarDeDepartamento() throws Exception {
        if (this.departamento != null) {
            Departamento depto = this.departamento;
            this.departamento = null;
            depto.eliminarProfesor(this);
        }
    }

    public Departamento getDepartamento() {
        return departamento;
    }

    public void setEsDirector(boolean esDirector) {
        this.esDirector = esDirector;
    }

    public boolean isEsDirector() {
        return esDirector;
    }

    // Getters y setters para nombre e id
    public String getNombre() {
        return nombre;
    }

    public String getId() {
        return id;
    }
}
```
