# LAB 5

# Variabilidad de la Frecuencia Cardíaca (HRV) y balance autonómico 

## Asignatura

Procesamiento Digital de Señales

## Programa

Ingeniería Biomédica – Universidad Militar Nueva Granada

## Práctica de laboratorio

**Variabilidad de la Frecuencia Cardíaca (HRV) y balance autonómico**

## Integrantes

Andrea Carolina Amórtegui Carrillo – Código 5600963


---

## Descripción

La práctica aborda el estudio de la variabilidad de la frecuencia cardíaca (HRV) mediante el procesamiento de señales ECG, con el propósito de evaluar la modulación autonómica cardíaca en condiciones de reposo y verbalización. A partir de la adquisición y depuración de la señal, se identifican los intervalos R-R y se analizan parámetros temporales y geométricos, incluyendo el diagrama de Poincaré y los índices CVI y CSI. Finalmente, se comparan las variaciones obtenidas entre ambos estados para interpretar los cambios en la actividad simpática y parasimpática del sistema nervioso autónomo.

---


## Metodología

La metodología se estructuró en etapas de adquisición, depuración y análisis de señales ECG para el estudio de la variabilidad cardíaca. Inicialmente, se registró la señal en condiciones de reposo y verbalización, asegurando parámetros adecuados de muestreo y cuantificación. Posteriormente, se implementó un filtrado digital IIR para atenuar interferencias y optimizar la calidad de la señal.

Luego, la señal procesada se segmentó en intervalos de dos minutos, permitiendo la detección de picos R y el cálculo de los intervalos R-R. Con esta información, se realizó el análisis temporal de la HRV mediante parámetros estadísticos básicos y, adicionalmente, se construyó el diagrama de Poincaré para evaluar la dinámica autonómica cardíaca.

Finalmente, se obtuvieron los índices CVI y CSI a partir de la dispersión de los intervalos R-R, comparando los resultados entre ambas condiciones experimentales para identificar modificaciones en la regulación simpática y parasimpática.

---
### Diagrama de flujo

<p align="center">
<img width="774" height="800" alt="image" src="https://github.com/user-attachments/assets/7ddebaf5-506e-45de-a269-b3f629e23daa" />
</p>
<p align="center">
  <em>Diagrama de flujo del codigo</em></p

---

### Parte A

---

### - Actividad simpática y parasimpática del sistema nervioso autónomo

El sistema nervioso autónomo (SNA) tiene dos partes principales: el sistema simpático y el parasimpático. El simpático se activa en situaciones de estrés, ejercicio o peligro, mientras que el parasimpático actúa principalmente durante el reposo y la digestión.

Una característica importante de ambos sistemas es que nunca están completamente "apagados": siempre mantienen un nivel mínimo de actividad, incluso en reposo. Esto es ventajoso porque significa que el cuerpo puede tanto aumentar como disminuir su respuesta según lo necesite. Si no existiera esta actividad de base, el sistema solo podría activarse, nunca reducir su acción.

La mayoría de los órganos internos, como el corazón, reciben señales de ambos sistemas al mismo tiempo, pero con efectos contrarios. Por ejemplo, el sistema simpático acelera el corazón liberando norepinefrina, mientras que el parasimpático lo frena liberando acetilcolina. Gracias a esta oposición coordinada, el organismo puede ajustar la función de sus órganos de forma rápida y precisa: cuando uno se activa más, el otro se inhibe, y viceversa.

Este equilibrio entre los dos sistemas no es fijo, sino que cambia constantemente según las circunstancias. En reposo y acostado predomina la actividad parasimpática, mientras que al ponerse de pie, hacer ejercicio o experimentar estrés, toma el control el sistema simpático. Precisamente estas variaciones en el balance autonómico son las que se pueden detectar y medir a través de la variabilidad de la frecuencia cardíaca (HRV), que es el método de análisis central propuesto por Toichi et al. (1997) y en el que se basa esta práctica.

### - Efecto de la actividad simpática y parasimpática en la frecuencia cardíaca

La frecuencia cardíaca es uno de los parámetros más sensibles al control autonómico, ya que el corazón recibe inervación directa de ambas divisiones del SNA. El nodo sinoauricular (SA), que actúa como marcapasos natural del corazón, es el principal punto de regulación de la frecuencia cardíaca y está sometido a la influencia continua tanto del sistema simpático como del parasimpático.

#### Efecto del sistema parasimpático

El nervio vago, principal representante de la división parasimpática a nivel cardíaco, libera acetilcolina sobre receptores muscarínicos M₂ en el nodo SA. Esto provoca una reducción en la velocidad de despolarización espontánea de las células marcapasos, lo que se traduce en una disminución de la frecuencia cardíaca, efecto conocido como bradicardia. Este mecanismo actúa de forma muy rápida, pudiendo modificar la frecuencia cardíaca en cuestión de milisegundos, lo que explica por qué las fluctuaciones rápidas del intervalo R–R, como las que ocurren con cada ciclo respiratorio, reflejan principalmente el tono vagal.

#### Efecto del sistema simpático

El sistema simpático libera norepinefrina sobre receptores β₁-adrenérgicos en el nodo SA. Esto incrementa la velocidad de despolarización espontánea de las células marcapasos, produciendo un aumento de la frecuencia cardíaca, conocido como taquicardia. A diferencia del parasimpático, la respuesta simpática es más lenta, con una latencia de varios segundos, ya que implica una cascada de señalización intracelular más compleja. Además de acelerar el corazón, el simpático también aumenta la fuerza de contracción del músculo cardíaco.

En condiciones normales, ambos sistemas actúan de manera simultánea y opuesta sobre el corazón, y la frecuencia cardíaca resultante refleja el equilibrio entre ellos. En reposo predomina el tono parasimpático, manteniendo la frecuencia cardíaca relativamente baja. Ante situaciones de estrés, ejercicio o cambios posturales como ponerse de pie, el sistema simpático gana protagonismo y la frecuencia cardíaca aumenta. Este balance dinámico es continuo y se refleja en las pequeñas variaciones que existen entre un latido y otro, es decir, en la variabilidad de la frecuencia cardíaca (HRV). Por esta razón, analizar la HRV permite estimar de manera indirecta y no invasiva el estado de actividad de cada rama del SNA.

### - Variabilidad de la frecuencia cardíaca (HRV) obtenida a partir de la señal electrocardiográfica (ECG)

La variabilidad de la frecuencia cardíaca (HRV, del inglés Heart Rate Variability) es la fluctuación natural que existe en el tiempo transcurrido entre latidos cardíacos consecutivos. Aunque a simple vista podría parecer que el corazón late de forma completamente regular, en realidad el intervalo entre un latido y el siguiente varía constantemente, y esa variación contiene información valiosa sobre el estado del sistema nervioso autónomo.

#### Obtención de la HRV a partir del ECG

La señal electrocardiográfica (ECG) registra la actividad eléctrica del corazón a lo largo del tiempo. Dentro de esta señal, el complejo QRS representa la despolarización ventricular, y su pico más prominente, la onda R, es el punto de referencia utilizado para medir el tiempo entre latidos. El intervalo entre dos ondas R consecutivas se denomina intervalo R–R y se expresa en milisegundos (ms). La secuencia de estos intervalos a lo largo del tiempo forma el llamado taquigrama o serie R–R, que es la base de todos los análisis de HRV.

Una HRV elevada generalmente indica un buen equilibrio autonómico y se asocia con buena salud cardiovascular. Por el contrario, una HRV reducida se ha relacionado con condiciones como diabetes, enfermedades cardíacas, estrés crónico y envejecimiento. Por esta razón, el análisis de la HRV se ha convertido en una herramienta ampliamente utilizada tanto en investigación como en la práctica clínica para evaluar de forma no invasiva el estado funcional del sistema nervioso autónomo.

### - Diagrama de Poincaré como herramienta de análisis de la serie R-R

El diagrama de Poincaré, es una herramienta de analisis que permite visualizar y cuantificar la dinámica de la serie R-R de forma geométrica para describir sistemas dinámicos no periódicos, en paricular de la variabilidad de la frecuencia cardíaca.

#### Construcción del diagrama

El diagrama se construye representando cada intervalo R–R en función del intervalo inmediatamente siguiente. Es decir, si la serie de intervalos consecutivos se denomina I₁, I₂, I₃, …, Iₙ, cada punto del diagrama corresponde al par ordenado (Iₖ, Iₖ₊₁), donde Iₖ es el intervalo actual e Iₖ₊₁ es el intervalo siguiente. Este proceso se repite para cada par consecutivo de la serie, generando una nube de puntos en un plano bidimensional.
En personas sanas, esta nube de puntos adopta característicamente una forma elipsoide orientada a lo largo de la diagonal del plano, conocida como línea identidad (Iₖ = Iₖ₊₁). La forma, el tamaño y la orientación de esta elipse contienen información directa sobre la dinámica autonómica del corazón.

#### Ejes de la elipse 

El eje transversal (T) es perpendicular a la línea identidad y refleja la variación latido a latido de los intervalos R–R. Cuando dos intervalos consecutivos difieren considerablemente entre sí, el punto correspondiente se aleja de la diagonal, lo que genera un eje T de mayor longitud. Esta variación rápida entre latidos está estrechamente relacionada con la actividad del sistema parasimpático, ya que el nervio vago puede modificar la frecuencia cardíaca de forma casi inmediata.

El eje longitudinal (L) es paralelo a la línea identidad y refleja la amplitud global de las fluctuaciones de la serie R–R a lo largo del tiempo. Cuando existe una variación lenta y de gran amplitud, los puntos se distribuyen ampliamente a lo largo de la diagonal, produciendo un eje L largo con un eje T relativamente corto. Este tipo de fluctuación está asociada a cambios más lentos en la frecuencia cardíaca, vinculados tanto al sistema simpático como a otros mecanismos de regulación de más largo plazo.

#### Índices autonómicos derivados

A partir de los ejes de la elipse se pueden calcular dos índices numéricos que permiten estimar de forma independiente la actividad de cada rama del sistema nervioso autónomo.

#### Índice Vagal Cardíaco (CVI)

Este índice representa el tamaño total de la elipse, ya que el producto L × T es proporcional a su área. Cuando la actividad parasimpática es alta, la variabilidad entre latidos es grande y la elipse ocupa mayor área en el diagrama, resultando en un CVI elevado. Por el contrario, cuando el tono vagal disminuye, la elipse se contrae y el CVI se reduce. Se utiliza el logaritmo en base 10 porque los valores del producto L × T pueden abarcar un rango muy amplio, y la transformación logarítmica permite manejar estos datos de forma más estable estadísticamente.

#### Índice Simpático Cardíaco (CSI)

Este índice representa la elongación de la elipse, es decir, qué tan larga y estrecha es en comparación con su ancho. Cuando la actividad simpática es elevada, las fluctuaciones lentas de la frecuencia cardíaca predominan sobre las rápidas, haciendo que la elipse se alargue a lo largo de la diagonal pero se estreche en sentido perpendicular, aumentando así el valor del cociente L/T. Cuando el tono simpático es bajo, la elipse tiende a ser más redondeada y el CSI disminuye.
La principal ventaja de estos dos índices es que son independientes entre sí: el CVI puede cambiar sin que lo haga el CSI y viceversa, lo que permite evaluar ambas ramas del sistema nervioso autónomo de forma simultánea y separada a partir de una única medición.

### - Variabilidad de la frecuencia cardíaca y balance autonómico

La variabilidad de la frecuencia cardíaca y el balance autonómico son dos conceptos estrechamente relacionados, ya que la HRV es precisamente el reflejo cuantitativo del equilibrio dinámico entre las dos ramas del sistema nervioso autónomo sobre el corazón.

El balance autonómico no es un estado fijo sino una condición en permanente ajuste. En cada momento, el corazón recibe simultáneamente señales del sistema simpático y del parasimpático, y la frecuencia cardíaca resultante depende de cuál de los dos predomina en ese instante. Cuando ambos sistemas están equilibrados y activos, el corazón presenta una variabilidad alta entre latidos, lo que se traduce en una HRV elevada. Cuando uno de los dos sistemas domina de forma sostenida sobre el otro, la variabilidad tiende a reducirse.

Desde esta perspectiva, una HRV alta no significa que el corazón sea irregular o inestable, sino todo lo contrario: indica que el sistema nervioso autónomo está respondiendo de forma flexible y apropiada a los cambios del entorno. Esta capacidad de adaptación es una señal de buena salud cardiovascular y de un sistema autonómico funcionalmente íntegro.

Por otro lado, una HRV baja sugiere una reducción en la capacidad del sistema nervioso autónomo para modular la frecuencia cardíaca, lo que puede deberse a un predominio sostenido del tono simpático, a una disminución de la actividad vagal, o a ambas cosas simultáneamente. Esta condición se ha asociado con diversas situaciones fisiopatológicas como el estrés crónico, el envejecimiento, la diabetes, las enfermedades cardiovasculares y los trastornos del sistema nervioso autónomo.

El análisis de la HRV permite entonces estimar indirectamente el estado del balance autonómico sin necesidad de ningún procedimiento invasivo. Dependiendo del método de análisis utilizado, es posible obtener información sobre la actividad vagal de forma aislada, sobre la actividad simpática, o sobre la interacción entre ambas. Esta es precisamente la razón por la que la HRV se ha consolidado como una herramienta valiosa tanto en la investigación fisiológica como en la evaluación clínica del sistema nervioso autónomo.

### Adquisición de la señal ECG

Una mujer de 19 años fue elegida como sujeto de prueba para llevar a cabo la adquisición de la señal electrocardiográfica. El registro tuvo una duración total de 4 minutos, estructurados en dos etapas: en la primera, correspondiente a los 2 minutos iniciales, la participante permaneció en reposo absoluto, sin hablar ni realizar ningún tipo de movimiento; en la segunda etapa, durante los 2 minutos finales, se le pidió leer en voz alta un fragmento de texto que el equipo había seleccionado previamente.
Para la captura de la señal se empleó el dispositivo BITalino, al cual se conectaron los electrodos siguiendo una disposición determinada. El electrodo del cable blanco fue colocado en la zona subclavicular izquierda, el del cable negro en la zona subclavicular derecha y el del cable rojo sobre las costillas del lado derecho.

<p align="center">
<img width="1024" height="651" alt="image" src="https://github.com/user-attachments/assets/8b32745a-8fff-4704-9f4a-84c38872eceb" />
</p>
<p align="center">
  <em>Colocación de electrodos</em></p

La señal original se muestrea a 1000 Hz, es decir, 1000 muestras por segundo. Esta frecuencia de muestreo es adecuada para el análisis de ECG, ya que cumple con el teorema de Nyquist para la banda de interés del filtro utilizado (0.5–40 Hz), y permite una resolución temporal de 1 ms entre muestras.

<p align="center">
<img width="1905" height="512" alt="image" src="https://github.com/user-attachments/assets/bb60221e-a7aa-412c-8d0c-84dc0f2a73ee" />
</p>
<p align="center">
  <em>Señal original</em></p

---

### Parte B

Inicialmente, se implementó una función para la carga de la señal ECG desde un archivo de texto, 
descartando líneas vacías o comentarios y seleccionando el canal correspondiente a la señal cardíaca. 
Posteriormente, los datos fueron convertidos a milivoltios considerando el voltaje de referencia del 
sistema y la ganancia empleada durante la adquisición.

## Filtrado de la señal

Se diseñó un filtro digital Butterworth pasa-banda de cuarto orden con frecuencias de corte entre 
0.5 Hz y 40 Hz, rango adecuado para señales electrocardiográficas. Este procedimiento permitió 
reducir componentes de ruido de baja y alta frecuencia, conservando las características principales 
del ECG.

```python
def disenar_filtro(fs, f_low=0.5, f_high=40.0, orden=4):
    nyq = fs / 2.0
    b, a = signal.butter(
        orden,
        [f_low/nyq, f_high/nyq],
        btype='band'
    )
```

La implementación del filtro se llevó a cabo utilizando la función `butter()` de la librería 
`scipy.signal`, la cual calcula los coeficientes del filtro digital en función de la frecuencia 
de muestreo y las frecuencias de corte establecidas. Posteriormente, el filtrado fue aplicado 
mediante la función `lfilter()`, obteniendo una señal ECG acondicionada para las etapas 
posteriores de análisis.

```python
def filtrar(ecg_mv, b, a):
    return signal.lfilter(b, a, ecg_mv)
```

<p align="center">
<img width="1905" height="512" alt="image" src="https://github.com/user-attachments/assets/bb60221e-a7aa-412c-8d0c-84dc0f2a73ee" />
</p>
<p align="center"><em>Señal original vs señal filtrada</em></p>

## Ecuación en diferencias

La ecuación en diferencias se obtuvo a partir de los coeficientes del filtro digital Butterworth 
calculados mediante la función `signal.butter()`. Esta función entrega dos conjuntos de coeficientes:

- **b:** coeficientes asociados a las entradas de la señal x[n]
- **a:** coeficientes asociados a las salidas anteriores y[n]

Con estos coeficientes se construyó la representación matemática discreta del filtro, expresando 
cada muestra de salida como una combinación lineal de muestras actuales y anteriores de la señal 
de entrada y de la propia salida.

```python
lhs = " + ".join(
    f"({b[k]:.8f})·x[n-{k}]"
    for k in range(len(b))
    if abs(b[k]) > 1e-12
)

rhs = " + ".join(
    f"({a[k]:.8f})·y[n-{k}]"
    for k in range(1, len(a))
    if abs(a[k]) > 1e-12
)

print(f"y[n] = {lhs}")
if rhs:
    print(f"     - {rhs}")
```

<p align="center">
<img width="1765" height="96" alt="image" src="https://github.com/user-attachments/assets/d846b9fd-c9a0-43aa-97cd-36c891552e11" />
</p>
<p align="center"><em>Ecuación en diferencias</em></p>

La ecuación en diferencias representa la implementación discreta del filtro digital IIR diseñado 
para el acondicionamiento de la señal ECG. En esta expresión, cada muestra de salida depende tanto 
de muestras actuales y pasadas de la señal de entrada como de salidas anteriores del sistema, 
permitiendo modelar el comportamiento dinámico del filtro de manera recursiva.

## Respuesta en frecuencia

<p align="center">
<img width="1484" height="717" alt="image" src="https://github.com/user-attachments/assets/1d5b5cef-5bd9-44d0-9df7-2351f29aaab2" />
</p>
<p align="center"><em>Respuesta en frecuencia del filtro pasa-banda</em></p>

Respuesta en frecuencia del filtro Butterworth pasa-banda de orden 4 aplicado a la señal ECG. 
La línea discontinua roja indica la frecuencia de corte inferior (0.5 Hz) y la línea discontinua 
azul indica la frecuencia de corte superior (40 Hz). La banda de paso comprendida entre ambas 
frecuencias presenta una atenuación cercana a 0 dB, lo que indica que las componentes frecuenciales 
de interés clínico del ECG se conservan sin distorsión significativa, mientras que las frecuencias 
fuera de este rango son atenuadas progresivamente.

## Segmentación de la señal

La señal filtrada fue dividida en dos segmentos de dos minutos cada uno, correspondientes a las 
condiciones de reposo y verbalización.

```python
tiempo_limite = 2 * 60                    
indice_2min   = int(tiempo_limite * FS)   

seg1 = ecg_filt[:indice_2min]            # Segmento 1 — Reposo (0–2 min)
seg2 = ecg_filt[indice_2min:]            # Segmento 2 — Lectura en voz alta (2–4 min)
```

<p align="center">
<img width="1908" height="929" alt="image" src="https://github.com/user-attachments/assets/58bfd394-0681-492c-a5a9-9c61efb443d7" />
</p>
<p align="center"><em>Segmento 1 y segmento 2</em></p>

El segmento superior corresponde a los primeros 2 minutos de registro, durante los cuales la 
participante permaneció en reposo completo y en silencio. El segmento inferior corresponde a los 
2 minutos siguientes, durante los cuales la participante realizó lectura en voz alta. En ambos 
segmentos se observa la morfología característica del ECG con los complejos QRS claramente 
identificables a lo largo del tiempo.

## Detección de picos R e intervalos R-R

Para la detección de los picos R en cada segmento se utilizó la función `find_peaks()` de 
`scipy.signal`, aplicando criterios de altura mínima, distancia mínima entre picos y prominencia 
para garantizar que únicamente se detectaran los picos R y no otras ondas del complejo ECG.

```python
def detectar_picos(seg, fs):
    media          = np.mean(seg)
    std            = np.std(seg)
    rango          = seg.max() - seg.min()
    height_min     = media + 1.5 * std
    dist_min       = int(0.40 * fs)
    prominence_min = 0.4 * rango

    picos, _ = signal.find_peaks(
        seg,
        height     = height_min,
        distance   = dist_min,
        prominence = prominence_min
    )
    return picos

picos1 = detectar_picos(seg1, fs)
picos2 = detectar_picos(seg2, fs)
```

<p align="center">
<img width="1904" height="870" alt="image" src="https://github.com/user-attachments/assets/1069ac40-180d-480f-accf-01f69277e836" />
</p>
<p align="center"><em>Detección de picos R — Segmento 1</em></p>

Detección de picos R sobre el segmento 1 de la señal ECG filtrada, correspondiente a los primeros 
2 minutos de registro en condición de reposo. Los triángulos rojos indican la posición temporal de 
cada pico R detectado. La distribución uniforme y regular de los picos refleja una frecuencia 
cardíaca estable, característica del estado de reposo en el que predomina el tono parasimpático.

<p align="center">
<img width="1909" height="865" alt="image" src="https://github.com/user-attachments/assets/c7f6693e-8c23-4484-913b-dbd5252d1c73" />
</p>
<p align="center"><em>Detección de picos R — Segmento 2</em></p>

Detección de picos R sobre el segmento 2 de la señal ECG filtrada, correspondiente a los 2 minutos 
de registro en condición de lectura en voz alta. En comparación con el segmento 1, se observa una 
mayor irregularidad en el espaciado entre picos, lo que sugiere una mayor variabilidad en los 
intervalos R-R asociada al esfuerzo cognitivo y vocal que implica la verbalización, condición que 
activa el sistema nervioso simpático y reduce el predominio vagal característico del reposo.

A partir de los picos R detectados se calcularon los intervalos R-R de cada segmento, expresados 
en milisegundos, mediante la diferencia entre muestras consecutivas escalada por la frecuencia 
de muestreo.

```python
rr1 = np.diff(picos1) / fs * 1000   # Segmento 1 — Reposo [ms]
rr2 = np.diff(picos2) / fs * 1000   # Segmento 2 — Lectura en voz alta [ms]
```

<p align="center">
<img width="1809" height="907" alt="image" src="https://github.com/user-attachments/assets/a7c2bd65-de47-4c80-9632-7a49bc880ca5" />
</p>
<p align="center"><em>Intervalos R-R</em></p>

La segmentación y el análisis de los intervalos R-R permitieron estudiar las variaciones de la 
frecuencia cardíaca bajo diferentes estados fisiológicos, estableciendo la base para el análisis 
de la HRV en el dominio del tiempo y el diagrama de Poincaré.

---

### Parte C 

#### Análisis de la HRV en el dominio del tiempo

Para el análisis de la variabilidad de la frecuencia cardíaca en el dominio del tiempo se 
calcularon los parámetros estadísticos básicos de la serie R-R para cada segmento: la media, 
la desviación estándar (SDNN), la raíz cuadrada de la media de las diferencias al cuadrado 
(RMSSD), el porcentaje de intervalos consecutivos que difieren más de 50 ms (pNN50) y el 
coeficiente de variación (CV).

```python
def hrv_dominio_tiempo(rr1, rr2):
    def calcular(rr, nombre):
        media  = np.mean(rr)
        sdnn   = np.std(rr, ddof=1)
        rmssd  = np.sqrt(np.mean(np.diff(rr)**2))
        pnn50  = np.sum(np.abs(np.diff(rr)) > 50) / len(np.diff(rr)) * 100
        cv     = sdnn / media * 100
        return {'nombre': nombre, 'media': media, 'sdnn': sdnn,
                'rmssd': rmssd, 'pnn50': pnn50, 'cv': cv}

    p1 = calcular(rr1, "Segmento 1 (0–2 min)")
    p2 = calcular(rr2, "Segmento 2 (2–4 min)")
    return p1, p2
```

<p align="center">
<img width="1658" height="642" alt="image" src="https://github.com/user-attachments/assets/78cde75e-1ac8-4ff8-864e-13a057b7e629" />

</p>
<p align="center"><em>Comparación HRV dominio del tiempo entre ambos segmentos</em></p>

La media del intervalo R-R fue de 682.7 ms en el segmento 1 y de 647.3 ms en el segmento 2, 
lo que indica un aumento de la frecuencia cardíaca durante la lectura en voz alta. La SDNN 
fue ligeramente mayor en el segmento 2 (39.6 ms) respecto al segmento 1 (35.2 ms), mientras 
que el RMSSD fue mayor en el segmento 1 (27.8 ms) frente al segmento 2 (25.4 ms), lo que 
sugiere una mayor actividad vagal en condición de reposo. El pNN50 también fue mayor en el 
segmento 1 (6.4%) que en el segmento 2 (4.9%), reforzando esta tendencia. El CV fue de 5.2% 
en reposo y de 6.1% en lectura, reflejando una ligera mayor variabilidad relativa durante 
la verbalización.

## Diagrama de Poincaré

El diagrama de Poincaré se construyó representando cada intervalo R-R en función del intervalo 
siguiente, generando una nube de puntos cuya forma elipsoide permite estimar de manera 
independiente la actividad simpática y parasimpática del sistema nervioso autónomo. A partir 
de los ejes de la elipse se calcularon los índices CVI y CSI propuestos por Toichi et al. (1997).

```python
def poincare_indices(rr_ms, nombre="Seg"):
    rr_n  = rr_ms[:-1]
    rr_n1 = rr_ms[1:]
    x_rot = (rr_n + rr_n1) / np.sqrt(2)   # Eje longitudinal
    y_rot = (rr_n - rr_n1) / np.sqrt(2)   # Eje transversal

    L = 4 * np.std(x_rot, ddof=1)          # Longitud eje longitudinal
    T = 4 * np.std(y_rot, ddof=1)          # Longitud eje transversal

    CVI = np.log10(L * T)                  # Índice vagal cardíaco
    CSI = L / T                            # Índice simpático cardíaco
```

<p align="center">
<img width="1810" height="770" alt="image" src="https://github.com/user-attachments/assets/5440e4d2-ab7d-47a6-b9ad-33c04a851ee3" />
</p>
<p align="center"><em>Diagrama de Poincaré — Segmento 1 y Segmento 2</em></p>

En el segmento 1 (reposo) se obtuvo L = 183.1 ms y T = 79.0 ms, con un CVI de 4.160 y un 
CSI de 2.318. En el segmento 2 (lectura en voz alta) se obtuvo L = 212.0 ms y T = 72.2 ms, 
con un CVI de 4.185 y un CSI de 2.937. La elipse del segmento 2 es más larga y estrecha que 
la del segmento 1, lo que visualmente confirma el incremento del tono simpático durante la 
verbalización. Cada punto del diagrama corresponde al par (RR[n], RR[n+1]) y la línea 
punteada representa la línea identidad donde RR[n] = RR[n+1].

## Comparación CVI y CSI entre segmentos

<p align="center">
<img width="1286" height="634" alt="image" src="https://github.com/user-attachments/assets/8007215f-ebcd-4095-8bd9-07ce2390a12e" />
</p>
<p align="center"><em>Comparación CVI y CSI entre segmentos (Toichi et al., 1997)</em></p>

El índice vagal cardíaco (CVI = log₁₀(L × T)) presentó valores similares entre ambos 
segmentos: 4.160 en reposo y 4.185 en lectura en voz alta, lo que indica que la actividad 
parasimpática no tuvo una variación significativa entre condiciones. Por otro lado, el índice 
simpático cardíaco (CSI = L/T) mostró un incremento notable, pasando de 2.318 en el segmento 
1 a 2.937 en el segmento 2, lo que evidencia una mayor activación del sistema nervioso 
simpático durante la verbalización. Estos resultados son consistentes con lo esperado 
fisiológicamente, ya que la lectura en voz alta representa una tarea que demanda mayor 
esfuerzo cognitivo y motor, favoreciendo el predominio simpático sobre el parasimpático.

---

## Discusión y analisis de resultados

En el dominio del tiempo, la media del intervalo R-R disminuyó de 682.7 ms en el segmento 1 a 647.3 ms en el segmento 2, lo que corresponde a un aumento de la frecuencia cardíaca durante la verbalización. Esta reducción es consistente con una mayor activación simpática, ya que el sistema nervioso simpático acelera el corazón al liberar norepinefrina sobre los receptores β1-adrenérgicos del nodo sinoauricular. Por otro lado, el RMSSD y el pNN50, parámetros que reflejan predominantemente la actividad vagal de corto plazo, fueron mayores en el segmento de reposo (27.8 ms y 6.4% respectivamente) que en el segmento de lectura (25.4 ms y 4.9%), lo que indica una mayor influencia parasimpática durante el reposo. La SDNN fue ligeramente mayor en el segmento 2 (39.6 ms frente a 35.2 ms), lo que podría explicarse por la mayor variabilidad global introducida por el esfuerzo respiratorio y vocal durante la lectura.

En el análisis no lineal mediante el diagrama de Poincaré, el segmento 1 presentó una elipse con L = 183.1 ms y T = 79.0 ms, mientras que en el segmento 2 los valores fueron L = 212.0 ms y T = 72.2 ms. El alargamiento del eje longitudinal y la reducción del eje transversal en el segmento 2 generan una elipse más estrecha y elongada, lo que es característico de un mayor predominio simpático. Esto se confirma con el índice simpático cardíaco (CSI), que aumentó de 2.318 en reposo a 2.937 durante la lectura, representando un incremento del 26.7%. El índice vagal cardíaco (CVI), en cambio, se mantuvo prácticamente constante entre ambas condiciones (4.160 vs 4.185), lo que sugiere que la actividad parasimpática no experimentó cambios significativos y que el desplazamiento del balance autonómico hacia el sistema simpático durante la verbalización se produjo principalmente por una activación adicional de este último, más que por una inhibición del tono vagal.

En conjunto, los resultados del dominio del tiempo y el diagrama de Poincaré son coherentes entre sí y con lo esperado fisiológicamente: la lectura en voz alta, al implicar esfuerzo cognitivo, control respiratorio y actividad motora vocal, activa el sistema nervioso simpático y produce un desplazamiento del balance autonómico respecto al estado de reposo.


---

## Conclusiones

La práctica permitió identificar y cuantificar cambios en el balance autonómico cardíaco a partir del análisis de la variabilidad de la frecuencia cardíaca obtenida de una señal ECG real adquirida con el dispositivo BITalino. El procesamiento digital de la señal, mediante el diseño e implementación de un filtro Butterworth pasa-banda de cuarto orden con frecuencias de corte entre 0.5 Hz y 40 Hz, permitió acondicionar adecuadamente la señal para la detección de los picos R y el cálculo de los intervalos R-R.

El análisis en el dominio del tiempo evidenció una reducción de la media del intervalo R-R y de los parámetros asociados a la actividad vagal (RMSSD y pNN50) durante la condición de lectura en voz alta, en comparación con el reposo. Estos hallazgos son consistentes con una mayor activación simpática durante la verbalización.

El diagrama de Poincaré y los índices derivados de Toichi et al. (1997) confirmaron estos resultados de manera independiente: el CSI aumentó considerablemente en el segmento de lectura, reflejando mayor actividad simpática, mientras que el CVI se mantuvo estable entre ambas condiciones, indicando que el tono parasimpático no varió de forma significativa. La capacidad del diagrama de Poincaré para separar la contribución simpática y parasimpática en una sola medición, sin necesidad de respiración controlada ni registros prolongados, representa una ventaja importante frente a otros métodos de análisis de HRV.

Finalmente, los resultados obtenidos validan el uso del análisis de HRV como herramienta no invasiva para la evaluación del balance autonómico en diferentes condiciones fisiológicas, y respaldan la utilidad clínica y experimental del diagrama de Poincaré como método confiable y de fácil interpretación para este propósito.

