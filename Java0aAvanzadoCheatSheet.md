# 🐘 Java Cheat Sheet

Referencia rápida de Java: descripción breve, métodos más usados y snippet de cada tema. Pensada para consultar mientras programas.

## Índice
- [I. Fundamentos del lenguaje](#i-fundamentos-del-lenguaje)
- [II. Tipos, variables y operadores](#ii-tipos-variables-y-operadores)
- [III. API básica de Java](#iii-api-básica-de-java)
- [IV. Control de flujo](#iv-control-de-flujo)
- [V. Arrays](#v-arrays)
- [VI. Programación Orientada a Objetos](#vi-programación-orientada-a-objetos)
- [VII. Empaquetado y excepciones](#vii-empaquetado-y-excepciones)
- [VIII. Entrada/Salida: Streams, Buffers, Ficheros](#viii-entradasalida-streams-buffers-ficheros)
- [IX. Genéricos y concurrencia](#ix-genéricos-y-concurrencia)
- [X. Colecciones](#x-colecciones)
- [XI. Redes y bases de datos](#xi-redes-y-bases-de-datos)
- [XII. Web y reflexión](#xii-web-y-reflexión)
- [XIII. Java Avanzado y Moderno (pre-Spring Boot)](#xiii-java-avanzado-y-moderno)

---

## I. Fundamentos del lenguaje

### 1. Características de Java

Java es un lenguaje **orientado a objetos**, **fuertemente tipado** y **multiplataforma**. Ideas clave:

- **OOP**: todo gira en torno a clases y objetos (encapsulación, herencia, polimorfismo, abstracción).
- **Sin punteros**: no se puede manipular directamente una dirección de memoria (a diferencia de C/C++), lo que evita errores de **desbordamiento de pila** (`StackOverflowError`) y **corrupción de memoria**. Java gestiona la memoria con el **Garbage Collector**.
- **Seguridad de acceso**: el propio lenguaje y la JVM impiden leer/escribir memoria sin permiso (sandbox).
- **Independiente de plataforma**: "write once, run anywhere". El código se compila a *bytecode* (`.class`) que ejecuta la JVM, no el sistema operativo directamente.
- **Compatibilidad**: nuevas versiones son (casi siempre) retrocompatibles con código antiguo.
- **Convención de nombres**: `camelCase` para variables y métodos (`miVariable`, `calcularTotal()`), `PascalCase` para clases (`MiClase`), `MAYUSCULAS_CON_GUION_BAJO` para constantes (`PI_APROX`).

**Crear un `.jar` ejecutable** (requiere un `Main-Class` en el manifiesto):

```bash
# Compilar
javac Main.java

# Generar el jar ejecutable
jar cfe MiPrograma.jar Main Main.class

# Ejecutar
java -jar MiPrograma.jar
```

---

### 2. Proceso de compilación de Java

1. **Código fuente** (`.java`) → escrito por el programador.
2. **Compilador `javac`** → traduce el `.java` a **bytecode** (`.class`), independiente de la plataforma.
3. **JVM (Java Virtual Machine)** → interpreta/compila (JIT) el bytecode a código máquina específico del sistema operativo donde se ejecuta.
4. **JRE** = JVM + librerías necesarias para ejecutar. **JDK** = JRE + herramientas para desarrollar (`javac`, `jar`, `javadoc`...).

```
MiClase.java --(javac)--> MiClase.class --(java / JVM)--> Ejecución
```

```bash
javac MiClase.java   # genera MiClase.class (bytecode)
java MiClase         # la JVM interpreta/ejecuta el bytecode
```

---

## II. Tipos, variables y operadores

### 3. Tipos primitivos

Java tiene 8 tipos primitivos, no son objetos y se almacenan directamente en la pila (*stack*).

| Tipo | Tamaño | Ejemplo | Uso típico |
|---|---|---|---|
| `byte` | 8 bits | `byte b = 10;` | ahorro de memoria, streams binarios |
| `short` | 16 bits | `short s = 100;` | poco usado |
| `int` | 32 bits | `int n = 42;` | número entero por defecto |
| `long` | 64 bits | `long l = 123L;` | enteros grandes (sufijo `L`) |
| `float` | 32 bits | `float f = 3.5f;` | decimales, poca precisión (sufijo `f`) |
| `double` | 64 bits | `double d = 3.14;` | decimal por defecto |
| `char` | 16 bits | `char c = 'A';` | un carácter Unicode |
| `boolean` | 1 bit (lógico) | `boolean ok = true;` | verdadero/falso |

```java
int edad = 25;
double precio = 19.99;
char inicial = 'J';
boolean activo = true;
```

---

### 4. Inferencia de tipos con `var`

`var` permite que el **compilador infiera el tipo** a partir del valor asignado. Solo se puede usar con variables locales inicializadas en la misma línea; el tipo queda fijo (sigue siendo tipado estático, no es como JS).

```java
var nombre = "Marc";      // el compilador infiere String
var contador = 0;         // infiere int
var lista = new ArrayList<String>(); // infiere ArrayList<String>

// var edad; // ❌ error, necesita inicialización
```

---

### 5. Operadores Java

**Aritméticos**: `+  -  *  /  %` (módulo/resto)

**Incremento/decremento**: `++` `--` (prefijo vs. postfijo importa en expresiones)

**Relacionales**: `==  !=  >  <  >=  <=`

**Lógicos**: `&&` (AND), `||` (OR), `!` (NOT) — con cortocircuito

**Asignación**: `=  +=  -=  *=  /=  %=`

```java
int a = 5, b = 2;
int suma = a + b;      // 7
int resto = a % b;     // 1

int x = 1;
x++;                    // post-incremento: x = 2
int y = ++x;             // pre-incremento: incrementa antes de asignar -> y = 3

boolean resultado = (a > b) && (b > 0); // true
boolean negado = !resultado;             // false
```

---

### 6. Variables y constantes

Una **variable** puede cambiar de valor. Una **constante** se declara con `final` y no puede reasignarse una vez inicializada. Convención: constantes en `MAYUSCULAS_CON_GUION_BAJO`.

```java
int contador = 0;          // variable normal
final double IVA = 0.21;   // constante de instancia/local
final int MAX_INTENTOS = 3;

// IVA = 0.10; // ❌ error de compilación, es final
```

---

### 7. Casting de tipos

**Conversión implícita (widening)**: Java lo hace automáticamente cuando no hay pérdida de datos (de tipo "pequeño" a "grande").

**Conversión explícita (narrowing / casting)**: hay que indicarlo con `(tipo)` porque puede perder información.

```java
// Implícita (automática)
int entero = 10;
double decimal = entero; // int -> double, sin pérdida

// Explícita (casting manual)
double pi = 3.14159;
int piEntero = (int) pi; // 3, se trunca (pierde decimales)

long numeroGrande = 123456789L;
int numeroPequeno = (int) numeroGrande; // riesgo de overflow
```

**Ver el tipo real de un objeto en tiempo de ejecución**:

```java
Object obj = "Hola";
System.out.println(obj.getClass().getSimpleName()); // "String"
```

---

## III. API básica de Java

### 8. API de Java moderna: Módulo → Paquete → Clase

La API de Java se organiza jerárquicamente: **Módulo** (`java.base`, `java.sql`...) → **Paquete** (`java.util`, `java.io`...) → **Clase** (`ArrayList`, `File`...).

El paquete `java.lang` es el **paquete padre**: se importa automáticamente en todas las clases, por eso no hace falta `import` para usar `String`, `Math`, `System`, `Object`, `Integer`, etc.

```java
// No necesita import, pertenece a java.lang
String texto = "Hola";
System.out.println(texto);
Math.max(1, 2);

// Otros paquetes SÍ requieren import
import java.util.ArrayList;
import java.util.Scanner;
```

---

### 9. Clase `Math`

Clase de `java.lang` con métodos **estáticos** para operaciones matemáticas. No se instancia, se llama directamente `Math.metodo(...)`.

**Métodos más usados**: `Math.pow(base, exp)`, `Math.sqrt(x)`, `Math.abs(x)`, `Math.max(a,b)`, `Math.min(a,b)`, `Math.round(x)`, `Math.random()` (double entre 0.0 y 1.0), `Math.floor(x)`, `Math.ceil(x)`.

```java
double potencia = Math.pow(2, 10);      // 1024.0
double raiz = Math.sqrt(16);            // 4.0
int mayor = Math.max(5, 9);             // 9

// Número aleatorio entre 0 y 99
int aleatorio = (int) (Math.random() * 100);
```

---

### 10. Datos masivos: `BigInteger` y `BigDecimal`

Para números que **superan el rango** de `long`/`double` o cuando se necesita **precisión exacta** (ej. cálculos financieros, donde `double` puede dar errores de redondeo). Están en `java.math`.

**`BigInteger`** — enteros de tamaño arbitrario. Métodos: `.add()`, `.subtract()`, `.multiply()`, `.divide()`, `.pow()`, `.compareTo()`.

**`BigDecimal`** — decimales de precisión exacta. Métodos: `.add()`, `.subtract()`, `.multiply()`, `.divide()`, `.setScale()`.

```java
import java.math.BigInteger;
import java.math.BigDecimal;

BigInteger a = new BigInteger("123456789012345678901234567890");
BigInteger b = new BigInteger("2");
BigInteger resultado = a.multiply(b);

BigDecimal precio = new BigDecimal("19.99");
BigDecimal cantidad = new BigDecimal("3");
BigDecimal total = precio.multiply(cantidad); // 59.97, sin errores de coma flotante
```

---

### 11. Clase `String`

Representa texto. Los `String` son **inmutables**: cualquier operación crea un nuevo objeto, no modifica el original.

**Métodos más usados**:
- `.length()` — número de caracteres.
- `.charAt(i)` — carácter en la posición `i`.
- `.substring(inicio, fin)` — subcadena.
- `.equals(otro)` — compara contenido (nunca usar `==` para comparar Strings).
- `.equalsIgnoreCase(otro)` — compara ignorando mayúsculas/minúsculas.
- `.toUpperCase()` / `.toLowerCase()`
- `.trim()` — quita espacios al inicio/fin.
- `.replace(a, b)`, `.split(regex)`, `.contains(texto)`, `.indexOf(texto)`.

```java
String frase = "Hola Mundo";

int longitud = frase.length();          // 10
char letra = frase.charAt(0);           // 'H'
String sub = frase.substring(0, 4);     // "Hola"

boolean iguales = frase.equals("Hola Mundo");             // true
boolean igualesSinCaso = frase.equalsIgnoreCase("HOLA MUNDO"); // true

String[] palabras = frase.split(" ");   // ["Hola", "Mundo"]
```

---

### 12. Clase `Scanner`

Permite leer datos de entrada (teclado, ficheros...). Está en `java.util`. **Siempre cerrarla** con `.close()` al terminar para liberar el recurso.

**Constructor típico**: `new Scanner(System.in)`.

**Métodos más usados**: `.nextInt()`, `.nextDouble()`, `.nextLine()`, `.next()` (una palabra), `.nextBoolean()`.

⚠️ **Cuidado**: `.nextInt()`, `.nextDouble()`, etc. **no consumen el salto de línea** (`\n`). Si después llamas a `.nextLine()`, te devolverá una cadena vacía porque lee lo que queda de la línea anterior. Solución: añadir un `.nextLine()` extra para "limpiar" el buffer.

```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);

System.out.print("Introduce tu edad: ");
int edad = sc.nextInt();
sc.nextLine(); // ⚠️ consume el salto de línea pendiente

System.out.print("Introduce tu nombre: ");
String nombre = sc.nextLine(); // ahora sí lee bien la línea completa

System.out.println(nombre + " tiene " + edad + " años");

sc.close(); // buena práctica: cerrar siempre el Scanner
```

---

## IV. Control de flujo

### 13. Condicionales

**`if / else if / else`**, **`switch`** y el **operador ternario**.

Desde Java 14, el `switch` moderno permite usar `->` y ejecutar bloques de código con `{}` devolviendo un valor con `yield`.

```java
int nota = 7;

// if / else
if (nota >= 9) {
    System.out.println("Sobresaliente");
} else if (nota >= 5) {
    System.out.println("Aprobado");
} else {
    System.out.println("Suspenso");
}

// Ternario: condicion ? valorSiTrue : valorSiFalse
String resultado = (nota >= 5) ? "Aprobado" : "Suspenso";

// switch clásico
switch (nota) {
    case 9:
    case 10:
        System.out.println("Sobresaliente");
        break;
    default:
        System.out.println("Otra nota");
}

// switch moderno (Java 14+), con bloque de código y yield
String texto = switch (nota) {
    case 9, 10 -> "Sobresaliente";
    case 7, 8 -> "Notable";
    default -> {
        String r = "Suspenso";
        yield r; // devuelve el valor del bloque
    }
};
```

---

### 14. Bucles

**Indeterminados** (no se sabe de antemano cuántas iteraciones): `while`, `do while`.

**Determinados** (se conoce el número de iteraciones o se recorre una colección): `for`, `for each`.

```java
// while: comprueba la condición ANTES de entrar
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}

// do-while: se ejecuta AL MENOS una vez, comprueba DESPUÉS
int j = 0;
do {
    System.out.println(j);
    j++;
} while (j < 5);

// for: inicialización; condición; incremento
for (int k = 0; k < 5; k++) {
    System.out.println(k);
}

// for-each: recorre cada elemento de un array/colección
int[] numeros = {1, 2, 3, 4};
for (int numero : numeros) {
    System.out.println(numero);
}
```

---

## V. Arrays

### 15. Arrays

Los arrays en Java son **estáticos**: su tamaño se fija al crearlos y no puede cambiar. Pueden contener **tipos primitivos** (`int[]`, `double[]`...) o **objetos** (`String[]`, `Persona[]`...). Los arrays de objetos en realidad guardan **referencias** a los objetos, no los objetos en sí.

```java
// Array simple
int[] numeros = new int[5];          // 5 huecos, inicializados a 0
int[] valores = {10, 20, 30};        // inicialización directa
valores[0] = 99;                     // modificar
System.out.println(valores.length);  // tamaño: 3 (propiedad, no método)

// Array de objetos (String es objeto)
String[] nombres = {"Ana", "Luis", "Marc"};

// Array bidimensional (matriz)
int[][] matriz = new int[3][3];
matriz[0][0] = 1;

int[][] matrizDirecta = {
    {1, 2, 3},
    {4, 5, 6}
};

for (int fila = 0; fila < matrizDirecta.length; fila++) {
    for (int col = 0; col < matrizDirecta[fila].length; col++) {
        System.out.print(matrizDirecta[fila][col] + " ");
    }
}
```

---

## VI. Programación Orientada a Objetos

### 16. Los 4 pilares de la POO

- **Encapsulación**: ocultar el estado interno (atributos `private`) y exponerlo controladamente mediante métodos (`getters`/`setters`).
- **Herencia**: una clase (hija) reutiliza y extiende el comportamiento de otra (padre) con `extends`.
- **Polimorfismo**: un mismo método se comporta de forma distinta según el objeto que lo invoca (sobreescritura) o según los parámetros (sobrecarga).
- **Abstracción**: modelar solo lo esencial de un objeto, ocultando los detalles de implementación (clases/métodos abstractos, interfaces).

Además, la POO aporta **modularidad**: el código se organiza en clases independientes y reutilizables.

```java
public class Animal {                 // Abstracción/Encapsulación
    private String nombre;             // encapsulación: atributo privado

    public Animal(String nombre) {
        this.nombre = nombre;
    }

    public void hacerSonido() {        // método sobreescribible -> polimorfismo
        System.out.println("Sonido genérico");
    }
}

public class Perro extends Animal {    // herencia
    public Perro(String nombre) {
        super(nombre);
    }

    @Override
    public void hacerSonido() {        // polimorfismo
        System.out.println("Guau");
    }
}
```

---

### 17. Constructores y `this`

El **constructor** es el método especial (mismo nombre que la clase, sin tipo de retorno) que se ejecuta al crear un objeto con `new`, e inicializa su estado. `this` hace referencia al **objeto actual**, útil para distinguir el atributo del parámetro cuando tienen el mismo nombre, o para llamar a otro constructor de la misma clase.

```java
public class Persona {
    private String nombre;
    private int edad;

    // Constructor
    public Persona(String nombre, int edad) {
        this.nombre = nombre; // this.nombre = atributo, nombre = parámetro
        this.edad = edad;
    }

    // Constructor que llama a otro constructor (this(...))
    public Persona(String nombre) {
        this(nombre, 18); // reutiliza el constructor de arriba
    }
}

Persona p1 = new Persona("Ana", 30);
Persona p2 = new Persona("Luis"); // edad por defecto 18
```

---

### 18. Getters y Setters

Métodos públicos para leer (`get`) y modificar (`set`) atributos privados, respetando la encapsulación. El **setter es opcional** (a diferencia del constructor, que suele ser obligatorio para inicializar el objeto): puedes decidir no dar un setter para que un atributo solo se pueda fijar en la creación (inmutable desde fuera).

```java
public class Producto {
    private String nombre;
    private double precio;

    public Producto(String nombre, double precio) {
        this.nombre = nombre;
        this.precio = precio;
    }

    // Getter
    public double getPrecio() {
        return precio;
    }

    // Setter (opcional, aquí con validación)
    public void setPrecio(double precio) {
        if (precio >= 0) {
            this.precio = precio;
        }
    }
    // "nombre" no tiene setter -> es inmutable tras la creación
}
```

---

### 19. Sobrecarga de métodos (Overloading) vs `@Override`

**Sobrecarga (overloading)**: varios métodos con el **mismo nombre** pero **distintos parámetros** (número o tipo) dentro de la misma clase. Se decide en tiempo de compilación.

**`@Override`**: anotación que indica que un método **redefine** uno heredado de la clase padre (o interfaz). No es sobrecarga, es sobreescritura (polimorfismo). El compilador verifica que realmente exista el método en el padre.

```java
public class Calculadora {
    // Sobrecarga: mismo nombre, distintos parámetros
    public int sumar(int a, int b) {
        return a + b;
    }

    public double sumar(double a, double b) {
        return a + b;
    }

    public int sumar(int a, int b, int c) {
        return a + b + c;
    }
}

public class CalculadoraCientifica extends Calculadora {
    @Override // sobreescritura, no sobrecarga
    public int sumar(int a, int b) {
        System.out.println("Suma con log");
        return super.sumar(a, b);
    }
}
```

---

### 20. Constantes de clase con `final`

Un atributo `final` no puede reasignarse una vez inicializado. Combinado con `static`, se convierte en una **constante de clase**, compartida por todos los objetos.

```java
public class Circulo {
    public static final double PI = 3.14159; // constante de clase

    private double radio;

    public double calcularArea() {
        return PI * radio * radio;
    }
}

// Acceso: Circulo.PI (no hace falta instanciar)
```

---

### 21. `static`

`static` indica que un miembro (atributo o método) **pertenece a la clase**, no a cada instancia individual. Se comparte entre todos los objetos y se puede acceder sin crear un objeto (`Clase.miembro`).

```java
public class Contador {
    private static int totalInstancias = 0; // compartida por todos los objetos
    private int id;

    public Contador() {
        totalInstancias++;         // se incrementa cada vez que se crea uno
        this.id = totalInstancias;
    }

    public static int getTotalInstancias() { // método static
        return totalInstancias;
    }
}

new Contador();
new Contador();
System.out.println(Contador.getTotalInstancias()); // 2, sin instanciar nada
```

---

### 22. Herencia y `super`

Una clase hija hereda atributos y métodos de la clase padre con `extends`. `super` se usa para invocar el **constructor** o los **métodos** de la clase padre. Java **no permite herencia múltiple de clases** (una clase solo puede extender de una única clase), aunque sí de interfaces.

```java
public class Vehiculo {
    protected String marca;

    public Vehiculo(String marca) {
        this.marca = marca;
    }

    public void arrancar() {
        System.out.println("El vehículo arranca");
    }
}

public class Coche extends Vehiculo {
    private int puertas;

    public Coche(String marca, int puertas) {
        super(marca); // llama al constructor del padre, debe ir primero
        this.puertas = puertas;
    }

    @Override
    public void arrancar() {
        super.arrancar(); // llama al método del padre
        System.out.println("Coche " + marca + " listo");
    }
}
```

---

### 23. Cadenas de herencia y `final`

Una clase puede heredar de otra que a su vez hereda de otra (`A -> B -> C`), formando una cadena. Se usa `final` sobre una clase para **impedir que se herede** de ella (marcar el "final" de la cadena), o sobre un método para impedir que se sobreescriba en las hijas.

```java
public class Ser {}
public class SerVivo extends Ser {}
public class Animal extends SerVivo {}

public final class Humano extends Animal {  // final: nadie puede heredar de Humano
    public final void respirar() {           // final: nadie puede sobreescribir este método
        System.out.println("Respirando");
    }
}

// public class Superhumano extends Humano {} // ❌ error, Humano es final
```

---

### 24. Clase `Object`

Es la **superclase raíz** de todas las clases de Java: todo objeto hereda (implícita o explícitamente) de `Object`. Aporta métodos comunes que se pueden sobreescribir.

**Métodos más usados**: `.toString()` (representación en texto), `.equals(obj)` (compara igualdad lógica, no de referencia), `.hashCode()` (código numérico para colecciones basadas en hash), `.getClass()`.

```java
public class Persona {
    private String nombre;

    public Persona(String nombre) { this.nombre = nombre; }

    @Override
    public String toString() { // por defecto imprime algo como Persona@1b6d3586
        return "Persona: " + nombre;
    }

    @Override
    public boolean equals(Object obj) { // igualdad "lógica" en vez de por referencia
        if (!(obj instanceof Persona)) return false;
        Persona otra = (Persona) obj;
        return this.nombre.equals(otra.nombre);
    }
}

Persona p = new Persona("Ana");
System.out.println(p); // usa toString() automáticamente -> "Persona: Ana"
```

---

### 25. Modificadores de acceso

| Modificador | Misma clase | Mismo paquete | Clase hija (otro paquete) | Todo el mundo |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| *(default, sin poner nada)* | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

```java
public class Cuenta {
    private double saldo;          // solo accesible dentro de Cuenta
    protected String titular;      // accesible en el paquete y en clases hijas
    String tipoCuenta;             // default: solo accesible en el mismo paquete
    public String numeroCuenta;    // accesible desde cualquier sitio
}
```

---

### 26. Polimorfismo

Capacidad de tratar objetos de distintas clases (relacionadas por herencia) de manera uniforme a través de una referencia del tipo padre, ejecutando en tiempo de ejecución el método específico de cada clase (**polimorfismo dinámico** / *dynamic dispatch*).

```java
public class Figura {
    public double calcularArea() { return 0; }
}
public class Circulo extends Figura {
    private double radio;
    public Circulo(double radio) { this.radio = radio; }
    @Override
    public double calcularArea() { return Math.PI * radio * radio; }
}
public class Cuadrado extends Figura {
    private double lado;
    public Cuadrado(double lado) { this.lado = lado; }
    @Override
    public double calcularArea() { return lado * lado; }
}

// Uso polimórfico: mismo tipo de referencia, distinto comportamiento
Figura[] figuras = { new Circulo(2), new Cuadrado(3) };
for (Figura f : figuras) {
    System.out.println(f.calcularArea()); // llama al método correcto en cada caso
}
```

---

### 27. Casting de objetos (Upcasting / Downcasting)

**Upcasting**: convertir una referencia de una clase hija a su clase padre. Es **implícito y siempre seguro** (una clase hija siempre "es" un padre).

**Downcasting**: convertir una referencia de padre a hija. Es **explícito** y puede fallar en tiempo de ejecución (`ClassCastException`) si el objeto real no es de ese tipo; se recomienda comprobar antes con `instanceof`.

```java
class Animal {}
class Perro extends Animal {
    void ladrar() { System.out.println("Guau"); }
}

// Upcasting (automático, seguro)
Animal miAnimal = new Perro();

// Downcasting (manual, puede fallar)
if (miAnimal instanceof Perro) {
    Perro miPerro = (Perro) miAnimal; // downcasting explícito
    miPerro.ladrar();
}
```

---

### 28. Abstracción: clases y métodos abstractos

Un **método abstracto** solo se declara (firma), sin implementación (sin `{}`), y termina con `;`. Una **clase abstracta** contiene al menos un método abstracto (o simplemente se marca `abstract` aunque no tenga ninguno) y **no se puede instanciar** con `new`. Las clases hijas están obligadas a implementar (dar cuerpo a) todos los métodos abstractos, o serán abstractas también.

```java
public abstract class Figura {
    public abstract double calcularArea(); // sin implementación

    public void mostrarInfo() { // método normal, sí implementado
        System.out.println("Área: " + calcularArea());
    }
}

// Figura f = new Figura(); // ❌ error, no se puede instanciar una clase abstracta

public class Triangulo extends Figura {
    private double base, altura;

    public Triangulo(double base, double altura) {
        this.base = base; this.altura = altura;
    }

    @Override
    public double calcularArea() { // obligatorio implementarlo
        return (base * altura) / 2;
    }
}
```

---

### 29. Interfaces

Contrato que define un conjunto de métodos que las clases implementadoras deben cumplir (`implements`). Permiten **herencia múltiple** (una clase puede implementar varias interfaces, algo que no se permite con `extends` de clases). Desde **Java 8** las interfaces pueden tener:

- **Constantes** (`public static final` implícito).
- **Métodos abstractos** (sin cuerpo, implementación obligatoria).
- **Métodos `default`** (con implementación por defecto, se puede sobreescribir).
- **Métodos `static`** (utilidad, se llaman desde la interfaz).
- **Métodos `private`** (Java 9+, para reutilizar código entre métodos `default`).

```java
public interface Volador {
    int ALTURA_MAXIMA = 1000; // constante implícita: public static final

    void volar(); // método abstracto

    default void aterrizar() { // método default, implementado
        System.out.println("Aterrizando...");
    }

    static Volador crearPorDefecto() { // método static
        return () -> System.out.println("Volando genérico");
    }
}

public class Avion implements Volador {
    @Override
    public void volar() { // obligatorio implementarlo
        System.out.println("El avión vuela");
    }
}
```

---

### 30. Clases inner (internas)

Clase definida **dentro de otra clase**. Permite acceder a los miembros privados de la clase externa y ayuda a **encapsular/ocultar** clases auxiliares que no tienen sentido fuera de su contexto. Existen varias variantes: internas de instancia, `static` (no necesitan instancia de la externa) y **locales** (definidas dentro de un método, solo visibles ahí).

```java
public class Coche {
    private String marca = "Toyota";

    // Clase inner (no static): puede acceder a atributos privados de Coche
    class Motor {
        void encender() {
            System.out.println("Encendiendo motor de " + marca); // accede al privado
        }
    }

    void metodoConClaseLocal() {
        // Clase interna LOCAL: solo existe dentro de este método
        class Ayudante {
            void ayudar() { System.out.println("Ayudando..."); }
        }
        new Ayudante().ayudar();
    }
}

Coche c = new Coche();
Coche.Motor motor = c.new Motor(); // necesita una instancia de Coche
motor.encender();
```

---

### 31. Clases internas anónimas

Clase **sin nombre** que se define e instancia a la vez, normalmente para implementar una interfaz o extender una clase de forma puntual (muy usada antes de las *lambdas*).

```java
public interface Saludo {
    void saludar();
}

public class Ejemplo {
    public static void main(String[] args) {
        // Clase anónima: implementación al vuelo, sin crear un .java aparte
        Saludo saludo = new Saludo() {
            @Override
            public void saludar() {
                System.out.println("¡Hola desde clase anónima!");
            }
        };
        saludo.saludar();

        // Equivalente moderno con lambda (interfaz funcional)
        Saludo saludoLambda = () -> System.out.println("¡Hola con lambda!");
        saludoLambda.saludar();
    }
}
```

---

## VII. Empaquetado y excepciones

### 32. Archivos `.jar`

Un `.jar` (*Java ARchive*) empaqueta clases `.class`, recursos y un manifiesto (`META-INF/MANIFEST.MF`) en un único fichero comprimido, para **distribuir aplicaciones** fácilmente.

```bash
# Crear jar ejecutable (e = entry point / clase con main)
jar cfe MiApp.jar paquete.Main *.class

# Ver contenido de un jar
jar tf MiApp.jar

# Ejecutar un jar
java -jar MiApp.jar
```

---

### 33. Excepciones

Mecanismo para gestionar errores en tiempo de ejecución sin que el programa "explote" de forma descontrolada.

- **`try`**: bloque donde puede ocurrir el error.
- **`catch`**: captura y gestiona el tipo de excepción indicado.
- **`finally`**: se ejecuta **siempre**, ocurra o no la excepción (ideal para cerrar recursos).
- **`throws`**: en la firma de un método, indica que **puede lanzar** una excepción (delega la gestión a quien lo llame).
- **`throw`**: lanza una excepción **manualmente**.
- **Excepciones personalizadas**: se crean extendiendo `Exception` (comprobada / *checked*) o `RuntimeException` (no comprobada), o `IOException` si es específica de entrada/salida.

```java
// Excepción personalizada
public class SaldoInsuficienteException extends Exception {
    public SaldoInsuficienteException(String mensaje) {
        super(mensaje);
    }
}

public class Cuenta {
    private double saldo = 100;

    public void retirar(double cantidad) throws SaldoInsuficienteException {
        if (cantidad > saldo) {
            throw new SaldoInsuficienteException("Saldo insuficiente"); // manual
        }
        saldo -= cantidad;
    }
}

public class Main {
    public static void main(String[] args) {
        Cuenta cuenta = new Cuenta();
        try {
            cuenta.retirar(500);
        } catch (SaldoInsuficienteException e) {
            System.out.println("Error: " + e.getMessage());
        } finally {
            System.out.println("Operación finalizada"); // siempre se ejecuta
        }
    }
}
```

---

## VIII. Entrada/Salida: Streams, Buffers, Ficheros

### 34. Streams de datos

Un **stream** (flujo) es el canal de comunicación entre la aplicación Java y una fuente/destino de datos (fichero, red, teclado...). Los datos "fluyen" en un sentido (entrada o salida). Reglas importantes:

- **No mezclar** streams de distinta naturaleza (byte vs. carácter) sobre el mismo recurso sin conversión.
- **Cerrar siempre** el stream al terminar (`.close()`), o usar *try-with-resources* para que se cierre automáticamente.

```java
import java.io.FileReader;
import java.io.IOException;

// try-with-resources: cierra el stream automáticamente al salir del bloque
try (FileReader lector = new FileReader("datos.txt")) {
    int caracter;
    while ((caracter = lector.read()) != -1) {
        System.out.print((char) caracter);
    }
} catch (IOException e) {
    e.printStackTrace();
} // no hace falta lector.close(), se cierra solo
```

---

### 35. Buffers

Un **buffer** es una zona de memoria intermedia donde se almacenan datos leídos/escritos en bloques, en lugar de acceder al fichero/recurso carácter a carácter o byte a byte. Mejora mucho el rendimiento: primero se llena el buffer (una sola operación de E/S "cara") y luego se accede a los datos desde memoria (rápido).

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

try (BufferedReader br = new BufferedReader(new FileReader("datos.txt"))) {
    String linea;
    while ((linea = br.readLine()) != null) { // lee línea a línea desde el buffer
        System.out.println(linea);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

---

### 36. Streams de bytes (binario)

Para datos **binarios** (imágenes, ejecutables, cualquier fichero no textual) se usan `FileInputStream` / `FileOutputStream`, que trabajan con `byte` en lugar de `char`.

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

try (FileInputStream entrada = new FileInputStream("origen.bin");
     FileOutputStream salida = new FileOutputStream("copia.bin")) {

    int byteLeido;
    while ((byteLeido = entrada.read()) != -1) {
        salida.write(byteLeido);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

---

### 37. Serialización

Convertir un objeto en Java en una secuencia de bytes para guardarlo en disco o enviarlo por red, y poder **reconstruirlo** después (deserializar). La clase debe implementar `Serializable` (interfaz marcadora, sin métodos). Se recomienda definir un `serialVersionUID` para controlar la compatibilidad entre versiones de la clase.

```java
import java.io.*;

public class Persona implements Serializable {
    private static final long serialVersionUID = 1L; // control de versión
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre; this.edad = edad;
    }
}

// Serializar (guardar objeto)
try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("persona.dat"))) {
    oos.writeObject(new Persona("Ana", 30));
} catch (IOException e) { e.printStackTrace(); }

// Deserializar (leer objeto)
try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("persona.dat"))) {
    Persona p = (Persona) ois.readObject();
} catch (IOException | ClassNotFoundException e) { e.printStackTrace(); }
```

---

### 38. Clase `File`

Representa una ruta a un fichero o directorio del sistema (no su contenido). Sirve para **comprobar, crear, borrar o listar** archivos.

**Métodos más usados**: `.exists()`, `.createNewFile()`, `.delete()`, `.mkdir()`, `.isDirectory()`, `.isFile()`, `.listFiles()`, `.getName()`, `.length()`.

```java
import java.io.File;
import java.io.IOException;

File fichero = new File("datos.txt");

if (!fichero.exists()) {
    try {
        fichero.createNewFile();
    } catch (IOException e) { e.printStackTrace(); }
}

System.out.println("Nombre: " + fichero.getName());
System.out.println("Tamaño: " + fichero.length() + " bytes");
System.out.println("¿Es directorio?: " + fichero.isDirectory());
```

---

## IX. Genéricos y concurrencia

### 39. Programación genérica

Permite escribir clases y métodos que trabajan con **cualquier tipo** de forma segura (sin necesidad de castear), indicado con parámetros de tipo entre `<>` (convención: `T`, `E`, `K`, `V`...). Los genéricos **no se llevan bien con la herencia de tipos primitivos** (no se puede usar `int`, solo `Integer`) ni son covariantes por defecto (`List<Perro>` no es un `List<Animal>`). Para eso existen los **tipos comodín** (`?`).

```java
// Clase genérica
public class Caja<T> {
    private T contenido;
    public void guardar(T contenido) { this.contenido = contenido; }
    public T obtener() { return contenido; }
}

Caja<String> cajaTexto = new Caja<>();
cajaTexto.guardar("Hola");

// Método genérico
public static <T> void imprimir(T elemento) {
    System.out.println(elemento);
}

// Tipo comodín con herencia: acepta la clase indicada o cualquier subclase
public static void mostrarAnimales(List<? extends Animal> animales) {
    for (Animal a : animales) {
        System.out.println(a);
    }
}
```

---

### 40. Programación multihilo (Threads)

Permite ejecutar varias tareas de forma **concurrente**. Pasos típicos con `Runnable` (forma recomendada, ya que Java no permite herencia múltiple y así la clase puede seguir extendiendo otra):

1. Crear una clase que **implemente `Runnable`**.
2. Escribir el código a ejecutar en el hilo dentro del método `run()`.
3. **Instanciar** esa clase (el objeto `Runnable`).
4. Crear un objeto `Thread`, **pasándole el `Runnable`** en el constructor.
5. **Poner en marcha** el hilo con `.start()` (nunca llamar a `.run()` directamente, o se ejecutaría en el hilo actual sin concurrencia).

```java
public class TareaImpresion implements Runnable {
    @Override
    public void run() { // código que ejecutará el hilo
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Runnable tarea = new TareaImpresion();  // 1. instancia del Runnable
        Thread hilo = new Thread(tarea);        // 2. crear Thread pasándole el runnable
        hilo.start();                           // 3. poner en marcha (¡no usar run()!)

        // Otra alternativa: extendiendo Thread directamente
        Thread hilo2 = new Thread(() -> System.out.println("Hilo con lambda"));
        hilo2.start();
    }
}
```

---

## X. Colecciones

### 41. Colecciones (`java.util`)

Estructuras de datos **dinámicas** (a diferencia de los arrays) para almacenar grupos de objetos.

**`List`** (permite duplicados, mantiene orden de inserción): implementación más usada `ArrayList`.
- Métodos: `.add(e)`, `.get(i)`, `.set(i, e)`, `.remove(i)`, `.size()`, `.contains(e)`, `.isEmpty()`, `.clear()`.

**`Set`** (no permite duplicados): `HashSet` (sin orden), `LinkedHashSet` (orden de inserción), `TreeSet` (ordenado).
- Métodos: `.add(e)`, `.remove(e)`, `.contains(e)`, `.size()`.

**`Map`** (pares clave-valor, claves únicas): `HashMap` (sin orden), `LinkedHashMap`, `TreeMap` (ordenado por clave).
- Métodos: `.put(k, v)`, `.get(k)`, `.remove(k)`, `.containsKey(k)`, `.keySet()`, `.values()`, `.entrySet()`.

**`Queue`** (cola FIFO): `LinkedList`, `ArrayDeque`.
- Métodos: `.offer(e)` (encolar), `.poll()` (desencolar), `.peek()` (mirar el primero sin sacarlo).

**Iteradores**: permiten recorrer una colección de forma segura (incluso eliminando elementos mientras se itera).

```java
import java.util.*;

// List
List<String> lista = new ArrayList<>();
lista.add("Ana");
lista.add("Luis");
lista.remove(0);
System.out.println(lista.get(0)); // "Luis"

// Set
Set<Integer> set = new HashSet<>();
set.add(1);
set.add(1); // se ignora, ya existe
System.out.println(set.size()); // 1

// Map
Map<String, Integer> edades = new HashMap<>();
edades.put("Ana", 30);
edades.put("Luis", 25);
for (Map.Entry<String, Integer> entry : edades.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}

// Queue
Queue<String> cola = new LinkedList<>();
cola.offer("primero");
cola.offer("segundo");
System.out.println(cola.poll()); // "primero"

// Iterator
Iterator<String> it = lista.iterator();
while (it.hasNext()) {
    String elemento = it.next();
    if (elemento.equals("Luis")) {
        it.remove(); // eliminación segura durante la iteración
    }
}
```

---

## XI. Redes y bases de datos

### 42. Sockets

Un **socket** es el canal de comunicación bidireccional entre un **cliente** y un **servidor** a través de la red (IP + puerto). `ServerSocket` espera conexiones; `Socket` representa la conexión establecida.

```java
import java.io.*;
import java.net.*;

// SERVIDOR
ServerSocket servidor = new ServerSocket(5000);   // escucha en el puerto 5000
Socket cliente = servidor.accept();                // espera conexión (bloqueante)
BufferedReader in = new BufferedReader(new InputStreamReader(cliente.getInputStream()));
System.out.println("Cliente dice: " + in.readLine());
servidor.close();

// CLIENTE
Socket socket = new Socket("localhost", 5000);      // se conecta al servidor
PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
out.println("Hola servidor");
socket.close();
```

---

### 43. JDBC (Java Database Connectivity)

API para conectar aplicaciones Java con bases de datos relacionales. Pasos: crear la **cadena de conexión**, obtener la `Connection`, crear un `Statement`, **ejecutar** la sentencia SQL y **leer** el `ResultSet`.

```java
import java.sql.*;

String url = "jdbc:mysql://localhost:3306/miBaseDeDatos"; // cadena de conexión
String usuario = "root";
String password = "1234";

try (Connection con = DriverManager.getConnection(url, usuario, password);
     Statement stmt = con.createStatement()) {

    ResultSet rs = stmt.executeQuery("SELECT id, nombre FROM usuarios"); // ejecutar SQL

    while (rs.next()) { // leer el ResultSet fila a fila
        int id = rs.getInt("id");
        String nombre = rs.getString("nombre");
        System.out.println(id + " - " + nombre);
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

---

### 44. Procedimientos almacenados y `PreparedStatement`

**`PreparedStatement`**: versión de `Statement` que **precompila** la consulta y usa parámetros (`?`), evitando inyección SQL y mejorando el rendimiento en consultas repetidas.

**Procedimientos almacenados**: lógica SQL guardada en la propia base de datos, invocada con `CallableStatement`.

```java
import java.sql.*;

// PreparedStatement: evita inyección SQL
String sql = "SELECT * FROM usuarios WHERE nombre = ?";
try (Connection con = DriverManager.getConnection(url, usuario, password);
     PreparedStatement ps = con.prepareStatement(sql)) {

    ps.setString(1, "Ana"); // sustituye el primer "?"
    ResultSet rs = ps.executeQuery();
    while (rs.next()) {
        System.out.println(rs.getString("nombre"));
    }
} catch (SQLException e) { e.printStackTrace(); }

// Llamada a procedimiento almacenado
try (Connection con = DriverManager.getConnection(url, usuario, password);
     CallableStatement cs = con.prepareCall("{call obtenerUsuario(?)}")) {

    cs.setInt(1, 5);
    ResultSet rs = cs.executeQuery();
} catch (SQLException e) { e.printStackTrace(); }
```

---

### 45. Transacciones y metadatos

**Transacción**: conjunto de operaciones SQL que se ejecutan como una unidad **atómica** (todo o nada). Se desactiva el `autoCommit`, se hace `commit()` si todo va bien o `rollback()` si algo falla.

**Metadatos**: información sobre la propia base de datos o el resultado de una consulta (`DatabaseMetaData`, `ResultSetMetaData`), como nombres de columnas, tipos, etc.

```java
import java.sql.*;

try (Connection con = DriverManager.getConnection(url, usuario, password)) {
    con.setAutoCommit(false); // inicia la transacción manualmente

    try (Statement stmt = con.createStatement()) {
        stmt.executeUpdate("UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1");
        stmt.executeUpdate("UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2");
        con.commit(); // confirma ambos cambios
    } catch (SQLException e) {
        con.rollback(); // deshace todo si algo falla
    }

    // Metadatos del resultado
    ResultSet rs = con.createStatement().executeQuery("SELECT * FROM cuentas");
    ResultSetMetaData meta = rs.getMetaData();
    System.out.println("Nº columnas: " + meta.getColumnCount());
    System.out.println("Nombre col 1: " + meta.getColumnName(1));
} catch (SQLException e) { e.printStackTrace(); }
```

---

## XII. Web y reflexión

### 46. Servlets

Clases Java que se ejecutan en un **servidor web** (contenedor como Tomcat) y gestionan peticiones/respuestas HTTP dinámicamente. Extienden `HttpServlet` y sobreescriben métodos como `doGet` / `doPost`.

```java
import javax.servlet.http.*;
import javax.servlet.*;
import java.io.*;

public class SaludoServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        response.setContentType("text/html");
        String nombre = request.getParameter("nombre"); // parámetro de la URL/formulario

        PrintWriter out = response.getWriter();
        out.println("<h1>Hola, " + nombre + "</h1>");
    }
}
```

---

### 47. JavaBeans

Clase Java que sigue una **convención estándar**: constructor vacío (sin argumentos), atributos `private`, y getters/setters siguiendo el patrón `getX()`/`setX()` (o `isX()` para `boolean`). Se usa para representar datos de forma que herramientas y frameworks (formularios web, ORMs, IDEs...) puedan manipularlos automáticamente.

```java
public class UsuarioBean implements java.io.Serializable {
    private String nombre;
    private int edad;

    public UsuarioBean() {} // constructor vacío obligatorio

    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }

    public int getEdad() { return edad; }
    public void setEdad(int edad) { this.edad = edad; }
}
```

---

### 48. Introspección (Reflection API)

Permite **inspeccionar y manipular en tiempo de ejecución** la estructura de una clase (atributos, métodos, constructores, modificadores) sin conocerla en tiempo de compilación. Paquete `java.lang.reflect`, clase principal `Class`.

**Clases más usadas**: `Class` (representa la clase), `Field` (atributos), `Method` (métodos), `Constructor`, `Modifier` (para leer modificadores como `public`/`private`/`static`).

```java
import java.lang.reflect.*;

public class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Hola, soy " + nombre); }
}

public class Main {
    public static void main(String[] args) throws Exception {
        Class<?> clase = Persona.class; // también: Class.forName("Persona")

        System.out.println("Clase: " + clase.getSimpleName());

        // Inspeccionar campos
        for (Field campo : clase.getDeclaredFields()) {
            System.out.println("Campo: " + campo.getName() +
                " Modificador: " + Modifier.toString(campo.getModifiers()));
        }

        // Inspeccionar métodos
        for (Method metodo : clase.getDeclaredMethods()) {
            System.out.println("Método: " + metodo.getName());
        }

        // Crear una instancia por reflexión e invocar un método
        Constructor<?> constructor = clase.getConstructor(String.class);
        Object persona = constructor.newInstance("Ana");
        Method saludar = clase.getMethod("saludar");
        saludar.invoke(persona); // "Hola, soy Ana"
    }
}
```

---

## XIII. Java Avanzado y Moderno

> Conceptos imprescindibles antes de dar el salto a **Spring Boot**: son la base que usan (implícita o explícitamente) los frameworks modernos.

### 49. Principios SOLID

Conjunto de 5 principios de diseño orientado a objetos para escribir código mantenible, flexible y desacoplado. Spring Boot está construido pensando en ellos (por eso encajan tan bien la inyección de dependencias, las interfaces de repositorio, etc.).

- **S — Single Responsibility (Responsabilidad única)**: una clase debe tener una única razón para cambiar, es decir, una única responsabilidad.
- **O — Open/Closed (Abierto/cerrado)**: el código debe estar abierto a extensión pero cerrado a modificación (extender comportamiento sin tocar lo que ya funciona, normalmente vía herencia/interfaces).
- **L — Liskov Substitution (Sustitución de Liskov)**: un objeto de una clase hija debe poder sustituir a uno de la clase padre sin romper el programa.
- **I — Interface Segregation (Segregación de interfaces)**: mejor muchas interfaces pequeñas y específicas que una única interfaz enorme que obligue a implementar métodos innecesarios.
- **D — Dependency Inversion (Inversión de dependencias)**: depender de **abstracciones** (interfaces), no de implementaciones concretas. Es la base de la **inyección de dependencias** que usa Spring.

```java
// ❌ Viola "D": depende de una clase concreta
public class ServicioPedido {
    private MySqlRepositorio repositorio = new MySqlRepositorio(); // acoplado
}

// ✅ Cumple "D": depende de una abstracción (interfaz)
public interface Repositorio {
    void guardar(String dato);
}

public class MySqlRepositorio implements Repositorio {
    public void guardar(String dato) { System.out.println("Guardado en MySQL: " + dato); }
}

public class ServicioPedido {
    private final Repositorio repositorio; // depende de la interfaz, no de MySqlRepositorio

    public ServicioPedido(Repositorio repositorio) { // "inyectada" desde fuera
        this.repositorio = repositorio;
    }

    public void procesar(String pedido) {
        repositorio.guardar(pedido);
    }
}

// Esto es justo lo que hace Spring con @Autowired / inyección por constructor
ServicioPedido servicio = new ServicioPedido(new MySqlRepositorio());
```

---

### 50. Patrones de diseño

Soluciones **reutilizables y probadas** a problemas comunes de diseño de software. Spring usa varios de estos patrones internamente (Singleton para los `@Bean`, Factory para crear objetos, Proxy para `@Transactional`...).

**Singleton** — garantiza que una clase tenga **una única instancia** en toda la aplicación, con un punto de acceso global.

```java
public class Configuracion {
    private static Configuracion instancia; // única instancia
    private Configuracion() {} // constructor privado, no se puede hacer "new" desde fuera

    public static Configuracion getInstancia() {
        if (instancia == null) {
            instancia = new Configuracion();
        }
        return instancia;
    }
}

Configuracion c1 = Configuracion.getInstancia();
Configuracion c2 = Configuracion.getInstancia();
System.out.println(c1 == c2); // true, es el mismo objeto
```

**Factory (fábrica)** — centraliza la **creación de objetos**, ocultando la lógica de qué clase concreta instanciar.

```java
public interface Notificacion { void enviar(String msg); }
public class Email implements Notificacion {
    public void enviar(String msg) { System.out.println("Email: " + msg); }
}
public class SMS implements Notificacion {
    public void enviar(String msg) { System.out.println("SMS: " + msg); }
}

public class NotificacionFactory {
    public static Notificacion crear(String tipo) {
        return switch (tipo) {
            case "email" -> new Email();
            case "sms" -> new SMS();
            default -> throw new IllegalArgumentException("Tipo no soportado");
        };
    }
}

Notificacion n = NotificacionFactory.crear("email");
n.enviar("Hola!");
```

**DAO (Data Access Object)** — aísla la lógica de **acceso a datos** (JDBC, ficheros...) del resto de la aplicación, exponiendo operaciones CRUD a través de una interfaz. Es el antecesor directo de `JpaRepository` en Spring.

```java
public interface UsuarioDAO {
    void guardar(Usuario u);
    Usuario buscarPorId(int id);
}

public class UsuarioDAOImpl implements UsuarioDAO {
    @Override
    public void guardar(Usuario u) {
        // lógica JDBC: PreparedStatement, INSERT...
        System.out.println("Usuario guardado: " + u);
    }
    @Override
    public Usuario buscarPorId(int id) {
        // lógica JDBC: SELECT...
        return new Usuario(id, "Ana");
    }
}
```

**Strategy** — define una **familia de algoritmos intercambiables** encapsulados en clases separadas, seleccionables en tiempo de ejecución.

```java
public interface EstrategiaDescuento {
    double aplicar(double precio);
}

public class SinDescuento implements EstrategiaDescuento {
    public double aplicar(double precio) { return precio; }
}
public class DescuentoNavidad implements EstrategiaDescuento {
    public double aplicar(double precio) { return precio * 0.8; }
}

public class Carrito {
    private EstrategiaDescuento estrategia; // se puede cambiar en tiempo de ejecución

    public Carrito(EstrategiaDescuento estrategia) { this.estrategia = estrategia; }

    public double calcularTotal(double precio) {
        return estrategia.aplicar(precio);
    }
}

Carrito carrito = new Carrito(new DescuentoNavidad());
System.out.println(carrito.calcularTotal(100)); // 80.0
```

**Builder** — construye objetos **complejos paso a paso**, evitando constructores con demasiados parámetros. Muy usado junto a `Lombok` en Spring Boot.

```java
public class Pizza {
    private final String masa;
    private final boolean queso;
    private final boolean jamon;

    private Pizza(Builder b) {
        this.masa = b.masa; this.queso = b.queso; this.jamon = b.jamon;
    }

    public static class Builder {
        private String masa = "fina";
        private boolean queso = false;
        private boolean jamon = false;

        public Builder masa(String masa) { this.masa = masa; return this; }
        public Builder conQueso() { this.queso = true; return this; }
        public Builder conJamon() { this.jamon = true; return this; }
        public Pizza build() { return new Pizza(this); }
    }
}

Pizza pizza = new Pizza.Builder().masa("integral").conQueso().conJamon().build();
```

---

### 51. Lambdas

Una **expresión lambda** es una función anónima y concisa que implementa una **interfaz funcional** (interfaz con un único método abstracto, anotada opcionalmente con `@FunctionalInterface`). Sustituyen a las clases anónimas para código más corto y legible. Sintaxis: `(parámetros) -> expresión_o_bloque`.

**Interfaces funcionales más usadas** (`java.util.function`): `Function<T,R>` (recibe T, devuelve R), `Consumer<T>` (recibe T, no devuelve nada), `Supplier<T>` (no recibe nada, devuelve T), `Predicate<T>` (recibe T, devuelve boolean).

```java
// Interfaz funcional propia
@FunctionalInterface
public interface Operacion {
    int calcular(int a, int b);
}

Operacion suma = (a, b) -> a + b;               // lambda con expresión
Operacion resta = (a, b) -> { return a - b; };   // lambda con bloque
System.out.println(suma.calcular(2, 3));         // 5

// Interfaces funcionales estándar de java.util.function
Function<Integer, Integer> cuadrado = x -> x * x;
System.out.println(cuadrado.apply(4)); // 16

Consumer<String> imprimir = texto -> System.out.println("Mensaje: " + texto);
imprimir.accept("Hola");

Supplier<Double> aleatorio = () -> Math.random();
System.out.println(aleatorio.get());

Predicate<Integer> esPar = n -> n % 2 == 0;
System.out.println(esPar.test(4)); // true
```

---

### 52. Referencias a métodos (Method References)

Forma **aún más corta** de escribir una lambda cuando esta simplemente llama a un método ya existente. Se usa el operador `::`. Tipos: a un método estático, a un método de instancia de un objeto concreto, a un método de instancia de un tipo arbitrario, o a un constructor.

```java
import java.util.List;
import java.util.function.Function;
import java.util.function.Supplier;

List<String> nombres = List.of("marc", "ana", "luis");

// Referencia a método de instancia de un tipo arbitrario (String::toUpperCase)
nombres.stream().map(String::toUpperCase).forEach(System.out::println);
// equivale a: nombres.stream().map(s -> s.toUpperCase()).forEach(s -> System.out.println(s));

// Referencia a método estático (Integer::parseInt)
Function<String, Integer> convertir = Integer::parseInt;
System.out.println(convertir.apply("42")); // 42

// Referencia a constructor (ClaseX::new)
Supplier<ArrayList<String>> creador = ArrayList::new;
ArrayList<String> lista = creador.get();
```

---

### 53. Streams API

**No confundir con los streams de E/S del tema 34.** Un `Stream` (de `java.util.stream`) es una secuencia de elementos sobre la que se aplican operaciones **funcionales y encadenables** (map, filter, reduce...) para procesar colecciones de forma declarativa, sin bucles explícitos. Los streams **no almacenan datos** y son de **un solo uso**.

- **Operaciones intermedias** (devuelven otro stream, son *lazy*): `.filter()`, `.map()`, `.sorted()`, `.distinct()`, `.limit()`.
- **Operaciones terminales** (ejecutan el stream y devuelven un resultado): `.collect()`, `.forEach()`, `.count()`, `.reduce()`, `.toList()`, `.anyMatch()`.

```java
import java.util.List;
import java.util.stream.Collectors;

List<String> nombres = List.of("Ana", "Luis", "Marc", "Alba", "Pedro");

// Filtrar + transformar + recolectar
List<String> resultado = nombres.stream()
    .filter(n -> n.length() > 3)      // intermedia: solo nombres > 3 letras
    .map(String::toUpperCase)          // intermedia: pasa a mayúsculas
    .sorted()                          // intermedia: ordena alfabéticamente
    .collect(Collectors.toList());     // terminal: recolecta en una List

System.out.println(resultado); // [ALBA, LUIS, MARC, PEDRO]

// reduce: combina todos los elementos en uno solo
int totalLetras = nombres.stream()
    .map(String::length)
    .reduce(0, Integer::sum); // suma todas las longitudes

// contar cuántos empiezan por "A"
long cuantosConA = nombres.stream().filter(n -> n.startsWith("A")).count();

// Streams numéricos (evitan boxing/unboxing constante)
int suma = List.of(1, 2, 3, 4).stream().mapToInt(Integer::intValue).sum();
```

---

### 54. `Optional`

Envoltorio (*wrapper*) de `java.util` para representar un valor que **puede estar presente o ausente**, evitando `NullPointerException` y el uso explícito de `null`. Muy usado en Spring Data (`findById()` devuelve un `Optional<T>`).

**Métodos más usados**: `Optional.of(valor)` (no admite null), `Optional.ofNullable(valor)` (admite null), `Optional.empty()`, `.isPresent()`, `.isEmpty()`, `.get()` (evitar, lanza excepción si está vacío), `.orElse(valorPorDefecto)`, `.orElseGet(supplier)`, `.orElseThrow()`, `.ifPresent(consumer)`, `.map()`.

```java
import java.util.Optional;

public Optional<String> buscarUsuario(int id) {
    if (id == 1) {
        return Optional.of("Ana"); // valor presente
    }
    return Optional.empty(); // ausencia explícita, en vez de devolver null
}

Optional<String> resultado = buscarUsuario(2);

// Forma segura de consumir el valor sin comprobar null a mano
resultado.ifPresent(nombre -> System.out.println("Encontrado: " + nombre));

// Valor por defecto si no existe
String nombre = resultado.orElse("Desconocido");

// Lanzar excepción si no existe (típico en repositorios Spring)
String nombreObligatorio = resultado.orElseThrow(() -> new RuntimeException("No encontrado"));

// Encadenar transformación solo si hay valor
Optional<Integer> longitud = resultado.map(String::length);
```

---

### 55. Records

Tipo especial de clase (desde **Java 16**) pensado para modelar **datos inmutables** de forma muy concisa: el compilador genera automáticamente el constructor, los getters (sin prefijo `get`, solo el nombre del campo), `equals()`, `hashCode()` y `toString()`. Ideales para DTOs, muy usados en las APIs REST de Spring Boot.

```java
// Antes (clase clásica): constructor + getters + equals + hashCode + toString a mano
public record Punto(int x, int y) {} // ¡una sola línea sustituye todo eso!

Punto p1 = new Punto(3, 4);
System.out.println(p1.x());        // getter automático: 3 (no "getX()")
System.out.println(p1.y());        // 4
System.out.println(p1);            // toString() automático: Punto[x=3, y=4]

Punto p2 = new Punto(3, 4);
System.out.println(p1.equals(p2)); // true, equals() automático por valor

// Records con validación en el constructor compacto
public record Persona(String nombre, int edad) {
    public Persona { // constructor compacto: valida antes de asignar
        if (edad < 0) {
            throw new IllegalArgumentException("La edad no puede ser negativa");
        }
    }
}

// Muy típico como DTO en Spring Boot
public record UsuarioDTO(String nombre, String email) {}
```

---

### 56. Enums

Tipo especial de clase que representa un **conjunto fijo de constantes** con seguridad de tipos (evita usar `Strings` o `int` "mágicos"). En Java, un `enum` puede tener atributos, constructores y métodos, no es solo una lista de nombres.

```java
public enum EstadoPedido {
    PENDIENTE("Esperando confirmación"),
    ENVIADO("En camino"),
    ENTREGADO("Recibido por el cliente");

    private final String descripcion; // los enums pueden tener atributos

    EstadoPedido(String descripcion) { // y constructor (siempre privado)
        this.descripcion = descripcion;
    }

    public String getDescripcion() { return descripcion; }
}

EstadoPedido estado = EstadoPedido.ENVIADO;

// switch moderno con enum, muy típico en lógica de negocio
String mensaje = switch (estado) {
    case PENDIENTE -> "Aún no se ha enviado";
    case ENVIADO -> "Está en camino";
    case ENTREGADO -> "Ya ha llegado";
};

System.out.println(estado.getDescripcion());
System.out.println(EstadoPedido.valueOf("ENVIADO")); // String -> enum
```

---

### 57. Anotaciones

Metadatos que se añaden a clases, métodos o atributos y que son leídos por el compilador o en tiempo de ejecución (vía Reflection, tema 48) por frameworks como Spring para modificar su comportamiento **sin escribir código boilerplate**. Se pueden crear anotaciones propias.

**Anotaciones estándar más usadas**: `@Override`, `@Deprecated`, `@SuppressWarnings`, `@FunctionalInterface`. En Spring aparecerán constantemente `@Component`, `@Autowired`, `@RestController`, etc. — todas funcionan bajo el mismo mecanismo.

```java
import java.lang.annotation.*;

// Crear una anotación personalizada
@Retention(RetentionPolicy.RUNTIME) // visible en tiempo de ejecución (necesario para leerla por reflexión)
@Target(ElementType.METHOD)          // solo se puede aplicar a métodos
public @interface Loggeable {
    String valor() default "INFO";
}

public class Servicio {
    @Loggeable(valor = "DEBUG") // metadato leído después con Reflection
    public void procesar() {
        System.out.println("Procesando...");
    }

    @Deprecated // anotación estándar: marca el método como obsoleto
    public void metodoAntiguo() {}
}
```

---

### 58. `Comparable` y `Comparator`

Formas de definir **criterios de ordenación** para objetos propios, usados por `Collections.sort()`, `.stream().sorted()`, `TreeSet`, `TreeMap`, etc.

**`Comparable<T>`**: se implementa **dentro de la propia clase**, define el orden "natural" (un único criterio) con el método `compareTo()`.

**`Comparator<T>`**: se define **fuera de la clase** (a menudo como lambda), permite múltiples criterios de ordenación distintos e intercambiables.

```java
import java.util.*;

public class Persona implements Comparable<Persona> {
    String nombre;
    int edad;

    Persona(String nombre, int edad) { this.nombre = nombre; this.edad = edad; }

    @Override
    public int compareTo(Persona otra) { // orden natural: por edad
        return Integer.compare(this.edad, otra.edad);
    }

    @Override
    public String toString() { return nombre + "(" + edad + ")"; }
}

List<Persona> personas = new ArrayList<>(List.of(
    new Persona("Ana", 30), new Persona("Luis", 20)
));

Collections.sort(personas); // usa compareTo() -> ordena por edad

// Comparator: criterio alternativo, por nombre, sin tocar la clase
personas.sort(Comparator.comparing(p -> p.nombre));

// Comparator con lambda y encadenado (por edad y luego por nombre)
personas.sort(Comparator.comparingInt((Persona p) -> p.edad).thenComparing(p -> p.nombre));
```

---

*Cheat sheet generada a partir de un curso de Java — usa `Ctrl+F` / la búsqueda de tu editor para encontrar el tema que necesites rápido.*