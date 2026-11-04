[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/open/github/v1?repo=moglyy/MSFP3)
# Práctica 3 — Modelado y control del sistema musculoesquelético

Esta práctica integra modelado viscoelástico del tejido muscular, analogía electromecánica fuerza-voltaje, análisis de la respuesta temporal y estabilidad, simulación computacional de condiciones patológicas (atrofia muscular), diseño e sintonización de un controlador PID *in silico*, así como la verificación cruzada mediante MATLAB, Simulink, Simscape Electrical y Python.

El caso de estudio representa la **dinámica musculoesquelética** mediante el modelo viscoelástico simplificado descrito por M. C. Khoo. Se busca analizar la respuesta en deformación o desplazamiento $x(t)$ ante una excitación de fuerza $F(t)$, comparando un individuo sano (control) contra un paciente con atrofia muscular (caso) y aplicando control por retroalimentación para reducir la discrepancia entre ambas respuestas.

**Docente:** Dr. Paul Antonio Valle Trujillo

**Correo:** paul.valle@tectijuana.edu.mx

**Departamento:** Ingeniería Eléctrica y Electrónica

**Institución:** Tecnológico Nacional de México / Instituto Tecnológico de Tijuana

**Asignatura:** Modelado de Sistemas Fisiológicos

**Programa:** Ingeniería Biomédica

**Estudiante:** Kiara Vianney Trinidad Diaz ·  23212240 · L23212240@tijuana.tecnm.mx

**Sede:** Blvd. Alberto Limón Padilla s/n, Tijuana, C. P. 22454, B. C., México.

## Contenido

 1. [Resumen](#resumen)

 2. [Competencia y objetivos](#objetivos)

 3. [Relación con el temario](#temario)

 4. [Modelo físico y analogía electromecánica](#analogia)

 5. [Formulación matemática](#modelo)

 6. [Análisis temporal y de estabilidad](#analisis)

 7. [Condiciones de simulación y señales](#senales)

 8. [Implementación en MATLAB, Simulink y Simscape](#matlab)

 9. [Estrategia de control in silico y sintonización PID](#pid)

10. [Reproducción en Python](#python)

11. [Diagrama biológico y correspondencia fisiológica](#biologico)

12. [Resultados y verificación cruzada](#resultados)

13. [Flujo de trabajo](#flujo)

14. [Actividades del estudiante](#actividades)

15. [Estructura del repositorio](#estructura)

16. [Requisitos](#requisitos)

17. [Reproducibilidad y entregables](#reproducibilidad)

18. [Referencias](#referencias)

## Resumen

Esta práctica tiene como propósito **modelar, analizar, simular y controlar la respuesta dinámica del sistema musculoesquelético** ante excitaciones de fuerza. Se aborda la caracterización del tejido pasivo y activo empleando una combinación de compliancias ($C_s$ y $C_p$) y amortiguación viscosa ($R$). Mediante la **analogía fuerza-voltaje**, el sistema mecánico se transforma en un circuito eléctrico equivalente para analizar su comportamiento dinámico.

Se evalúa la condición de un **individuo sano (control)** y se simula una condición de **atrofia muscular (caso)** caracterizada por un incremento en la componente de amortiguamiento viscoso ($R$). Se realiza el análisis de estabilidad y respuesta al impulso en lazo abierto. Posteriormente, se implementa un **controlador PID *in silico*** tomando la respuesta del individuo sano como señal de referencia para ajustar el tratamiento del paciente. Todos los modelos se validan en MATLAB, Simulink, Simscape Electrical y Python.

**Palabras clave:** Sistema musculoesquelético; Modelado matemático; Analogía fuerza-voltaje; Función de transferencia; Controlador PID; Atrofia muscular.

[Volver al contenido](#contenido)

## Competencia y objetivos

### Competencia a desarrollar

Modelizar sistemas fisiológicos mediante analogías entre sistemas físicos, ecuaciones diferenciales y funciones de transferencia para analizar su comportamiento dinámico y diseñar estrategias de control mediante técnicas de control clásico. En particular, analizar el sistema musculoesquelético, construir su circuito eléctrico equivalente, comparar la respuesta de un individuo sano (control) con un caso simulado de atrofia muscular (caso) y diseñar una estrategia de control *in silico* orientada a reducir las diferencias entre ambas respuestas.

### Objetivo general

Obtener el modelo matemático y circuito equivalente de un compartimento musculoesquelético, simular la respuesta dinámica de un individuo sano y de un paciente con atrofia muscular, y diseñar un sistema de control *in silico* mediante sintonización PID para reducir el error entre la respuesta del caso y la referencia del control.

### Objetivos específicos

* Identificar los componentes viscoelásticos ($F_0$, $F$, $R$, $C_s$, $C_p$) del modelo mecánico muscular.

* Formular la conversión al circuito eléctrico equivalente aplicando la analogía fuerza-voltaje.

* Obtener la función de transferencia $G(s) = X(s)/F(s)$ para las condiciones de control y caso.

* Determinar la estabilidad y los polos de la planta para ambas condiciones mecánicas.

* Implementar la simulación en Simulink mediante ecuaciones diferenciales, función de transferencia y componentes físicos de Simscape Electrical.

* Diseñar la estrategia de control por retroalimentación negativa donde la referencia es la salida del individuo sano $x_{\text{control}}(t)$.

* Sintonizar las ganancias del controlador PID ($k_P$, $k_I$, $k_D$) mediante la herramienta *Tune*.

* Validar y comparar numéricamente los resultados utilizando cuadernos computacionales de MATLAB y scripts en Python.

* Elaborar un diagrama biológico que relacione los componentes mecánicos/eléctricos con las estructuras anatómicas del músculo.

[Volver al contenido](#contenido)

## Relación con el temario

La práctica integra contenidos pertenecientes a la asignatura **Modelado de Sistemas Fisiológicos (IBF-1020)**:

| Unidad | Área | Contenidos aplicados | 
| ----- | ----- | ----- | 
| I | Sistemas de control retroalimentados | Diagramas de bloques, señales de referencia, seguimiento de trayectoria y criterios de estabilidad | 
| II | Modelado matemático | Analogías electromecánicas (fuerza-voltaje), modelos viscoelásticos musculares, ecuaciones diferenciales y funciones de transferencia | 
| III | Controladores | Tipos de controladores (PID), sintonización *in silico* de ganancias y reducción del error | 

[Volver al contenido](#contenido)

## Modelo físico y analogía electromecánica

El modelo mecánico simplificado del compartimento muscular (basado en M. C. Khoo) consta de una fuerza activa $F_0(t) = \alpha F(t)$ ($0 < \alpha < 1$), un elemento elástico en serie $C_s$, un amortiguador viscoso $R$ y un elemento elástico en paralelo $C_p$.

### Tabla de analogía fuerza-voltaje

| Sistema mecánico | Sistema eléctrico equivalente | 
| ----- | ----- | 
| Fuerza $F(t)$ | Voltaje $V(t)$ | 
| Velocidad $\frac{dx(t)}{dt}$ | Corriente $I(t)$ | 
| Desplazamiento $x(t)$ | Carga eléctrica $q(t)$ | 
| Amortiguador ($R$) | Resistencia ($R$) | 
| Compliancia mecánica ($C$) | Capacitancia ($C$) | 
| Masa ($M$) | Inductancia ($L$) | 

*Nota de topología:* Las conexiones mecánicas en paralelo equivalen a conexiones eléctricas en serie, y elementos mecánicos en serie equivalen a elementos eléctricos en paralelo.

### Parámetros del modelo (Control vs. Caso)

Para la simulación *in silico*, el deterioro muscular por atrofia se representa mediante el incremento en el parámetro de amortiguamiento viscoso ($R$):

| Parámetro | Individuo sano (Control) | Paciente (Caso: Atrofia) | Unidad eléctrica | 
| ----- | ----- | ----- | ----- | 
| Fuerza aplicada $F(t)$ / Voltaje | $1$ | $1$ | $\mathrm{V}$ | 
| Parámetro de fuerza activa $\alpha$ | $0.25$ | $0.25$ | Adimensional | 
| Compliancia serie $C_s$ | $10\times10^{-6}$ | $10\times10^{-6}$ | $\mu\mathrm{F}$ | 
| Compliancia paralelo $C_p$ | $100\times10^{-6}$ | $100\times10^{-6}$ | $\mu\mathrm{F}$ | 
| Amortiguamiento $R$ | $100$ | $10\,000$ ($10\,\mathrm{k}$) | $\Omega$ | 

[Volver al contenido](#contenido)

## Formulación matemática

### 1. Ecuaciones constitutivas

Considerando las relaciones de deformación $x(t)$ y $x_1(t)$ en las ramas del modelo mecánico y aplicando el principio de D'Alembert o las leyes Kirchhoff en el circuito análogo, la dinámica del sistema se describe por la interacción de las compliancias y la componente viscosa.

### 2. Función de transferencia

Al aplicar la transformada de Laplace con condiciones iniciales nulas, la relación entre el desplazamiento $X(s)$ y la fuerza aplicada $F(s)$ adopta la forma general:

$$
\boxed{G(s) = \frac{X(s)}{F(s)}}
$$

Sustituyendo las compliancias $C_s$, $C_p$ y la resistencia $R$, se obtienen dos funciones de transferencia particulares: $G_{\text{control}}(s)$ para el sujeto sano y $G_{\text{caso}}(s)$ para la condición de atrofia muscular.

[Volver al contenido](#contenido)

## Análisis temporal y de estabilidad

### 1. Error en estado estacionario

Se evalúa la respuesta en régimen permanente ante entradas de prueba (como el impulso o escalón). La señal de error del sistema sin controlar está dada por:

$$
e(t) = x_{\text{control}}(t) - x_{\text{caso}}(t)
$$

### 2. Ubicación de polos y estabilidad

Se determinan las raíces del denominador de $G_{\text{control}}(s)$ y $G_{\text{caso}}(s)$. Se analiza cómo el incremento de la resistencia $R$ de $100\,\Omega$ a $10\,\mathrm{k}\Omega$ desplaza los polos en el plano complejo $s$, modificando la velocidad de respuesta temporal y el grado de amortiguamiento del tejido muscular.

[Volver al contenido](#contenido)

## Condiciones de simulación y señales

Las simulaciones se realizan nominalmente en un intervalo de tiempo $t \in [0, 10]\,\mathrm{s}$.

| Señal de entrada | Propósito en el experimento *in silico* | 
| ----- | ----- | 
| Impulso unitario | Evaluar la respuesta natural del sistema musculoesquelético y comparar el comportamiento transitorio entre el control y el caso. | 

[Volver al contenido](#contenido)

## Implementación en MATLAB, Simulink y Simscape

Se construyen tres representaciones equivalentes para validar la consistencia del modelado:

1. **Ecuaciones diferenciales:** Bloques matemáticos y de integración en Simulink.

2. **Función de transferencia:** Bloque `Transfer Fcn` ajustado con $G_{\text{control}}(s)$ y $G_{\text{caso}}(s)$.

3. **Circuito físico equivalente:** Ensamble de componentes pasivos mediante Simscape Electrical.

Desde el cuaderno de MATLAB (`.mlx`) se automatiza la ejecución de las simulaciones y la extracción hacia el *Workspace* de las variables:

* $x_{\text{control}}(t)$: Respuesta de referencia del sujeto sano.

* $x_{\text{caso}}(t)$: Respuesta en lazo abierto del paciente con atrofia.

* $x_{\text{PID}}(t)$: Respuesta del paciente bajo el esquema de control.

[Volver al contenido](#contenido)

## Estrategia de control in silico y sintonización PID

### 1. Esquema de retroalimentación

Se utiliza la respuesta del individuo sano $x_{\text{control}}(t)$ como el perfil de movimiento deseado (referencia). La señal de error que alimenta al controlador es:

$$
e(t) = x_{\text{control}}(t) - x_{\text{caso}}(t)
$$

El controlador PID procesa esta diferencia y aplica la acción de control $u(t)$ sobre el modelo del paciente (Tratamiento):

$$
u(t) = k_P e(t) + k_I \int_0^t e(\tau)d\tau + k_D \frac{de(t)}{dt}
$$

```
               +---+  x_control  +---+             +-----+  u(t)  +----------+  x_PID
  F(t) ------->|Sano|----------->|(+) |  e(t) ---->| PID |------->| Paciente |--------+-->
               +---+             |   |             +-----+        | (Caso)   |        |
                                 |(-)|<-----------------------------------------------|
                                 +---+

```

### 2. Sintonización mediante *Tune*

Utilizando el afinador del bloque `PID Controller` de Simulink, se ajustan las ganancias $k_P$, $k_I$ y $k_D$ para lograr que $x_{\text{PID}}(t)$ rastree de forma estable y rápida a $x_{\text{control}}(t)$, minimizando la discrepancia del tratamiento *in silico*.

*Nota académica:* La estrategia de control desarrollada tiene fines estrictamente educativos y de simulación; no constituye un protocolo médico o terapéutico real.

[Volver al contenido](#contenido)

## Reproducción en Python

Mediante Visual Studio Code y librerías científicas (`scipy.signal`, `matplotlib`, `numpy`), se implementa un script independiente para:

1. Definir $G_{\text{control}}(s)$ y $G_{\text{caso}}(s)$.

2. Calcular la respuesta al impulso en lazo abierto.

3. Construir la función de transferencia en lazo cerrado incorporando $k_P$, $k_I$ y $k_D$.

4. Graficar y exportar las respuestas en formato PDF para contrastarlas con MATLAB.

[Volver al contenido](#contenido)

## Diagrama biológico y correspondencia fisiológica

Se incluye una representación gráfica (elaborada en BioRender o equivalente) para asociar la analogía física con la anatomía y fisiología del músculo esquelético:

| Elemento mecánico | Equivalente eléctrico | Estructura biológica asociada | 
| ----- | ----- | ----- | 
| Fuerza activa $F_0(t)$ | Fuente de voltaje | Contracción activa de puentes cruzados (Miosina-Actina) | 
| Compliancia serie $C_s$ | Capacitancia $C_s$ | Elasticidad del tendón y elementos en serie | 
| Compliancia paralelo $C_p$ | Capacitancia $C_p$ | Elasticidad del sarcolema y tejido conectivo pasivo (Epimisio/Perimisio) | 
| Amortiguamiento $R$ | Resistencia $R$ | Viscosidad del sarcoplasma y fricción interna del tejido muscular | 
| Desplazamiento $x(t)$ | Carga $q(t)$ | Deformación / Acortamiento muscular total | 

[Volver al contenido](#contenido)

## Resultados y verificación cruzada

Se comparan las curvas temporales de desplazamiento para las tres condiciones principales:

* **Línea/Color A:** Respuesta de referencia (Individuo sano / Control).

* **Línea/Color B:** Respuesta alterada sin control (Paciente / Caso de atrofia).

* **Línea/Color C:** Respuesta corregida por el sistema de control (Paciente con PID).

### Archivos de resultados y evidencias esperadas

| Simulación | MATLAB / Simulink | Python | 
| ----- | ----- | ----- | 
| Respuesta al impulso | `impulse_matlab.pdf` | `impulse_python.pdf` | 
| Esquema de Control | `diagrama_control.png` | N/A | 
| Diagrama Biológico | `diagrama_biologico.png` | N/A | 

### Aspectos a discutir

* Efecto del incremento de $R$ sobre la amplitud y la velocidad de deformación en la atrofia muscular.

* Capacidad del controlador PID para compensar las alteraciones viscoelásticas.

* Grado de coincidencia entre las simulaciones de MATLAB, Simscape Electrical y Python.

[Volver al contenido](#contenido)

## Flujo de trabajo

```
flowchart TD
    A[Modelo mecanico muscular Khoo] --> B[Analogia fuerza-voltaje]
    B --> C[Ecuaciones diferenciales y circuito análogo]
    C --> D[Obtencion de G_control y G_caso]
    D --> E[Analisis de polos y estabilidad]
    C --> F[Simulink: Ecuaciones]
    D --> G[Simulink: Transfer Fcn]
    B --> H[Simscape Electrical: Circuito]
    F --> I[Respuesta en lazo abierto]
    G --> I
    H --> I
    D --> J[Esquema de control con referencia x_control]
    J --> K[Sintonizacion PID mediante Tune]
    K --> L[Respuesta en lazo cerrado x_PID]
    D --> M[Python: Script independiente]
    K --> M
    I --> N[Verificacion cruzada]
    L --> N
    M --> N
    N --> O[Diagrama biologico BioRender]
    O --> P[Documentacion en GitHub y CITATION.cff]

```

[Volver al contenido](#contenido)

## Actividades del estudiante

### Etapa 1. Análisis y formulación

* \[ \] Identificar la función de $F_0, F, R, C_s, C_p, x(t), x_1(t)$.

* \[ \] Construir el circuito equivalente mediante la analogía fuerza-voltaje.

* \[ \] Deducir las funciones de transferencia $G_{\text{control}}(s)$ y $G_{\text{caso}}(s)$.

* \[ \] Obtener y graficar los polos en el plano complejo.

### Etapa 2. Simulación computacional

* \[ \] Construir el modelo en Simulink (Bloques matemáticos y `Transfer Fcn`).

* \[ \] Implementar el circuito equivalente en Simscape Electrical.

* \[ \] Simular la respuesta al impulso en lazo abierto ($t \in [0, 10]\,\mathrm{s}$).

### Etapa 3. Control y verificación *in silico*

* \[ \] Configurar el diagrama de retroalimentación donde la referencia es $x_{\text{control}}(t)$.

* \[ \] Sintonizar las ganancias $k_P, k_I, k_D$ usando el afinador PID.

* \[ \] Exportar las respuestas en MATLAB mediante cuaderno `.mlx` a formato `.pdf`.

* \[ \] Reproducir el experimento y la lazo cerrado mediante Python en VS Code.

### Etapa 4. Integración y documentación

* \[ \] Elaborar el diagrama biológico comparativo.

* \[ \] Discutir los resultados e impactos del amortiguamiento en la atrofia muscular.

* \[ \] Subir todos los modelos, scripts, imágenes y el archivo `CITATION.cff` al repositorio.

[Volver al contenido](#contenido)

## Estructura del repositorio

```
.
├── README.md                      # Reporte y guía de reproducción
├── CITATION.cff                   # Archivo de citación del repositorio
├── Apellido_NoControl.mlx         # Cuaderno MATLAB de automatización
├── Apellido_NoControl.pdf         # Exportación del cuaderno MATLAB
├── modelo_muscular.slx            # Modelo Simulink/Simscape (Control, Caso, PID)
├── modelo_muscular.py            # Script en Python para verificación
├── diagrama_biologico.png         # Diagrama biológico (BioRender/equivalente)
├── impulse_matlab.pdf             # Resultados de simulación en MATLAB
├── impulse_python.pdf             # Resultados de simulación en Python
└── evidencias/                   # Gráficas y capturas adicionales

```

[Volver al contenido](#contenido)

## Requisitos

### Software

* **MATLAB** con Simulink, Simscape Electrical y Control System Toolbox.

* **Python 3.x** en Visual Studio Code (con `numpy`, `scipy`, `matplotlib`).

* **Git y GitHub** para el control de versiones.

[Volver al contenido](#contenido)

## Reproducibilidad y entregables

### Resultados mínimos esperados

1. Demostración analítica de la analogía electromecánica y funciones de transferencia.

2. Gráfica de polos y estabilidad.

3. Simulaciones en lazo abierto mostrando el impacto de la atrofia muscular.

4. Ganancias del controlador PID sintonizado ($k_P, k_I, k_D$) y gráfica de seguimiento de trayectoria en lazo cerrado.

5. Verificación de consistencia entre Simulink, Simscape y Python.

6. Diagrama biológico explicativo.

7. Repositorio completo con `README.md` formateado y `CITATION.cff`.

[Volver al contenido](#contenido)

## Referencias

\[1\] M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2nd ed., IEEE Press, 2018.

\[2\] F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.

\[3\] J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14th ed., Elsevier, 2021.

\[4\] K. Ogata, *Ingeniería de Control Moderna*, 5.ª ed., Pearson Educación, 2010.

\[5\] P. A. Valle, *Syllabus para Modelado de Sistemas Fisiológicos*, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, 2026. Disponible en: https://biomath.xyz/course/

## Uso académico

Este repositorio se utiliza como evidencia de la **Práctica 5.5: Modelado y control del sistema musculoesquelético**. Su finalidad es aplicar principios de modelado viscoelástico, analogías físicas y diseño de control *in silico* para la evaluación de condiciones fisiopatológicas.
