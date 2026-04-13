# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

La herencia es un mecanismo que permite que una clase (subclase) herede atributos y métodos de otra clase (superclase), estableciendo una relación "es-un". Esta relación significa que un objeto de la subclase puede ser considerado también como un objeto de la superclase. A diferencia de los lenguajes procedurales como C/C++, en Java esta relación es fundamental para modelar jerarquías de tipos.

Las dos implicaciones principales son: (1) la **compatibilidad de tipos**, que permite usar una referencia de la superclase para apuntar a objetos de cualquier subclase, evitando duplicación de código en tratamiento uniforme; (2) la **herencia de estado y comportamiento**, donde la subclase adquiere automáticamente los atributos privados y públicos de la superclase, así como todos sus métodos, pudiendo extenderlos o sobrescribirlos.

```java
class Soldado {
    private String nombre;
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;
    
    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }
    
    public int getCohetes() {
        return cohetes;
    }
    
    public void disparar() {
        System.out.println("¡Disparando cohetes!");
    }
}

class Zapador extends Soldado {
    private int minas;
    
    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }
    
    public int getMinas() {
        return minas;
    }
    
    public void ponerMinas() {
        System.out.println("Poniendo minas");
    }
}

public class TestSoldados {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Juan", 10);
        ejercito[1] = new Zapador("María", 5);
        ejercito[2] = new Artillero("Pedro", 8);
        
        for (Soldado s : ejercito) {
            s.saludar();  // Todos comparten este comportamiento
        }
    }
}
```


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Cuando se crea una instancia de una subclase, se ejecutan múltiples constructores en una secuencia precisa. El flujo es de arriba hacia abajo en la jerarquía: primero se ejecuta el constructor de la superclase, y posteriormente el de la subclase. Por ejemplo, al crear un `new Artillero("Juan", 10)`, se ejecutan dos constructores: primero el de `Soldado` y luego el de `Artillero`.

La palabra clave `super` dentro de un constructor invoca explícitamente el constructor de la superclase. Esto permite pasar argumentos necesarios para inicializar los atributos heredados de la clase base. Si no se llama a `super` explícitamente, Java automáticamente invoca el constructor sin parámetros de la superclase. Si la superclase no posee un constructor sin parámetros (porque solo define constructores con parámetros), entonces la compilación fallará y es obligatorio llamar a `super` de forma explícita con los argumentos adecuados.

```java
class Soldado {
    private String nombre;
    
    public Soldado(String nombre) {
        // No hay constructor sin parámetros
        this.nombre = nombre;
        System.out.println("Constructor de Soldado ejecutado");
    }
}

class Artillero extends Soldado {
    private int cohetes;
    
    public Artillero(String nombre, int cohetes) {
        super(nombre);  // Obligatorio: inicializa el nombre del Soldado
        System.out.println("Constructor de Artillero ejecutado");
        this.cohetes = cohetes;
    }
}

// En el main:
Artillero a = new Artillero("Juan", 10);
// Salida:
// Constructor de Soldado ejecutado
// Constructor de Artillero ejecutado
```

En resumen, el constructor de la superclase siempre se ejecuta primero (ya sea explícitamente con `super` o implícitamente), garantizando que los atributos heredados se inicialicen correctamente antes de que la subclase inicialice los suyos.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Sí, los atributos privados de la superclase forman parte de la instancia de la subclase en memoria. Cuando se crea un objeto `Artillero`, la memoria reservada incluye el espacio para el atributo `nombre` (heredado de `Soldado`) más el espacio para el atributo `cohetes` (específico de `Artillero`). Esta es una diferencia conceptual importante respecto a la composición: en herencia, los datos de la superclase están literalmente contenidos en el objeto de la subclase, no como una referencia a un objeto separado.

Sin embargo, la privacidad de los atributos se mantiene: aunque `nombre` existe en memoria como parte del objeto `Artillero`, el código dentro de la clase `Artillero` **no puede acceder directamente** a `nombre` porque es privado en `Soldado`. El acceso a atributos privados es restringido únicamente al código de la clase que los define. Para que la subclase pueda usar `nombre`, debe hacerlo a través de métodos públicos o protegidos proporcionados por la superclase.

```java
class Artillero extends Soldado {
    private int cohetes;
    
    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }
    
    public void presentarse() {
        // INCORRECTO: System.out.println(nombre);  // Error: nombre es privado
        
        // CORRECTO: usar el método público de la superclase
        saludar();  // Acceso al método público publicado por Soldado
    }
}
```

En conclusión, los atributos privados sí residen en memoria como parte del objeto subclase, pero la encapsulación previene su acceso directo desde el código de la subclase, obligando al uso de métodos intermediarios.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos entre clases en una jerarquía de herencia permite escribir código genérico que funciona con la superclase, sin necesidad de modificarlo cuando se añaden nuevas subclases. Esta propiedad es fundamental para la extensibilidad del código, ya que permite agregar nuevas funcionalidades (nuevas subclases) sin alterar la lógica existente. Esto es especialmente valioso en proyectos grandes, donde modificar código existente introduce riesgo de introducir errores.

Por ejemplo, el código que itera sobre un array de `Soldado` e invoca `saludar()` en cada elemento funciona indistintamente con `Artillero`, `Zapador` o cualquier nueva subclase de `Soldado` que se defina en el futuro. El contrato es simple: cualquier cosa que sea un `Soldado` tiene el método `saludar()`, independientemente de su tipo específico.

```java
class Francotirador extends Soldado {
    private int balas;
    
    public Francotirador(String nombre, int balas) {
        super(nombre);
        this.balas = balas;
    }
    
    public int getBalas() {
        return balas;
    }
}

public class TestExtensibilidad {
    public static void main(String[] args) {
        // El código original sigue siendo exactamente igual
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Juan", 10);
        ejercito[1] = new Zapador("María", 5);
        ejercito[2] = new Francotirador("Carlos", 200);  // Nueva subclase
        
        // Este código NO precisa modificación alguna
        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

Esta propiedad demuestra por qué la herencia es tan poderosa en orientación a objetos: permite crear sistemas flexibles que crecen sin modificar el código cliente, siempre que se respete el contrato establecido por la superclase.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

Sí, es posible tener una referencia del supertipo (por ejemplo, `Soldado`) que apunte a objetos reales de un subtipo (por ejemplo, `Artillero`). Esta es una característica fundamental de la herencia en Java. Sin embargo, una referencia del supertipo solo proporciona acceso a los métodos definidos en la superclase, no a los métodos específicos del subtipo. Por ejemplo, con `Soldado s = new Artillero(...);`, no se puede invocar `s.disparar()` aunque el objeto real sea un `Artillero`, porque `disparar()` no existe en `Soldado`.

El **upcasting** es la asignación de un objeto de subtipo a una referencia de supertipo, y ocurre automáticamente sin necesidad de cast explícito: `Soldado s = new Artillero(...);`. El **downcasting** es lo inverso: convertir una referencia de supertipo a una de subtipo, y requiere cast explícito: `Artillero a = (Artillero) s;`. El riesgo del downcasting es que puede fallar en tiempo de ejecución si la referencia no apunta realmente a un objeto de ese tipo. Para evitar errores, se utiliza el operador **`instanceof`**, que verifica si un objeto es instancia de una clase determinada antes de intentar el downcasting.

```java
public class TestDowncasting {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Juan", 10);
        ejercito[1] = new Zapador("María", 5);
        ejercito[2] = new Artillero("Pedro", 8);
        
        for (Soldado s : ejercito) {
            // Mostrar info común a todos
            s.saludar();
            
            // Downcasting seguro con instanceof
            if (s instanceof Artillero) {
                Artillero a = (Artillero) s;  // Downcasting
                System.out.println("Cohetes: " + a.getCohetes());
            } else if (s instanceof Zapador) {
                Zapador z = (Zapador) s;
                System.out.println("Minas: " + z.getMinas());
            }
        }
    }
}
```

En resumen, el upcasting es seguro y automático (siempre funciona), mientras que el downcasting requiere verificación explícita mediante `instanceof` para garantizar que la operación es válida en tiempo de ejecución.


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El acceso **protegido** (protected) es un nivel de visibilidad intermedio entre `private` (visibilidad solo en la clase) y `public` (visibilidad universal). Un atributo o método protegido es accesible dentro de la clase que lo define, en sus subclases, y en otras clases del mismo paquete, pero no desde código externo al paquete. En Java, el modificador `protected` implementa este nivel de acceso, permitiendo que las subclases accedan directamente a atributos y métodos de la superclase que, de otro modo, permanecerían privados.

El acceso protegido es especialmente útil en herencia, ya que permite a las subclases manipular directamente ciertos datos o métodos de la superclase sin exponerlos al mundo exterior. Esto representa un equilibrio entre la encapsulación (no exponer datos privados) y la flexibilidad (permitir que las subclases personalicem el comportamiento). Sin embargo, debe utilizarse con cuidado, ya que un uso excesivo de atributos protegidos puede comprometer la encapsulación y dificultar futuros cambios.

```java
class Soldado {
    protected String nombre;  // Ahora es protegido, no privado
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soy " + nombre);
    }
}

class Zapador extends Soldado {
    private int minas;
    
    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }
    
    public void ponerMinas() {
        // Acceso directo a nombre porque es protegido
        System.out.println(nombre + " está poniendo minas");
    }
    
    public int getMinas() {
        return minas;
    }
}
```

En este ejemplo, el atributo `nombre` es `protected` en `Soldado`, por lo que la clase `Zapador` puede acceder directamente a él en el método `ponerMinas()`. Si hubiera permanecido como `private`, el acceso habría sido imposible y habría sido necesario utilizar un método público de `Soldado`.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

No todos los lenguajes orientados a objetos tienen una clase base universal. Algunos lenguajes como C++ permiten crear jerarquías de clases sin una raíz común obligatoria (aunque es posible definir una explícitamente), ofreciendo mayor flexibilidad pero a costa de menor uniformidad. En contraste, Java y otros lenguajes modernos tienen una clase base única para todos los objetos: **`Object`**.

En Java, toda clase que no hereda explícitamente de otra clase automáticamente hereda de `Object`. Incluso `Soldado`, `Artillero` y `Zapador`, aunque no lo declaren explícitamente, heredan de `Object`. Esta jerarquía garantiza que todos los objetos comparten un conjunto mínimo de métodos y comportamientos comunes, como `equals()`, `hashCode()`, `toString()`, `getClass()`, entre otros. Esta uniformidad es muy potente: permite escribir código genérico que opera sobre cualquier objeto sin necesidad de conocer su tipo específico.

```java
// Estas dos declaraciones son equivalentes:
class Soldado extends Object {  // Explícito (aunque redundante)
    // ...
}

class Soldado {  // Implícitamente extiende Object
    // ...
}

// Todas las instancias de Soldado (y sus subclases) son también Object
Object obj = new Soldado("Juan");
System.out.println(obj.toString());  // Método heredado de Object
```

Esta característica hace que Java sea un lenguaje muy uniforme: la compatibilidad de tipos que existe entre `Soldado` y `Artillero` también existe entre cualquier clase y `Object`, permitiendo construir sistemas donde `Object` es el denominador común.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple es la capacidad de una clase de heredar de más de una superclase simultáneamente. Por ejemplo, en C++ es posible que una clase `Anfibio` herede tanto de `Animal` como de `Vehículo`. Lenguajes como C++ soportan herencia múltiple, aunque su uso está fuertemente desaconsejado debido a su complejidad y a los problemas que puede generar, especialmente cuando dos superclases definen métodos o atributos con el mismo nombre (conocido como "problema del diamante").

Java **no permite herencia múltiple de clases**. Una clase solo puede heredar de una única superclase directa, aunque sí puede formar parte de una jerarquía más compleja: una clase puede heredar de otra que a su vez hereda de una tercera. Sin embargo, Java sí permite implementar múltiples **interfaces** (un mecanismo distinto a la herencia que se verá en tema posterior), lo que proporciona parte de la flexibilidad de la herencia múltiple sin sus complejidades.

```java
// CORRECTO en Java: herencia simple
class Soldado { }
class Artillero extends Soldado { }

// INCORRECTO en Java: herencia múltiple (no compila)
// class Anfibio extends Animal, Vehículo { }

// CORRECTO en Java: herencia de interfaces (tema posterior)
// interface Movible { }
// interface Transportable { }
// class Vehículo implements Movible, Transportable { }
```

Esta limitación es deliberada en el diseño de Java para mantener la simplicidad y evitar los problemas asociados a la herencia múltiple. La filosofía del lenguaje prefiere diseños más claros y mantenibles, aunque esto requiera ocasionalmente más código o composición.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

Las excepciones en Java son objetos que heredan de clases del framework de excepciones. Una excepción **no controlada** (unchecked exception) es aquella que hereda de `RuntimeException`, lo que significa que no es obligatorio capturarla o declararla en la firma del método. Las excepciones personalizadas se crean definiendo una nueva clase que extiende `RuntimeException` (para excepciones no controladas) o `Exception` (para excepciones controladas), permitiendo almacenar información específica del problema.

Una excepción personalizada puede llevar asociados atributos y métodos que proporcionen contexto adicional sobre el error. En este caso, se incluye un atributo `usuario` que permite conocer qué usuario causó la excepción. Además, Java proporciona la posibilidad de encadenar excepciones a través de la "causa" (cause), permitiendo preservar la excepción original que provocó la actual, lo que es crucial para la depuración. Esto se logra mediante la sobrecarga de constructores que acepten un parámetro `Throwable` para la causa.

```java
class Usuario {
    private String id;
    private String nombre;
    
    public Usuario(String id, String nombre) {
        this.id = id;
        this.nombre = nombre;
    }
    
    public String getId() { return id; }
    public String getNombre() { return nombre; }
    
    @Override
    public String toString() {
        return "Usuario{" + "id='" + id + "', nombre='" + nombre + "'}";
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;
    
    // Constructor básico
    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario);
        this.usuario = usuario;
    }
    
    // Constructor con causa
    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario, causa);
        this.usuario = usuario;
    }
    
    public Usuario getUsuario() {
        return usuario;
    }
}

// Uso:
public class TestExcepcion {
    public static void main(String[] args) {
        Usuario u = new Usuario("123", "Juan");
        
        try {
            throw new UsuarioNoEncontradoException(u, new NullPointerException("Referencia nula en BD"));
        } catch (UsuarioNoEncontradoException e) {
            System.out.println("Error: " + e.getMessage());
            System.out.println("Usuario problema: " + e.getUsuario());
            System.out.println("Causa: " + e.getCause());
        }
    }
}
```

La sobrecarga de constructores permite mayor flexibilidad: se puede usar el constructor básico cuando solo interesa registrar el usuario problemático, o el constructor con causa cuando se desea preservar información sobre la excepción subyacente para propósitos de depuración.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

La herencia establece una relación "es-un" muy fuerte entre la superclase y la subclase, lo que implica que la subclase debe ser un tipo válido de la superclase en el sentido semántico del dominio del problema. Cuando se usa herencia únicamente para reutilizar código sin que exista realmente esta relación "es-un", se crea un diseño frágil y confuso. Por ejemplo, heredar de una clase `ListaOrdenada` solo porque se necesitan los métodos de manejo de listas, cuando el nuevo tipo no es realmente una "lista ordenada", genera una jerarquía de tipos que no refleja la realidad del dominio.

La herencia acoplada al cambio: los atributos y métodos de la superclase se heredan automáticamente, por lo que cualquier modificación en la superclase afecta a todas las subclases. Si la herencia se estableció solamente por conveniencia de reutilización de código (no por una relación "es-un" legítima), estos cambios pueden romper comportamientos que no deberían verse afectados. Además, la herencia es un acoplamiento muy fuerte: la subclase está profundamente vinculada a los detalles de implementación de la superclase.

En contraste, la composición permite reutilizar código de forma flexible: si la funcionalidad necesaria se obtiene como un componente interno (un atributo que es instancia de otra clase), los cambios en ese componente tienen un impacto limitado, y la relación es más suelta y fácilmente modificable. Por estas razones, se prefiere composición para reutilizar código, reservando herencia para las situaciones donde existe realmente una relación "es-un" legítima en el modelo del dominio.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

La composición proporciona mayor flexibilidad que la herencia en el diseño de sistemas orientados a objetos. Mientras que la herencia establece una relación rígida "es-un" interpretada en tiempo de compilación, la composición permite cambiar el comportamiento en tiempo de ejecución simplemente reemplazando el componente interno. Por ejemplo, si una clase depende de un comportamiento que se encapsula en un atributo (composición), ese atributo puede intercambiarse por uno de un tipo compatible sin alterar el código cliente. Con herencia, la relación es inmutable una vez que la clase se define.

Además, la composición respeta el principio de responsabilidad única de forma más clara: cada clase tiene una única responsabilidad y colabora con otras clases para lograr su objetivo. La herencia, en cambio, mezcla la responsabilidad de la clase extendida con la de la clase base, creando una responsabilidad compuesta que puede ser confusa. Otro beneficio es la evitación del acoplamiento a los detalles de implementación de la clase base, que es inevitable en herencia pero minimizado en composición.

Desde el punto de vista de mantenibilidad, la composición es superior porque permite cambios en las clases indvidividuales sin efectos en cascada sobre otras. Un cambio en una clase que participa en una relación de composición solo afecta a las clases que la usan conscientemente, no a toda una jerarquía de subclases. Por todas estas razones, la recomendación en la ingeniería de software moderna es favorecer composición como estrategia de diseño predeterminada, usando herencia únicamente cuando existe una verdadera relación "es-un" que justifique sus costos en términos de flexibilidad y acoplamiento.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

La encapsulación es el principio de ocultar los detalles internos de una clase (sus atributos privados y métodos internos) exponiendo únicamente una interfaz pública controlada. La herencia compromete este principio porque las subclases necesitan acceso a los atributos y métodos de la superclase para extender su funcionalidad. Si todos los atributos y métodos fueran privados, las subclases no podrían reutilizarlos; por tanto, deben ser `protected` o `public`, exponiendo internamente la estructura de la superclase.

Esta exposición interna es problemática por varias razones. Primero, los detalles de implementación de la superclase quedan visibles a las subclases, lo que las acopla a esos detalles. Si la implementación cambia internamente en la superclase, las subclases podrían verse afectadas de formas inesperadas. Segundo, la superclase pierde control sobre cómo se utilizan sus atributos y métodos internos, ya que las subclases pueden acceder y modificar directamente atributos `protected` sin pasar por getters/setters que podrían validar cambios. Tercero, en una jerarquía profunda, toda la cadena de superclases expone sus internals, complicando la evolución del código.

En contraste, la composición mantiene la encapsulación intacta: un objeto simplemente utiliza los servicios públicos de otro objeto mediante su interfaz pública, sin acceso a detalles internos. Por esto, la composición se considera más compatible con los principios fundamentales de la orientación a objetos. La solución a menudo es usar herencia solo donde la relación "es-un" es indiscutiblemente válida, y en otros casos recurrir a composición, que permite reutilizar funcionalidad sin comprometer la encapsulación.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

A continuación se presentan dos enfoques complementarios para modelar `Estudiante` y `Trabajador` que comparten atributos comunes. El primer enfoque usa **herencia** con una superclase `Persona`, estableciendo una relación "es-un": un estudiante es una persona, un trabajador es una persona. El segundo enfoque usa **composición**, donde `Estudiante` y `Trabajador` tienen internamente un objeto `DatosPersonales` que encapsula los datos comunes.

La ventaja del enfoque de **herencia** es que la relación "es-un" es semánticamente correcta: tanto estudiantes como trabajadores son personas. La jerarquía es clara y la compatibilidad de tipos permite tratar todos como `Persona`. Sin embargo, este enfoque expone los internals de `Persona` en sus subclases. El enfoque de **composición**, en cambio, mantiene una separación clara: `DatosPersonales` es un componente completamente encapsulado que tanto `Estudiante` como `Trabajador` utilizan. Los datos personales are managed solely through the public interface of `DatosPersonales`, no habrá problemas cuando `DatosPersonales` evolucione internamente.

```java
// ========== ENFOQUE 1: HERENCIA ==========
class Persona {
    protected String dni;
    protected String nombre;
    
    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    
    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

class EstudianteC extends Persona {
    private double notaMedia;
    
    public EstudianteC(String dni, String nombre, double notaMedia) {
        super(dni, nombre);
        this.notaMedia = notaMedia;
    }
    
    public double getNotaMedia() { return notaMedia; }
}

class TrabajadorC extends Persona {
    private double salario;
    
    public TrabajadorC(String dni, String nombre, double salario) {
        super(dni, nombre);
        this.salario = salario;
    }
    
    public double getSalario() { return salario; }
}

// ========== ENFOQUE 2: COMPOSICIÓN ==========
class DatosPersonales {
    private String dni;
    private String nombre;
    
    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    
    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

class EstudianteG {  // G de "composition" (Composición)
    private DatosPersonales datos;
    private double notaMedia;
    
    public EstudianteG(DatosPersonales datos, double notaMedia) {
        this.datos = datos;
        this.notaMedia = notaMedia;
    }
    
    public DatosPersonales getDatos() { return datos; }
    public double getNotaMedia() { return notaMedia; }
}

class TrabajadorG {
    private DatosPersonales datos;
    private double salario;
    
    public TrabajadorG(DatosPersonales datos, double salario) {
        this.datos = datos;
        this.salario = salario;
    }
    
    public DatosPersonales getDatos() { return datos; }
    public double getSalario() { return salario; }
}

// ========== USO ==========
public class TestApproaches {
    public static void main(String[] args) {
        // Herencia
        Persona es1 = new EstudianteC("12345", "Juan", 8.5);
        Persona tr1 = new TrabajadorC("67890", "María", 2000);
        
        // Composición
        DatosPersonales dp1 = new DatosPersonales("11111", "Pedro");
        EstudianteG es2 = new EstudianteG(dp1, 7.8);
        
        DatosPersonales dp2 = new DatosPersonales("22222", "Ana");
        TrabajadorG tr2 = new TrabajadorG(dp2, 2500);
    }
}
```

El enfoque de composición ofrece mayor flexibilidad: si en el futuro se necesita que un estudiante y un trabajador compartan también datos académicos o laborales, se pueden agregar nuevos componentes sin modificar `Estudiante` ni `Trabajador`. En herencia, esto requeriría reorganizar la jerarquía de clases. Por tanto, aunque ambos enfoques funcionan, la composición es más escalable para requisitos cambiantes.
