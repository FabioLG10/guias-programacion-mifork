# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

La composición en C se realiza anidando estructuras dentro de otras estructuras. En este caso, la estructura `Punto` contiene dos coordenadas, y la estructura `Linea` contiene dos `Punto`s, implementando así la relación "tiene-un" (o en este caso, "tiene-dos").

A continuación se presentan las estructuras y funciones necesarias:

```c
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

double distancia(Punto a, Punto b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}

double longitudLinea(Linea linea) {
    return distancia(linea.p1, linea.p2);
}
```

Este enfoque es simple pero tiene limitaciones: no hay encapsulación, cualquier código puede modificar directamente las coordenadas, y no hay garantía de que los datos se mantengan en un estado válido.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

En Java, la composición se implementa mediante atributos privados que contienen referencias a otros objetos. La inmutabilidad se garantiza no proporcionando métodos para modificar los atributos y utilizando `final` para las referencias. De esta forma, una vez creados los objetos, su estado no puede cambiar.

```java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }

    public Punto getPunto1() { return p1; }
    public Punto getPunto2() { return p2; }
}
```

Esta implementación mejora significativamente respecto a C: los atributos privados protegen los datos, los métodos proporcionan una interfaz controlada, y los modificadores `final` garantizan la inmutabilidad. Los objetos, una vez creados, no pueden ser alterados, lo que facilita el razonamiento sobre el código y evita efectos secundarios no deseados.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad describe cuántas instancias de una clase están asociadas con una instancia de otra clase. Es una forma de especificar las cardinalidades de las relaciones entre objetos. En los diagramas de clases UML, se expresa con notaciones como "1", "*" (muchos), "0..1" (cero o uno), o "1..*" (uno o más).

En el ejemplo de `Linea` y `Punto`, la multiplicidad es:
- De `Linea` a `Punto`: **1 a 2** (una línea tiene exactamente dos puntos)
- De `Punto` a `Linea`: **0 a muchos** (un punto puede pertenecer a ninguna línea, una línea, o múltiples líneas)

Esta multiplicidad asimétrica refleja que la línea necesita exactamente dos puntos para existir, mientras que un punto puede existir independientemente o ser compartido entre múltiples líneas.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La composición fuerte (o composición propiamente) es una relación en la cual el contenedor es responsable del ciclo de vida completo de los objetos contenidos. Cuando el contenedor se destruye, los objetos contenidos se destruyen también de forma obligatoria. Por el contrario, la composición débil (o agregación) es una relación en la cual el contenedor mantiene referencias a objetos cuyo ciclo de vida es independiente. Los objetos contenidos pueden sobrevivir a la destrucción del contenedor.

Las consecuencias en cuanto al ciclo de vida son significativas: en la composición fuerte, existe una relación de propiedad exclusiva (el contenedor "posee" los objetos), mientras que en la composición débil, existe una relación de colaboración donde los objetos pueden ser compartidos o existir independientemente. Convencionalmente, se utiliza el término "agregación" o "asociación" para referirse a la composición débil, y se reserva el término "composición" para la relación fuerte con ciclo de vida ligado.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

En estos casos se habla de "dependencia" y no de composición. La dependencia es una relación más temporal y menos estructural que la composición. Mientras que la composición establece una relación estructural permanente entre clases (mediante atributos de clase), la dependencia representa una relación de uso temporal donde una clase necesita otra para realizar su trabajo, pero no la contiene permanentemente.

Los tres escenarios mencionados ilustran diferentes tipos de dependencia: recibir parámetros o devolver valores crea una dependencia léxica (el método depende del tipo); crear objetos locales con `new` crea una dependencia temporalizada al ámbito de ejecución; y usar variables locales establece dependencias del ciclo de vida del método. En contraste, la composición requiere que los objetos se declaren como atributos privados de la clase, estableciendo una relación más íntima y permanente entre las clases implicadas.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

La composición fuerte se implementa creando los puntos dentro del contenedor (la línea es responsable de su creación). La composición débil se implementa recibiendo referencias a puntos que ya existen, sin ser responsables de crearlos.

**Composición Fuerte:**
```java
public class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);  // Crea los puntos
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```

**Composición Débil:**
```java
public class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;  // Recibe referencias existentes
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```

La diferencia crucial es que en la composición fuerte, la línea crea los puntos en su constructor, vinculando sus ciclos de vida. En la composición débil, la línea recibe puntos ya existentes, pudiendo estos ser reutilizados en otras líneas o existir independientemente.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java, la destrucción de objetos no es explícita como en C++. En su lugar, existe un mecanismo automático llamado "garbage collection" (recolección de basura) que se encarga de liberar la memoria de objetos que ya no son accesibles. Cuando la línea es destruida (o más precisamente, cuando todas las referencias a ella se eliminan), sus atributos `p1` y `p2` dejan de ser accesibles.

El recolector de basura detecta automáticamente que los puntos ya no tienen referencias alcanzables y los marca para su eliminación, sin necesidad de código explícito. Este es uno de los grandes beneficios de Java sobre C++: se evitan errores de memoria, fugas de memoria (memory leaks) y la complejidad de administración manual. Por lo tanto, aunque conceptualmente decimos que "la línea destruye los puntos", en realidad Java realiza esta tarea automáticamente mediante el garbage collection.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

Este ejemplo implementa una composición débil con una invariante compleja: el departamento mantiene referencias a varios profesores y, además, uno de ellos debe ser siempre el director. Se utiliza un array primitivo interno, pero se oculta su implementación mediante una interfaz pública que no revela la estructura interna. Las invariantes se validan en los métodos que podrían violarlas.

```java
public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Departamento {
    private static final int MAX_PROFESORES = 50;
    private Profesor[] profesores = new Profesor[MAX_PROFESORES];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor director) throws IllegalArgumentException {
        if (director == null) {
            throw new IllegalArgumentException("El director no puede ser nulo");
        }
        this.director = director;
        agregarProfesor(director);
    }

    public void agregarProfesor(Profesor profesor) throws IllegalArgumentException {
        if (profesor == null) {
            throw new IllegalArgumentException("El profesor no puede ser nulo");
        }
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalArgumentException("Límite de profesores alcanzado");
        }
        profesores[numProfesores++] = profesor;
    }

    public void eliminarProfesor(int posicion) throws IllegalArgumentException {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IllegalArgumentException("Posición inválida");
        }
        Profesor aEliminar = profesores[posicion];
        
        // Invariante: si el eliminado es el director, se viola
        if (aEliminar == director) {
            throw new IllegalArgumentException(
                "No se puede eliminar al director del departamento");
        }
        
        // Desplazar elementos
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        numProfesores--;
    }

    public void cambiarDirector(Profesor nuevoDirector) throws IllegalArgumentException {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El nuevo director no puede ser nulo");
        }
        
        // Verificar que el nuevo director está en la lista
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }
        
        if (!encontrado) {
            throw new IllegalArgumentException(
                "El nuevo director debe ser un profesor del departamento");
        }
        
        this.director = nuevoDirector;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int posicion) throws IllegalArgumentException {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IllegalArgumentException("Posición inválida");
        }
        return profesores[posicion];
    }

    public Profesor getDirector() {
        return director;
    }
}
```

Esta implementación mantiene las invariantes correctamente: el director siempre es un profesor del departamento, no se permite eliminarlo, y cualquier cambio de director verifica que el nuevo director existe en la lista de profesores. El array interno es completamente ocultado, proporcionando solo métodos que no revelan su existencia.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

Utilizar `List` simplifica significativamente el código al eliminar la necesidad de gestionar manualmente el tamaño del array, validar límites de capacidad y desplazar elementos al eliminar. La clase de colecciones proporciona estos servicios automáticamente, permitiendo enfocarse en la lógica de negocio y las invariantes.

```java
import java.util.List;
import java.util.ArrayList;
import java.util.Collections;

public class Departamento {
    private List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor director) throws IllegalArgumentException {
        if (director == null) {
            throw new IllegalArgumentException("El director no puede ser nulo");
        }
        this.director = director;
        profesores.add(director);
    }

    public void agregarProfesor(Profesor profesor) throws IllegalArgumentException {
        if (profesor == null) {
            throw new IllegalArgumentException("El profesor no puede ser nulo");
        }
        profesores.add(profesor);
    }

    public void eliminarProfesor(int posicion) throws IllegalArgumentException {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IllegalArgumentException("Posición inválida");
        }
        Profesor aEliminar = profesores.get(posicion);
        if (aEliminar == director) {
            throw new IllegalArgumentException(
                "No se puede eliminar al director del departamento");
        }
        profesores.remove(posicion);
    }

    public void cambiarDirector(Profesor nuevoDirector) throws IllegalArgumentException {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El nuevo director no puede ser nulo");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException(
                "El nuevo director debe ser un profesor del departamento");
        }
        this.director = nuevoDirector;
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int posicion) throws IllegalArgumentException {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IllegalArgumentException("Posición inválida");
        }
        return profesores.get(posicion);
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }

    public Profesor getDirector() {
        return director;
    }
}
```

En cuanto al problema de devolver la colección completa: si se devolviera directamente la `List` interna sin protección, un código externo podría modificarla (agregar, eliminar o cambiar profesores) sin pasar por las validaciones de invariantes que los métodos de la clase realizan. Esto violaría la encapsulación. La solución es devolver una lista inmodificable mediante `Collections.unmodifiableList()`, que envuelve la lista interna proporcionando solo acceso de lectura. Cualquier intento de modificación lanzará una excepción, protegiendo así las invariantes de la clase.


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

La composición recursiva ocurre cuando una clase contiene una instancia de sí misma (directa o indirectamente). Esto permite modelar estructuras jerárquicas naturales. En el ejemplo, cada `Persona` puede tener una madre, que es otra `Persona`, permitiendo representar árboles genealógicos.

```java
public class Persona {
    private final String nombre;
    private final Persona madre;  // Referencia recursiva

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public Persona(String nombre) {  // Persona sin madre (caso base)
        this.nombre = nombre;
        this.madre = null;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }

    public void mostrarArbolGenelogico(int profundidad) {
        for (int i = 0; i < profundidad; i++) {
            System.out.print("  ");
        }
        System.out.println(nombre);
        
        if (madre != null) {
            madre.mostrarArbolGenelogico(profundidad + 1);
        }
    }

    public static void main(String[] args) {
        // Construir árbol genealógico: abuela -> madre -> hijo
        Persona abuela = new Persona("María");
        Persona madre = new Persona("Ana", abuela);
        Persona hijo = new Persona("Carlos", madre);

        hijo.mostrarArbolGenelogico(0);
    }
}
```

Otros ejemplos clásicos de composiciones recursivas incluyen: estructuras de directorios del sistema operativo (un directorio contiene archivos y otros directorios), árboles binarios de búsqueda (cada nodo contiene dos referencias a otros nodos), estructuras de menús jerárquicos (un menú puede contener submenús), y sistemas de categorías de productos en tiendas (una categoría puede tener subcategorías).

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Una relación bidireccional es aquella en la que ambas clases mantienen referencias entre sí. Mientras que en el ejemplo anterior solo el `Departamento` tiene referencias a los `Profesor`es, en una relación bidireccional cada `Profesor` también tendría una referencia al `Departamento` al que pertenece. Esto facilita navegación desde ambas direcciones pero requiere sincronización cuidadosa para mantener coherencia.

Implementar esto requiere coordinar los cambios en ambas direcciones para mantener la invariante de bidireccionalidad. Cuando se agrega un profesor al departamento, el profesor debe registrarse así mismo en el departamento. Cuando se elimina, se debe desvincular.

```java
public class Profesor {
    private final String nombre;
    private Departamento departamento;  // Referencia al departamento

    public Profesor(String nombre) {
        this.nombre = nombre;
        this.departamento = null;
    }

    public String getNombre() {
        return nombre;
    }

    public Departamento getDepartamento() {
        return departamento;
    }

    // Método de bajo nivel, solo para uso interno de Departamento
    protected void setDepartamento(Departamento depto) {
        this.departamento = depto;
    }
}

public class DepartamentoBidireccional {
    private List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public DepartamentoBidireccional(Profesor director) throws IllegalArgumentException {
        if (director == null) {
            throw new IllegalArgumentException("El director no puede ser nulo");
        }
        this.director = director;
        profesores.add(director);
        director.setDepartamento(this);  // Bidireccional
    }

    public void agregarProfesor(Profesor profesor) throws IllegalArgumentException {
        if (profesor == null) {
            throw new IllegalArgumentException("El profesor no puede ser nulo");
        }
        // El profesor no puede estar en otro departamento
        if (profesor.getDepartamento() != null) {
            throw new IllegalArgumentException(
                "El profesor ya pertenece a otro departamento");
        }
        profesores.add(profesor);
        profesor.setDepartamento(this);  // Mantener relación bidireccional
    }

    public void eliminarProfesor(int posicion) throws IllegalArgumentException {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IllegalArgumentException("Posición inválida");
        }
        Profesor aEliminar = profesores.get(posicion);
        if (aEliminar == director) {
            throw new IllegalArgumentException(
                "No se puede eliminar al director del departamento");
        }
        profesores.remove(posicion);
        aEliminar.setDepartamento(null);  // Romper relación bidireccional
    }

    public Profesor getDirector() {
        return director;
    }
}
```

La clave es utilizar métodos protegidos o privados para sincronizar ambas referencias sin exponer directamente los atributos. De esta manera, cada acción de agregación o eliminación mantiene automáticamente la consistencia bidireccional entre profesor y departamento.
