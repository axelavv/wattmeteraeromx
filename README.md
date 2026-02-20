# ⚡ Wattmeter - AerodesignMx

Un sistema inalámbrico de medición de potencia de alta precisión diseñado para monitorear el consumo de energía en aeronaves RC y aplicaciones UAV durante pruebas de vuelo y competencias.

## 📋 Descripción General

Este wattmeter proporciona monitoreo en tiempo real de voltaje, corriente y potencia con acceso a interfaz web inalámbrica. Perfecto para Aerodesign Mx y análisis de rendimiento de aeronaves RC.

**Especificaciones Clave:**
- Rango de Voltaje: Hasta 6S LiPo (25.2V máx)
- Rango de Corriente: Hasta 50A
- Rango de Potencia: 0-1200W (optimizado para 800-1000W)
- Tasa de Actualización: 10 muestras por segundo

## ✨ Características

### Monitoreo en Tiempo Real
- **Panel Web en Vivo** - Visualiza las mediciones desde cualquier dispositivo con WiFi
- **Alertas Visuales** - Indicadores LED para alertas de umbral de potencia
- **Seguimiento de Picos** - Registro automático de valores máximos de corriente y potencia
- **No Requiere Internet** - Crea su propio Punto de Acceso WiFi

### Registro de Datos
- **Registro Automático en Tarjeta SD** - Todas las mediciones guardadas en tarjeta SD
- **Nomenclatura Secuencial** - Nunca sobrescribe datos de pruebas anteriores
- **Formato CSV** - Fácil de analizar en Excel, Python, MATLAB, etc.
- **Alta Tasa de Muestreo** - 10 muestras por segundo para análisis detallado

### Características de Seguridad
- **Alertas Visuales**:
  - 850-1000W: LED parpadeando (zona de advertencia)
  - 1000W+: LED encendido fijo (zona crítica)
- **Alertas en Tiempo Real** - Retroalimentación inmediata sobre niveles de potencia

## 🚀 Inicio Rápido

### 1. Enciende el Dispositivo
Conecta tu batería y fuente de poder al wattmeter.

### 2. Conéctate al WiFi
- **Nombre de Red (SSID):** `Wattmeter_AeroMx`
- **Contraseña:** `aeromx2024`

### 3. Accede al Panel de Control
Abre un navegador web y navega a:
```
http://192.168.4.1
```

### 4. Monitorea tu Sistema
El panel muestra:
- Voltaje actual (V)
- Corriente actual (A)
- Potencia actual (W)
- Corriente pico (A)
- Potencia pico (W)
- Contador de muestras

## 📊 Usando los Datos Registrados

### Ubicación de Archivos
Los datos se guardan automáticamente en la tarjeta SD con nomenclatura secuencial:
```
wattmeter_0000.txt
wattmeter_0001.txt
wattmeter_0002.txt
...
```

### Formato de Archivo
Cada archivo contiene datos en formato CSV:
```csv
Wattmeter Data Log - AerodesignMx
====================================
File: wattmeter_0000.txt
Sample,Timestamp(ms),Voltage(V),Current(A),Power(W)
0,1234,22.45,15.32,344.01
1,1334,22.43,15.45,346.54
2,1434,22.41,15.58,349.11
...
```

### Analizando los Datos

#### Excel / Google Sheets
1. Retira la tarjeta SD del wattmeter
2. Insértala en la computadora
3. Abre el archivo `.txt` en Excel/Sheets
4. Omite las filas de encabezado (primeras 4 líneas)
5. Crea gráficas con las columnas:
   - Columna A: Número de muestra
   - Columna B: Tiempo (milisegundos)
   - Columna C: Voltaje
   - Columna D: Corriente
   - Columna E: Potencia

#### Análisis con Python
```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

# Cargar datos
data = pd.read_csv('wattmeter_0002.txt', skiprows=3)

# Convertir tiempo a segundos
time_s = data['Timestamp(ms)'] / 1000
power = data['Power(W)']

# Calcular estadísticas
potencia_promedio = power.mean()
potencia_pico = power.max()
energia_total = power.sum() * 0.1 / 3600  # Ajusta si el intervalo no es 0.1 s

# Encontrar índice del pico
peak_index = power.idxmax()
peak_time = time_s.iloc[peak_index]
peak_value = power.iloc[peak_index]

# Crear figura
fig, ax = plt.subplots(figsize=(12, 6))

# Línea principal
ax.plot(time_s, power, label='Potencia', linewidth=1.5)

# Línea promedio
ax.axhline(potencia_promedio, color='orange', linestyle='--',
           linewidth=2, label=f'Promedio ({potencia_promedio:.2f} W)')

# Marcar pico
ax.plot(peak_time, peak_value, 'ro', label='Pico')
ax.annotate(f'{peak_value:.2f} W',
            xy=(peak_time, peak_value),
            xytext=(10, 10),
            textcoords='offset points',
            arrowprops=dict(arrowstyle='->'),
            fontsize=10)

# Etiquetas
ax.set_xlabel('Tiempo (segundos)', fontsize=12)
ax.set_ylabel('Potencia (Watts)', fontsize=12)
ax.set_title('Consumo de Potencia en el Tiempo', fontsize=14, fontweight='bold')
ax.grid(True, linestyle='--', alpha=0.6)

# Caja de estadísticas
stats_text = (
    f"Potencia Promedio: {potencia_promedio:.2f} W\n"
    f"Potencia Pico: {potencia_pico:.2f} W\n"
    f"Energía Total: {energia_total:.4f} Wh"
)

ax.text(
    0.02, 0.98,
    stats_text,
    transform=ax.transAxes,
    fontsize=11,
    verticalalignment='top',
    bbox=dict(boxstyle='round', facecolor='white', alpha=0.9)
)

# Leyenda
ax.legend()

plt.tight_layout()
plt.show()
```

#### Análisis con MATLAB
```matlab
% Cargar datos
data = readtable('wattmeter_0000.txt', 'HeaderLines', 4);

% Extraer columnas
time_s = data.Timestamp_ms_ / 1000;
power = data.Power_W_;

% Graficar
figure;
plot(time_s, power, 'LineWidth', 2);
xlabel('Tiempo (segundos)');
ylabel('Potencia (Watts)');
title('Análisis de Consumo de Potencia');
grid on;

% Estadísticas
avg_power = mean(power);
peak_power = max(power);
fprintf('Potencia Promedio: %.2f W\n', avg_power);
fprintf('Potencia Pico: %.2f W\n', peak_power);
```

## 🎛️ Controles de la Interfaz Web

### Botón Reset Peaks (Reiniciar Picos)
- **Función:** Limpia los valores de corriente pico y potencia pico
- **Acceso:** Protegido por contraseña
- **Contraseña por Defecto:** `AeroMx2024!`
- **Caso de Uso:** Reiniciar entre diferentes corridas de prueba

### Botón Refresh (Actualizar)
- **Función:** Recarga la página web
- **Caso de Uso:** Si se pierde la conexión o los datos parecen congelados

## 📱 Acceso Multi-Dispositivo

Múltiples dispositivos pueden conectarse simultáneamente:
- Piloto monitoreando desde cabina
- Equipo en tierra analizando datos en tiempo real
- Miembros del equipo visualizando desde área de pits

Todos los dispositivos verán los mismos datos en vivo.

## ⚠️ Indicadores LED de Advertencia

El LED integrado (Pin D4) proporciona retroalimentación visual:

| Nivel de Potencia | Comportamiento LED | Estado |
|------------------|-------------------|--------|
| < 850W | APAGADO | Operación normal |
| 850-999W | Parpadeando (500ms) | Zona de advertencia |
| ≥ 1000W | Encendido FIJO | Crítico - excediendo objetivo |

## 💾 Manejo de Tarjeta SD

### Tarjetas SD Recomendadas
- **Tipo:** MicroSD o tarjeta SD
- **Formato:** FAT16 o FAT32
- **Tamaño:** 2GB - 32GB (tarjetas más grandes pueden necesitar FAT32)
- **Velocidad:** Clase 4 o superior

### Manejo de Archivos
- Cada encendido crea un nuevo archivo automáticamente
- Los archivos se numeran secuencialmente (0000-9999)
- Máximo 10,000 sesiones de prueba por tarjeta SD
- Recomendado: Formatear la tarjeta SD antes de competencias

### Almacenamiento Estimado
- ~100 bytes por muestra
- 10 muestras/segundo = 1KB/segundo
- Prueba de 1 minuto ≈ 60KB
- Prueba de 10 minutos ≈ 600KB
- Tarjeta de 8GB ≈ 13,000+ minutos de datos

## 📞 Soporte

Para preguntas o problemas durante la competencia:
- Verifica el indicador LED para alertas de potencia
- Verifica que la tarjeta SD esté insertada correctamente
- Asegura que la conexión WiFi sea estable
- Revisa las conexiones de batería/alimentación

---

**Equipo AerodesignMx**  
*Construido para la Competencia Aerodesign MX*

¡Buena suerte con sus vuelos! 🛩️
