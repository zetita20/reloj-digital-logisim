# Esquema Detallado del Reloj Digital

## 1. CONTADOR DE SEGUNDOS (0-59)

### Estructura
```
Flip-Flop 0  →  Flip-Flop 1  →  Flip-Flop 2  →  Flip-Flop 3  →  Flip-Flop 4  →  Flip-Flop 5
   1 seg        2 seg          4 seg          8 seg          16 seg         32 seg
   (LSB)                                                                      (MSB)
```

### Lógica de Reset
- Decodificador detecta cuando la salida es 111011 (59 en binario)
- Genera señal de reset síncrono
- Envía carry al contador de minutos

### Tabla de conteo (últimos valores)
```
Segundo  | FF5 | FF4 | FF3 | FF2 | FF1 | FF0
---------|-----|-----|-----|-----|-----|-----
   57    |  1  |  1  |  1  |  0  |  0  |  1
   58    |  1  |  1  |  1  |  0  |  1  |  0
   59    |  1  |  1  |  1  |  0  |  1  |  1  ← RESET
   00    |  0  |  0  |  0  |  0  |  0  |  0
```

---

## 2. CONTADOR DE MINUTOS (0-59)

### Estructura
Idéntica al contador de segundos, pero:
- Se activa con el CARRY del contador de segundos
- Clock = Carry del contador anterior

### Señales de entrada
```
CLK = Carry_Segundos
RESET = Reset_Global O (Minutos == 59 Y Horas == 23)
```

---

## 3. CONTADOR DE HORAS (0-23)

### Estructura
```
Flip-Flop 0  →  Flip-Flop 1  →  Flip-Flop 2  →  Flip-Flop 3  →  Flip-Flop 4
   1 hora       2 hora        4 hora         8 hora        16 hora
   (LSB)                                                     (MSB)
```

### Lógica especial
- Detecta cuando llega a 23 (10111 en binario)
- NO a 24 como un contador normal
- Reset en 23:59:59

### Decodificador para 23
```
Salida = FF4 AND FF3 AND NOT(FF2) AND FF1 AND FF0
         16  AND  8  AND   4    AND  2 AND  1 = 23
```

---

## 4. LÓGICA DE RESET GLOBAL

### Condición de Reset
```
RESET_GLOBAL = (Horas == 23) AND (Minutos == 59) AND (Segundos == 59)
```

### Implementación con compuertas
```
Reset = FF5_seg AND FF4_seg AND FF3_seg AND (NOT FF2_seg) AND FF1_seg AND FF0_seg
    AND FF5_min AND FF4_min AND FF3_min AND (NOT FF2_min) AND FF1_min AND FF0_min
    AND FF4_hor AND FF3_hor AND (NOT FF2_hor) AND FF1_hor AND FF0_hor
```

---

## 5. COMPONENTES EN LOGISIM

### Flip-Flops JK
- **Entrada J**: Determina si el FF se pone en 1
- **Entrada K**: Determina si el FF se pone en 0
- **CLK**: Clock del sistema
- **Salida Q y Q'**: Estado actual e invertido

### Decodificadores
- Entrada: 6 bits (para segundos/minutos)
- Salida: 1 línea activada cuando detecta el valor especificado

### Compuertas lógicas
- AND: Para la lógica de reset
- OR: Para combinaciones de condiciones
- NOT: Para inversiones necesarias

---

## 6. TABLA DE TRANSICIONES

### Transición de Segundos a Minutos
```
Segundos | Carry a Minutos
---------|----------------
  0-58   |        0
  59     |        1 (genera carry)
  00     |        0 (reset y carry se transmite)
```

### Transición de Minutos a Horas
```
Minutos | Carry a Horas
--------|---------------
  0-58  |       0
  59    |       1 (genera carry)
  00    |       0 (reset y carry se transmite)
```

---

## 7. SIMULACIÓN EN LOGISIM

### Pasos para verificar
1. Colocar clock a velocidad lenta (1 Hz)
2. Observar incremento cada segundo
3. Verificar carry cuando llega a 59
4. Confirmar reset en 23:59:59

### Pruebas recomendadas
- [ ] Contador de segundos llega a 59 y reset
- [ ] Contador de minutos avanza con cada carry
- [ ] Contador de horas avanza con cada carry de minutos
- [ ] Reset global ocurre en 23:59:59
- [ ] Valores no exceden los límites

---

## 8. DIFERENCIAS DE DISEÑO

| Característica | Original | Este diseño |
|----------------|----------|------------|
| FF para segundos | 6 FF | 6 FF (igual) |
| FF para minutos | 6 FF | 6 FF (igual) |
| FF para horas | 5 FF | 5 FF (igual) |
| Tipo de FF | Solo JK | JK + opciones D |
| Reset | Asíncrono | Síncrono |
| Detección límite | Lógica fija | Decodificador flexible |
| Modularidad | Baja | Alta (subcircuitos) |

