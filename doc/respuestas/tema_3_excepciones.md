# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

En C, al no existir excepciones, el control de errores se realiza mediante convenciones de diseño. Una opción consiste en devolver un valor especial que indique el error, por ejemplo, un número negativo o cero si la entrada es inválida. El usuario debe comprobar el resultado y actuar en consecuencia. Otra opción es utilizar una variable global o un parámetro adicional para indicar el estado del error, permitiendo que la función informe del problema sin alterar el valor de retorno principal.

Ejemplo 1 (valor especial):
```c
float raiz(float x) {
    if (x < 0) return -1.0; // Valor especial
    return sqrt(x);
}
// Uso:
float resultado = raiz(-5);
if (resultado < 0) {
    printf("Error: número negativo\n");
}
```

Ejemplo 2 (parámetro de error):
```c
int raiz(float x, float *resultado) {
    if (x < 0) return 0; // 0 indica error
    *resultado = sqrt(x);
    return 1; // 1 indica éxito
}
// Uso:
float r;
if (!raiz(-5, &r)) {
    printf("Error: número negativo\n");
}
```

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un mecanismo que permite señalar la ocurrencia de un error o situación inesperada durante la ejecución de un programa. Cuando se produce una excepción, el flujo normal del programa se interrumpe y se transfiere el control a un bloque especial encargado de manejar el error. El objetivo principal es separar la lógica de manejo de errores del código funcional, facilitando la detección, propagación y gestión de problemas sin mezclar ambos aspectos.

El programador utiliza excepciones para informar de errores de manera estructurada, permitiendo que el código llamador decida cómo actuar ante situaciones anómalas. Al implementar funciones, se puede lanzar una excepción cuando no es posible continuar normalmente. Al llamar funciones, se puede capturar y tratar la excepción, evitando que el programa termine abruptamente y permitiendo una respuesta adecuada.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java, el control de errores se realiza mediante excepciones. El método de cálculo de la raíz puede lanzar una excepción si el número es negativo, y el método `main` puede capturarla para informar al usuario. Esto permite separar la lógica de cálculo de la gestión de errores.

Ejemplo:
```java
class Calculadora {
    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo");
        }
        return Math.sqrt(x);
    }
}

public class Main {
    public static void main(String[] args) {
        try {
            double resultado = Calculadora.raiz(-5);
            System.out.println("Raíz: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

Lanzar una excepción consiste en crear y emitir un objeto de error cuando ocurre una condición anómala, interrumpiendo el flujo normal del programa. Controlar o capturar una excepción implica interceptarla mediante un bloque `catch`, permitiendo gestionar el error y evitar que el programa termine abruptamente. Propagar una excepción significa que, si no se captura en la función donde se lanza, la excepción se transmite hacia arriba en la pila de llamadas hasta encontrar un manejador adecuado.

Durante la propagación, las funciones que no capturan la excepción finalizan su ejecución y liberan sus recursos, sin reanudar después. Solo la función que contiene el bloque `catch` puede continuar ejecutando código tras el manejo del error. En el ejemplo de la raíz cuadrada, si `Calculadora.raiz` lanza una excepción, el método `main` la captura y puede informar al usuario; si no existiera el bloque `catch`, la excepción seguiría propagándose y el programa terminaría.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La propagación natural de excepciones en Java permite que los errores sean gestionados en el nivel más adecuado, sin necesidad de comprobar manualmente el estado tras cada llamada. Esto simplifica el código, reduce la posibilidad de omitir controles y mejora la legibilidad. En C, el programador debe verificar explícitamente los valores de retorno o estados de error, lo que puede llevar a errores si se olvida alguna comprobación.

Además, la propagación automática facilita la recuperación de recursos y la limpieza del entorno, ya que las funciones interrumpidas por una excepción liberan sus variables locales y memoria. Esto contribuye a una gestión más robusta y segura de los errores, evitando fugas y comportamientos inesperados.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

En Java y otros lenguajes orientados a objetos, las excepciones se representan mediante objetos, generalmente derivados de una clase base como `Exception`. Esto permite encapsular información relevante sobre el error, como el mensaje, la causa y otros datos específicos, facilitando su gestión y transmisión.

La encapsulación permite crear excepciones personalizadas, adaptadas a las necesidades de cada aplicación. Se pueden definir clases propias que incluyan atributos y métodos adicionales, proporcionando mayor flexibilidad y claridad en el manejo de errores. Así, el programador puede distinguir entre diferentes tipos de problemas y actuar en consecuencia.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

Un objeto excepción en Java contiene información esencial como el mensaje descriptivo del error, el tipo de excepción, la causa original (si existe) y la traza de pila que indica el recorrido de llamadas hasta el punto donde se produjo el error. Esta información facilita la identificación y diagnóstico del problema.

En C, al no existir objetos excepción, la información suele limitarse a un código de error o un mensaje simple, lo que dificulta el análisis detallado. La traza de pila y la causa permiten comprender el contexto y origen del error, mejorando la capacidad de respuesta y depuración.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

En Java, es posible colocar varios bloques `catch` tras un mismo bloque `try`, cada uno destinado a capturar un tipo específico de excepción. Esto permite tratar de manera diferenciada los distintos errores que puedan surgir durante la ejecución del código protegido.

Sin embargo, solo se ejecuta el primer bloque `catch` que coincida con el tipo de excepción lanzada. Una vez capturada la excepción, los demás bloques `catch` se omiten y el flujo continúa tras el bloque de manejo de errores.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

Para garantizar la ejecución de código esencial, como el cierre de ficheros o la liberación de recursos, Java proporciona el bloque `finally`. Este bloque se ejecuta siempre, ocurra o no una excepción, asegurando la limpieza del entorno antes de continuar o finalizar el programa.

Ejemplo con `catch`:
```java
try {
    // Abrir fichero
} catch (IOException e) {
    // Manejar error
} finally {
    // Cerrar fichero
}
```

Ejemplo sin `catch`:
```java
try {
    // Abrir fichero
} finally {
    // Cerrar fichero
}
```

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

El bloque `finally` puede utilizarse sin necesidad de un bloque `catch`, y se ejecuta siempre, independientemente de si ocurre o no una excepción. Esto garantiza que el código de limpieza se realice en cualquier circunstancia.

Incluso si hay un `return` dentro del bloque `try`, el código del bloque `finally` se ejecuta antes de que el método retorne. Esta característica asegura la correcta liberación de recursos y la finalización de tareas críticas.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

Las excepciones controladas (`checked`) son aquellas que el compilador obliga a capturar o declarar mediante `throws`, como `IOException`. Las no controladas (`unchecked`) derivan de `RuntimeException` y no requieren declaración ni captura explícita, como `NullPointerException` o `IllegalArgumentException`.

`RuntimeException` representa errores de programación o condiciones inesperadas que suelen indicar fallos lógicos. Ejemplos de excepciones controladas: `IOException`, `SQLException`, `FileNotFoundException`. Ejemplos de no controladas: `NullPointerException`, `IllegalArgumentException`, `ArithmeticException`.

Situaciones para excepciones controladas:
- Error al abrir un fichero
- Fallo en la conexión a base de datos
- Problemas de entrada/salida

Situaciones para excepciones no controladas:
- Argumento inválido en método
- División por cero
- Acceso a objeto nulo

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

La palabra clave `throws` se utiliza en la firma de un método para indicar que puede lanzar una excepción controlada, delegando la responsabilidad de manejarla al código llamador. Esto permite que el método no tenga que capturar la excepción internamente, sino que la propague hacia arriba en la pila de llamadas.

Es una alternativa a capturar la excepción porque permite que el manejo se realice en un nivel superior, donde se tenga más contexto o capacidad de respuesta. Así, se facilita la modularidad y la claridad del código, evitando la gestión de errores en cada función.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

Ejemplo:
```java
import java.io.*;

public void abrirFichero(String nombre) throws FileNotFoundException {
    FileInputStream fis = null;
    try {
        fis = new FileInputStream(nombre);
        // Operaciones con el fichero
    } finally {
        if (fis != null) {
            try { fis.close(); } catch (IOException e) { /* Ignorar */ }
        }
    }
}
```

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Es posible declarar excepciones no controladas en `throws`, como `RuntimeException`, aunque no es obligatorio. El método llamador puede capturarlas mediante `try-catch`, pero normalmente estas excepciones indican errores de programación que deben corregirse, no gestionarse.

Capturar una excepción no controlada puede tener sentido en casos donde se desea evitar la terminación abrupta del programa o registrar información adicional, pero en general se recomienda revisar el código para evitar que ocurran.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Las excepciones controladas se recomiendan para situaciones previsibles, como errores de entrada/salida, donde el programador puede anticipar y gestionar el problema. Las no controladas se usan para condiciones inesperadas o fallos lógicos, como argumentos inválidos o errores de programación.

No todos los lenguajes distinguen entre ambos tipos; en muchos, solo existen excepciones no controladas, siendo la opción más habitual. Java es uno de los pocos que implementa ambas categorías de manera explícita.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Lanzar excepciones dentro de un bloque `catch` puede ser útil para encapsular el error original en una excepción de mayor nivel o para informar de un problema adicional detectado durante el manejo. Relanzar la misma excepción capturada permite que el error siga propagándose, por ejemplo, cuando no se puede resolver localmente.

Ejemplo de lanzar nueva excepción:
```java
catch (IOException e) {
    throw new RuntimeException("Error al procesar fichero", e);
}
```

Ejemplo de relanzar la misma excepción:
```java
catch (IOException e) {
    // Registrar el error
    throw e;
}
```

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Una excepción puede ser la causa de otra cuando se encapsula el error original dentro de una excepción personalizada, permitiendo conservar el contexto y la información del problema inicial. Esto facilita el diagnóstico y la trazabilidad de errores complejos.

Ejemplo:
```java
catch (IOException e) {
    throw new MiExcepcionPersonalizada("Error de alto nivel", e);
}
```

Cuando una excepción se imprime en pantalla, la causa suele mostrarse en la traza de pila, indicando el origen y el recorrido de los errores encadenados.

