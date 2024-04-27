# TA2_CC82_TopicosCS

 <h2 align="center">Universidad Peruana de Ciencias Aplicadas</h2>
<h2 align="center">Tópicos en Ciencias de la Computación - CC58</h2>
 
<h3 align="center"> TB2 </h3>
 
<h3 align="center"> Sección</h3>
<h3 align="center"> Profesor: Luis Martín Canaval Sanchez</h3>
<h3 align="center"> Alumnos</h3>
 <ul>
   <li align="center">Camargo Ramírez, Enzo Fabricio (U202010122)</li>
   <li align="center">Costa Mondragón, Paulo Sergio (U201912086)</li>
   <li align="center">Caballero Lara, Eduardo Roman (U202019644)</li>
 </ul>
 
 
 <h3 align="center">CICLO 2024-1</h3>

## Definición del problema

<p align="justify">
 El problema que enfrentamos surge de la necesidad de gestionar eficazmente los recursos hospitalarios durante una pandemia que está en aumento. El objetivo principal es desarrollar un algoritmo capaz de asignar a las personas infectadas con hospitales, teniendo en cuenta múltiples criterios como la gravedad de la enfermedad, la edad del paciente, la ubicación geográfica, la capacidad del hospital y la disponibilidad de equipo médico necesario.
<p align="justify">Para simplificar el problema, consideraremos inicialmente cuatro parámetros fundamentales:</p>
 <ol>
  <li><b>Ubicación de las personas infectadas</b>: Este parámetro ayuda a determinar la proximidad de los pacientes a los diferentes hospitales.</li>
  <li><b>Gravedad de la infección de las personas</b>: Es crucial para priorizar el tratamiento a quienes tienen condiciones más graves.</li>
  <li><b>Ubicación de los hospitales</b>: Fundamental para asignar a los pacientes al hospital más cercano y adecuado.</li>
  <li><b>Número de camas disponibles en cada hospital</b>: Esencial para asegurar que hay capacidad suficiente para atender a los pacientes.</li>
 </ol>
</p>

## Marco Teórico

<p align="justify">
 Este problema se modelará como un problema de satisfacción de restricciones, donde se definen variables que representan si una cama específica en un hospital está ocupada por un paciente particular. Las variables se definen de la siguiente manera:
  <ul>
  <li> $xijk ∈ {0, 1}$, donde $(i, j, k)$ representa los índices correspondientes a hospitales, camas en ese hospital y pacientes, respectivamente. Si la cama $j$ del hospital $i$ está ocupada por el paciente $k$, entonces $xijk = 1$.</li>
  </ul>
 
 
 <p><b>Restricciones Duras:</b></p>
 
 <uL>
  <li>Cada cama puede estar ocupada por a lo máximo una persona.</li>
  <li>A cada persona se le debe asignar a lo máximo una cama.</li>
 </uL>
 
 <p><b>Restricciones Suaves:</b></p>
 <ul>
  <li>Se debe intentar ubicar a cada persona infectada en una cama.</li>
  <li>Se debe priorizar que cada persona sea tratada en el hospital más cercano.</li>
  <li>Las personas con condiciones más graves deben ser priorizadas cuando no hay suficientes camas disponibles.</li>
 </ul>
 
 <p>
  El reto está en diseñar un algoritmo que utilice estas variables y restricciones para optimizar la asignación de recursos y asegurar que los pacientes reciban el tratamiento adecuado de la manera más eficiente posible. Utilizaremos Google Optimization Tools (OR-Tools), un conjunto de software de código abierto para la optimización combinatoria, para implementar y resolver este modelo.
 </p>
 </p>

## Modelo Formal del Problema de Satisfacción de Restricciones (CSP)
<p align="justify">

Este problema se presenta en el contexto de la asignación de pacientes infectados a camas de hospital durante una pandemia. El CSP se define formalmente por:

### Variables `X`
Las variables representan las decisiones que deben tomarse:

- $X = \{x_{ijk} | i \in H, j \in Bi, k \in P\}$
  - Donde $H$ es el conjunto de hospitales, $Bi$ es el conjunto de camas en el hospital $i$, y $P$ es el conjunto de pacientes.

### Dominios `D`
El dominio de cada variable especifica los posibles valores que la variable puede tomar:

- $D(x_{ijk}) = \{0, 1\}$
  - $1$ significa que la cama $j$ en el hospital $i$ está asignada al paciente $k$; $0$ significa que no lo está.

### Restricciones `C`
Las restricciones pueden dividirse en restricciones duras y suaves:

#### Restricciones Duras
- **Unicidad de la Cama:** Cada cama solo puede tener un paciente.
  - $\forall i \in H, \forall j \in Bi, \sum_{k \in P} x_{ijk} \leq 1$
- **Unicidad del Paciente:** Cada paciente solo puede ser asignado a una cama.
  - $\forall k \in P, \sum_{i \in H} \sum_{j \in Bi} x_{ijk} \leq 1$

#### Restricciones Suaves
- **Asignar Pacientes a Camas:** Idealmente, cada paciente debería ser asignado a una cama.
  - $\forall k \in P, \sum_{i \in H} \sum_{j \in Bi} x_{ijk} = 1$
- **Proximidad:** Los pacientes deberían ser asignados al hospital más cercano.
- **Prioridad por Gravedad:** Los pacientes en condiciones graves deberían ser priorizados en la asignación de camas cuando las camas son escasas.

El modelo formal del CSP para este problema, `{X, D, C}`, captura la complejidad de gestionar los recursos hospitalarios durante una pandemia. Este modelo asegura que las soluciones generadas por algoritmos como Google OR-Tools sean tanto factibles como optimizadas según las restricciones definidas.

</p>

### Code
<p align="justify">
 Código desarrollado por el grupo tomando en cuenta el desarrollo del modelo formal:

 
 ```python
from ortools.sat.python import cp_model

# Modelo
model = cp_model.CpModel()

# Convertir DataFrame a diccionario para un acceso más rápido
hospital_beds = {(i, j): int(hospital['ZNC_MONT_CAM_TOTAL'])
                 for i, hospital in simplified_dataset1.iterrows()
                 for j in range(int(hospital['ZNC_MONT_CAM_TOTAL']))}

# Variables
x = {}  # x[(i, j, k)] = 1 si el paciente k está asignado a la cama j en el hospital i
for (i, j), beds in hospital_beds.items():
    for k in simplified_dataset2.index:
        x[(i, j, k)] = model.NewBoolVar(f'x[{i},{j},{k}]')

# Restricciones
# 1. Cada paciente es asignado a lo más a una cama en un hospital
for k in simplified_dataset2.index:
    model.Add(sum(x[(i, j, k)] for (i, j) in hospital_beds) <= 1)

# 2. Cada cama en un hospital es asignada a lo más a un paciente
for (i, j) in hospital_beds:
    model.Add(sum(x[(i, j, k)] for k in simplified_dataset2.index) <= 1)

# Objetivo: Maximizar el número de pacientes asignados a camas
model.Maximize(sum(x[(i, j, k)]
                   for (i, j) in hospital_beds for k in simplified_dataset2.index))

# Resolver el modelo
solver = cp_model.CpSolver()
solver.parameters.max_time_in_seconds = 300  # Límite de tiempo de 5 minutos
solver.parameters.log_search_progress = True  # Activar log de progreso

status = solver.Solve(model)

if status == cp_model.OPTIMAL:
    print("Solución óptima encontrada.")
elif status == cp_model.FEASIBLE:
    print("Se encontró una solución factible.")
else:
    print("No se pudo encontrar una solución óptima o factible en el tiempo dado.")
# Verificar el estado de la solución y mostrar las asignaciones
if status == cp_model.OPTIMAL or status == cp_model.FEASIBLE:
    print("Pacientes asignados a hospitales:")
    for (i, j) in hospital_beds:
        for k in simplified_dataset2.index:
            if solver.Value(x[(i, j, k)]) == 1:
                print(f'Hospital: {simplified_dataset1.loc[i, "NOMBRE"]}, Cama: {j}, Paciente ID: {k}')
else:
    print("No se encontró solución.")


```
</p>

## Conclusiones

### Promising Results

### Why use Constraint Programming for this kind of problems?

<p align="justify">
 Usar Constraing Programming en el tipo de problema de asignación de pacientes a hospitales durante una pandemia es relevante por varias razones, y aunque las técnicas de aprendizaje automático (Machine Learning, ML) también podrían abordar este tipo de problemas, hay diferencias clave en cómo cada enfoque puede ser aplicado y por qué CP puede ser más adecuado en ciertos contextos:
</p>

#### Razones para user Constraint Programming:
<p align="justify">
<ol>
 <li><b>Especificidad de las Restricciones:</b> Constraint Programming es excepcionalmente útil en problemas donde las restricciones juegan un papel crítico. Puede manejar de manera eficiente múltiples y complejas restricciones como la capacidad limitada de los hospitales, la proximidad geográfica, y la priorización basada en la gravedad de la condición de los pacientes. CP busca soluciones que satisfagan todas estas restricciones de manera simultánea.</li>
 <li><b>Soluciones Óptimas y Completitud:</b> Constraint Programming es capaz de encontrar soluciones óptimas y garantizar que se respeten todas las restricciones duras. Si una solución existe, CP la encontrará, y si no, proporcionará pruebas de que no hay solución bajo las restricciones dadas.</li>
 <li><b>Flexibilidad en la Modelización:</b> Permite modificar fácilmente las restricciones y parámetros sin necesidad de reentrenar o reajustar modelos, lo cual es una ventaja significativa en situaciones dinámicas como una pandemia donde las condiciones cambian rápidamente.</li>
</ol>
</p>

#### Uso de Técnicas de ML

<p align="justify">
 El aprendizaje automático podría aplicarse para predecir necesidades, comportamientos o para optimizar ciertos aspectos del problema, como:

 <ul>
  <li><b>Predicción de la Demanda: </b> Modelos de ML pueden prever la cantidad de pacientes que requerirán hospitalización, lo cual ayuda a planificar y asignar recursos.</li>
  <li><b>Optimización de Recursos: </b>Algoritmos de ML pueden ayudar a optimizar la distribución de recursos médicos limitados de manera más eficiente, basándose en patrones de uso y demanda.</li>
 </ul>
</p>

#### ¿Por qué no usar solo ML?

<p align="justify">
 <ul>
  <li>Dependencia de Datos: ML requiere grandes volúmenes de datos históricos y relevantes para entrenar modelos efectivos. En situaciones emergentes o con datos escasos (como puede ser el inicio de una nueva pandemia), este requisito puede ser un gran desafío.</li>
  <li>Incapacidad para Manejar Restricciones Duras: ML es muy bueno para identificar patrones y hacer predicciones, pero no es naturalmente adecuado para manejar restricciones duras y específicas sin una considerable adaptación y supervisión.</li>
  <li>Interpretabilidad y Verificabilidad: Las soluciones de ML a menudo no son fácilmente interpretables, lo cual puede ser un problema en aplicaciones médicas y de salud pública donde la transparencia en la toma de decisiones es crucial.</li>
 </ul>
 <p>
  Aunque ML puede complementar a la CP, especialmente en aspectos de predicción y análisis de tendencias, CP es más adecuado para garantizar que todas las restricciones operativas y médicas se cumplan. En problemas críticos como la asignación de recursos en una pandemia, la capacidad de CP para ofrecer garantías de completitud y respeto de restricciones específicas es invaluable. Por lo tanto, usar CP, posiblemente en conjunto con ML, ofrece una combinación poderosa para abordar este tipo de desafíos complejos y dinámicos.
 </p>
</p>
