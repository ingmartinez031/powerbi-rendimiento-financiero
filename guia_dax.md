# 🧮 Guía de Medidas DAX — Dashboard Cartera Bancaria

Todas las medidas DAX utilizadas en el dashboard, organizadas por categoría con descripción y código listo para copiar en Power BI.

---

## 📌 Cómo crear una medida en Power BI

1. En el panel **Datos**, selecciona la tabla `Cartera_Prestamos`
2. Click en **Nueva medida** (cinta superior)
3. Pega el código DAX y presiona Enter
4. Repite para cada medida

---

## 1️⃣ KPIs de Cartera

```dax
Total Prestamos Activos = 
COUNTROWS(
    FILTER(Cartera_Prestamos, Cartera_Prestamos[Estado] <> "Cancelado")
)
```

```dax
Cartera Total Desembolsada = 
SUM(Cartera_Prestamos[Monto_Original])
```

```dax
Saldo Total Vigente = 
CALCULATE(
    SUM(Cartera_Prestamos[Saldo_Pendiente]),
    Cartera_Prestamos[Estado] <> "Cancelado"
)
```

```dax
Tasa Interes Promedio = 
ROUND(AVERAGE(Cartera_Prestamos[Tasa_Interes_%]), 2)
```

```dax
Tasa Promedio Ponderada = 
DIVIDE(
    SUMX(Cartera_Prestamos, Cartera_Prestamos[Saldo_Pendiente] * Cartera_Prestamos[Tasa_Interes_%]),
    SUM(Cartera_Prestamos[Saldo_Pendiente])
)
```

---

## 2️⃣ Análisis de Mora

```dax
Prestamos en Mora = 
CALCULATE(
    COUNTROWS(Cartera_Prestamos),
    Cartera_Prestamos[Estado] IN {"En mora", "Vencido"}
)
```

```dax
Tasa Mora % = 
DIVIDE(
    CALCULATE(
        COUNTROWS(Cartera_Prestamos),
        Cartera_Prestamos[Estado] IN {"En mora", "Vencido"}
    ),
    CALCULATE(
        COUNTROWS(Cartera_Prestamos),
        Cartera_Prestamos[Estado] <> "Cancelado"
    )
) * 100
```

```dax
Saldo en Riesgo = 
CALCULATE(
    SUM(Cartera_Prestamos[Saldo_Pendiente]),
    Cartera_Prestamos[Estado] IN {"En mora", "Vencido"}
)
```

```dax
Dias Mora Promedio = 
CALCULATE(
    AVERAGE(Cartera_Prestamos[Dias_Mora]),
    Cartera_Prestamos[Dias_Mora] > 0
)
```

---

## 3️⃣ Análisis de Transacciones

```dax
Total Pagos Cuota = 
CALCULATE(
    SUM(Transacciones[Monto]),
    Transacciones[Tipo] = "Pago cuota"
)
```

```dax
Total Cobrado Mora = 
CALCULATE(
    SUM(Transacciones[Monto]),
    Transacciones[Tipo] = "Pago mora"
)
```

```dax
Total Transacciones = 
COUNTROWS(Transacciones)
```

```dax
Monto Promedio Cuota = 
CALCULATE(
    AVERAGE(Transacciones[Monto]),
    Transacciones[Tipo] = "Pago cuota"
)
```

---

## 4️⃣ Medidas de Comparación y Variación

```dax
% Cartera Al Dia = 
DIVIDE(
    CALCULATE(COUNTROWS(Cartera_Prestamos), Cartera_Prestamos[Estado] = "Al día"),
    CALCULATE(COUNTROWS(Cartera_Prestamos), Cartera_Prestamos[Estado] <> "Cancelado")
) * 100
```

```dax
% Saldo Recuperado = 
DIVIDE(
    SUM(Cartera_Prestamos[Monto_Original]) - SUM(Cartera_Prestamos[Saldo_Pendiente]),
    SUM(Cartera_Prestamos[Monto_Original])
) * 100
```

```dax
Cartera Riesgo Alto = 
CALCULATE(
    SUM(Cartera_Prestamos[Saldo_Pendiente]),
    Cartera_Prestamos[Nivel_Riesgo] = "Alto"
)
```

---

## 5️⃣ Medidas de Tiempo (para gráficos de evolución)

> Requiere tener una columna de fecha en la tabla Transacciones

```dax
Pagos Mes Actual = 
CALCULATE(
    SUM(Transacciones[Monto]),
    DATESMTD(Transacciones[Fecha])
)
```

```dax
Pagos Mes Anterior = 
CALCULATE(
    SUM(Transacciones[Monto]),
    DATEADD(Transacciones[Fecha], -1, MONTH)
)
```

```dax
Variacion Pagos % = 
DIVIDE(
    [Pagos Mes Actual] - [Pagos Mes Anterior],
    [Pagos Mes Anterior]
) * 100
```

---

## 🎨 Sugerencia de formato para tarjetas KPI

| Medida | Formato sugerido |
|---|---|
| Cartera Total Desembolsada | `RD$ #,##0` |
| Saldo Total Vigente | `RD$ #,##0` |
| Tasa Mora % | `0.00 "%"` |
| Tasa Interes Promedio | `0.00 "%"` |
| Total Prestamos Activos | `0` |
| Dias Mora Promedio | `0.0 "días"` |

---

## 🔗 Relaciones recomendadas entre tablas

```
Cartera_Prestamos[Prestamo_ID]  →  Transacciones[Prestamo_ID]  (1 a muchos)
```

Crear en Power BI: **Modelado → Administrar relaciones → Nueva**
