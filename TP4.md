# Trabajo Práctico 4: Cómputo Paralelo en la Nube

## 01. Objetivo
El objetivo principal de esta actividad consiste en aplicar los conocimientos adquiridos en el módulo cuatro de la asignatura. Específicamente se abordará el concepto de **paralelismo** mediante el uso de cómputo paralelo en la nube. La actividad debe ser llevada a cabo de manera individual.

**Objetivos específicos:**
* Uso de la plataforma **GitHub Codespaces** para efectuar cómputo paralelo.
* Comprender conceptos de paralelismo.
* Aprender algunos algoritmos de **machine learning** que se pueden paralelizar.

---

## 02. Situación problemática
Para adquirir experiencia en cómputo paralelo es clave comprender cómo se implementan los algoritmos en paralelo. Además, es importante conocer ámbitos de aplicación como el cálculo matemático y el machine learning.

---

## 03. Documentación extra (Lectura obligatoria)
* **Cuarto Marco Teórico-MPI.pdf**: Material con las bases del estándar MPI.

### Bases: Algoritmo para calcular $\pi$ (Montecarlo)
* **Fundamento:** Se usa un cuadrado de lado 1 que contiene un cuarto de círculo de radio 1.
* **Relación:** El área del círculo es $\pi/4$. La probabilidad de que un dardo caiga dentro del círculo permite estimar $\pi$ como:
  $$\pi = 4 * \frac{\text{aciertos}}{\text{total de lanzamientos}}$$
* **Paralelización en MPI:**
  1. El **proceso 0** define el total de lanzamientos y realiza un `MPI_Bcast`.
  2. Cada proceso calcula su cuota local y usa una semilla aleatoria diferente (`srand(time(NULL) + rank)`).
  3. Se cuentan los aciertos locales y se envían al proceso 0 mediante `MPI_Reduce` con la operación `MPI_SUM`.

### Bases: Algoritmo K-means
* **Objetivo:** Dividir datos en K grupos (clusters) donde los puntos internos sean parecidos y los grupos diferentes entre sí.
* **Paralelización en MPI:**
  1. Los puntos se reparten entre varios procesos.
  2. Cada proceso calcula a qué centro pertenecen sus puntos y acumula sumas.
  3. Se usa `MPI_Allreduce` para combinar sumas y obtener nuevos centros globales.
  4. `MPI_Bcast` distribuye los nuevos centros para la siguiente iteración.

---

## 04. Consigna abierta
Realizar los siguientes pasos y documentar la experiencia en un documento PDF.

1. **GitHub:** Iniciar sesión y realizar un **Fork** del repositorio: [https://github.com/edupiray/tp4_acyp](https://github.com/edupiray/tp4_acyp).
2. **Estructura del Repo:**
   * `.devcontainer`: Configuración de Docker para MPI.
   * `ejemplos_base`: Multiplicación de matrices (`matmul.c`, `matmul.cpp`).
   * `desafios_ml`: Algoritmos de K-means y Montecarlo $\pi$.
3. **Codespaces:** Desplegar el entorno desde el botón **Code** -> **Codespace** -> **+**.
4. **Ejecución:**
   * **C:** `mpicc matmul.c -o matmul_c && mpirun -np 4 ./matmul_c`