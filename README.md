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

### Definición del problema

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

### Marco Teórico

<p align="justify">
Este problema se modelará como un problema de satisfacción de restricciones, donde se definen variables que representan si una cama específica en un hospital está ocupada por un paciente particular. Las variables se definen de la siguiente manera:
 <ul>
 <li> $xijk ∈ {0, 1}$, donde $(i, j, k)$ representa los índices correspondientes a hospitales, camas en ese hospital y pacientes, respectivamente. Si la cama $j$ del hospital $i$ está ocupada por el paciente $k$, entonces $xijk = 1$.</li>
 </ul>
</p>

<p><b>Restricciones Duras:</b></p>

<uL>
 <li>Cada cama puede estar ocupada por a lo máximo una persona.</li>
 <li>A cada persona se le debe asignar a lo máximo una cama.</li>
</uL>

<p><strong>Restricciones Suaves:</strong></p>
<ul>
 <li>Se debe intentar ubicar a cada persona infectada en una cama.</li>
 <li>Se debe priorizar que cada persona sea tratada en el hospital más cercano.</li>
 <li>Las personas con condiciones más graves deben ser priorizadas cuando no hay suficientes camas disponibles.</li>
</ul>



