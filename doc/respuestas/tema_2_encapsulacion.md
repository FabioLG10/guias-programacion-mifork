# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

La encapsulación en la Programación Orientada a Objetos (POO) consiste en agrupar los datos (atributos) y los métodos que operan sobre ellos dentro de una misma unidad llamada clase. Este principio permite que los detalles internos de funcionamiento de un objeto queden protegidos del acceso directo desde el exterior, lo que se conoce como ocultación de información. Así, solo se expone lo necesario a través de una interfaz pública, mientras que el resto permanece inaccesible para el usuario de la clase.

La ocultación de información tiene varias ventajas. En primer lugar, ayuda a evitar que el estado interno de un objeto sea modificado de manera incorrecta o inesperada, ya que solo se puede interactuar con él mediante los métodos definidos por la clase. Además, facilita el mantenimiento y la evolución del código, ya que los cambios internos en la implementación no afectan a quienes utilizan la clase, siempre que la interfaz pública se mantenga estable.

Otra ventaja importante es que se reduce la posibilidad de errores, ya que se limita el acceso a los datos sensibles o críticos. También se promueve la reutilización del código, al permitir que los objetos sean tratados como "cajas negras" que cumplen una función específica sin necesidad de conocer sus detalles internos.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La interfaz pública de un objeto o clase en Programación Orientada a Objetos (POO) se refiere al conjunto de métodos y atributos que pueden ser utilizados desde fuera de la clase. Es decir, representa el “contrato” que la clase ofrece a quienes la usan, definiendo cómo se puede interactuar con los objetos creados a partir de esa clase. Normalmente, la interfaz pública está formada por los métodos declarados como `public` en Java.

Esta interfaz pública es fundamental para la ocultación de información, ya que permite separar claramente qué detalles internos quedan protegidos y cuáles están disponibles para el usuario de la clase. De este modo, se puede modificar la implementación interna sin afectar a quienes utilizan la clase, siempre que la interfaz pública permanezca igual. Así, la ocultación de información y la interfaz pública trabajan juntas para garantizar la robustez y flexibilidad del software.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

El diseño de la interfaz pública de una clase debe realizarse con especial atención, ya que define cómo otros componentes del programa interactúan con esa clase. Una interfaz pública mal diseñada puede dificultar el uso correcto de la clase o permitir usos no deseados, lo que puede llevar a errores o a un mantenimiento más complejo del software.

Cambiar la interfaz pública de una clase no suele ser una tarea sencilla, especialmente si la clase ya está siendo utilizada en diferentes partes del programa o incluso en otros proyectos. Modificar la interfaz implica revisar y posiblemente modificar todo el código que depende de ella, lo que puede ser costoso y propenso a errores. Por ello, se recomienda definir cuidadosamente qué métodos y atributos deben ser accesibles desde fuera y cuáles deben permanecer ocultos.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las invariantes de clase son condiciones o reglas que deben cumplirse en todo momento para que el estado de un objeto sea considerado válido. Estas invariantes suelen establecerse en el diseño de la clase y se espera que se mantengan durante toda la vida del objeto, excepto durante la ejecución de algunos métodos internos que pueden modificarlas temporalmente.

La ocultación de información facilita el mantenimiento de las invariantes de clase, ya que impide que el estado interno del objeto sea modificado directamente desde fuera de la clase. De este modo, solo los métodos definidos por la propia clase pueden alterar los atributos internos, permitiendo controlar y garantizar que las invariantes se respeten siempre. Así, se reduce el riesgo de que el objeto entre en un estado inconsistente o erróneo.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

En Java, una clase `Punto` que utiliza la ocultación de información puede definirse de la siguiente manera:

```java
public class Punto {
	private double x;
	private double y;

	public Punto(double x, double y) {
		this.x = x;
		this.y = y;
	}

	public double calcularDistanciaAOrigen() {
		return Math.sqrt(x * x + y * y);
	}
}
```

La interfaz pública de la clase `Punto` está formada por el constructor y el método `calcularDistanciaAOrigen`, ambos declarados como `public`. Esto significa que pueden ser utilizados desde fuera de la clase. Por otro lado, los atributos `x` e `y` son `private`, lo que implica que solo pueden ser accedidos y modificados desde dentro de la propia clase.

En Java, la palabra clave `public` indica que un elemento (clase, método o atributo) es accesible desde cualquier parte del programa. En cambio, `private` restringe el acceso únicamente al interior de la clase donde se declara, favoreciendo la ocultación de información y la protección del estado interno del objeto.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores `public` y `private` se pueden aplicar a clases, métodos, constructores y atributos (variables de instancia o de clase). Cuando se utiliza `public`, el elemento es accesible desde cualquier parte del programa. En cambio, `private` restringe el acceso únicamente al interior de la clase donde se declara.

En el caso de las clases, solo las clases internas pueden ser `private`; las clases principales solo pueden ser `public` o tener visibilidad por defecto (package-private). Para métodos, constructores y atributos, ambos modificadores pueden emplearse libremente, permitiendo controlar el nivel de acceso y proteger la información interna de la clase.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

En Java, los miembros de instancia privados de un objeto están ocultos para otras clases, pero no para otras instancias de la misma clase. Esto significa que un método de la clase puede acceder a los atributos privados de cualquier objeto de esa clase, no solo a los suyos propios.

Por ejemplo, en la clase `Punto` se puede añadir el siguiente método:

```java
public double calcularDistanciaAPunto(Punto otro) {
	double dx = this.x - otro.x;
	double dy = this.y - otro.y;
	return Math.sqrt(dx * dx + dy * dy);
}
```

En este caso, aunque `x` e `y` son privados, el método puede acceder a `otro.x` y `otro.y` porque ambos objetos son instancias de la misma clase. Por tanto, la ocultación de información protege frente a otras clases, pero no entre instancias de la misma clase.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

Cuando se dice que la ocultación de información mejora la "seguridad" del programa, no se hace referencia a la seguridad frente a ataques externos o hackeos, sino a la protección de la integridad interna del objeto. La ocultación de información evita que el estado interno de un objeto sea modificado de manera incorrecta o inesperada por otras partes del programa.

La seguridad en este contexto se refiere a la robustez y fiabilidad del software, no a la protección frente a accesos no autorizados desde fuera del sistema. Para proteger un programa frente a hackeos se requieren otras medidas, como el control de acceso, la validación de entradas y el uso de mecanismos de seguridad a nivel de sistema operativo o red.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Un miembro de instancia es un atributo o método que pertenece a cada objeto individual de una clase; cada instancia tiene su propia copia. En cambio, un miembro de clase (también llamado estático) pertenece a la clase en sí y es compartido por todas las instancias.

En Java, tanto los miembros de instancia como los de clase pueden ser declarados `private`, `public` u otros modificadores de visibilidad. Por tanto, los miembros de clase también pueden ocultarse, restringiendo su acceso solo a la propia clase o al paquete, según el modificador utilizado.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

En algunos casos, tiene sentido que los constructores sean privados. Esto se utiliza, por ejemplo, para evitar que se creen instancias directamente desde fuera de la clase. Es común en patrones de diseño como el Singleton o cuando se emplean métodos factoría estáticos para controlar la creación de objetos.

Al hacer privado el constructor, se obliga a que la creación de instancias se realice solo a través de métodos específicos definidos por la propia clase, permitiendo mayor control sobre el proceso de inicialización y las invariantes de clase.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

En Java, los miembros de clase se indican utilizando la palabra clave `static`. Estos miembros son compartidos por todas las instancias de la clase. Por ejemplo, en la clase `Punto` se pueden añadir dos atributos estáticos para registrar los valores máximos de `x` e `y`:

```java
public class Punto {
	private double x;
	private double y;
	private static double maxX = Double.NEGATIVE_INFINITY;
	private static double maxY = Double.NEGATIVE_INFINITY;

	public Punto(double x, double y) {
		this.x = x;
		this.y = y;
		if (x > maxX) maxX = x;
		if (y > maxY) maxY = y;
	}

	public static double getMaxX() { return maxX; }
	public static double getMaxY() { return maxY; }
}
```

En este ejemplo, `maxX` y `maxY` son miembros de clase y se actualizan cada vez que se crea un nuevo punto. Los métodos `getMaxX` y `getMaxY` permiten consultar estos valores.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

La clase `Punto` puede modificarse para usar un array interno de dos posiciones, manteniendo la misma interfaz pública:

```java
public class Punto {
	private double[] coords;

	public Punto(double x, double y) {
		coords = new double[2];
		coords[0] = x;
		coords[1] = y;
	}

	public double calcularDistanciaAOrigen() {
		return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
	}
}
```

De este modo, la implementación interna cambia, pero la interfaz pública sigue siendo la misma para los usuarios de la clase.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Aunque un atributo tenga métodos getter y setter públicos, la convención más habitual en Java y en la mayoría de lenguajes orientados a objetos es declararlo como privado. Esto permite mantener la encapsulación y la posibilidad de añadir validaciones o lógica adicional en los métodos de acceso.

Declarar los atributos como públicos elimina la posibilidad de controlar los valores asignados y puede poner en riesgo las invariantes de clase, ya que cualquier parte del programa podría modificar el estado del objeto sin restricciones. Por tanto, la ocultación de información y el uso de getters y setters ayudan a proteger la integridad del objeto.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase se considera inmutable cuando, una vez creado un objeto, su estado no puede cambiar. Esto significa que no existen métodos que permitan modificar los valores de sus atributos después de la construcción.

Un método modificador es aquel que cambia el estado interno de un objeto. Aunque los setters son un tipo común de método modificador, no todos los métodos modificadores son setters; también pueden existir otros métodos que alteren atributos internos.

Las clases inmutables tienen varias ventajas: son más fáciles de razonar, resultan seguras en entornos concurrentes y evitan errores relacionados con cambios inesperados en el estado de los objetos. Un ejemplo clásico de clase inmutable en Java es `String`.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No es recomendable incluir métodos setter siempre por defecto. Solo deben proporcionarse cuando realmente se necesita permitir la modificación de un atributo desde fuera de la clase. Incluir setters innecesarios puede exponer el estado interno y dificultar el mantenimiento de las invariantes de clase.

La decisión de incluir un setter debe basarse en los requisitos de diseño y en la necesidad real de modificar el atributo. En muchos casos, es preferible que los objetos sean inmutables o que solo se permita la modificación a través de métodos controlados.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

En POO, los objetos pueden compararse por su identidad (si son el mismo objeto en memoria) o por su contenido (si los valores de sus atributos son iguales). En Java, el método `equals` se utiliza para comparar el contenido de los objetos.

Por defecto, el método `equals` compara la identidad, es decir, solo devuelve `true` si ambos objetos son exactamente el mismo. Para comparar el contenido, es necesario sobrescribir el método `equals` en la clase correspondiente.

En el caso de las cadenas (`String`), se debe utilizar el método `equals` para comparar su contenido, no el operador `==`, que solo compara la identidad de los objetos.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Las clases "wrapper" son clases que encapsulan un tipo de dato primitivo (como int, double, etc.) en un objeto. En Java, por ejemplo, existen clases como `Integer`, `Double` o `Boolean` que permiten tratar los valores primitivos como objetos.

En Java, la conversión entre tipos primitivos y sus wrappers puede hacerse de forma automática (autoboxing y unboxing) o manualmente. Las clases wrapper permiten utilizar los valores primitivos en contextos donde se requieren objetos, como en colecciones genéricas.

No todos los lenguajes orientados a objetos tienen tipos primitivos separados de los objetos, por lo que no siempre necesitan wrappers. Sin embargo, en lenguajes como Java, las clases wrapper aportan flexibilidad y funcionalidad adicional.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un tipo de dato enumerado es una estructura que permite definir un conjunto limitado y predefinido de valores posibles. En Java, los enumerados (`enum`) son clases especiales que representan estos conjuntos de valores.

Los enumerados en Java pueden tener atributos, métodos y constructores privados, lo que permite encapsular información y comportamiento asociado a cada valor. Esto aporta mayor seguridad y claridad al código, ya que se restringen los valores posibles y se agrupa la lógica relacionada en un solo lugar.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

Un ejemplo de tipo enumerado en Java que cumple con lo solicitado sería:

```java
public enum Mes {
	ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4), MAYO(31, 5), JUNIO(30, 6),
	JULIO(31, 7), AGOSTO(31, 8), SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

	private final int dias;
	private final int ordinalMes;

	Mes(int dias, int ordinalMes) {
		this.dias = dias;
		this.ordinalMes = ordinalMes;
	}

	public int getDias() { return dias; }
	public int getOrdinal() { return ordinalMes; }

	public boolean esDePrimavera(boolean esHemisferioNorte) {
		return esHemisferioNorte ? (ordinalMes >= 3 && ordinalMes <= 5) : (ordinalMes >= 9 && ordinalMes <= 11);
	}
	public boolean esDeVerano(boolean esHemisferioNorte) {
		return esHemisferioNorte ? (ordinalMes >= 6 && ordinalMes <= 8) : (ordinalMes >= 12 || ordinalMes <= 2);
	}
	public boolean esDeOtoño(boolean esHemisferioNorte) {
		return esHemisferioNorte ? (ordinalMes >= 9 && ordinalMes <= 11) : (ordinalMes >= 3 && ordinalMes <= 5);
	}
	public boolean esDeInvierno(boolean esHemisferioNorte) {
		return esHemisferioNorte ? (ordinalMes == 12 || ordinalMes <= 2) : (ordinalMes >= 6 && ordinalMes <= 8);
	}
}
```

Este enumerado encapsula los datos y métodos asociados a cada mes, permitiendo consultar los días, el ordinal y la estación según el hemisferio.
