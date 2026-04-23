[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/EbtZGzoI)
[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=23668612)

# Práctica 1 - Mini Cloud Log Analyzer en ARM64

## Autor

Lopez Mendoza Alejandro - 23212001

---

## Descripción

En esta práctica se desarrolló un analizador de registros (logs) utilizando lenguaje ensamblador ARM64. El programa es capaz de leer códigos de estado HTTP desde la entrada estándar (`stdin`) y procesarlos en tiempo real.

El sistema examina cada código recibido y permite identificar condiciones específicas según la variante asignada.

---

## Variante implementada

### Variante C: Identificación del primer evento crítico (503)

El propósito de esta variante es localizar la primera aparición del código HTTP **503 (Service Unavailable)** dentro del flujo de datos de entrada.

---

## Lógica implementada

El funcionamiento del programa se basa en los siguientes pasos:

1. Se realiza la lectura de datos desde la entrada estándar (`stdin`) en bloques.
2. Cada byte leído es analizado individualmente.
3. Si el byte corresponde a un carácter numérico, se va formando un número entero de manera progresiva.
4. Al detectar un salto de línea (`\n`), se considera que el número está completo y se interpreta como un código HTTP.
5. El valor obtenido se compara contra el número **503**.
6. En caso de coincidencia con 503:
   - Se muestra un mensaje indicando que se detectó un evento crítico.
   - La ejecución del programa finaliza inmediatamente.
7. Si el valor no corresponde a 503:
   - El programa continúa con el procesamiento normal de la entrada.

---

## Implementación en ARM64

La lógica de detección se implementó mediante instrucciones básicas de ensamblador:

- `cmp` → realiza la comparación entre el valor leído y 503  
- `b.ne` → salta si el valor no es igual  
- `b` → redirige la ejecución para finalizar el programa  
- `write syscall` → permite imprimir mensajes en la salida estándar  

Fragmento representativo:

```asm
cmp x22, #503
b.ne continuar

adrp x0, msg_critico
add x0, x0, :lo12:msg_critico
bl write_cstr

b salida_ok

## Nota

Aunque este problema puede resolverse fácilmente en lenguajes de alto nivel, el propósito de la práctica es implementar **cómo lo resolvería la arquitectura**, no únicamente obtener el resultado.

```

---

##  Compilación

```bash
make
```

---

##  Ejecución

```bash
cat data/Palabras.txt | ./run.sh
```

---

##  Ejemplo de salida

```
Primer evento crítico detectado: 503
```

---

##  Evidencia de ejecución

Grabación realizada con asciinema:

Parte 1: https://asciinema.org/a/GOKUf0WJwdbtqjLU
Parte 2 sin error: https://asciinema.org/a/LYJGRLexwBq3y68X


---

##  Pruebas

Se ejecutaron pruebas para validar el correcto funcionamiento del programa:

```bash
make test
```

---

##  Estructura del proyecto

```
cloud-log-analyzer/
│
├── src/
│   └── analyzer.s
├── data/
│   └── logs_*.txt
├── tests/
├── run.sh
├── Makefile
└── README.md
```

---

## Restricciones cumplidas

✔ Implementación en ARM64 Assembly
✔ Uso exclusivo de syscalls Linux
✔ No se utilizó C ni Python
✔ Se respetó la variante asignada

---

## Conclusión

Esta práctica permitió trabajar directamente con procesamiento de datos a bajo nivel, utilizando instrucciones de ARM64 para manipular registros y memoria.

Se logró implementar correctamente la detección del primer evento crítico dentro de un flujo de logs, simulando el comportamiento de herramientas reales de monitoreo utilizadas en entornos de computación en la nube.
