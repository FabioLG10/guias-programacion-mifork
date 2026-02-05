# Tema 1 – Clases y Objetos
## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos?
Las cuatro características básicas de la programación orientada a objetos son:
- **Abstracción**: permite representar entidades del mundo real mediante clases, ocultando los detalles innecesarios.
- **Encapsulación**: consiste en agrupar datos y métodos dentro de una clase y controlar el acceso a ellos.
- **Herencia**: permite crear nuevas clases a partir de otras existentes, reutilizando código.
- **Polimorfismo**: permite que un mismo método tenga comportamientos distintos según el objeto que lo utilice.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos
Algunos lenguajes populares orientados a objetos son:
- Java
- C++
- Python
- C

## 3. ¿Qué es la programación estructurada y qué es la programación modular?
La **programación estructurada** es un paradigma basado en el uso de estructuras de control como secuencias, condicionales y bucles, evitando el uso de saltos incontrolados como `goto`.  
La **programación modular** consiste en dividir un programa en módulos independientes, cada uno con una funcionalidad concreta, para facilitar el mantenimiento y la reutilización del código.

## 4. ¿Qué tres elementos definen a un objeto en POO?
Un objeto se define por:
- **Identidad**
- **Estado** (valores de sus atributos)
- **Comportamiento** (métodos que puede ejecutar)

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes POO usan clases?
Una **clase** es una plantilla que define atributos y métodos.  
Un **objeto** es una instancia concreta de una clase.  
Una **instancia** es el proceso de crear un objeto a partir de una clase.  
No todos los lenguajes orientados a objetos usan clases, por ejemplo JavaScript usa un modelo basado en prototipos.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la recolección de basura?
Normalmente los objetos se almacenan en el **heap**. No es igual en todos los lenguajes, ya que depende de su implementación.  
La **recolección de basura** es un mecanismo automático que libera la memoria ocupada por objetos que ya no se utilizan.

## 7. ¿Qué es un método? ¿Qué es la sobrecarga de métodos?
Un **método** es una función definida dentro de una clase que describe el comportamiento de un objeto.  
La **sobrecarga de métodos** consiste en definir varios métodos con el mismo nombre pero distintos parámetros.

## 8. Ejemplo mínimo de clase Punto en Java

public class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        System.out.println(p.calculaDistanciaAOrigen());
    }
}

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

public static void main(String[] args)

'static':significa que el miembro (variable o método) pertenece a la clase, no a un objeto concreto y sirve para llamar sin crear un objeto.

No solo se usa para main si no que static se usa también para variables de clase que se comparten entre todos los objetos y métodos utilitarios que no dependen de un objeto (por ejemplo Math.max()).

Cuando se combina static con final se crea una constante de clase, que no puede modificarse.
Ejemplo:
class Constantes {
    static final double PI = 3.141592;
}


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Compilar: javac MiPrograma.java
Ejecutar: java MiPrograma
Java es compilado? Sí, pero no a código máquina nativo, sino a bytecode, que es independiente de la plataforma.
Máquina Virtual (JVM): Interpreta y ejecuta el bytecode en cualquier sistema operativo. Permite portabilidad “write once, run anywhere”.
Bytecode: lenguaje intermedio entre Java y máquina.
.class: fichero que almacena el bytecode compilado.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos
new: Crea un objeto en memoria y devuelve su referencia.
Constructor: Método especial para inicializar un objeto, tiene el mismo nombre que la clase y no tiene tipo de retorno.

class Empleado {
    String DNI, nombre, apellidos;

    public Empleado(String DNI, String nombre, String apellidos) {
        this.DNI = DNI;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`
this es una referencia al objeto que invoca un método. Permite diferenciar atributos de parámetros con el mismo nombre.
En otros lenguajes se llama distinto (Python: self, C++: this).
class Punto {
    int x, y;

    public void mover(int x, int y) {
        this.x = x; // atributo x
        this.y = y; // atributo y
    }
}




## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

public double distanciaA(Punto otro) {
    int dx = this.x - otro.x;
    int dy = this.y - otro.y;
    return Math.sqrt(dx*dx + dy*dy);
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

Objetos: se pasan por copia de referencia, por lo que los cambios en los atributos afectan al objeto original.
Primitivos (int, double…): se pasan por valor, por lo que los cambios dentro del método no afectan al original.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java
Devuelve una representación en texto de un objeto.
Otros Ejemplos:Python __str__(), C++ sobrecarga de operator<<
@Override
public String toString() {
    return "(" + x + ", " + y + ")";
}



## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Una clase tiene atributos, métodos, constructores, encapsulamiento y herencia.
Un struct solo contiene datos; no puede tener métodos ni control de acceso en C puro.
Variables de clase son instancias, mientras que variables de struct son solo bloques de memoria.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

#include <stdio.h>
#include <math.h>

typedef struct {
    int x, y;
} Punto;

double distancia_al_origen(Punto* p) {
    return sqrt(p->x*p->x + p->y*p->y);
}

int main() {
    Punto p1 = {3, 4};
    printf("%f\n", distancia_al_origen(&p1));
    return 0;
}

