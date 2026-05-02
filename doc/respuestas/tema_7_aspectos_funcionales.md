# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un puntero a una función es una variable que almacena la dirección de memoria de una función. Permite tratarla como un dato que se puede pasar entre funciones, almacenar en estructuras o invocar indirectamente. La sintaxis en C es `tipo (*nombre)(parámetros)`.

A continuación se presenta un ejemplo completo:

```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>

char* convertirMayusculas(char* cadena) {
    static char resultado[100];
    strcpy(resultado, cadena);
    for (int i = 0; resultado[i]; i++) {
        resultado[i] = toupper(resultado[i]);
    }
    return resultado;
}

int main() {
    char* (*aMayusculas)(char*) = convertirMayusculas;
    char* salida = aMayusculas("hola mundo");
    printf("%s\n", salida);
    return 0;
}
```

El puntero `aMayusculas` almacena la dirección de la función `convertirMayusculas`. Se invoca de la misma forma que se invocaría la función original, pero a través del puntero.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una función lambda (o función anónima) es una función sin nombre definida directamente en el lugar donde se utiliza. Es más concisa que un puntero a función en C, ya que permite definir la función en línea sin necesidad de declararla por separado.

En JavaScript:

```javascript
const aMayusculas = (cadena) => cadena.toUpperCase();
console.log(aMayusculas("hola mundo"));
```

En Java:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = 
            (cadena) -> cadena.toUpperCase();
        System.out.println(aMayusculas.apply("hola mundo"));
    }
}
```

Las lambdas son más legibles y concisas que los punteros a funciones en C. En Java, la variable `aMayusculas` tiene tipo `Function<String, String>`, que es una interfaz funcional que representa una función que recibe un String y devuelve un String.


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es un enfoque de programación que enfatiza el uso de funciones puras, evita el estado mutable y trata el cómputo como la evaluación de funciones matemáticas. Contrasta con la programación imperativa y orientada a objetos.

Java 8 se considera multi-paradigma porque permite programación orientada a objetos junto con características funcionales. Que las funciones sean "ciudadanos de primera clase" significa que pueden tratarse como cualquier otro valor: asignarse a variables, pasarse como parámetros, devolverse como resultados, almacenarse en colecciones. En lenguajes funcionales modernos como Java 8 con lambdas, las funciones son conceptos de primer nivel.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La sintaxis básica de una lambda en Java es: `(parámetros) -> cuerpo`. Los parámetros se encierran entre paréntesis (pueden omitirse si hay solo uno). El cuerpo puede ser una expresión única o un bloque de sentencias.

Ejemplos:

```java
Function<Integer, Integer> duplica = x -> x * 2;
BiFunction<Integer, Integer, Integer> suma = (a, b) -> a + b;
Function<String, String> mayuscula = (String s) -> s.toUpperCase();
Function<String, String> procesa = (s) -> {
    String resultado = s.toUpperCase();
    System.out.println("Procesando: " + resultado);
    return resultado;
};
Runnable saludar = () -> System.out.println("Hola");
```

Cuando el cuerpo es una única expresión, el compilador infiere el tipo de retorno. Si hay múltiples sentencias, se deben encerrar en llaves y es necesario un `return` explícito. El tipo de los parámetros puede inferirse del contexto o especificarse explícitamente.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Este patrón permite que un método reciba una función como parámetro y la aplique a datos. Es el inicio de la composición funcional, donde el comportamiento se parametriza a través de funciones.

En JavaScript:

```javascript
const aMayusculas = (cadena) => cadena.toUpperCase();

function transformar(texto, funcionTransformadora) {
    return funcionTransformadora(texto);
}

console.log(transformar("hola", aMayusculas));
```

En Java:

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String texto, Function<String, String> func) {
        return func.apply(texto);
    }
    
    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();
        System.out.println(transformar("hola", aMayusculas));
    }
}
```

En ambos casos, el método `transformar` recibe dos parámetros: los datos a transformar y la función transformadora. Dentro del método, se invoca la función pasada como parámetro.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Una ventaja de las lambdas es que pueden definirse en línea, sin necesidad de crear una variable intermedia. Esto resulta en código más conciso cuando la función es simple y se usa solo una vez.

En JavaScript:

```javascript
function transformar(texto, func) { return func(texto); }

const resultado = transformar("hola mundo", (cadena) => {
    return cadena.split('').reverse().join('');
});
console.log(resultado);
```

En Java:

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String texto, Function<String, String> func) {
        return func.apply(texto);
    }
    
    public static void main(String[] args) {
        String resultado = transformar("hola mundo", 
            s -> new StringBuilder(s).reverse().toString());
        System.out.println(resultado);
    }
}
```

Esta forma de escribir código es más declarativa: se describe qué hacer en el mismo lugar donde se usa, evitando la creación de funciones nombradas que se usan solo una vez.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un closure es una función que captura y retiene referencias a variables de su contexto envolvente. Las lambdas en Java pueden capturar variables locales, pero estas deben ser efectivamente finales (no cambiar de valor después de su captura).

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        String prefijo = ">>> ";
        String sufijo = " <<<";
        
        Function<String, String> transformar = (cadena) -> 
            prefijo + cadena + sufijo;
        
        System.out.println(transformar.apply("Hola"));
        
        int factor = 10;
        Function<Integer, Integer> multiplicar = (n) -> n * factor;
        System.out.println(multiplicar.apply(5));
    }
}
```

La lambda captura las variables `prefijo`, `sufijo` y `factor` del contexto. Este mecanismo es más poderoso que los punteros a funciones en C, que solo acceden a variables globales.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

Aunque ambos permiten pasar funciones como parámetros, las diferencias son significativas. Los punteros a funciones en C son simplemente direcciones de memoria de funciones previamente definidas. No pueden capturar estado; una función en C es estática desde el momento en que se compila. Para parametrizar comportamiento en C, se suelen usar estructuras con punteros a función y datos.

Las lambdas son más versátiles. Pueden definirse en línea donde se necesiten, haciendo el código más conciso. Crucialmente, pueden capturar variables del contexto (closures), permitiendo crear variaciones de comportamiento sin crear funciones nombradas por separado. La sintaxis es más limpia y menos propensa a errores de tipos.

Los closures de Java son imposibles en C sin estructuras adicionales. Esto hace que el código funcional sea más natural en lenguajes modernos como Java 8+.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

Una función que devuelve otra función es un ejemplo de programación de orden superior. Este patrón permite crear "fabricas" de funciones con comportamiento parametrizado. Cada función retornada captura el parámetro que recibió `crearDescuento`.

```java
import java.util.function.Function;

public class Main {
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return precio -> precio * (1 - porcentaje / 100.0);
    }
    
    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento25 = crearDescuento(25);
        
        double precio = 100.0;
        System.out.println(descuento10.apply(precio));
        System.out.println(descuento25.apply(precio));
    }
}
```

Cada función devuelta captura un valor diferente de `porcentaje` en su closure. Aunque `crearDescuento` ha terminado su ejecución, las lambdas retornadas siguen teniendo acceso a la variable `porcentaje`. Esto es especialmente poderoso comparado con C.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

Una **interfaz funcional** es una interfaz que define exactamente un método abstracto (aparte de métodos por defecto o estáticos). Cuando se escribe una lambda en Java, internamente se crea una instancia anónima de una interfaz funcional, por lo que el compilador necesita saber cuál es esa interfaz.

Los requisitos de una interfaz funcional son:
1. Debe ser una interfaz (no una clase)
2. Debe tener exactamente **un método abstracto** (es decir, un método sin implementación)
3. Puede tener métodos por defecto, estáticos u heredados adicionales
4. Opcionalmente, puede anotarse con `@FunctionalInterface`

```java
@FunctionalInterface
public interface Operacion {
    int calcular(int a, int b);
}

Operacion suma = (a, b) -> a + b;
System.out.println(suma.calcular(3, 4));
```

Java proporciona interfaces funcionales predefinidas como `Function`, `Consumer`, `Predicate`, etc., pero es posible crear las propias. La anotación `@FunctionalInterface` no es obligatoria, pero ayuda a documentar la intención y permite que el compilador verifique que la interfaz cumple los requisitos.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

La interfaz `Transformador` encapsula el contrato de una función que transforma un String en otro String. Es una forma de dar nombre específico a una interfaz funcional según su propósito de dominio.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String entrada);
}

public class Main {
    public static String aplicar(String texto, Transformador trans) {
        return trans.transformar(texto);
    }
    
    public static void main(String[] args) {
        Transformador aMayusculas = s -> s.toUpperCase();
        Transformador aMinusculas = s -> s.toLowerCase();
        Transformador invertir = s -> new StringBuilder(s).reverse().toString();
        
        System.out.println(aplicar("Hola", aMayusculas));
        System.out.println(aplicar("Hola", aMinusculas));
        System.out.println(aplicar("Hola", invertir));
    }
}
```

Al definir una interfaz funcional personalizada, el código se hace más expresivo: `Transformador` comunica claramente que se trata de una transformación de strings. Aunque `Function<String, String>` haría lo mismo, una interfaz nombrada es más legible en un contexto específico de dominio.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Al añadir parámetros de tipo genéricos a la interfaz funcional, se consigue máxima reutilización. La interfaz `Transformador<T, R>` representa una transformación de un tipo T a un tipo R.

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}

public class Main {
    public static <T, R> R aplicar(T valor, Transformador<T, R> trans) {
        return trans.transformar(valor);
    }
    
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear = 
            d -> Math.round(d).intValue();
        
        Transformador<Integer, String> aString = 
            i -> "Número: " + i;
        
        Transformador<String, Integer> contarCaracteres = 
            s -> s.length();
        
        System.out.println(aplicar(3.7, redondear));
        System.out.println(aplicar(42, aString));
        System.out.println(aplicar("Hola", contarCaracteres));
    }
}
```

Esta versión genérica es altamente flexible. Permite expresar cualquier transformación de un tipo a otro, manteniendo la seguridad de tipos en tiempo de compilación. El compilador verifica que entrada y salida coincidan con los tipos parametrizados.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Java proporciona un conjunto de interfaces funcionales predefinidas en el paquete `java.util.function`, diseñadas para los casos más comunes. Estas evitan la necesidad de crear interfaces propias repetidamente.

**Transformación:**
- `Function<T, R>`: T → R (la más general)
- `UnaryOperator<T>`: T → T (misma entrada y salida)
- `BiFunction<T, U, R>`: (T, U) → R (dos entradas)

**Consumo (sin retorno):**
- `Consumer<T>`: T → void (procesa sin devolver)
- `BiConsumer<T, U>`: (T, U) → void

**Predicados (retornan boolean):**
- `Predicate<T>`: T → boolean (prueba una condición)
- `BiPredicate<T, U>`: (T, U) → boolean

**Proveedores (sin entrada):**
- `Supplier<T>`: () → T (genera valores)

```java
Function<String, Integer> longitud = s -> s.length();
Consumer<String> imprimir = System.out::println;
Predicate<Integer> esPositivo = n -> n > 0;
Supplier<Double> generarAleatorio = Math::random;
UnaryOperator<Integer> duplicar = x -> x * 2;
```

Esta librería estándar cubre la mayoría de casos comunes, reduciendo la necesidad de crear interfaces funcionales personalizadas.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método `forEach` es un ejemplo de cómo la programación funcional hace el código más conciso y expresivo. En lugar de escribir un bucle imperativo tradicional, se describe qué acción realizar sobre cada elemento.

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-5, 3, -2, 8, 0, -1, 4);
        
        // Forma imperativa (bucle for tradicional)
        for (Integer n : numeros) {
            if (n > 0) {
                System.out.println("El número " + n + " es positivo");
            }
        }
        
        // Forma funcional (forEach)
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("El número " + n + " es positivo");
            }
        });
        
        // Aún más conciso con referencia a método
        numeros.stream()
               .filter(n -> n > 0)
               .forEach(n -> System.out.println("El número " + n + " es positivo"));
    }
}
```

El `forEach` recibe un `Consumer<Integer>`. Aunque la primera versión imperativa y la funcional son equivalentes, la ventaja del enfoque funcional es que escala mejor con operaciones más complejas, especialmente cuando se combinan con streams.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

La firma de `forEach` es `void forEach(Consumer<? super T> action)`. Usa `? super T` en lugar de `T` porque `forEach` es una operación de **escritura** (consume, no produce): pasa elementos de tipo T al Consumer. Un `Consumer<? super T>` puede aceptar un Consumer que trabaje con T o con cualquier supertipo de T, lo que es más flexible.

**PECS** significa "Producer Extends, Consumer Super": (i) Si el genérico **produce** valores (salida), usar `? extends`; (ii) Si el genérico **consume** valores (entrada), usar `? super`. Este principio maximiza flexibilidad en tiempo de compilación sin comprometer seguridad de tipos.

Para el método `transformar`, mejorado con PECS:

```java
// Original: solo acepta Function<String, String>
public static String transformar(String texto, Function<String, String> func) {
    return func.apply(texto);
}

// Mejorado: acepta Function que recibe String y devuelve Object o supertipo
public static Object transformar(String texto, Function<? super String, ? extends Object> func) {
    return func.apply(texto);
}
```

En contextos de lectura (como `Function.apply` al recuperar resultados), se usa `? extends`. En contextos de escritura (como `Consumer` recibiendo parámetros), se usa `? super`. PECS es una heurística crucial para diseñar APIs genéricas flexibles.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Una referencia a método es similar a una lambda, pero en lugar de definir la lógica, se apunta a un método existente. La sintaxis es `objeto::metodo`. Esto es útil cuando ya existe un método que hace exactamente lo que se necesita.

En JavaScript:

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    
    saludar() {
        return `Hola, soy ${this.nombre}`;
    }
}

const persona = new Persona("Juan");
const refSaludar = persona.saludar.bind(persona);
console.log(refSaludar());
```

En Java:

```java
import java.util.function.Supplier;

public class Persona {
    private String nombre;
    
    public Persona(String nombre) {
        this.nombre = nombre;
    }
    
    public String saludar() {
        return "Hola, soy " + nombre;
    }
    
    public static void main(String[] args) {
        Persona persona = new Persona("Juan");
        Supplier<String> refSaludar = persona::saludar;
        System.out.println(refSaludar.get());
    }
}
```

En Java, `persona::saludar` es una forma concisa de crear una lambda que llamaría `persona.saludar()`. En este caso, se usa `Supplier<String>` porque `saludar()` no recibe parámetros y devuelve un String.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

Java soporta cuatro tipos de referencias a métodos, cada una con sintaxis particular. Todas pueden convertirse a interfaces funcionales apropiadas según su firma.

```java
import java.util.function.*;

public class Ejemplo {
    public static String procesarEstatic(String s) {
        return s.toUpperCase();
    }
    
    public String procesarInstancia(String s) {
        return s.toLowerCase();
    }
    
    public static void main(String[] args) {
        Ejemplo obj = new Ejemplo();
        
        // 1. Método estático: Clase::metodo
        Function<String, String> ref1 = Ejemplo::procesarEstatic;
        System.out.println(ref1.apply("Hola"));
        
        // 2. Método de instancia (instancia específica): instancia::metodo
        Function<String, String> ref2 = obj::procesarInstancia;
        System.out.println(ref2.apply("Hola"));
        
        // 3. Método de instancia (cualquier instancia): Clase::metodo
        BiFunction<Ejemplo, String, String> ref3 = Ejemplo::procesarInstancia;
        System.out.println(ref3.apply(obj, "Hola"));
        
        // 4. Constructor: Clase::new
        Supplier<Ejemplo> ref4 = Ejemplo::new;
        Ejemplo nuevoObj = ref4.get();
    }
}
```

Cada tipo de referencia se adapta a una interfaz funcional diferente según la firma del método. Las referencias a métodos hacen el código más legible cuando se reutilizan métodos existentes en lugar de escribir lambdas equivalentes.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

Este ejemplo ilustra cómo las lambdas hacen código de ordenamiento más legible y flexible que pasar objetos Comparator tradicionales. `Collections.sort` acepta un `Comparator`, que es una interfaz funcional con un método `compare(T, T)`.

```java
import java.util.*;

public class Persona {
    private String nombre;
    private int edad;
    
    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    
    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
    
    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }
}

public class Main {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Ana", 30),
            new Persona("Carlos", 25),
            new Persona("Ana", 25),
            new Persona("David", 30)
        );
        
        // Versión 1: Lambda manual
        Collections.sort(personas, (p1, p2) -> {
            if (p1.getEdad() != p2.getEdad()) {
                return Integer.compare(p1.getEdad(), p2.getEdad());
            }
            return p1.getNombre().compareTo(p2.getNombre());
        });
        System.out.println("Ordenado (manual): " + personas);
        
        // Versión 2: Usando Comparator
        personas = Arrays.asList(
            new Persona("Ana", 30),
            new Persona("Carlos", 25),
            new Persona("Ana", 25),
            new Persona("David", 30)
        );
        Collections.sort(personas, 
            Comparator.comparingInt(Persona::getEdad)
                      .thenComparing(Persona::getNombre));
        System.out.println("Ordenado (Comparator): " + personas);
    }
}
```

La segunda versión es más legible y composable: `Comparator` proporciona métodos como `comparingInt` y `thenComparing` que permiten construir comparadores complejos declarativamente. Ambas versiones demuestran la potencia del paradigma funcional: flexibilidad, concisión y claridad en operaciones comunes como ordenamiento.
