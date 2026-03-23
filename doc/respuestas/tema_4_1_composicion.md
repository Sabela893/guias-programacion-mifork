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

```jsx
    #include <stdio.h>
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
        double dx = b.x - a.x;
        double dy = b.y - a.y;
        return sqrt(dx*dx + dy*dy);
    }

    double longitud(Linea l) {
        return distancia(l.p1, l.p2);
    }
```


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

*Clase punto*:

```jsx
    public final class Punto {
        private final double x;
        private final double y;

        public Punto(double x, double y) {
            this.x = x;
            this.y = y;
        }

        public double getX() {
            return x;
        }

        public double getY() {
            return y;
        }

        // Distancia a otro punto
        public double distanciaA(Punto otro) {
            double dx = otro.x - this.x;
            double dy = otro.y - this.y;
            return Math.sqrt(dx * dx + dy * dy);
        }
    }
```

*Clase Linea*: compuesta por 2 puntos

```jsx
    public final class Linea {

        private final Punto p1;
        private final Punto p2;

        public Linea(Punto p1, Punto p2) {
            this.p1 = p1;
            this.p2 = p2;
        }

        public Punto getP1() {
            return p1;
        }

        public Punto getP2() {
            return p2;
        }

        // Longitud de la línea
        public double longitud() {
            return p1.distanciaA(p2);
        }
    }
```

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La **multiplicidad** describe cuántas instancias de una clase pueden estar relacionadas con una instancia de otra clase dentro de una relación.
En *composición*, la multiplicidad indica cuántos objetos “contenidos” forman parte del objeto “contenedor”, y también cuántos contenedores pueden incluir a un mismo contenido.

Una `Linea` está compuesta por dos puntos y cada punto pertenece a una única línea.
 - **Multiplicidad `Linea` --> `Punto`**: `Linea 1 ---- 2 Punto`
 - **Multiplicidad `Punto` --> `Linea`**: `Punto 1 ----Linea 1`


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

**COMPOSICIÓN FUERTE**: es la relación más estrecha posible entre 2 clases. Sin el *objeto "contenedor"* los objetos contenidos no existen, por lo que si el contenedor muere sus partes también.

Los objetos contenidos dependen totalmente del contenedor. Se crean juntos, se destruyen juntos y no se comparten entre varios contenedores. A esto se le llama **composición (rombo negro)**

    -Ejemplo: Una linea tiene dos puntos, y estos solo existen dentro de esa linea
        Linea -> Punto: 2
        Punto -> Linea: 1

**COMPOSICIÓN DÉBIL**: es una relación más flexible entre las clases. El objeto "contenedor" usa o agrupa objetos, pero *no los posee completamente*. Es decir, los objetos contenidos pueden existir por sí mismos y pueden ser compartidos entre varios contenedores.

La vida del objeto contenido es independiente a la del contenedor. Si este desaparece, los objetos contenidos siguen existiendo. A esto se le llama **agregación (rombo blanco) o asociación**.

    -Ejemplo: Una clase tiene varios alumnos, pero los alumnos no dejan de existir si la clase se elimina. Los alumnos pueden estar en varias clases.
    

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
