<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

OPCIÓN 1:

```jsx
    #include <math.h>

    int raiz(double x, double *resultado) {
        if (x < 0) {
            return 1;   // código de error
        }
        *resultado = sqrt(x);   // el resultado se devuelve mediante un puntero
        return 0;       // OK
    }

```

OPCIÓN 2:

```jsx
#include <math.h>
#include <errno.h>

double raiz(double x) {
    if (x < 0) {
        errno = EDOM;   // dominio matemático incorrecto
        return 0.0;     // valor especial
    }
    errno = 0;          // sin error
    return sqrt(x);
}

```

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un mecanismo que permite interrumpir el flujo normal de un programa cuando ocurre un error y trasladar ese problema a un punto donde pueda manejarse de forma controlada.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

```jsx
    public class Calculadora {
        public double raiz (double x) {
            if (x < 0) {
                throw new IllegalArgumentException("x debe ser positiva");
            } else {
                return Math.sqrt(x);
            }
        }
    }

```


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

**Lanzar una excepción** es *crear y emitir un error* de forma explícita cuando una función detecta una situación que no puede o no debe resolver.

```jsx
    public static double raizCuadrada(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo");
        }
        return Math.sqrt(x);
}
```
 - Aquí la función lanza la excepción `throw` porque no puede devolver un resultado válido.

**Capturar o controlar** una excepción es interceptarla para evitar que el programa termine abruptamente y decidir qué hacer con el error.

```jsx
    try {
        double r = raizCuadrada(-9);
        System.out.println(r);
    } catch (IllegalArgumentException e) {
        System.out.println("Error: " + e.getMessage());
    }
```
 - El bloque `catch` controla la excepción y permite reaccionar.

Si una función lanza una excepción y nadie la captura, la excepción “sube” automáticamente a la función que la llamó. Si esa tampoco la captura, sube a la siguiente, y así sucesivamente. Este proceso se llama **PROPAGACIÓN**.
Las funciones por las que pasa la excepción no se reanudan. Se abortan inmediatamente y no vuelven a ejecutarse desde ese punto.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

En **C** si una función falla ella misma debe decidir cómo comunicar el error:
 - Devolviendo un código especial
 - Modificando `errno`
 - Usando un parámetro de salida

```jsx
    int f3() {
        int r = f2();
        if (r != 0) return r;
        ...
    }

    int f2() {
        int r = f1();
        if (r != 0) return r;
        ...
    }

    int f1() {
        if (algo_mal) return -1;
        ...
    }

```
Cada función que llame a esa función debe volver a comprobar y propagar el error manualmente. Esto provoca que cada nivel de la pila tenga que detectar el error, comprobarlo y propagarlo, lo que es tedioso y propenso a errores.

En **lenguajes con excepciones** la propagación es automática. Cuando una función lanza una excepción sube sola por la pila hasta que alguien la capture.

```jsx
    void f3() {
    f2();   // si f2 lanza, f3 no tiene que hacer nada
    }

    void f2() {
        f1();   // si f1 lanza, f2 tampoco hace nada
    }

    void f1() {
        throw new IllegalArgumentException();
    }

```

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

Las excepciones suelen ser instancias de clase normalmente derivada de una jerarquía común:
 - Java: `Exception`, `RuntimeException`, `IOException`
 - C#: `System.Exception`
 - Python: `Exception`, `ValueError`, `TypeError`
 - C++: cualquier objeto lanzado con `throw`

*VENTAJAS*:
 - **Transportar info sobre el error**: puede contener un mensaje descriptivo, el tipo de error, la causa, la traza de la pila (stack trace)
 - **Ocultar detalles internos**: El código que lanza la excepción no necesita revelar cómo detectó el error. Solo lanza un objeto que describe el problema.
 - **Permite diferentes tipos de errores**: cada clase de excepción representa un tipo diferente de fallo
 - **Evita mezclar lógica normal y lógica de error**: el flujo normal del programa queda limpio y la gestión del error se encapsula en bloques `try/catch`.

Se pueden crear excepciones personalizadas, lo que permite representar errores específicos del programa.
    - Ejemplo:

    ```jsx
        public class NumeroNegativoException extends Exception {
            public NumeroNegativoException(double valor) {
                super("El número " + valor + " es negativo");
            }
        }

        // Uso:
        public double raiz(double x) throws NumeroNegativoException {
            if (x < 0) {
                throw new NumeroNegativoException(x);
            }
            return Math.sqrt(x);
        }

    ```

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

TODAS las excepciones en la mayoría de los lenguajes OO incluyen:

 - *Mensaje descriptivo del error*
 - *Tipo de excepción* (`NumberFormatException`, `IOException`, `NullPointerException`, `MiExcepcionPersonalizada`)
 - *Traza de la pila*: La excepción guarda el camino exacto de llamadas que llevó al error (qué función falló, quién la llamó y en qué línea)


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Se puede tener más de un bloque `catch`, pero **solo se ejecuta 1**. Por eso es importante *empezar con los casos específicos y seguir con los generales*.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

El mecanismo que garantiza que cierto código siempre se ejecute, incluso cuando una excepción rompe el flujo normal, es el bloque `finally`. Su función es asegurar la liberación de recursos antes de que la excepción siga su camino o incluso cuando no hay excepción.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Un bloque `finally` se ejecuta siempre, puede ir con o sin catch:
- Haya o no excepción
- Se capture o no la excepción
- Si la excepción se propaga hacia arriba
- Varios `return` dentro del `try` o `catch`


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

**CONTROLADAS**: son excepciones que el *compilador obliga a manejar*. Representan errores previsibles y externos al programa, que el código debe anticipar: fallos de E/S, problemas de red, ausencia de un fichero, etc.
 - Deben aparecer en un `try/catch` o declararse con `throws` en la firma del método
 - Son subclases de `Exception`, excepto las que derivan de `RuntimeException`.

 - EJEMPLOS:
    `IOException` — error al leer/escribir un fichero.
    `SQLException` — error al acceder a una base de datos.
    `FileNotFoundException` — el fichero no existe.`ClassNotFoundException` — no se encuentra una clase al cargarla dinámicamente.

    ```jsx
    public static String leerFichero(String ruta) throws IOException {
        return Files.readString(Path.of(ruta));
    }
    ```
    El compilador obliga a manejar `IOException` porque el error es externo y esperable.

**NO CONTROLADAS**: Son excepciones que *no es obligatorio capturar*. Representan errores de programación, normalmente internos y lógicos: índices fuera de rango, nulos, operaciones ilegales…

 - Derivan de `RuntimeException`.
 - Si ocurren, suelen indicar un bug o un mal uso de la API.
 - Pueden capturarse, pero no es obligatorio.
 
 EJEMPLOS:
    `NullPointerException` — se usa un objeto que es null.`IllegalArgumentException` — un argumento no es válido (como raíz de un número negativo).
    `IndexOutOfBoundsException` — índice fuera de rango.`ArithmeticException` — división entre cero.

    ```jsx
    public static double raizCuadrada(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo");
        }
        return Math.sqrt(x);
    }
    ```
    Aquí no se obliga a capturar nada: si el programador pasa un valor inválido, es un error suyo.

 - `RuntimeException`: es la clase base de todas las excepciones no controladas. Su función es agrupar los errores que:
    - indican fallos de programación
    - no deben forzar al programador a escribir try/catch en cada llamada
    - pueden aparecer en cualquier punto si se viola un contrato (por ejemplo, pasar un argumento inválido).


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

Es una declaración en la firma de un método que indica que ese método puede producir una excepción controlada y que no la gestiona él mismo, sino que delega su manejo en quien lo llame.
Es una alternativa directa a capturar la excepción con `try`/`catch` porque permite que el error se trate en un nivel superior, donde quizá tenga más sentido decidir qué hacer. (`throw` ≠ `throws`)

```jsx
    public String leerFichero(String ruta) throws IOException {
        return Files.readString(Path.of(ruta));
    }
```
Aquí el método no captura la excepción: simplemente declara que puede ocurrir.

Se debe utilizar cuando:

- El método no sabe cómo manejar el error.
- El error debe gestionarse en un nivel superior (por ejemplo, en la interfaz de usuario).
- Se quiere mantener el método simple y centrado en su tarea.
- El método forma parte de una API y quieres que el usuario decida cómo reaccionar.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

```jsx
    public static String leerFichero(String ruta) throws IOException {
        FileReader fr = null;

        try {
            fr = new FileReader(ruta);   // Puede lanzar FileNotFoundException (checked)
            StringBuilder sb = new StringBuilder();
            int c;

            while ((c = fr.read()) != -1) {
                sb.append((char) c);
            }

            return sb.toString();

        } finally {
            // Este bloque SIEMPRE se ejecuta, haya o no excepción
            if (fr != null) {
                try {
                    fr.close();
                } catch (IOException e) {
                    // Aquí normalmente solo se registra el error
                    System.out.println("Error cerrando el fichero: " + e.getMessage());
                }
            }
        }
    }
```
 1. *El método no captura la excepción*
    No hay catch. Si new FileReader(ruta) falla, la excepción sube al llamador.

 2. *El método declara que puede lanzar la excepción*
 `throws IOException` obliga al código que llame a este método a decidir si: la captura o la propaga

 3. *El bloque finally siempre se ejecuta*
 Incluso si el fichero no existe, ocurre un error leyendo, el método sale por return o la excepción se propaga.

Esto garantiza que el recurso se cierre correctamente.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

En Java sí puedes poner excepciones no controladas (subclases de `RuntimeException`) en la cláusula `throws`, pero hacerlo no obliga al método llamador a capturarlas.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

La elección entre una y otra depende de si el error es parte normal del funcionamiento o si indica un fallo de programación.
Esta distinción no existe igual en todos los lenguajes, y donde no existe, suele adoptarse un modelo más cercano al de las no controladas.

**Momentos en los que usar excepciones controladas:** acceso a ficheros o bases de datos, operaciones de red y lectura de datos externos

**Momentos en los que usar excepciones no controladas:** argumentos inválidos, estados ilegales, errores lógicos y violación de precondiciones

No existen ambas opciones en todos los lenguajes. Java es uno de los pocos lenguajes modernos que distingue entre excepciones controladas y no controladas.
En los que sólo existe una opción la más habitual es el modelo equivalente a las *no controladas*


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Tiene sentido. A veces capturas una excepción porque quieres traducirla a otra más adecuada para tu aplicación. Esto se llama exception **translation** o **wrapping**.
Esto se utiliza cuando la *excepción original es demasiado técnica*, cuando se quiere dar un *mensaje claro* o cuando se quiere *encapsular datos internos*.

    - Ejemplo:

    ```jsx
        try {
            int n = Integer.parseInt("abc");
        } catch (NumberFormatException e) {
            throw new IllegalArgumentException("El formato del número es inválido");
        }
    ```

También se puede lanzar la misma excepción capturada. Se suele hacer cuando se quiere *registrar el error* pero no manejarlo, *añadir información* pero mantener la excepción original o cuando *el método actual no puede resolver el problema*, pero un nivel superior si.

    - Ejemplo:

    ```jsx
        try {
            leerArchivo("datos.txt");
        } catch (IOException e) {
            System.err.println("Error leyendo archivo: " + e.getMessage());
            throw e;   // se relanza la misma excepción
        }
    ```


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Cuando una excepción es la **“causa”** de otra, significa que una excepción de nivel bajo (más técnica) ha provocado otra excepción de nivel más alto (más abstracta).
Esta relación se encapsula dentro del propio objeto excepción, de modo que el manejador final puede ver toda la cadena de errores, no solo el último.
*Cuando una excepción tiene causa*, Java imprime TODA la cadena.