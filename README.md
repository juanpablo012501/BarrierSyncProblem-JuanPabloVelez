# Barrier Synchronization Problem

---

## Problema

El programa que se entregó pone a 20 hilos a trabajar en la misma tarea en paralelo, estos hilos
difieren en el tiempo de ejecución de la misma. Se supone que el programa calcula el promedio de
los tiempos de demora de ejecución de cada hilo. El problema es que al iniciar cada hilo estos
al estar trabajando en paralelo, el hilo del método `main(String[] args)` de la clase `Main`
no tiene forma de saber si los hilos ya han terminado la ejecución de su tarea para así poder
calcular el promedio con los datos finales. Así pues, al correr el programa sin modificar se da que:

![promedio con los hilos aún en ejecución](/images/img.png)

Cómo se puede ver el resultado del promedio sale antes que los logs de ejecución de cada hilo. Esto
muestra que el promedio se calculó con los tiempos iniciales de cada hilo, en vez de los finales.

---

## Solución
Para resolver este inconveniente es necesario aplicar una barrera que detenga el hilo del método
`main(String[] args)` hasta que todos los hilos de las tareas terminen y así calcular el promedio
correctamente. Para ello, se actualizó el método ya mencionado agregandole:

```
    for (int i=0;i<numHilos;i++){
        try {
            hilos[i].join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }   
```
Con este fragmento de código el hilo del método `main` se detendra hilo a hilo hasta que todos terminen.
De tal manera que para `hilos[1].join()` hasta que no termine, no continua a la siguiente sentencia. Luego
al terminar el **hilo 1** se aplica `hilos[2].join()` y hasta que termine (de hecho el **hilo 2** pudo haber terminado
primero que el **hilo 1**) y así sucesivamente. Al final, se demora tanto como el hilo más lento. Finalmente,
con la barrera implementa el resultado de correr el código es:

![resultado con barrera implementada](/images/img_1.png)

Como se evidencia en la imagen, ahora el promedio es calculado leugo de la ejecución de todos los hilos
y mostrado correctamente en el terminal.