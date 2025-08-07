# Informe Final: Análisis de Evasión de Clientes (Churn)

## Introducción
El objetivo de este análisis es identificar los patrones y factores que influyen en la evasión de clientes (churn) en una empresa de telecomunicaciones. La evasión de clientes representa un problema crítico que impacta directamente en los ingresos recurrentes y la rentabilidad del negocio.

**Problema de Churn**:
- Tasa global de evasión: `28.8%`
- Pérdida estimada: `$2.8M` anuales (considerando cargos promedio)
- Objetivo: Reducir la evasión en un `15%` mediante estrategias basadas en datos

## Limpieza y Tratamiento de Datos
**Proceso realizado**:
1. **Extracción de datos**:
   - Datos obtenidos desde GitHub en formato JSON
   - `7,267` registros iniciales con `21` variables

2. **Limpieza de inconsistencias**:
   - Corrección de valores categóricos (género, servicios)
   - Imputación de cargos mensuales/totales inconsistentes (`12` registros)
   - Estandarización de servicios adicionales para clientes sin internet

3. **Transformaciones clave**:
```python

# Creación de nueva métrica
df['Cuentas_Diarias'] = df['Cargos_Mensuales'] / 30

# Conversión a binarios
binary_mapping = {'Yes': 1, 'No': 0}
df['Evasion'] = df['Churn'].map(binary_mapping)

# Renombrado de variables
translation_dict = {'account.tenure': 'Meses_Contrato', ...}
```

3. **Transformaciones clave**:
- **Cantidad_Servicios**: Suma de servicios adicionales contratados
- **Tipo_Contrato_Num**: Valor ordinal para contratos:
  - `Mensual = 1`
  - `Anual = 2`
  - `Bianual = 3`

## Análisis Exploratorio de Datos
### Hallazgos clave
1. **Distribución de Evasión**  
   ![Distribución](https://distribucion_evasion.png)
   - Clientes retenidos: `71.2%`
   - Clientes evadidos: `28.8%`
   - Ratio evasión/retención: `0.40`

2. **Factores de Alto Impacto**  
   ![Evasión por contrato](https://evasion_por_contrato.png)
   - Contrato Mensual: `44.6%` evasión
   - Fibra Óptica: `38.1%` evasión
   - Pago con Cheque Electrónico: `34.2%` evasión

3. **Correlaciones Significativas**  
   ![Correlaciones](https://top_correlaciones_evasion.png)
   | Variable               | Correlación con Evasión |
   |------------------------|-------------------------|
   | Meses_Contrato         | `-0.352`                |
   | Tipo_Contrato_Num      | `-0.202`                |
   | Cantidad_Servicios     | `-0.132`                |
   | Cuentas_Diarias        | `+0.085`                |

4. **Patrones en Servicios**  
   ![Tasa de evasión](https://tasa_evasion_servicios.png)
   - 0-2 servicios: `30-35%` evasión
   - 3-5 servicios: `20-25%` evasión
   - 6+ servicios: `<15%` evasión

5. **Grupos de Alto Riesgo**  
   ![Grupos de riesgo](https://grupos_alto_riesgo.png)
   - Mensual + Fibra Óptica: `46.2%` evasión
   - Digital + Cheque Electrónico: `45.3%` evasión
   - Mensual + Sin Internet: `42.1%` evasión

## Conclusiones e Insights
### Principales Hallazgos
- **El tiempo es oro**:
  - Clientes con `<6 meses` tienen `4x` más probabilidad de evasión
  - Cada mes adicional reduce `3.5%` el riesgo de abandono
- **El contrato importa**:
  - Contratos mensuales tienen `6x` más evasión que contratos bianuales
  - `82%` de los clientes que evaden tienen contrato mensual
- **Paradoja de la fibra óptica**:
  - Servicio premium con mayor tasa de abandono (`38.1%`)
  - Causas probables: Expectativas no cumplidas, problemas técnicos
- **El poder de los servicios adicionales**:
  - Cada servicio adicional reduce `13%` el riesgo de evasión
  - Clientes con `5+` servicios tienen `<15%` probabilidad de abandono
- **Perfil de alto riesgo**:
  - Cliente nuevo (`0-3 meses`)
  - Contrato mensual + Fibra óptica
  - Pago con cheque electrónico
  - `<3` servicios adicionales

## Recomendaciones Estratégicas
### Acciones Inmediatas (Quick Wins)
- **Programa de fidelización para nuevos clientes**:
  - Descuento del `15%` en primeros `6 meses`
  - Kit de bienvenida con servicios adicionales gratis
- **Migración a contratos largos**:
  - Oferta de `2 meses gratis` al cambiar a contrato anual
  - Beneficios escalonados por antigüedad
- **Paquetes de servicios estratégicos**:
  | Paquete           | Servicios Incluidos             | Descuento |
  |-------------------|----------------------------------|-----------|
  | Seguridad Plus    | OnlineSecurity + DeviceProtection| `20%`     |
  | Entretenimiento   | StreamingTV + StreamingMovies   | `15%`     |
  | Full Experience   | Todos los servicios             | `30%`     |

### Estrategias a Mediano Plazo
- **Revisión de servicio fibra óptica**:
  - Auditoría técnica de calidad de servicio
  - Programa "Satisfacción Garantizada" con `30 días` de prueba gratis
- **Reestructuración de métodos de pago**:
  - Incentivos del `5%` para pagos automáticos
  - Asistencia personalizada para migración a métodos automáticos
- **Sistema de alerta temprana**:
```python
# Modelo predictivo de riesgo
riesgo_evasion = (
  0.35 * (Meses_Contrato < 3) +
  0.25 * (Tipo_Contrato == 'Mensual') +
  0.20 * (Servicio_Internet == 'Fibra Óptica') +
  0.15 * (Cantidad_Servicios < 3) +
  0.05 * (Metodo_Pago == 'Cheque Electrónico')
)
```
## Estrategias de Prevención
### Programa "Escudo Anti-Evasión"
- Revisión proactiva a clientes con `score > 0.75`
- Ofertas personalizadas basadas en perfil de consumo
- Asignación de gestor de cuenta para clientes premium

### Transformación digital
- App móvil con autocontrol de consumo
- Sistema de notificaciones proactivas
- Chatbot `24/7` para solución inmediata de problemas

## Impacto Esperado
| Estrategia                  | Reducción Estimada de Evasión | Timeline |
|-----------------------------|-------------------------------|----------|
| Migración a contratos largos| `8-10%`                       | 3 meses  |
| Paquetes de servicios       | `5-7%`                        | 6 meses  |
| Sistema de alerta temprana  | `3-5%`                        | 9 meses  |
| **Total estimado**          | **`16-22%`**                  | 1 año    |

## Conclusión General

Este análisis revela que la evasión de clientes en telecomunicaciones **no es aleatoria**, sino que sigue patrones predecibles vinculados a:

- ⏱️ **Temporalidad**: Los primeros meses son críticos  
- 📑 **Estructura contractual**: Los contratos cortos son vulnerables  
- 🔗 **Complejidad del servicio**: Más servicios = mayor retención  
- 💳 **Experiencia de pago**: Los métodos manuales aumentan riesgo  

Las estrategias propuestas abordan estos factores de forma integrada, combinando:

- 🎯 **Incentivos inmediatos** para grupos de riesgo  
- ♻️ **Reestructuración de servicios** para aumentar valor percibido  
- 🔮 **Tecnología predictiva** para intervenciones proactivas  

**Impacto potencial de implementación:**  
| Métrica | Valor | Plazo |
|---------|-------|-------|
| Reducción de evasión | 16-22% | 12 meses |
| Retorno de inversión (ROI) | >300% | 12 meses |

**Claves de éxito:**  
1. Coordinación interáreas (comercial + técnica + atención al cliente)  
2. Sistema de monitoreo continuo basado en indicadores  
3. Adaptación dinámica a cambios en patrones de comportamiento  

Este plan transforma datos en acciones estratégicas para convertir la gestión de clientes en una ventaja competitiva sostenible.
