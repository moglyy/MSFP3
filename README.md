[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/open/github/v1?repo=moglyy/MSFP3)
# Práctica 3: Modelado y control del sistema musculoesquelético

## Información de la estudiante

Kiara Vianney Trinidad Diaz [23212240]; L23212240@tijuana.tecnm.mx

Modelado de Sistemas Fisiológicos

Ingeniería Biomédica

## Docente

Dr. Paul Antonio Valle Trujillo; paul.valle@tectijuana.edu.mx

Departamento de Ingeniería Eléctrica y Electrónica, Tecnológico Nacional de México/IT Tijuana, Blvd. Alberto Limón Padilla s/n, Tijuana, C.P. 22454, B.C., México.

## Descripción de la asignatura

El modelizado de sistemas fisiológicos es una herramienta importante en Ingeniería Biomédica, permite comprender el funcionamiento del cuerpo humano, así como diseñar y evaluar terapias y dispositivos médicos; se define como el proceso de formular modelos matemáticos o computacionales que representan el comportamiento y la interacción de los sistemas biológicos y fisiológicos. Esta asignatura pretende aportar al perfil del Ingeniero Biomédico la capacidad de realizar investigación científica en el área de Biología de Sistemas con la finalidad de dirigir y participar en equipos de trabajo interdisciplinarios en contextos nacionales e internacionales, así como de proporcionar soluciones informáticas para resolver problemas en el campo de la Ingeniería Biomédica con ética profesional; lo anterior al proporcionar al estudiante bases sólidas para modelizar sistemas y diseñar controladores para la solución de problemas en las áreas de atención médica y del sector industrial médico. La construcción de analogías entre circuitos eléctricos y sistemas fisiológicos para la formulación de modelos matemáticos y el diseño de controladores mediante la experimentación in silico brindan herramientas de gran aplicación en el quehacer profesional del Ingeniero Biomédico.

La asignatura de Modelado de Sistemas Fisiológicos forma parte del plan de estudios de la carrera en Ingeniería Biomédica con la siguiente competencia general del curso: Utiliza las propiedades de los circuitos RLC para describir la dinámica de sistemas fisiológicos, obtener modelos matemáticos y aplicar el control clásico, esto con el objetivo de integrar los principios de la Ingeniería de Control, la Electrónica Analógica y las Ciencias de la Computación con la Anatomía y Fisiología del cuerpo humano para proporcionar descripciones cuantitativas y cualitativas de sistemas fisiológicos complejos con el objetivo de modelizar, analizar, controlar, ilustrar y predecir su dinámica tanto en el corto como en el largo plazo.

## Descripción de la práctica

En esta práctica se realizó el control del sistema musculoesquelético mediante un modelo mecánico simplificado y su representación eléctrica equivalente utilizando la analogía fuerza-voltaje. Se formularon las ecuaciones matemáticas y funciones de transferencia para analizar la dinámica del sistema, comparando las condiciones de un individuo sano (control) y un caso simulado de atrofia muscular. Asimismo, se analizaron la estabilidad y el error en estado estacionario, implementando los modelos en MATLAB/Simulink. Finalmente, se diseñó un controlador PID para reducir las diferencias entre ambas respuestas y se verificaron los resultados mediante Python, utilizando Visual Studio Code.

## Objetivos

1. Identificar los elementos del modelo mecánico simplificado del sistema musculoesquelético y relacionarlos con sus propiedades fisiológicas.
2. Construir el circuito eléctrico equivalente mediante la analogía fuerza-voltaje, estableciendo las correspondencias entre los elementos mecánicos y eléctricos.
3. Formular las ecuaciones matemáticas del sistema y obtener las funciones de transferencia correspondientes a las condiciones de control y caso.
4. Analizar la estabilidad de los modelos mediante la localización de sus polos en el plano complejo y calcular el error en estado estacionario.
5. Implementar y simular las representaciones matemáticas y eléctricas del sistema en MATLAB/Simulink y Simscape Electrical, comparando sus respuestas dinámicas.
6. Diseñar y sintonizar un controlador PID mediante la herramienta Tune de Simulink, utilizando la respuesta del individuo sano como referencia para el caso simulado de atrofia muscular.
7. Analizar la respuesta en lazo cerrado y verificar la reducción del error de seguimiento mediante la comparación de las respuestas del control, caso y sistema controlado.
8. Implementar las funciones de transferencia y el sistema de control en Python mediante Visual Studio Code, realizando una verificación cruzada de los resultados obtenidos en MATLAB/Simulink.
9. Elaborar un diagrama biológico mediante BioRender que relacione los elementos del modelo mecánico y eléctrico con las estructuras del sistema musculoesquelético.

Palabras clave: Sistema musculoesquelético; Modelado matemático; Analogía fuerza-voltaje; Función de transferencia; Controlador PID; Atrofia muscular.

## Lista de archivos incluidos en el repositorio

1. Cuaderno computacional de MATLAB [.mlx].
2. Modelos de Simulink [.slx].
3. Archivos de Visual Studio Code y código fuente de Python [.py].
4. Imagen con los parámetros y ganancias del controlador PID.
5. Imágenes de las simulaciones y respuestas del sistema [.pdf y .png].
6. Análisis matemático: Ecuaciones del sistema, funciones de transferencia, análisis de estabilidad y error en estado estacionario.
7. Diagrama biológico elaborado en BioRender [.png].



## Referencias

\[1] P. A. Valle, Syllabus para Modelado de Sistemas Fisiológicos, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, Tijuana, B.C., México, 2025. Permalink: https://biomath.xyz/course/

\[2] M. C. Khoo, Physiological Control Systems: Analysis, Simulation, and Estimation, 2nd ed. Piscataway, New Jersey, USA: IEEE Press, 2018.

\[3] F. C. Hoppensteadt and C. S. Peskin, Modeling and Simulation in Medicine and the Life Sciences. Springer.

\[4] J. E. Hall, Guyton and Hall Textbook of Medical Physiology, 14th ed. Elsevier, 2021.

