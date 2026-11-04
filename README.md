[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/open/github/v1?repo=moglyy/MSFP3)
# Práctica 3 — Modelado y control del sistema musculoesquelético

Esta práctica integra el modelado viscoelástico del tejido muscular, la analogía electromecánica fuerza-voltaje, el análisis matemático analítico mediante el principio de superposición, el análisis temporal y de estabilidad, la simulación computacional de la patología de atrofia muscular, el diseño y sintonización de un controlador PI *in silico*, y la verificación cruzada mediante MATLAB, Simulink, Simscape Electrical y Python.

El caso de estudio representa la **dinámica musculoesquelética** mediante el modelo viscoelástico simplificado descrito por M. C. Khoo. Se busca analizar y comparar la respuesta de la deformación/fuerza transmitida ante una excitación de fuerza $F(t)$ en un individuo sano (control) contra un paciente con atrofia muscular (caso), implementando un esquema de control por retroalimentación para reducir la discrepancia entre ambas respuestas.

**Docente:** Dr. Paul Antonio Valle Trujillo  
**Correo:** paul.valle@tectijuana.edu.mx  
**Departamento:** Ingeniería Eléctrica y Electrónica  
**Institución:** Tecnológico Nacional de México / Instituto Tecnológico de Tijuana  
**Asignatura:** Modelado de Sistemas Fisiológicos  
**Programa:** Ingeniería Biomédica  
**Estudiante:** Kiara Vianney Trinidad Diaz · 23212240 · L23212240@tijuana.tecnm.mx  
**Sede:** Blvd. Alberto Limón Padilla s/n, Tijuana, C. P. 22454, B. C., México.

---

<a id="contenido"></a>
## Contenido

 1. [Resumen](#resumen)
 2. [Competencia y objetivos](#objetivos)
 3. [Relación con el temario](#temario)
 4. [Modelo físico y analogía electromecánica](#analogia)
 5. [Formulación y análisis matemático detallado](#modelo)
 6. [Análisis temporal y de estabilidad](#analisis)
 7. [Condiciones de simulación y señales](#senales)
 8. [Implementación en MATLAB, Simulink y Simscape](#matlab)
 9. [Estrategia de control in silico y sintonización PI](#pid)
10. [Reproducción en Python](#python)
11. [Diagrama biológico y correspondencia fisiológica](#biologico)
12. [Resultados y verificación cruzada](#resultados)
13. [Flujo de trabajo](#flujo)
14. [Actividades del estudiante](#actividades)
15. [Estructura del repositorio](#estructura)
16. [Requisitos](#requisitos)
17. [Reproducibilidad y entregables](#reproducibilidad)
18. [Referencias](#referencias)

---

<a id="resumen"></a>
## Resumen

Esta práctica tiene como propósito **modelar, analizar, simular y controlar la respuesta dinámica del sistema musculoesquelético** ante excitaciones de fuerza. Se aborda la caracterización del tejido pasivo y activo empleando una combinación de compliancias ($C_s$ y $C_p$) y amortiguación viscosa ($R$). Mediante la **analogía fuerza-voltaje**, el sistema mecánico se transforma en un circuito eléctrico equivalente para analizar su comportamiento dinámico.

A partir del **principio de superposición**, se deducen analíticamente las componentes de respuesta parciales $F_{s1}(t)$ y $F_{s2}(t)$ debidas a la fuerza externa $F(t)$ y la fuerza activa interna $F_0(t) = \alpha F(t)$, obteniendo la función de transferencia global $G(s) = \frac{F_s(s)}{F(s)}$. Se evalúa la condición de un **individuo sano (control)** y se simula una condición de **atrofia muscular (caso)** caracterizada por un incremento drástico en el amortiguamiento viscoso ($R$). Se realiza el análisis de estabilidad y error en estado estacionario. Posteriormente, se implementa un **controlador PI *in silico*** tomando la respuesta del individuo sano como señal de referencia. Todos los modelos se validan en MATLAB, Simulink, Simscape Electrical y Python.

**Palabras clave:** Sistema musculoesquelético; Modelado matemático; Analogía fuerza-voltaje; Función de transferencia; Controlador PI; Atrofia muscular.

[Volver al contenido](#contenido)

---

<a id="objetivos"></a>
## Competencia y objetivos

### Competencia a desarrollar

Modelizar sistemas fisiológicos mediante analogías entre sistemas físicos, ecuaciones diferenciales y funciones de transferencia para analizar su comportamiento dinámico y diseñar estrategias de control mediante técnicas de control clásico. En particular, analizar el sistema musculoesquelético, construir su circuito eléctrico equivalente, comparar la respuesta de un individuo sano (control) con un caso simulado de atrofia muscular (caso) y diseñar una estrategia de control *in silico* orientada a reducir las diferencias entre ambas respuestas.

### Objetivo general

Obtener el modelo matemático explícito y circuito equivalente de un compartimento musculoesquelético mediante superposición, simular la respuesta dinámica de un individuo sano y de un paciente con atrofia muscular, y diseñar un sistema de control *in silico* mediante sintonización PI para reducir el error entre la respuesta del caso y la referencia del control.

### Objetivos específicos

* Identificar los componentes viscoelásticos ($F_0$, $F$, $R$, $C_s$, $C_p$) del modelo mecánico muscular.
* Formular la conversión al circuito eléctrico equivalente aplicando la analogía fuerza-voltaje.
* Aplicar el principio de superposición para apagar $F_0(t)$ y $F(t)$ secuencialmente y deducir la función de transferencia global $G(s) = \frac{F_s(s)}{F(s)}$.
* Calcular el error en estado estacionario $e_{ss}$ y los polos del sistema en lazo abierto.
* Implementar la simulación en Simulink (`sistema4.slx`) mediante ecuaciones diferenciales, función de transferencia y componentes físicos de Simscape Electrical.
* Diseñar la estrategia de control por retroalimentación negativa donde la referencia es la salida del individuo sano $x_{\text{control}}(t)$.
* Sintonizar las ganancias del controlador PI ($k_P$, $k_I$) mediante la herramienta *Tune*.
* Validar y comparar numéricamente los resultados utilizando cuadernos computacionales de MATLAB (`Trinidad23212240.mlx`) y scripts en Python (`Trinidad23212240.py`).

[Volver al contenido](#contenido)

---

<a id="temario"></a>
## Relación con el temario

La práctica integra contenidos pertenecientes a la asignatura **Modelado de Sistemas Fisiológicos (IBF-1020)**:

| Unidad | Área | Contenidos aplicados | 
| ----- | ----- | ----- | 
| **I** | Sistemas de control retroalimentados | Diagramas de bloques, señales de referencia, seguimiento de trayectoria y criterios de estabilidad | 
| **II** | Modelado matemático | Analogías electromecánicas (fuerza-voltaje), superposición de fuentes, ecuaciones integro-diferenciales y funciones de transferencia | 
| **III** | Controladores | Tipos de controladores (PI), sintonización *in silico* de ganancias y reducción del error | 

[Volver al contenido](#contenido)

---

<a id="analogia"></a>
## Modelo físico y analogía electromecánica

El modelo mecánico simplificado del compartimento muscular consta de una fuerza activa $F_0(t) = \alpha F(t)$ ($0 < \alpha < 1$), un elemento elástico en serie $C_s$, un amortiguador viscoso $R$ y un elemento elástico en paralelo $C_p$.

### Tabla de analogía fuerza-voltaje

| Sistema mecánico | Sistema eléctrico equivalente | 
| ----- | ----- | 
| Fuerza $F(t)$ | Voltaje $V(t)$ | 
| Velocidad $\frac{dx(t)}{dt}$ | Corriente $I(t)$ | 
| Desplazamiento $x(t)$ | Carga eléctrica $q(t)$ | 
| Amortiguador ($R$) | Resistencia ($R$) | 
| Compliancia mecánica ($C$) | Capacitancia ($C$) | 
| Masa ($M$) | Inductancia ($L$) | 

### Parámetros del modelo (Control vs. Caso)

Para la simulación *in silico*, el deterioro muscular por atrofia se representa mediante el incremento en el parámetro de amortiguamiento viscoso ($R$):

| Parámetro | Individuo sano (Control) | Paciente (Caso: Atrofia) | Unidad eléctrica | 
| ----- | ----- | ----- | ----- | 
| Fuerza aplicada $F(t)$ | $1$ | $1$ | $\mathrm{V}$ | 
| Fuerza activa $\alpha$ | $0.25$ | $0.25$ | Adimensional | 
| Compliancia serie $C_s$ | $10\times10^{-6}$ | $10\times10^{-6}$ | $\mathrm{F}$ ($10\,\mu\mathrm{F}$) | 
| Compliancia paralelo $C_p$ | $100\times10^{-6}$ | $100\times10^{-6}$ | $\mathrm{F}$ ($100\,\mu\mathrm{F}$) | 
| Amortiguamiento $R$ | $100$ | $10\,000$ | $\Omega$ ($10\,\mathrm{k}\Omega$) | 

[Volver al contenido](#contenido)

---

<a id="modelo"></a>
## Formulación y análisis matemático detallado

El desarrollo matemático explícito de esta práctica se encuentra resguardado en el documento manuscrito [`AnalisisMatematicoP3.pdf`](AnalisisMatematicoP3.pdf). De acuerdo con el análisis del sistema eléctrico equivalente y considerando el desplazamiento total $x(t) = x_1(t) + x_2(t)$, se aplica el **Principio de Superposición** para encontrar la fuerza total transmitida $F_s(t) = F_{s1}(t) + F_{s2}(t)$.

### 1. Circuito 1: Apagando $F_0(t)$

Al apagar la fuente interna $F_0(t) = 0$, se analiza la contribución de $F(t)$ al nodo $F_{s1}(t)$:

$$
x(t) = C_p \frac{d F_{s1}(t)}{dt}, \quad x_1(t) = C_s \frac{d[F(t) - F_{s1}(t)]}{dt}, \quad x_2(t) = \frac{F(t) - F_{s1}(t)}{R}
$$

Igualando las relaciones de velocidad y desplazamiento en el nodo:

$$
C_p \frac{d F_{s1}(t)}{dt} = C_s \frac{d[F(t) - F_{s1}(t)]}{dt} + \frac{F(t) - F_{s1}(t)}{R}
$$

Aplicando la transformada de Laplace con condiciones iniciales nulas:

$$
C_p s F_{s1}(s) = C_s s [F(s) - F_{s1}(s)] + \frac{F(s) - F_{s1}(s)}{R}
$$

Reagrupando términos para $F_{s1}(s)$ y $F(s)$:

$$
C_p s F_{s1}(s) + C_s s F_{s1}(s) + \frac{F_{s1}(s)}{R} = C_s s F(s) + \frac{F(s)}{R}
$$

$$
\left( C_p s + C_s s + \frac{1}{R} \right) F_{s1}(s) = \left( C_s s + \frac{1}{R} \right) F(s)
$$

Multiplicando por $R$ ambos lados:

$$
\boxed{F_{s1}(s) = \frac{C_s R s + 1}{R(C_p + C_s)s + 1} F(s)}
$$

### 2. Circuito 2: Apagando $F(t)$

Al apagar la fuente externa $F(t) = 0$, se analiza la contribución de la fuente activa $F_0(t) = \alpha F(t)$:

$$
-\alpha F(t) = R x(t) + \frac{1}{C_p + C_s} \int x(t)\,dt
$$

La fuerza producida en este modo es $F_{s2}(t) = \frac{1}{C_p + C_s} \int x(t)\,dt$. En el dominio de Laplace:

$$
-\alpha F(s) = R X(s) + \frac{X(s)}{(C_p + C_s)s}, \qquad F_{s2}(s) = \frac{X(s)}{(C_p + C_s)s}
$$

Despejando $X(s)$ en función de $F(s)$:

$$
F(s) = -\frac{1}{\alpha} \left( R + \frac{1}{(C_p + C_s)s} \right) X(s) = -\frac{1}{\alpha} \frac{R(C_p + C_s)s + 1}{(C_p + C_s)s} X(s)
$$

Sustituyendo $X(s)$ en la expresión de $F_{s2}(s)$:

$$
\frac{F_{s2}(s)}{F(s)} = \frac{\frac{X(s)}{(C_p + C_s)s}}{-\frac{1}{\alpha}\frac{R(C_p + C_s)s + 1}{(C_p + C_s)s} X(s)} = -\frac{\alpha}{R(C_p + C_s)s + 1}
$$

$$
\boxed{F_{s2}(s) = -\frac{\alpha}{R(C_p + C_s)s + 1} F(s)}
$$

### 3. Función de transferencia global

Sumando las componentes parciales $F_s(s) = F_{s1}(s) + F_{s2}(s)$:

$$
F_s(s) = \frac{C_s R s + 1}{R(C_p + C_s)s + 1} F(s) - \frac{\alpha}{R(C_p + C_s)s + 1} F(s)
$$

$$
\boxed{G(s) = \frac{F_s(s)}{F(s)} = \frac{C_s R s + 1 - \alpha}{R(C_p + C_s)s + 1}}
$$

[Volver al contenido](#contenido)

---

<a id="analisis"></a>
## Análisis temporal y de estabilidad

### 1. Error en estado estacionario

Aplicando el Teorema del Valor Final para una entrada escalón unitario $F(s) = \frac{1}{s}$:

$$
e_{ss} = \lim_{s \to 0} s F(s) \left[ 1 - \frac{F_s(s)}{F(s)} \right] = \lim_{s \to 0} s \left(\frac{1}{s}\right) \left[ 1 - \frac{C_s R s + 1 - \alpha}{R(C_p + C_s)s + 1} \right]
$$

$$
e_{ss} = 1 - \frac{1 - \alpha}{1} = 1 - 1 + \alpha = \boxed{\alpha}
$$

Dado que $\alpha = 0.25$, existe un error en estado estacionario permanente en lazo abierto de $0.25$ ($25\%$).

### 2. Estabilidad en lazo abierto

El polo del sistema se determina igualando el denominador a cero:

$$
R(C_p + C_s)s + 1 = 0 \implies \boxed{\lambda = -\frac{1}{R(C_p + C_s)}}
$$

* **Sujeto Sano (Control):** $R = 100\,\Omega \implies \lambda_{\text{control}} = -\frac{1}{100(110\times10^{-6})} \approx -90.91\,\mathrm{s}^{-1}$ (Respuesta rápida).
* **Paciente (Caso Atrofia):** $R = 10\,000\,\Omega \implies \lambda_{\text{caso}} = -\frac{1}{10000(110\times10^{-6})} \approx -0.909\,\mathrm{s}^{-1}$ (Respuesta sumamente lenta).

Como $\lambda < 0$ en ambos casos, **el sistema es estrictamente estable**, pero la atrofia muscular retarda drásticamente la velocidad de respuesta del tejido.

[Volver al contenido](#contenido)

---

<a id="senales"></a>
## Condiciones de simulación y señales

Las simulaciones se realizan nominalmente en un intervalo de tiempo $t \in [0, 10]\,\mathrm{s}$.

| Señal de entrada | Propósito en el experimento *in silico* | 
| ----- | ----- | 
| **Impulso unitario** | Evaluar la respuesta natural del sistema musculoesquelético y comparar la dinámica del sujeto sano vs atrofia. | 

[Volver al contenido](#contenido)

---

<a id="matlab"></a>
## Implementación en MATLAB, Simulink y Simscape

Se construyen tres representaciones equivalentes dentro del modelo [`sistema4.slx`](sistema4.slx):

1. **Ecuaciones diferenciales:** Bloques matemáticos e integradores en Simulink.
2. **Función de transferencia:** Bloque `Transfer Fcn` configurado con $G(s) = \frac{C_s R s + 1 - \alpha}{R(C_p + C_s)s + 1}$.
3. **Circuito físico equivalente:** Ensamble de componentes pasivos mediante Simscape Electrical.

Desde el cuaderno de MATLAB [`Trinidad23212240.mlx`](Trinidad23212240.mlx) (exportado a PDF como [`Trinidad23212240.pdf`](Trinidad23212240.pdf)) se automatiza la simulación y extracción hacia el *Workspace* de las variables:

* $x_{\text{control}}(t)$: Respuesta del sujeto sano.
* $x_{\text{caso}}(t)$: Respuesta en lazo abierto del paciente con atrofia.
* $x_{\text{PI}}(t)$: Respuesta del paciente con tratamiento de control PI.

[Volver al contenido](#contenido)

---

<a id="pid"></a>
## Estrategia de control in silico y sintonización PI

### 1. Esquema de retroalimentación

Se utiliza la respuesta del individuo sano $x_{\text{control}}(t)$ como el perfil de movimiento deseado (referencia). La señal de error que alimenta al controlador es:

$$
e(t) = x_{\text{control}}(t) - x_{\text{caso}}(t)
$$

El controlador PI procesa esta diferencia y aplica la acción de control $u(t)$ sobre el modelo del paciente (Tratamiento):

$$
u(t) = k_P e(t) + k_I \int_0^t e(\tau)\,d\tau
$$

<br>
<div align="center">
  <table style="border: none; background-color: #f8f9fa; border-radius: 12px; padding: 15px; font-family: sans-serif;">
    <tr>
      <td align="center" style="padding: 10px;"><b>F(t)</b><br>⬇️</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td align="center" style="background-color: #0ea5e9; color: white; padding: 15px 25px; border-radius: 8px;"><b>Sujeto Sano</b><br><small>(Control)</small></td>
      <td align="center" style="padding: 0 10px; color: #0ea5e9; font-weight: bold;">x_control ➔</td>
      <td align="center" style="background-color: #f59e0b; color: white; padding: 15px; border-radius: 50%; font-size: 18px;"><b>Σ</b></td>
      <td align="center" style="padding: 0 10px; color: #10b981; font-weight: bold;">e(t) ➔</td>
      <td align="center" style="background-color: #10b981; color: white; padding: 15px 25px; border-radius: 8px;"><b>Controlador PI</b><br><small>(Protocolo)</small></td>
      <td align="center" style="padding: 0 10px; color: #ef4444; font-weight: bold;">u(t) ➔</td>
      <td align="center" style="background-color: #ef4444; color: white; padding: 15px 25px; border-radius: 8px;"><b>Paciente</b><br><small>(Tratamiento)</small></td>
      <td align="center" style="padding: 0 10px; color: #ef4444; font-weight: bold;">➔ x_PI</td>
    </tr>
  </table>
  <p style="font-size: 12px; color: #6b7280; margin-top: 8px;"><i>* La señal x_PI(t) se retroalimenta negativamente al nodo sumador Σ.</i></p>
</div>
<br>

### 2. Sintonización mediante *Tune*

Utilizando el afinador del bloque `PID Controller` de Simulink (configurado en modo PI), se sintonizan $k_P$ y $k_I$ buscando estabilizar el sistema y hacer que la respuesta del paciente con atrofia $x_{\text{PI}}(t)$ rastree de forma exacta la respuesta de referencia $x_{\text{control}}(t)$. La evidencia del panel de sintonización y las ganancias elegidas se incluye en [`gananciasPI.png`](gananciasPI.png).

[Volver al contenido](#contenido)

---

<a id="python"></a>
## Reproducción en Python

Mediante Visual Studio Code y el script [`Trinidad23212240.py`](Trinidad23212240.py), se implementa la simulación independiente con librerías científicas (`scipy.signal`, `matplotlib`, `numpy`) para:

1. Definir las funciones de transferencia de control y caso.
2. Calcular la respuesta al impulso en lazo abierto.
3. Construir el sistema en lazo cerrado con las ganancias $k_P, k_I$.
4. Graficar y exportar las respuestas en la figura [`Musculoesqueletico_python.pdf`](Musculoesqueletico_python.pdf).

[Volver al contenido](#contenido)

---

<a id="biologico"></a>
## Diagrama biológico y correspondencia fisiológica

El esquema representativo del modelo muscular y la equivalencia biológica se presenta en [`Musculoesqueletico.png`](Musculoesqueletico.png).

| Elemento mecánico | Equivalente eléctrico | Estructura biológica asociada | 
| ----- | ----- | ----- | 
| **Fuerza activa** $F_0(t)$ | Fuente de voltaje | Contracción activa por acoplamiento miosina-actina | 
| **Compliancia serie** $C_s$ | Capacitancia $C_s$ | Tendón y elementos elásticos en serie | 
| **Compliancia paralelo** $C_p$ | Capacitancia $C_p$ | Sarcolema y tejido conectivo pasivo (Epimisio/Perimisio) | 
| **Amortiguamiento** $R$ | Resistencia $R$ | Viscosidad sarcoplásmica y fricción tisular interna | 

[Volver al contenido](#contenido)

---

<a id="resultados"></a>
## Resultados y verificación cruzada

Se comparan las curvas temporales para las tres condiciones:

* **Línea Azul:** Individuo Sano (Control de Referencia).
* **Línea Roja:** Paciente con Atrofia (Caso en Lazo Abierto).
* **Línea Verde:** Paciente Tratado (Caso con Controlador PI).

### Archivos de resultados del repositorio

| Simulación / Evidencia | Archivo en el repositorio | 
| ----- | ----- | 
| Análisis analítico manuscrito | [`AnalisisMatematicoP3.pdf`](AnalisisMatematicoP3.pdf) | 
| Captura de sintonización de ganancias PI | [`gananciasPI.png`](gananciasPI.png) | 
| Diagrama biológico de correspondencia | [`Musculoesqueletico.png`](Musculoesqueletico.png) | 
| Respuestas al impulso en MATLAB | [`Control y caso.matlab.pdf`](Control%20y%20caso.matlab.pdf) | 
| Respuestas al impulso en Python | [`Musculoesqueletico_python.pdf`](Musculoesqueletico_python.pdf) | 

[Volver al contenido](#contenido)

---

<a id="flujo"></a>
## Flujo de trabajo

<br>
<div align="center">
  <table style="border: none; font-family: sans-serif;">
    <tr>
      <td align="center" style="background-color: #3b82f6; color: white; padding: 12px 20px; border-radius: 8px; font-weight: bold;">Modelo mecánico muscular Khoo</td>
      <td align="center" style="padding: 0 15px; font-size: 20px;">➡️</td>
      <td align="center" style="background-color: #8b5cf6; color: white; padding: 12px 20px; border-radius: 8px; font-weight: bold;">Analogía fuerza-voltaje</td>
    </tr>
  </table>
  
  <table style="border: none; margin-top: 15px; font-family: sans-serif;">
    <tr>
      <td align="center" style="background-color: #10b981; color: white; padding: 12px 20px; border-radius: 8px; font-weight: bold; width: 200px;">Ecuaciones Diferenciales<br>y Circuito Análogo</td>
      <td align="center" style="padding: 0 20px; font-style: italic; color: #4b5563;">⬅️ Superposición ➡️</td>
      <td align="center" style="background-color: #10b981; color: white; padding: 12px 20px; border-radius: 8px; font-weight: bold; width: 200px;">Función de Transferencia<br>G_control y G_caso</td>
    </tr>
  </table>

  <table style="border: none; margin-top: 15px; font-family: sans-serif;">
    <tr>
      <td align="center" style="padding: 5px 0; font-size: 20px;">⬇️</td>
    </tr>
    <tr>
      <td align="center" style="background-color: #f59e0b; color: white; padding: 15px; border-radius: 8px; font-weight: bold; width: 450px; font-size: 16px;">Implementación MATLAB / Simulink / Simscape<br><small style="font-weight: normal;">(Análisis de polos, estabilidad y lazo abierto)</small></td>
    </tr>
    <tr>
      <td align="center" style="padding: 5px 0; font-size: 20px;">⬇️</td>
    </tr>
    <tr>
      <td align="center" style="background-color: #ef4444; color: white; padding: 15px; border-radius: 8px; font-weight: bold; width: 450px; font-size: 16px;">Control PI In Silico<br><small style="font-weight: normal;">(Esquema con referencia x_control y sintonización Tune)</small></td>
    </tr>
    <tr>
      <td align="center" style="padding: 5px 0; font-size: 20px;">⬇️</td>
    </tr>
    <tr>
      <td align="center" style="background-color: #475569; color: white; padding: 15px; border-radius: 8px; font-weight: bold; width: 450px; font-size: 16px;">Verificación cruzada en Python<br><small style="font-weight: normal;">(Comparativa de respuestas, diagramas y GitHub)</small></td>
    </tr>
  </table>
</div>
<br>

[Volver al contenido](#contenido)

---

<a id="actividades"></a>
## Actividades del estudiante

### Etapa 1. Análisis analítico

* \[x\] Identificar la función de $F_0, F, R, C_s, C_p, x(t)$.
* \[x\] Construir el circuito equivalente mediante analogía fuerza-voltaje.
* \[x\] Derivar $G(s)$ por superposición apagando $F_0(t)$ y $F(t)$ ([`AnalisisMatematicoP3.pdf`](AnalisisMatematicoP3.pdf)).
* \[x\] Demostrar $e_{ss} = \alpha$ y hallar los polos $\lambda$.

### Etapa 2. Simulación computacional

* \[x\] Construir el modelo en Simulink ([`sistema4.slx`](sistema4.slx)).
* \[x\] Implementar el circuito equivalente en Simscape Electrical.
* \[x\] Obtener la respuesta al impulso en lazo abierto ($t \in [0, 10]\,\mathrm{s}$).

### Etapa 3. Control y verificación in silico

* \[x\] Implementar retroalimentación con referencia $x_{\text{control}}(t)$.
* \[x\] Sintonizar $k_P, k_I$ con la herramienta *Tune* ([`gananciasPI.png`](gananciasPI.png)).
* \[x\] Exportar la figura de MATLAB ([`Control y caso.matlab.pdf`](Control%20y%20caso.matlab.pdf)) y ejecutar el script en Python ([`Trinidad23212240.py`](Trinidad23212240.py)).

### Etapa 4. Documentación

* \[x\] Incluir el diagrama biológico ([`Musculoesqueletico.png`](Musculoesqueletico.png)).
* \[x\] Publicar el repositorio con sus fuentes y archivo [`CITATION.cff`](CITATION.cff).

[Volver al contenido](#contenido)

---

<a id="estructura"></a>
## Estructura del repositorio

Archivos contenidos en la raíz de este repositorio:

```
.
├── AnalisisMatematicoP3.pdf      # Demostración analítica manuscrita por superposición
├── CITATION.cff                  # Fichero de citación académica del repositorio
├── Control y caso.matlab.pdf     # Exportación gráfica de simulación en MATLAB
├── LICENSE                       # Licencia BSD 2-Clause del repositorio
├── Musculoesqueletico.png        # Diagrama biológico de correspondencia
├── Musculoesqueletico_python.pdf # Exportación gráfica de simulación en Python
├── README.md                     # Reporte académico principal de la práctica
├── Trinidad23212240.mlx          # Cuaderno computacional Live Script de MATLAB
├── Trinidad23212240.pdf          # Exportación en PDF del cuaderno Live Script
├── Trinidad23212240.py           # Script ejecutable en Python (VS Code)
├── gananciasPI.png               # Evidencia de la sintonización del controlador PI
└── sistema4.slx                  # Modelo de Simulink / Simscape Electrical

```

[Volver al contenido](#contenido)

---

<a id="requisitos"></a>
## Requisitos

### Software

* **MATLAB** con Simulink, Simscape Electrical y Control System Toolbox.
* **Python 3.x** en Visual Studio Code (`numpy`, `scipy`, `matplotlib`).
* **Git y GitHub** para la gestión y presentación del repositorio.

[Volver al contenido](#contenido)

---

<a id="reproducibilidad"></a>
## Reproducibilidad y entregables

1. Demostración analítica por superposición de $G(s) = \frac{C_s R s + 1 - \alpha}{R(C_p + C_s)s + 1}$ registrada en [`AnalisisMatematicoP3.pdf`](AnalisisMatematicoP3.pdf).
2. Cálculo de polos y demostración de $e_{ss} = \alpha$.
3. Modelo de Simulink [`sistema4.slx`](sistema4.slx) con representaciones en lazo abierto y cerrado.
4. Ganancias PI sintonizadas registradas en [`gananciasPI.png`](gananciasPI.png).
5. Cuaderno computacional [`Trinidad23212240.mlx`](Trinidad23212240.mlx) y ejecutable de Python [`Trinidad23212240.py`](Trinidad23212240.py).
6. Figuras de resultados en PDF: [`Control y caso.matlab.pdf`](Control%20y%20caso.matlab.pdf) y [`Musculoesqueletico_python.pdf`](Musculoesqueletico_python.pdf).
7. Diagrama biológico [`Musculoesqueletico.png`](Musculoesqueletico.png) y archivo [`CITATION.cff`](CITATION.cff).

[Volver al contenido](#contenido)

---

<a id="referencias"></a>
## Referencias

\[1\] M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2nd ed., IEEE Press, 2018.

\[2\] F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.

\[3\] J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14th ed., Elsevier, 2021.

\[4\] K. Ogata, *Ingeniería de Control Moderna*, 5.ª ed., Pearson Educación, 2010.

\[5\] P. A. Valle, *Syllabus para Modelado de Sistemas Fisiológicos*, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, 2026. Disponible en: https://biomath.xyz/course/

## Uso académico

Este repositorio se utiliza como evidencia de la **Práctica 5.5: Modelado y control del sistema musculoesquelético**. Su finalidad es aplicar principios de modelado viscoelástico, analogías físicas y diseño de control *in silico* para la evaluación de condiciones fisiopatológicas.
