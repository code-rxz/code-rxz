# Simulación de agua 2D
[

![Demo en vivo](https://img.shields.io/badge/demo-en%20vivo-00b4d8)



![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)




![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)




![Dependencias](https://img.shields.io/badge/dependencias-0-brightgreen)




![Optimizado para móvil](https://img.shields.io/badge/optimizado%20para-m%C3%B3vil-blue)




![Licencia](https://img.shields.io/github/license/code-rxz/NOMBRE-DEL-REPO)




![Último commit](https://img.shields.io/github/last-commit/code-rxz/NOMBRE-DEL-REPO)




![Estrellas](https://img.shields.io/github/stars/code-rxz/NOMBRE-DEL-REPO?style=flat)

Simulación de agua con fluidos dinámicos, en un solo archivo HTML y sin librerías.

## Demostración

https://github.com/user-attachments/assets/3d93a2f9-027e-4602-b981-864e29004d93

## La malla de números

Primero creé la malla de números (128 × 280). Cada número es la altura del agua en ese punto: positivo es una cresta y negativo es un valle.

<img width="400" alt="Malla de alturas con una gota en el centro" src="https://github.com/user-attachments/assets/4db6956f-9089-41e7-b40e-2dc0b1a1de2e" />

*Ejemplo reducido de 9 × 9. La simulación real usa una malla mucho más grande.*

## Cómo funciona

### 2. Cada celda sigue a sus vecinas

Después hice que la altura nueva de cada celda dependa de sus 4 vecinas (izquierda, derecha, arriba y abajo) y de su propia altura en el paso anterior

```
h_nueva = ( (izq + der + arriba + abajo) / 2 - h_anterior ) × amortiguación
```

Esta fórmula sale de la **ecuación de onda**. La amortiguación es un número un poco menor que 1 (por ejemplo 0.99): cada paso le quita un poco de energía a las olas y por eso se van apagando. Los bordes de la malla siempre valen 0 y funcionan como paredes donde las olas rebotan.

### 3. Dos mallas que se intercambian

Guardo dos copias de la malla: la actual y la del paso anterior. El resultado nuevo se escribe en la copia vieja y luego se intercambian. Así no se crea memoria nueva en cada paso, y eso ayuda a que vaya rápido en un móvil.

### 4. Cómo se dibuja

Para cada píxel miro la **pendiente** del agua (la diferencia de altura entre vecinos)

1. **Refracción:** la pendiente mueve el punto del fondo que se ve, y por eso las baldosas se deforman.
2. **Luz:** las pendientes que miran hacia la luz se aclaran y las contrarias se oscurecen.
3. **Destello:** si la pendiente apunta casi directo a la luz, se suma un brillo blanco.

## Cómo se usa

| Acción | Resultado |
| --- | --- |
| Tocar la pantalla | Cae una gota |
| Arrastrar el dedo | Deja una estela de olas |
| Lluvia | Caen gotas al azar |
| Pausa | Congela la simulación |
| Limpiar | Deja el agua en calma |
| Calidad | Cambia entre Alta, Media, Baja y Mínima |
| Deslizador | Controla cuánto duran las olas |

