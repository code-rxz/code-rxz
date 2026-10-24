# Simulación de agua 2D
<p align="center">
  <a href="https://code-rxz.github.io/code-rxz/"><img src="https://img.shields.io/badge/demo-en%20vivo-0ea5e9?style=flat-square&logo=githubpages&logoColor=white&labelColor=1e293b" alt="Demo en vivo"></a>
  <img src="https://img.shields.io/badge/HTML5-1e293b?style=flat-square&logo=html5&logoColor=E34F26" alt="HTML5">
  <img src="https://img.shields.io/badge/JavaScript-1e293b?style=flat-square&logo=javascript&logoColor=F7DF1E" alt="JavaScript">
  <img src="https://img.shields.io/badge/dependencias-0-22c55e?style=flat-square&labelColor=1e293b" alt="Cero dependencias">
  <img src="https://img.shields.io/badge/optimizado%20para-m%C3%B3vil-38bdf8?style=flat-square&labelColor=1e293b" alt="Optimizado para móvil">
</p>

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

