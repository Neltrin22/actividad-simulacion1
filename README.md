# Actividad de seguimiento - Simulación 1

|Integrante|correo|usuario github|
|---|---|---|
|Clara Isabel Villadiego Martinez|clara.villadiego@udea.edu.co|ClaraVilladiego|
|William Alexander Torres Zambrano|walexander.torres@udea.edu.co|watorres|
|Nelson Alcides Puerta García|alcides.puerta@udea.edu.co|Neltrin22|

## Instrucciones

Antes de empezar a realizar esta actividad haga un **fork** de este repositorio y sobre este trabaje en la solución de las preguntas planteadas en la actividad de simulación. Las respuestas deben ser respondidas en español o si lo prefiere en ingles en el lugar señalado para ello (La palabra **answer** muestra donde).

**Importante**:
* Como la actividad es en las parejas del laboratorio, solo uno de los integrantes tiene que hacer el fork; y sobre repositorio bifurcado que se genera, la modificación se realiza en equipo.
* Como la entrega se debe hacer modificando el archivo READNE, se recomienda que consulte mas sobre el lenguaje **Markdown**. En el repo adjuntan dos cheatsheet ([cheat sheet 1](Markdown_Cheat_Sheet.pdf), [cheatsheet 2](markdown-cheatsheet.pdf)) para consulta rapida.
* Entre mas creativo mejor.

## Homework (Simulation)

This program, [`process-run.py`](process-run.py), allows you to see how process states change as programs run and either use the CPU (e.g., perform an add instruction) or do I/O (e.g., send a request to a disk and wait for it to complete). See the [README](https://github.com/remzi-arpacidusseau/ostep-homework/blob/master/cpu-intro/README.md) for details.

### Questions

1. Run `process-run.py` with the following flags: `-l 5:100,5:100`. What should the CPU utilization be (e.g., the percent of time the CPU is in use?) Why do you know this? Use the `-c` and `-p` flags to see if you were right.
   
   <details>
   <summary>Answer</summary>
    Como no hay IO, ningún proceso se suspende o se reprograma. Así que el sistema ejecuta el primer proceso (Process 0) hasta que termina, luego pasa al segundo proceso (Process 1) y lo ejecuta hasta el final, lo cual produce un porcentaje de utilización de la CPU del 100%.
      <img width="428" alt="{07FD9205-F29D-4172-93B7-CA53F389D549}" src="https://github.com/user-attachments/assets/4f396bc9-0293-404b-ae20-9a24d27920bd" />


   </details>
   <br>

2. Now run with these flags: `./process-run.py -l 4:100,1:0`. These flags specify one process with 4 instructions (all to use the CPU), and one that simply issues an I/O and waits for it to be done. How long does it take to complete both processes? Use `-c` and `-p` to find out if you were right. 
   
   <details>
   <summary>Answer</summary>
   Como hay una una IO el proceso se bloquea y le da paso a los demas procesos, entonce espera hasta que culmine la operacion de IO que puede durar un promedio de 6 ciclos 
   mas otros 4 del otro proceso podría tomarse al rededor de 14 ciclos.
      <img width="432" alt="{8AF90B67-E8E4-4684-AB65-8BA520329E32}" src="https://github.com/user-attachments/assets/66ed5698-a299-4263-951c-5720e00488c5" />

   </details>
   <br>

3. Switch the order of the processes: `-l 1:0,4:100`. What happens now? Does switching the order matter? Why? (As always, use `-c` and `-p` to see if you were right)
   
   <details>
   <summary>Answer</summary>
   Se reduciría el tiempo de ejecución, porque mientras se procesa la IO se ejecuta el otro proceso.
      <img width="418" alt="{48321A28-58A8-413F-BBE8-A6499D120404}" src="https://github.com/user-attachments/assets/937c2899-374e-43a1-8f96-303047b75245" />

   </details>
   <br>

4. We'll now explore some of the other flags. One important flag is `-S`, which determines how the system reacts when a process issues an I/O. With the flag set to SWITCH ON END, the system will NOT switch to another process while one is doing I/O, instead waiting until the process is completely finished. What happens when you run the following two processes (`-l 1:0,4:100 -c -S SWITCH ON END`), one doing I/O and the other doing CPU work?
   
   <details>
   <summary>Answer</summary>
   Con esa instrucción el sistema no cambiara a los demas procesos hasta que termine el proceso que tiene la IO y esto sucede despues de que retorne de la IO, en ese tiempo     no habrá utilización de la CPU.
      <img width="489" alt="{A7E653E7-11FB-49C7-B558-C4AA8CBE487C}" src="https://github.com/user-attachments/assets/a8938a5a-fb73-4e7a-8d05-4f8543bdf22c" />

   </details>
   <br>

5. Now, run the same processes, but with the switching behavior set to switch to another process whenever one is WAITING for I/O (`-l 1:0,4:100 -c -S SWITCH ON IO`). What happens now? Use `-c` and `-p` to confirm that you are right.
   
   <details>
   <summary>Answer</summary>
   Em este caso si se permite cambiar a otros procesos cuando se realiza la solicitud de IO, por lo tanto se mejora la eficiencia y utilización de la CPU.
      <img width="491" alt="{CB1EE1D9-2F59-4D55-A175-6DFD592A3E0C}" src="https://github.com/user-attachments/assets/dc22a2c1-0bf8-4aac-a32a-65ae49c5f84c" />

   </details>
   <br>

6. One other important behavior is what to do when an I/O completes. With `-I IO RUN LATER`, when an I/O completes, the process that issued it is not necessarily run right away; rather, whatever was running at the time keeps running. What happens when you run this combination of processes? (`./process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN LATER`) Are system resources being effectively utilized?
   
   <details>
   <summary>Answer</summary>
   Con estas instrucciones el sistema otorga cierta prioridades y deja esperando al proceso que hizo la IO sin robarle el control al proceso en ejecución, esto puede hacer      que el sistema sea mas equilibrado planificando tambien los retornos de las IO.
      <img width="502" alt="{8AC4A59D-9D3A-476C-B059-740810512C62}" src="https://github.com/user-attachments/assets/9e13ab0d-27a3-45d9-9dd3-6e334f01ad60" />

   </details>
   <br>

7. Now run the same processes, but with `-I IO RUN IMMEDIATE` set, which immediately runs the process that issued the I/O. How does this behavior differ? Why might running a process that just completed an I/O again be a good idea?
   
   <details>
   <summary>Answer</summary>
   Como en este caso el retorno de una IO interrumpe el proceso en ejecución, se puede incrementar considerablemente el tiempo de ejecución de las procesos que no están 
   ejecutando peticiones IO, la ventaja es que se aumenta el tiempo de utilización de la CPU.
      <img width="511" alt="{D9B492CE-B7A3-4694-AF35-63B8EC41EE90}" src="https://github.com/user-attachments/assets/b0305a18-6e89-463f-8975-4b0f26c9f452" />

      
   </details>
   <br>


### Criterios de evaluación
- [x] Despligue de los resultados y analisis claro de los resultados respecto a lo visto en la teoria.
- [x] Creatividad y orden.
