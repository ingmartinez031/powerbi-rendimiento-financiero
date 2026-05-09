# 📊 Dashboard de Rendimiento Financiero

Dashboard interactivo desarrollado en Power BI que visualiza el rendimiento de la cartera de préstamos de una entidad bancaria. Permite analizar mora, riesgo, distribución por producto y evolución mensual de desembolsos a nivel de sucursal.

---

## 📌 Descripción

Este proyecto simula el panel de control que usaría un analista financiero o gerente de sucursal para monitorear el comportamiento de la cartera crediticia en tiempo real. Los datos provienen del proyecto complementario de SQL (`datos_bancarios.xlsx`).

---

## 🗂️ Estructura del proyecto

```
powerbi-rendimiento-financiero/
│
├── dashboard_cartera_bancaria.pbix   # Archivo Power BI principal
├── datos_bancarios.xlsx              # Fuente de datos (3 hojas)
├── guia_dax_medidas.md               # Medidas DAX documentadas
└── README.md
```

---

## 🛠️ Tecnologías utilizadas

- **Power BI Desktop** (versión recomendada: 2024 o superior)
- **Microsoft Excel** — fuente de datos estructurada
- **DAX** — lenguajes de medidas y KPIs calculados
- **Power Query** — transformación y limpieza de datos

---

## 📐 Páginas del Dashboard

### Página 1 — Resumen Ejecutivo
> Vista gerencial con los KPIs más importantes de la cartera.

| Visual | Descripción |
|---|---|
| Tarjetas KPI | Total cartera, Saldo vigente, Tasa de mora, Saldo en riesgo |
| Gráfico de dona | Distribución por estado (Al día / En mora / Vencido) |
| Gráfico de barras | Cartera por tipo de préstamo |
| Mapa de árbol | Saldo pendiente por segmento de cliente |

---

### Página 2 — Análisis de Mora y Riesgo
> Detalle de clientes con atrasos y segmentación por nivel de riesgo.

| Visual | Descripción |
|---|---|
| Tabla detallada | Clientes en mora con días, saldo y sucursal |
| Gráfico de barras apiladas | Mora por sucursal y nivel de riesgo |
| Gráfico de dispersión | Tasa de interés vs días de mora |
| Segmentador | Filtro por sucursal y tipo de préstamo |

---

### Página 3 — Evolución Mensual
> Tendencia de desembolsos y pagos en el tiempo.

| Visual | Descripción |
|---|---|
| Gráfico de líneas | Desembolsos mensuales acumulados |
| Gráfico de columnas | Pagos de cuota vs pagos de mora por mes |
| Gráfico de área | Evolución del saldo pendiente total |
| Segmentador | Filtro por año y sucursal |

---

## 🧮 Medidas DAX principales

```dax
-- Tasa de mora global
Tasa Mora % = 
DIVIDE(
    COUNTROWS(FILTER(Cartera_Prestamos, Cartera_Prestamos[Estado] IN {"En mora","Vencido"})),
    COUNTROWS(FILTER(Cartera_Prestamos, Cartera_Prestamos[Estado] <> "Cancelado"))
) * 100

-- Saldo en riesgo
Saldo en Riesgo = 
CALCULATE(
    SUM(Cartera_Prestamos[Saldo_Pendiente]),
    Cartera_Prestamos[Estado] IN {"En mora","Vencido"}
)

-- Tasa de interés promedio ponderada
Tasa Promedio Ponderada = 
DIVIDE(
    SUMX(Cartera_Prestamos, Cartera_Prestamos[Saldo_Pendiente] * Cartera_Prestamos[Tasa_Interes_%]),
    SUM(Cartera_Prestamos[Saldo_Pendiente])
)

-- Total cobrado en mora
Total Cobrado Mora = 
CALCULATE(
    SUM(Transacciones[Monto]),
    Transacciones[Tipo] = "Pago mora"
)
```

---

## 🚀 Cómo usar este proyecto

1. Clona el repositorio:
   ```bash
   git clone https://github.com/tu-usuario/powerbi-rendimiento-financiero.git
   ```
2. Abre **Power BI Desktop**
3. Click en **Obtener datos → Excel**
4. Selecciona `datos_bancarios.xlsx` y conecta las 3 hojas
5. Abre `dashboard_cartera_bancaria.pbix`
6. Si es necesario, actualiza la ruta del origen de datos en **Transformar datos → Configuración de origen**
7. Click en **Actualizar** para cargar los datos

---

## 💡 Habilidades demostradas

- Conexión y transformación de datos con Power Query
- Modelado de datos y relaciones entre tablas
- Creación de medidas DAX avanzadas
- Diseño de dashboards ejecutivos multicapa
- Visualización de KPIs financieros y análisis de mora
- Uso de segmentadores y filtros interactivos

---

## 🔗 Proyecto relacionado

Este dashboard utiliza los mismos datos del proyecto SQL:  
👉 [sql-cartera-bancaria](https://github.com/tu-usuario/sql-cartera-bancaria)

---

## 👤 Autor

# Richard Martinez

💻 # Ingeniero en Software / Analista de Datos  
📊 Power BI | SQL | Python | Excel

## 🌐 Conecta conmigo

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Richard%20Martinez-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ing-martinez-057b6b181/)

[![GitHub](https://img.shields.io/badge/GitHub-ingmartinez031-black?style=for-the-badge&logo=github)](https://github.com/ingmartinez031)
---

> *Proyecto desarrollado como parte de un portafolio de análisis de datos orientado al sector financiero y bancario.*
