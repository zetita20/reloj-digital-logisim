# Reloj Digital con Flip-Flops en Logisim

Este proyecto contiene un circuito de reloj digital (horas, minutos, segundos) diseñado en Logisim con flip-flops JK y D.

## 📋 Características

- **Contador de Segundos**: 0-59 (6 flip-flops)
- **Contador de Minutos**: 0-59 (6 flip-flops)
- **Contador de Horas**: 0-23 (5 flip-flops)
- **Reset global**: A las 23:59:59
- **Diferencias con el diseño original**:
  - Estructura modular y escalable
  - Uso mixto de flip-flops JK y D
  - Decodificadores BCD para detección de límites
  - Control independiente por bloque

## 📁 Archivos

- `reloj_digital.circ` - Circuito principal en Logisim
- `contador_mod60.circ` - Subcircuito contador módulo 60 (reutilizable)
- `contador_mod24.circ` - Subcircuito contador módulo 24
- `README.md` - Documentación
- `ESQUEMA.md` - Explicación detallada del diseño

## 🔧 Cómo usar

1. Descarga e instala [Logisim Evolution](https://github.com/logisim-evolution/logisim-evolution)
2. Abre el archivo `reloj_digital.circ`
3. Ejecuta la simulación con el botón Play
4. Observa cómo avanzan horas, minutos y segundos

## ⚙️ Componentes principales

### Flip-Flops utilizados
- **JK Flip-Flop**: Para contadores en cascada
- **D Flip-Flop**: Para almacenamiento de estados

### Circuitos auxiliares
- Decodificadores (detectan cuando llega a 59 o 24)
- Compuertas AND/OR para lógica de control
- Multiplexores para seleccionar qué contador avanza

## 📊 Diagrama de bloques

```
    CLK
     |
     ├─→ [CONTADOR SEGUNDOS 0-59]
     |        |
     |        └─→ Genera CARRY cada 60 ciclos
     |
     ├─→ [CONTADOR MINUTOS 0-59]
     |        |
     |        └─→ Genera CARRY cada 60 ciclos
     |
     ├─→ [CONTADOR HORAS 0-23]
     |        |
     |        └─→ Genera CARRY cada 24 ciclos
     |
     └─→ [LÓGICA DE RESET GLOBAL]
              |
              └─→ Reset cuando es 23:59:59
```

## 🎯 Diferencias con la imagen original

| Aspecto | Original | Este diseño |
|---------|----------|------------|
| Estructura | Monolítica | Modular con subcircuitos |
| Flip-flops | Solo JK | JK + D (híbrido) |
| Divisores | Algunos específicos | Reutilizables (mod60, mod24) |
| Reset | Condicional simple | Detecta 23:59:59 exacto |
| Escalabilidad | Difícil | Fácil de modificar |

## 📝 Notas

- El circuito usa un clock de entrada que se puede acelerar en la simulación
- Los displays mostrarán el tiempo en formato HH:MM:SS
- Puedes pausar y reanudar la simulación en cualquier momento

---

**Autor**: Proyecto educativo  
**Fecha**: 2024  
**Herramienta**: Logisim Evolution
