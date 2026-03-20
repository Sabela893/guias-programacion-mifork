<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

- **ABSTRACCIÓN**: modela solo los aspectos esenciales de un objeto, ocultando detalles irrelevantes. Es decir, se centra en QUÉ HACE un objeto, no en cómo lo hace

- **ENCAPSULACIÓN**: protege datos internos de un objeto permitiendo acceder a ellos mediante métodos controlados (GETTERS Y SETTERS)

- **HERENCIA**: permite que una clase derive de otra, facilitando la creación de jerarquías lógicas

- **POLIMORFISMO**: permite que métodos con el mismo nombre se comporten de forma distinta según el objeto que los invoque

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

C++, Java, Python y C#


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

**PROGRAMCIÓN ESTRUCTURADA**:
Paradigma que propone organizar el código en bloques lógicos y controlados, evita saltos arbitrarios (goto). Inspiró a la claridad interna de los métodos en la programación a objetos (POO)
Se basa en 3 estructuras de control:
 - `Secuencia`: instrucciones que se ejecutan una tras otra
 - `Selección`: decisiones (if, switch)
 - `Iteración`: bucles (for, while)

**PROGRAMACIÓN MODULAR**:
Paradigma que propone dividir un programa en partes independientes llamadas `MÓDULOS`. Esto anticipó la idea de clases y objetos como unidades autónomas.
Principios clave:
 - `Descomposición`: divide problemas grandes en otros más pequeños
 - `Independencia`: cada módulo se desarrolla y prueba casi por separado
 - `Interfaces claras`: los módulos se comuncian mediante funciones o contratos bien definidos
 - `Reutilización`: un módulo puede utilizarse en otros programas

La POO añadió, además de esto, la encapsulación, herencia y polimorfismo

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

- ESTADO (atributos o propiedades): información almacenada por el objeto. Sus atributos describen las características en un momento concreto

- COMPORTAMIENTO (métodos): define lo que el objeto puede hacer o cómo responde a acciones.

- IDENTIDAD (referencia única):  distingue a un objeto de cualquier otro, incluso si tienen el mismo estado. 

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase no es lo mismo que un objeto. La `clase` (receta) sería la definición, mientras que el `objeto` (plato resultante) es la realidad creada a partir de la definición

 - **CLASE**: es un molde o plantilla que describe cómo serán los objetos: qué datos tendrán (atributos) y qué podrán hacer (métodos). No ocupa memoria por sí misma; es una definición.

 - **INSTANCIA**: un objeto creado a partir de una clase. Decir “instancia” o “objeto” suele ser equivalente, aunque “instancia” `enfatiza el acto de creación`.

No todos los lenguajes orientados a objetos utilizan clases, pero sí la mayoría:
    --> Basados en `clases`: Java, C++, C#, Python, Ruby
    --> Basados en `prototipos`: JavaScript

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En la mayoría de lenguajes orientados a objetos modernos, los objetos se guardan en el **HEAP**, una zona de memoria destinada a datos dinámicos que viven más allá del ámbito inmediato de una función.

 - **Stack (pila)**: almacena variables locales, direcciones de retorno y, en muchos lenguajes, referencias a objetos, pero no los objetos en sí.
 --> `Almacena referencias o valores temporales`

 - **Heap (montículo)**: almacena los objetos reales, creados dinámicamente con new
 --> `Crea objetos que sobreviven más allá de la función`

La memoria no se gestiona igual en todos los lenguajes:

 - `Lenguajes con objetos siempre en el HEAP`: Java, Python, C#, Ruby. Los objetos se crean siempre en el heap y se accede a ellos mediante referencias.

 - `Lenguajes con objetos en STACK o HEAP`: C++.
 En el **STACK** (Coche miCoche;), se destruye al salir del ámbito --> `VIDA AUTOMÁTICA`.
 En el **HEAP** (Coche* p = new Coche();), requiere delete o smart pointers --> `VIDA DINÁMICA`

 - `Lenguajes basados en prototipos`: JavaScript.
 Los objetos también viven en el **HEAP**, pero su modelo no se basa en clases sino en PROTOTIPOS.

La **RECOLECCIÓN DE BASURA** es un mecanismo automático que libera la memoria del heap ocupada por objetos que ya no están siendo utilizados por el programa. (Java, Python, C#, JavaScript, Ruby)

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

**MÉTODO**: una función asociada a una clase. Representa el comportamiento del objeto y suele operar sobre su estado interno (sus atributos).

**SOBRECARGA DE MÉTODOS**: define varias versiones de un mismo método dentro de una clase, siempre que:
    - Tengan diferentes parámetros (cantidad, tipo o ambos).
    - Compartan el mismo nombre.
    - El lenguaje lo permita (Java, C++, C#, entre otros).
    
Esto permite que un mismo método se adapte a distintas necesidades sin cambiar su nombre.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

    public class Punto {
        int x;   // visibilidad por defecto
        int y;   // visibilidad por defecto

        // Constructor opcional para inicializar el punto
        public Punto(int x, int y) {
            this.x = x;
            this.y = y;
        }

        // Método que calcula la distancia al origen (0,0)
        public double calculaDistanciaAOrigen() {
            return Math.sqrt(x * x + y * y);
        }
    }

Ejemplo de uso:

    public class Main {
        public static void main(String[] args) {
            Punto p = new Punto(3, 4);   // instancia de Punto
            double distancia = p.calculaDistanciaAOrigen();
            System.out.println("Distancia al origen: " + distancia);
        }
    }



## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada de un programa Java es el método main estático
public static void main(String[] args)

 - **FINAL**: hace una clase, atributo o método inmutable

 - **STATIC**: indica que algo pertenece a la clase, no a los objetos. Por lo tanto se puede utilizar sin generar instancias, se aplica a métodos, atributos, bloques estáticos y clases internas estéticas
    Static es un método muy común fuera de main. Suele combinarse con `final` porque así se garantiza que la constante sea única, inmutable y accesible sin instanciar la clase.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Suponiendo un archivo llamado `Hola.java` con clase pública `Hola` y un método `main` para compilar con `javac` utilizaríamos: 
    javac Hola.java

Lo que genera un archivo `Hola.class`. Ejecutaríamos con `java`:
    java Hola

El comando busca la clase `Hola` y ejecuta su método main.

Java es compilado e interpretado, es decir, HÍBRIDO:
    - El código fuente `.java` se compila a bytecode `.class`
    - Ese bytecode lo ejecuta la Máquina Virtual de Java (JVM), que lo interpreta u optimiza mediante JIT compilation (compilación en tiempo de ejecución)

- **bytecode**: es un código intermedio, no es código máquina real, pero tampoco es código fuente.
- **.class**: archivos generados por `javac`


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

 - `new` es un operador que crea un objeto en memoria y debuelve una referencia a él. Entonces cuando se escribe:
    Punto p = new Punto(3, 4);

 Lo que ocurre es que se reserva memoria en el HEAP para el nuevo objeto de tipo `Punto`, se llama al constructor y la referencia resultante se guarda en la variable p

Un **constructor** es un método especial cuya misión es inicializar un objeto recién creado.
    - Tiene el mismo nombre que la clase
    - No devuelve nada, tampoco es `void`
    - Puede haber varios

Ejemplo clase `Empleado`:

    public class Empleado {
        String dni;
        String nombre;
        String apellidos;

        // Constructor
        public Empleado(String dni, String nombre, String apellidos) {
            this.dni = dni;
            this.nombre = nombre;
            this.apellidos = apellidos;
        }
    }


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

 - `this` es una referencia a sí mismo. Permite distinguir entre atributos y parámetros con el mismo nombre, acceder a métodos o constructores de la propia clase y dejar claro que algo pertenece al objeto actual.
    Dentro de un método, `this.x` significa “el atributo `x` de este objeto”.

 No se llama igual en todos los lenguajes, aunque la idea es universal:
    - `this` --> Java, JavaScript, C#, Kotlin, C++ (puntero)
    - `self`--> Ruby, Swift, Python (parámetro explícito)

Ejemplo de `this` en la clase `Punto`:

    public class Punto {
        int x;
        int y;

        public Punto(int x, int y) {
            this.x = x;   // "this.x" es el atributo; "x" es el parámetro
            this.y = y;
        }

        public double calculaDistanciaAOrigen() {
            return Math.sqrt(this.x * this.x + this.y * this.y);
        }
    }

Ejemplo de uso:
    public class Main {
        public static void main(String[] args) {
            Punto p = new Punto(3, 4);
            System.out.println(p.calculaDistanciaAOrigen());
        }
    }


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

    public class Punto {
        int x;
        int y;

        public Punto(int x, int y) {
            this.x = x;
            this.y = y;
        }

        public double calculaDistanciaAOrigen() {
            return Math.sqrt(this.x * this.x + this.y * this.y);
        }

        // Nuevo método
        public double distanciaA(Punto otro) {
            int dx = otro.x - this.x;
            int dy = otro.y - this.y;
            return Math.sqrt(dx * dx + dy * dy);
        }
    }

Ejemplo de uso del método:
    public class Main {
        public static void main(String[] args) {
            Punto p1 = new Punto(3, 4);
            Punto p2 = new Punto(7, 1);

            double d = p1.distanciaA(p2);
            System.out.println("Distancia entre p1 y p2: " + d);
        }
    }


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

El paso de un punto `p` de tipo `Punto` es por referencia, es decir, los cambios que se produzcan en el método se ven reflejados fuera de él. Esto ocurre porque el método trabaja sobre el mismo objeto en memoria.

En cambio, un entero se pasa por valor, se copia el número y por lo tanto no se modifica el valor original.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java?

 - `toString()` en java es la forma estándar de obtener una representación textual de un objeto. Si no se sobreescribe Java muestra algo como:
    Punto@3f99bd52
 
 Existe en otros lenguajes:
    - toString() --> Java, Kotlin, C#, JavaScript
    - __ str __() y __ rpr __(): python
    - operator<< --> C++
    - to_s --> Ruby

Ejemplo `toString()` en la clase `Punto`:
    public class Punto {
        int x;
        int y;

        public Punto(int x, int y) {
            this.x = x;
            this.y = y;
        }

        public double calculaDistanciaAOrigen() {
            return Math.sqrt(this.x * this.x + this.y * this.y);
        }

        public double distanciaA(Punto otro) {
            int dx = otro.x - this.x;
            int dy = otro.y - this.y;
            return Math.sqrt(dx * dx + dy * dy);
        }

        @Override
        public String toString() {
            return "(" + this.x + ", " + this.y + ")";
        }
    }


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Una clase puede verse como un struct en C. Le faltaría:
    - Métodos asociados: en C no se pueden definir funciones dentro del struct
    - Encapsulación y visibilidad: no se puede escribir `private`, `public` o `protected`
    - Constructores y destrucción controlada
    - En C el comportamiento del struct está fuera, en funciones independientes, mientras que en una clase hay métodos que operan sobre sus atributos
    -Herencia y polimorfismo: un struct en C no puede heredar de otro y no hay jerarquías


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

    #include <math.h>

    typedef struct {
        int x;
        int y;
    } Punto;

    // “Método” que calcula la distancia al origen
    double calculaDistanciaAOrigen(Punto *p) {
        return sqrt(p->x * p->x + p->y * p->y);
    }

Para utilizarlo:
    #include <stdio.h>

    int main() {
        Punto p = {3, 4};
        double d = calculaDistanciaAOrigen(&p);
        printf("Distancia al origen: %f\n", d);
        return 0;
    }

En C `this` no existe, por lo que hay que pasar el objeto como parámetro (Punto *p)