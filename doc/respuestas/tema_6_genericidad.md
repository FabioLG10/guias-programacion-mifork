# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

En Java, se puede crear un contenedor genérico basado en `Object` aprovechando que todas las clases heredan de esta clase base. A continuación se presenta un ejemplo de un contenedor dinámico simple:

```java
public class Contenedor {
    private Object[] elementos;
    private int tamaño = 0;
    
    public Contenedor(int capacidad) {
        elementos = new Object[capacidad];
    }
    
    public void agregar(Object obj) {
        if (tamaño < elementos.length) {
            elementos[tamaño++] = obj;
        }
    }
    
    public Object obtener(int indice) {
        return elementos[indice];
    }
}
```

Este contenedor puede almacenar cualquier tipo de objeto (String, Integer, Double, etc.) porque todos heredan de `Object`. Sin embargo, al recuperar un elemento, se obtiene como `Object` y es necesario realizar un casting manual al tipo específico para utilizarlo.

En C, el equivalente sería utilizar `void*` para apuntar a datos de cualquier tipo, aunque se requiere mayor cuidado en la gestión de memoria y conversión de tipos.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta

La programación genérica es un paradigma que permite escribir estructuras de datos y algoritmos que funcionan con múltiples tipos diferentes, manteniendo la seguridad de tipos en tiempo de compilación. El objetivo es evitar duplicación de código mientras se preserva la verificación de tipos.

El ejemplo anterior es un ejemplo **primitivo** de programación genérica, pero no completo. Permite alojar cualquier tipo de dato, pero tiene un grave problema: la seguridad de tipos se pierden en tiempo de compilación. Es necesario realizar casting manual al recuperar elementos, y si el casting es incorrecto, el error solo se detectará en tiempo de ejecución, no en compilación. La verdadera programación genérica, como los generics de Java o templates de C++, mantiene esta flexibilidad de tipos pero garantiza la seguridad en tiempo de compilación.
### Respuesta

El principal problema es la **pérdida de seguridad de tipos en tiempo de compilación**. El compilador no puede verificar que los tipos recuperados del contenedor sean correctos, por lo que errores como hacer un casting incorrecto solo se detectan en tiempo de ejecución mediante excepciones (ClassCastException).

Adicionalmente, surge la necesidad de realizar **castings manuales repetitivos** cada vez que se recupera un elemento, lo que hace el código más tedioso y propenso a errores. Otro problema importante es que no se puede forzar que todos los elementos del contenedor sean del mismo tipo; se puede mezclar Strings con Integers sin que el compilador se queje. Finalmente, se pierde información de tipos en el código fuente, dificultando su lectura y mantenimiento, pues no queda claro qué tipo de datos espera o devuelve una estructura.

### Respuesta

Los parámetros de tipo (también llamados variables de tipo o type parameters) son marcadores que representan un tipo desconocido que se especificará cuando se utilice la clase o método. Se denotan típicamente con letras mayúsculas como `T`, `U`, `K`, `V`, etc. Permiten crear código reutilizable sin comprometer la seguridad de tipos.

Cuando se define una clase con parámetros de tipo, como `Contenedor<T>`, se está diciendo que esta clase funcionará con un tipo genérico `T` que será reemplazado por un tipo concreto en tiempo de compilación. El parámetro se especifica en el momento de instanciar la clase, como `Contenedor<String>` o `Contenedor<Integer>`. A partir de ese momento, el compilador conoce exactamente qué tipo contiene la estructura, lo que permite verificar todas las operaciones y elimina la necesidad de castings explícitos.

### Respuesta

En Java:

```java
List<String> palabras = new ArrayList<String>();
palabras.add("Java");
palabras.add("Generics");
palabras.add("Seguridad");

for (String palabra : palabras) {
    System.out.println(palabra.toUpperCase()); // No requiere casting
}
```

En C++:

```cpp
#include <vector>
#include <string>
using namespace std;

vector<string> palabras;
palabras.push_back("C++");
palabras.push_back("Templates");
palabras.push_back("Seguridad");

for (string palabra : palabras) {
    // Procesamiento directo del tipo concreto
    cout << palabra << endl;
}
```

En ambos casos, el tipo `String` se especifica en el momento de la instantiación. El compilador asegura que solo se pueden agregar Strings a la colección y que cada elemento recuperado es un String sin necesidad de casting. Esto es muy diferente a usar un contenedor basado en `Object`, donde sería necesario hacer un casting manual cada vez.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Java y C++ toman enfoques radicalmente diferentes. En **C++**, el compilador genera un código completamente diferente para cada instanciación concreta. Cuando se crea `vector<string>` y `vector<int>`, el compilador genera dos versiones completamente separadas del código template, cada una optimizada para su tipo específico. Este mecanismo se llama **instanciación de plantillas** y resulta en un ejecutable más grande pero altamente optimizado.

En **Java**, el compilador utiliza un mecanismo llamado **type erasure** (borrado de tipos). Cuando se compila `List<String>`, el compilador genera código que funciona con `Object` (el tipo base), pero inserta castings automáticos donde es necesario. Durante la compilación, toda la información de tipos genéricos se **borra**, dejando solo `List` (sin tipo paramétrico). Esto tiene la ventaja de mantener compatibilidad con código antiguo y un ejecutable más pequeño, pero pierde información de tipos en tiempo de ejecución. Por ejemplo, en tiempo de ejecución no es posible distinguir entre `List<String>` y `List<Integer>`.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta

La clase `Par` puede definirse con dos parámetros de tipo independientes:

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
}
```

Un ejemplo de uso para retornar la media y desviación típica:

```java
public static Par<Double, Double> estadisticas(double[] datos) {
    double suma = 0;
    for (double d : datos) suma += d;
    double media = suma / datos.length;
    
    double sumaCuadrados = 0;
    for (double d : datos) sumaCuadrados += Math.pow(d - media, 2);
    double desviacion = Math.sqrt(sumaCuadrados / datos.length);
    
    return new Par<Double, Double>(media, desviacion);
}

// Uso:
Par<Double, Double> resultado = estadisticas(new double[]{1, 2, 3, 4, 5});
System.out.println("Media: " + resultado.getPrimero());
System.out.println("Desviación: " + resultado.getSegundo());
```

Esta clase permite retornar dos valores de tipos diferentes con total seguridad de tipos: no es necesario casting, y el compilador verifica que se acceda a cada valor con el tipo correcto.

### Respuesta

Sin generics (usando `Object`):

```java
public static Object seleccionaSinGenerics(Object obj1, Object obj2) {
    return Math.random() < 0.5 ? obj1 : obj2;
}

// Uso:
Object resultado = seleccionaSinGenerics("Hola", "Mundo");
String palabra = (String) resultado; // Necesita casting explícito
```

Con generics (método genérico):

```java
public static <T> T seleccionaUno(T obj1, T obj2) {
    return Math.random() < 0.5 ? obj1 : obj2;
}

// Uso:
String palabra = seleccionaUno("Hola", "Mundo"); // No requiere casting
```

La diferencia crucial es que con generics: (i) **no es necesario casting**, porque el compilador sabe que el retorno es del mismo tipo que los parámetros, por lo que la asignación directa es segura; (ii) **se fuerza que ambos objetos sean del mismo tipo**, si se intenta `seleccionaUno("Hola", 42)`, el compilador rechaza el código porque mezcla String con Integer. Sin generics, el compilador permitiría esta llamada inconsistente, lo que sería un error lógico.

### Respuesta

**Solución 1: Usando `Number`** (sin generics):

```java
public class Punto {
    private Number x, y;
    
    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }
    
    public Number getX() { return x; }
    public Number getY() { return y; }
    
    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

**Solución 2: Usando generics con bounded type parameter**:

```java
public class Punto<T extends Number> {
    private T x, y;
    
    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }
    
    public T getX() { return x; }
    public T getY() { return y; }
    
    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}

// Uso:
Punto<Integer> p1 = new Punto<Integer>(3, 4);
Punto<Double> p2 = new Punto<Double>(0.0, 0.0);
Double distancia = p1.calcularDistanciaA(p1);
```

La diferencia es que con generics, `T` está **vinculado** a `Number` mediante `extends`. Esto permite usar métodos de `Number` como `doubleValue()`. Tras el **type erasure**, el tipo final en ambos casos es `Number`: el compilador reemplaza `T` por `Number`, sus límites superiores. Sin embargo, con generics el código fuente es más específico (sabes si tienes Integer o Double en tiempo de compilación), mientras que con `Number` puro se pierde esa información.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

**Solución 1 (`Number`):** `new Punto(3, 4.5)` es perfectamente válido. Cada coordenada puede ser cualquier tipo de `Number`, porque en el constructor se aceptan `Number` individuales sin vinculación entre ellas. El método `getX()` devuelve `Number`, obligando al usuario a hacer casting si desea trabajar con el tipo específico, como `int x = ((Integer) punto.getX()).intValue()`.

**Solución 2 (generics):** `new Punto<Integer>(3, 4.5)` **no es válido** porque las dos coordenadas deben ser del mismo tipo `T`. Si se desea una coordenada entera y otra real, no es posible hacerlo con una única instancia. Se forzaría usar `Punto<Double>` para ambas. El método `getX()` devuelve `T`, el tipo específico (Integer, Double, etc.), sin necesidad de casting explícito.

Esta es la diferencia clave: generics refuerza la consistencia de tipos dentro de una instancia, mientras que `Number` permite mezclar. En términos de seguridad de tipos, generics es más restrictivo pero proporciona mejor control y claridad.


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

### Respuesta

La solución utiliza un parámetro de tipo en la interfaz, permitiendo que cada implementación especifique su propio tipo:

```java
public interface Punto<T extends Punto<T>> { 
    public double distanciaA(T p); 
} 

public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 
    public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto2D p) { 
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2)); 
    } 
} 

public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z; 
    public Punto3D(double x, double y, double z) { 
        this.x = x; this.y = y; this.z = z; 
    } 

    @Override 
    public double distanciaA(Punto3D p) { 
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2)); 
    } 
} 
```

Esta técnica, conocida como **patrón CRTP (Curiously Recurring Template Pattern)**, garantiza que el parámetro sea exactamente del tipo correcto. El compilador verifica que `distanciaA` reciba un `Punto2D` para `Punto2D` o un `Punto3D` para `Punto3D`, eliminando completamente la necesidad de `instanceof` y casting, y previniendo errores de tipo en tiempo de compilación.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

**`List<String>` NO es subtipo de `List<Object>`**, pero **`String[]` SÍ es subtipo de `Object[]`**. Esta diferencia es fundamental. Con arrays, `String[] arr = new String[5]; Object[] obj = arr;` es válido. Sin embargo, esto introduce un problema: `obj[0] = new Integer(42);` es permitido en tiempo de compilación (porque `obj` es `Object[]`), pero falla en tiempo de ejecución con `ArrayStoreException`. Los arrays son **covariantes** en Java.

Con generics, para evitar este problema, `List<String>` es **invariante** respecto a `String`. No se puede asignar `List<String>` a `List<Object>`. Aunque parecería intuitivo permitirlo, causaría errores: si se pudiera hacer `List<Object> lista = new ArrayList<String>()`, entonces `lista.add(42)` sería válido (porque acepta Object), corrompiendo una lista que debería contener solo Strings.

Los conceptos son: **Covariante** (un subtipo de T es un subtipo válido de una estructura genérica de T), **Contravariante** (un supertipo de T es un subtipo válido de una estructura genérica de T), e **Invariante** (solo el tipo exacto T es válido, no sus subtipos ni supertipos). Los generics de Java son invariantes por defecto para garantizar seguridad.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un **wildcard (`?`)** es un marcador que representa un tipo desconocido. `List<? extends Number>` significa "una lista de algo que es Number o un subtipo de Number", permitiendo **covarianza** controlada. Es seguro leer de esta lista porque se garantiza que cualquier elemento es al menos un Number. Sin embargo, no se puede escribir (excepto `null`), porque el compilador no sabe exactamente qué tipo acepta.

`List<? super Integer>` significa "una lista de algo que es Integer o un supertipo de Integer", permitiendo **contravarianza** controlada. Es seguro escribir Integers en esta lista, pero no se puede leer con seguridad (se obtendría Object).

**Ejemplo 1 (lectura, `? extends`):**
```java
public static double sumaNumeros(List<? extends Number> numeros) {
    double suma = 0;
    for (Number n : numeros) suma += n.doubleValue();
    return suma;
}
// Uso:
List<Integer> enteros = Arrays.asList(1, 2, 3);
List<Double> reales = Arrays.asList(1.5, 2.5);
double s1 = sumaNumeros(enteros);
double s2 = sumaNumeros(reales);
```

**Ejemplo 2 (escritura, `? super`):**
```java
public static void añadeEnteros(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
    lista.add(30);
}
// Uso:
List<Number> numeros = new ArrayList<Number>();
añadeEnteros(numeros);
```
