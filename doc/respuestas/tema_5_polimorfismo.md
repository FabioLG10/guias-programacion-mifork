# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

El polimorfismo (literalmente "muchas formas") es un principio fundamental de la orientación a objetos que permite que un mismo método pueda tener diferentes implementaciones según el tipo específico del objeto. Sirve para escribir código genérico que funciona con referencias de una superclase pero produce comportamientos diferentes según el tipo real (subclase) del objeto. Esto es lo que hace que un array de `Soldado` puede contener tanto `Artillero` como `Zapador`, y llamar a un método sobre ellos produce comportamientos distintos según el tipo específico.

La **sobreescritura** (overriding) de métodos es el mecanismo mediante el cual una subclase proporciona su propia implementación de un método que ya existe en su superclase, reemplazando completamente el comportamiento heredado. Es importante destacar que una subclase solo puede sobreescribir un método si este existe en la superclase; en caso contrario se está definiendo un nuevo método. La sobreescritura es lo que permite que distintas subclases reaccionen de forma diferente al mismo mensaje (llamada a método), realizando acciones apropiadas a su naturaleza específica.

Es la combinación de la compatibilidad de tipos (una referencia de superclase puede apuntar a objetos de cualquier subtipo) con la sobreescritura de métodos lo que crea el polimorfismo. Sin este mecanismo, Java sería un lenguaje mucho menos flexible, ya que sería necesario conocer el tipo exacto de cada objeto en tiempo de compilación para invocar sus métodos correctamente.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La **ligadura dinámica** o **enlace tardío** (dynamic binding/late binding) es el mecanismo por el cual se determina en tiempo de ejecución (no en tiempo de compilación) qué método concreto se invoca. Cuando se llama a un método sobre una referencia de tipo `Soldado`, el sistema debe decidir en tiempo de ejecución si ejecutar el `saludar()` de `Soldado`, de `Artillero` o de `Zapador`. Esta decisión se toma observando el tipo real del objeto en memoria, no el tipo de la referencia.

La relación con el polimorfismo es directa: sin ligadura dinámica no habría polimorfismo real. El polimorfismo depende completamente de que las decisiones sobre qué método ejecutar se posterguen hasta el momento de corrida del programa. En **Java**, el enlace tardío es el **comportamiento por defecto y obligatorio** para todos los métodos (excepto los estáticos y los privados). El programador no necesita hacer nada especial; Java gestiona automáticamente la ligadura dinámica.

En cambio, **C++** requiere declaración explícita: los métodos deben marcarse como `virtual` para que se use ligadura dinámica. Sin la palabra clave `virtual`, C++ utiliza ligadura estática (static binding), determinando en tiempo de compilación qué método invocar. **Python**, como lenguaje dinámico, siempre usa ligadura dinámica por defecto; de hecho, Python no verifica tipos en tiempo de compilación (porque no tiene compilación en el sentido tradicional).

Esta diferencia filosófica muestra que Java potencia el polimorfismo como mecanismo fundamental, mientras que C++ ofrece el programador la opción de elegir entre rigidez (ligadura estática) y flexibilidad (ligadura dinámica).


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

El ejemplo a continuación ilustra cómo una referencia de tipo `Soldado` puede apuntar a objetos de distintos tipos, y cada uno ejecuta su propia versión del método `saludar()` gracias a la ligadura dinámica:

```java
class Soldado {
    protected String nombre;
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
}

class Artillero extends Soldado {
    public Artillero(String nombre) {
        super(nombre);
    }
    
    // Artillero hereda saludar() sin sobreescribir
}

class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }
    
    @Override
    public void saludar() {
        System.out.println("Soy zapador, especialista en minas");
    }
}

public class TestPolimorfismo {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];
        ejercito[0] = new Soldado("GenéricA");
        ejercito[1] = new Artillero("Juan");
        ejercito[2] = new Zapador("María");
        ejercito[3] = new Artillero("Pedro");
        
        for (Soldado s : ejercito) {
            s.saludar();  // El método ejecutado depende del tipo real
        }
    }
}
// Salida:
// Soy el soldado GenéricA
// Soy el soldado Juan
// Soy zapador, especialista en minas
// Soy el soldado Pedro
```

Este ejemplo demuestra que aunque todas las referencias son de tipo `Soldado`, el método que se ejecuta es el específico de cada clase. El `Artillero` usa el `saludar()` heredado de `Soldado`, mientras que `Zapador` tiene su propia implementación. La belleza del polimorfismo es que el código que recorre el array no necesita saber qué tipo específico es cada objeto; simplemente llama a `saludar()` y confía en que cada objeto ejecutará su versión correcta.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí, una subclase puede invocar la implementación del método en la superclase, incluso después de sobreescribirlo. Esto es particularmente útil cuando la subclase desea extender el comportamiento existente en lugar de reemplazarlo completamente. Para ello, se utiliza la palabra clave **`super`**, que refiere explícitamente a la superclase y permite llamar a sus métodos como `super.nombreMetodo()`. Esto representa el "método base" y facilita la reutilización de código en la jerarquía de clases.

```java
class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }
    
    @Override
    public void saludar() {
        super.saludar();  // Invoca el saludar() de Soldado
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}

// Uso:
Soldado z = new Zapador("María");
z.saludar();
// Salida:
// Soy el soldado María
// ZAPADOR A SUS ORDENES
```

En este ejemplo, `Zapador.saludar()` primero ejecuta lo que haría `Soldado.saludar()` (gracias a `super.saludar()`), y luego añade su propio comportamiento. Este patrón es muy común en la práctica real: permite que las subclases personalicen comportamientos sin necesidad de reescribir completamente la lógica de la superclase, reduciendo duplicación de código y haciendo el diseño más mantenible. La palabra clave `super` también se usa en constructores (como vimos en el tema de herencia) para invocar al constructor de la superclase.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al sobreescribir un método en Java existen restricciones importantes para garantizar que la sobreescritura es válida. Los **tipos de los parámetros deben ser exactamente iguales**: no se puede cambiar el número, orden o tipos de los parámetros. Si se cambia, se está creando un nuevo método (sobrecarga), no sobreescribiendo. Respecto al **tipo de retorno**, Java permite un refinamiento: el tipo de retorno puede ser un subtipo del original (contravariance). Por ejemplo, si la superclase retorna `Soldado`, la subclase puede retornar `Zapador` (un subtipo más específico). Esto es seguro porque el código que usa la referencia de superclase espera un `Soldado`, y un `Zapador` es compatible.

La **sobreescritura** (overriding) y la **sobrecarga** (overloading) son conceptos distintos. La sobreescritura ocurre cuando una subclase proporciona una nueva implementación de un método heredado de la superclase (mismos parámetros, ligadura dinámica). La sobrecarga ocurre cuando la misma clase (o sus subclases) definen múltiples métodos con el mismo nombre pero **diferentes parámetros**; la sobrecarga se resuelve en tiempo de compilación (ligadura estática). Por ejemplo, una clase puede tener `void atacar()` y `void atacar(int potencia)`; estos son métodos sobrecargados.

```java
class Soldado {
    public void saludar() { System.out.println("Hola"); }
    public void atacar() { System.out.println("Atacando"); }
    public void atacar(int poder) { System.out.println("Atacando con poder " + poder); }  // Sobrecarga
}

class Zapador extends Soldado {
    @Override
    public void saludar() { System.out.println("Zapador saluda"); }  // Sobreescritura
}
```

La anotación **`@Override`** es un marcador que indica al compilador que el método intenta sobreescribir uno de la superclase. Si el compilador detecta que la firma no coincide (por ejemplo, se cambió el tipo de retorno de forma inválida), genera un error de compilación. Es recomendable usarla siempre porque: (1) previene errores accidentales donde se cree que se está sobreescribiendo pero en realidad se está creando un nuevo método, (2) sirve como documentación clara del intento del programador, (3) mejora la legibilidad del código. Aunque `@Override` es opcional, es considerada una práctica profesional esencial.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Sí, exactamente. Desde las primeras clases, cuando se define una clase y se sobreescribe `toString()` o `equals()`, se está usando polimorfismo, aunque probablemente no se haya nombrado así explícitamente. Estos métodos existen en la superclase `Object` (la clase base de todas las clases en Java), y cuando se proporciona una implementación específica en una clase personal, se está sobreescribiendo. El polimorfismo ocurre automáticamente cuando el código utiliza referencias de tipo `Object` (aunque raramente se hace explícitamente con `Object`), o cuando frameworks y librerías de Java invocan estos métodos sobre objetos desconocidos.

Un ejemplo clásico es el método `System.out.println()`. Cuando se pasa cualquier objeto a esta función, internamente invoca el método `toString()` del objeto. Si la clase sobreescribe `toString()`, esa versión personalizada se ejecuta (ligadura dinámica), no la versión genérica heredada de `Object`. Lo mismo ocurre con colecciones como `ArrayList` o `HashSet`: cuando usan `equals()` para comparar objetos, invocan la versión específica de cada clase si ha sido sobreescrita.

Por tanto, el polimorfismo no es un concepto avanzado que aparece solo en código complejo: está presente desde las primeras clases. La diferencia es que cuando se llega al tema de herencia y polimorfismo formalmente, se aprende a utilizarlo de forma consciente y deliberada, con jerarquías de clases que se diseñan específicamente para aprovechar este poder. Pero el mecanismo subyacente (sobreescritura y ligadura dinámica) opera desde el principio.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una **clase abstracta** es una clase que no está destinada a ser instanciada directamente, sino a servir como plantilla para sus subclases. Se declara con la palabra clave `abstract` y representa un concepto general del cual no tiene sentido crear objetos concretos. Un **método abstracto** es un método sin implementación que se declara en una clase abstracta; solo existe su firma (nombre, parámetros, tipo de retorno), pero no tiene cuerpo. Las subclases están obligadas a proporcionar una implementación concreta de todos los métodos abstractos. No se puede crear una instancia de una clase abstracta; si se intenta, el compilador genera un error. Una clase que extiende una clase abstracta debe (1) implementar todos los métodos abstractos, o (2) también ser abstracta, dejando la responsabilidad a la siguiente generación de subclases.

```java
abstract class Soldado {
    protected String nombre;
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {  // Método concreto
        System.out.println("Soy el soldado " + nombre);
    }
    
    abstract public void atacar();  // Método abstracto, sin cuerpo
}

class Artillero extends Soldado {
    public Artillero(String nombre) {
        super(nombre);
    }
    
    @Override
    public void atacar() {
        System.out.println("¡Disparando cohetes!");
    }
}

class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }
    
    @Override
    public void atacar() {
        System.out.println("¡Poniendo minas!");
    }
}

// Uso:
// Soldado s = new Soldado("Juan");  // ERROR: no se puede instanciar clase abstracta
Soldado a = new Artillero("Juan");
Soldado z = new Zapador("María");

a.atacar();  // Salida: ¡Disparando cohetes!
z.atacar();  // Salida: ¡Poniendo minas!
```

La palabra clave `abstract` se coloca en dos lugares: (1) antes de la palabra clave `class` cuando la clase es abstracta, (2) antes del tipo de retorno del método cuando es un método abstracto. En el ejemplo, `atacar()` carece de llaves `{}` y termina con punto y coma, indicando que es un método abstracto sin implementación. Las clases concretas (`Artillero` y `Zapador`) implementan `atacar()` de acuerdo a su naturaleza específica. Las clases abstractas son fundamentales para el polimorfismo: permiten definir contratos que las subclases deben cumplir, garantizando una interfaz consistente mientras se permite diferentes comportamientos.


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

La palabra clave `final` en Java se utiliza para **denegar la modificación**. Cuando se aplica a un método, indica que ese método no puede ser sobreescrito en las subclases. Cuando se aplica a una clase, indica que la clase no puede ser extendida (no se puede crear subclases). `final` es lo opuesto al espíritu del polimorfismo: mientras que el polimorfismo fomenta la flexibilidad y la extensión, `final` la restringe. Sin embargo, `final` tiene propósitos legítimos: asegurar que cierto comportamiento no sea modificado, porque es crítico para la correctitud, la seguridad o el rendimiento.

```java
final class Constante {  // No se puede extender
    // ...
}

class Soldado {
    final public void regla() {  // No se puede sobreescribir
        System.out.println("Esta es una regla inquebrantable");
    }
    
    public void atacar() {
        // Puede ser sobreescrito
    }
}

class Artillero extends Soldado {
    // Esto es OK:
    @Override
    public void atacar() { /* ... */ }
    
    // Esto genera ERROR de compilación:
    // @Override
    // public void regla() { /* ... */ }
}
```

En la API estándar de Java, existen varias clases `final`: `String`, `Integer`, `Double`, y otras clases envolutorio de tipos primitivos. La clase `String` es `final` porque es fundamental para el lenguaje y su inmutabilidad es criticada; permitir subclases podría quebrantar supuestos de seguridad. Del mismo modo, las clases numéricas son `final` para garantizar ciertos comportamientos invariantes. El uso de `final` debe ser deliberado y documentado: se recomienda cuando se tiene una razón específica para prevenir que se sobrescriba un método o se extienda una clase, no como comportamiento por defecto.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Una **interfaz** en Java es un contrato que especifica qué métodos debe poseer una clase que la implemente. Es similar a una clase abstracta, pero con diferencias importantes: una interfaz contiene principalmente declaraciones de métodos abstractos (sin implementación) y constantes, mientras que una clase abstracta puede tener métodos concretos y atributos con estado. Una interfaz representa el "qué hace" de un objeto (su comportamiento), mientras que una clase abstracta es más una plantilla que define "qué es" un objeto. Tradicionalmente en Java, todas las variables en una interfaz son implícitamente `static final`, y todos los métodos son implícitamente `abstract public`. Aunque versiones recientes de Java (Java 8+) permiten métodos con implementación en interfaces mediante `default`, el propósito fundamental sigue siendo definir contratos.

La interfaz se declara con la palabra clave `interface` y se implementa con la palabra clave `implements`. Crucialmente, **una clase puede implementar múltiples interfaces**, lo que soluciona el problema de la herencia múltiple de clases de una forma controlada. Mientras que una clase solo puede extender una única clase abstracta (herencia simple), puede implementar tantas interfaces como sea necesario. Esto permite que un objeto cumpla múltiples roles o contratos simultáneamente.

```java
interface Atacable {
    void atacar();
}

interface Defensor {
    void defender();
}

class Soldado implements Atacable, Defensor {
    public void atacar() {
        System.out.println("Atacando");
    }
    
    public void defender() {
        System.out.println("Defendiendo posición");
    }
}

// Uso:
Atacable a = new Soldado();  // Polimorfismo de interfaz
a.atacar();

Defensor d = new Soldado();  // El mismo objeto, otro contrato
d.defender();
```

Las interfaces son fundamentales para el polimorfismo de contrato: una clase que implementa una interfaz puede ser tratada como cualquier tipo de objeto que implemente esa interfaz, independientemente de su jerarquía de herencia. Esta es una forma más flexible y potente de polimorfismo que la herencia de clases.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

Este ejemplo demuestra el poder del polimorfismo aplicado a un cálculo geométrico. La clase `Punto` es abstracta y define un contrato: cualquier punto debe poder calcular su distancia a otro punto. Las subclases concretas `Punto2D` y `Punto3D` proporcionan las fórmulas específicas. La clase `Linea` utiliza polimorfismo: solo trabaja con referencias de tipo `Punto`, sin necesidad de conocer si son 2D o 3D. La validación con `instanceof` y downcasting garantiza que se calcula correctamente la distancia entre puntos del mismo tipo.

```java
abstract class Punto {
    abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    double x, y;
    
    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Distancia 2D entre puntos de diferente dimensión");
        }
        Punto2D p = (Punto2D) otro;  // Downcasting
        return Math.sqrt((x - p.x) * (x - p.x) + (y - p.y) * (y - p.y));
    }
}

class Punto3D extends Punto {
    double x, y, z;
    
    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }
    
    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Distancia 3D entre puntos de diferente dimensión");
        }
        Punto3D p = (Punto3D) otro;  // Downcasting
        return Math.sqrt((x - p.x) * (x - p.x) + (y - p.y) * (y - p.y) + (z - p.z) * (z - p.z));
    }
}

class Linea {
    Punto inicio, fin;
    
    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }
    
    public double getLongitud() {
        return inicio.calcularDistanciaA(fin);  // Polimorfismo: sin saber qué tipo de Punto
    }
}

// Uso:
public static void main(String[] args) {
    Punto p1 = new Punto2D(0, 0);
    Punto p2 = new Punto2D(3, 4);
    Linea linea2D = new Linea(p1, p2);
    System.out.println("Longitud 2D: " + linea2D.getLongitud());  // 5.0
    
    Punto p3 = new Punto3D(0, 0, 0);
    Punto p4 = new Punto3D(1, 1, 1);
    Linea linea3D = new Linea(p3, p4);
    System.out.println("Longitud 3D: " + linea3D.getLongitud());  // 1.732...
}
```

Este diseño ilustra un principio clave: la clase `Linea` es completamente agnóstica respecto a si trabaja con puntos 2D o 3D. Solo invoca `calcularDistanciaA()` sobre referencias de tipo `Punto`, confiando en que la ligadura dinámica ejecutará el método correcto. Los tipos específicos (`Punto2D` o `Punto3D`) están implícitos; la cálculo de distancia se realiza automáticamente según el tipo real de los objetos. Este es el polimorfismo en su mejor expresión: código genérico que funciona correctamente con múltiples tipos concretos.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

La **herencia de interfaces** permite que una interfaz extienda otra interfaz usando la palabra clave `extends`. Una interfaz que extiende otra hereda todos sus métodos abstractos, así como sus constantes. La clase que finalmente implemente la interfaz derivada debe proporcionar implementaciones para todos los métodos heredados más los nuevos. Esto es diferente de la herencia de clases convencional: una interface puede extender múltiples interfaces (herencia múltiple de interfaces), ya que interfaces son puras abstracciones sin estado.

```java
interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}

class FicheroTexto implements FicheroEscribible {
    private String contenido = "";
    
    @Override
    public String leer() {
        return contenido;
    }
    
    @Override
    public void escribir(String contenido) {
        this.contenido = contenido;
    }
    
    @Override
    public void eliminar() {
        this.contenido = "";
    }
}

// Uso:
public static void main(String[] args) {
    FicheroEscribible f = new FicheroTexto();
    f.escribir("Hola mundo");
    System.out.println(f.leer());  // Hola mundo
    f.eliminar();
    System.out.println(f.leer());  // (vacío)
    
    // Polimorfismo: la interfaz del supertipo funciona también
    Fichero simple = f;
    System.out.println(simple.leer());  // (vacío)
}
```

Sí existe **herencia múltiple de interfaces**. Una interfaz puede extender múltiples otras interfaces: `interface A extends B, C, D { }`. La clase que implementa `A` debe proporcionar implementaciones para todos los métodos de `A`, `B`, `C` y `D`. Esto resuelve elegantemente el problema de la herencia múltiple de clases: las interfaces son "contratos" puros sin estado, por lo que no hay conflictos de inicialización ni dilemas de diamante como en C++. La herencia de interfaces es una forma poderosa de composición de comportamiento: permite construir interfaces complejas combinando simples de forma modular.
