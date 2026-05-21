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

## Explicación del código

En esta sección se explica el funcionamiento del código, en el cual se emplean herramientas de programación en Python para el procesamiento y análisis de señales electromiográficas (EMG). Se destaca la importancia del análisis espectral mediante la Transformada Rápida de Fourier (FFT) y métodos como Welch, así como la aplicación de filtros digitales pasa banda implementados con funciones de la librería scipy.signal. Adicionalmente, se utilizan librerías como NumPy, Pandas y Matplotlib para la manipulación de datos, el cálculo de parámetros relevantes en el dominio de la frecuencia y la visualización de la señal, permitiendo evaluar el comportamiento espectral y su relación con la fatiga muscular.

### Importación de librerías

Primero se importan las librerías necesarias para la lectura, procesamiento y análisis de las señales electromiográficas:

* `numpy `: Se utiliza para el manejo de arreglos numéricos y la realización de operaciones matemáticas como promedios, sumas, aplicación de ventanas (Hamming) y cálculos espectrales.

* `matplotlib.pyplot `: Se emplea para la generación de gráficas en el dominio del tiempo y de la frecuencia, permitiendo visualizar la señal EMG original, filtrada y sus espectros.

* `pandas `: Permite la lectura y manipulación de archivos de datos, especialmente en formato `.txt `, facilitando la organización y extracción de la señal adquirida.

* `scipy.fft (fft, fftfreq) `: Se utiliza para calcular la Transformada Rápida de Fourier (FFT) y obtener el espectro de frecuencias de la señal.

* `scipy.signal (butter, filtfilt, welch) `:

* `butter `:Se usa para diseñar filtros digitales tipo Butterworth, tanto pasa bajos como pasa banda, adecuados para el procesamiento de señales EMG.

* `filtfilt `: Aplica el filtrado de forma bidireccional, evitando desfases en la señal.

* `welch `: Permite estimar la densidad espectral de potencia (PSD), útil para el análisis comparativo del contenido frecuencial en diferentes segmentos de la señal.

Estas herramientas son fundamentales para el procesamiento digital de señales electromiográficas, ya que permiten el acondicionamiento de la señal, su análisis en el dominio del tiempo y la frecuencia, y la extracción de características relevantes para evaluar la fatiga muscular.

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


---

### Parte B

Inicialmente, se implementó una función para la carga de la señal ECG desde un archivo de texto, descartando líneas vacías o comentarios y seleccionando el canal correspondiente a la señal cardíaca. Posteriormente, los datos fueron convertidos a milivoltios considerando el voltaje de referencia del sistema y la ganancia empleada durante la adquisición.

Luego, se diseñó un filtro digital Butterworth pasa-banda de cuarto orden con frecuencias de corte entre 0.5 Hz y 40 Hz, rango adecuado para señales electrocardiográficas. Este procedimiento permitió reducir componentes de ruido de baja y alta frecuencia, conservando las características principales del ECG. Adicionalmente, se obtuvo la ecuación en diferencias del filtro con el fin de representar matemáticamente su implementación discreta.

Posteriormente, el filtrado fue aplicado mediante procesamiento digital de señales utilizando la función lfilter, obteniendo una señal ECG acondicionada para las etapas posteriores de análisis. Finalmente, se implementó un algoritmo de detección de picos R basado en criterios estadísticos de amplitud, prominencia y distancia mínima entre picos, permitiendo calcular los intervalos R-R y parámetros básicos asociados a la frecuencia cardíaca.

El filtro se realizó mediante un filtro digital Butterworth pasa-banda de cuarto orden, diseñado para conservar únicamente las frecuencias características de la señal ECG entre 0.5 Hz y 40 Hz, eliminando componentes de ruido de baja y alta frecuencia.


```python
def disenar_filtro(fs, f_low=0.5, f_high=40.0, orden=4):
    nyq = fs / 2.0
    b, a = signal.butter(
        orden,
        [f_low/nyq, f_high/nyq],
        btype='band'
    )
```
La implementación del filtro se llevó a cabo utilizando la función butter() de la librería scipy.signal, la cual calcula los coeficientes del filtro digital en función de la frecuencia de muestreo y las frecuencias de corte establecidas.

<p align="center">
<img width="1905" height="512" alt="image" src="https://github.com/user-attachments/assets/bb60221e-a7aa-412c-8d0c-84dc0f2a73ee" />
</p>
<p align="center">
  <em>Señal original vs Señal filtrada</em></p

La ecuación en diferencias se obtuvo a partir de los coeficientes del filtro digital Butterworth calculados mediante la función signal.butter(). Esta función entrega dos conjuntos de coeficientes:

- b: coeficientes asociados a las entradas de la señal x[n],
- a: coeficientes asociados a las salidas anteriores y[n].

Con estos coeficientes se construyó la representación matemática discreta del filtro, expresando cada muestra de salida como una combinación lineal de muestras actuales y anteriores de la señal de entrada y de la propia salida.

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
<p align="center">
  <em>Ecuación de diferencias</em></p

La ecuación en diferencias representa la implementación discreta del filtro digital IIR diseñado para el acondicionamiento de la señal ECG. En esta expresión, cada muestra de salida depende tanto de muestras actuales y pasadas de la señal de entrada como de salidas anteriores del sistema. Esto permite modelar el comportamiento dinámico del filtro y realizar el procesamiento digital de la señal de manera recursiva.

Después del diseño del filtro Butterworth pasa-banda, este fue aplicado a la señal ECG utilizando parámetros iniciales iguales a cero mediante la función lfilter() de la librería scipy.signal. La implementación se realizó con el siguiente fragmento de código:

```python
def filtrar(ecg_mv, b, a):
    return signal.lfilter(b, a, ecg_mv)
```
Posteriormente, la señal filtrada fue dividida en dos segmentos de dos minutos cada uno, correspondientes a las condiciones de reposo y verbalización.

```python
seg1 = ecg_filtrada[:2*60*fs]
seg2 = ecg_filtrada[2*60*fs:4*60*fs]
```

La segmentación permitió analizar las variaciones de la frecuencia cardíaca bajo diferentes estados fisiológicos.

<p align="center">
<img width="1809" height="907" alt="image" src="https://github.com/user-attachments/assets/a7c2bd65-de47-4c80-9632-7a49bc880ca5" />
</p>
<p align="center">
  <em>Intervalos R-R</em></p

La frecuencia media inicia alrededor de valores cercanos a 120–125 Hz y presenta una disminución progresiva hasta valores próximos a 115 Hz. Esta tendencia se confirma con la pendiente negativa de la regresión lineal (-0.06 Hz/s), lo que indica una reducción gradual del contenido frecuencial de la señal.

Por su parte, la frecuencia mediana presenta una disminución más pronunciada, pasando aproximadamente de 90 Hz a valores cercanos a 80 Hz. La pendiente de la regresión (-0.11 Hz/s) es más negativa que la de la frecuencia media, lo que sugiere que este parámetro es más sensible a los cambios asociados a la fatiga.

Además de la tendencia general, se observan fluctuaciones locales en ambas curvas, las cuales pueden atribuirse a variaciones en la activación de las unidades motoras durante la contracción. Sin embargo, estas variaciones no afectan la tendencia global descendente.

Desde el punto de vista fisiológico, este comportamiento se relaciona con la disminución en la velocidad de conducción de las fibras musculares y la fatiga progresiva de las unidades motoras. Como consecuencia, el contenido espectral de la señal se desplaza hacia frecuencias más bajas, lo cual se refleja directamente en la disminución de la frecuencia media y mediana.

En conjunto, estos resultados evidencian que el análisis espectral de la señal EMG permite identificar de manera efectiva la aparición de fatiga muscular durante contracciones sostenidas.

Adicionalmente, se realizó un ajuste mediante regresión lineal sobre estos parámetros, con el fin de identificar tendencias en su comportamiento. La pendiente obtenida proporciona una medida cuantitativa del cambio en el contenido frecuencial de la señal.

Para complementar el análisis, se seleccionaron tres segmentos representativos de la señal (inicio, mitad y final), sobre los cuales se calculó la FFT. Esto permitió comparar directamente el contenido espectral en diferentes momentos de la contracción.

```python
X_seg = np.abs(fft(xi))
```
<p align="center">
<img width="1297" height="621" alt="image" src="https://github.com/user-attachments/assets/0a66fea1-e741-46e0-b7de-52e7cea56a1f" />
</p>
<p align="center">
  <em>Transformada de fourier en segmento</em></p
 
Se identificó la frecuencia pico en cada uno de estos segmentos, evidenciando un desplazamiento hacia frecuencias más bajas a medida que avanza la contracción. Este comportamiento es característico de la fatiga muscular y refleja cambios en la activación de las fibras musculares.


#### Análisis de los resultados

Los resultados obtenidos a partir del análisis espectral de la señal electromiográfica evidencian una variación progresiva en los parámetros de frecuencia media y frecuencia mediana a lo largo del tiempo. Esta variación se manifiesta principalmente como una tendencia decreciente, lo que indica un desplazamiento del contenido espectral hacia frecuencias más bajas.

Este comportamiento puede explicarse por diferentes factores asociados a la fisiología de la fatiga muscular:

- Disminución en la velocidad de conducción de las fibras musculares.
- Fatiga progresiva de las unidades motoras durante la contracción sostenida.
- Cambios en el reclutamiento y sincronización de las fibras musculares.
- Acumulación de metabolitos como el lactato, que afectan la eficiencia del músculo.

Además, se observaron fluctuaciones en los valores de frecuencia a lo largo del tiempo, las cuales pueden atribuirse a:

- Variabilidad natural en la activación muscular.
- Presencia de ruido en la señal, incluso después del filtrado.
- Movimiento o leve variación en la posición de los electrodos.
- Limitaciones propias del proceso de segmentación de la señal.

Es importante considerar que el cálculo de la frecuencia media y mediana depende de la calidad del espectro obtenido. Factores como la longitud de las ventanas, el solapamiento y el filtrado aplicado pueden influir en la precisión de estos parámetros.

En conjunto, los resultados muestran que el análisis en el dominio de la frecuencia es una herramienta útil para identificar la fatiga muscular, permitiendo observar de manera objetiva la disminución del contenido de altas frecuencias en la señal EMG durante contracciones sostenidas.

---

### Parte C 
En esta sección se realizó un análisis espectral detallado de la señal electromiográfica (EMG) mediante la aplicación de la Transformada Rápida de Fourier (FFT), con el objetivo de observar cómo varía el contenido frecuencial a lo largo de una contracción muscular sostenida.

Se seleccionaron segmentos representativos de la señal correspondientes al inicio, la mitad y el final de la contracción. A cada uno de estos segmentos se le aplicó una ventana de Hamming para reducir efectos de fuga espectral y mejorar la estimación del contenido en frecuencia.

```python
xi = xi * np.hamming(N_seg)
X_seg = np.abs(fft(xi))
```

Para cada segmento se calculó el espectro de magnitud considerando únicamente el rango de frecuencias entre 20 y 450 Hz, correspondiente a la actividad muscular. Estos espectros fueron representados en escala logarítmica, lo que permitió una mejor visualización de las diferencias en la distribución de energía.

La comparación entre los espectros mostró una disminución progresiva del contenido de altas frecuencias a medida que avanza la contracción. En el segmento inicial se observa una mayor presencia de componentes de alta frecuencia, mientras que en el segmento final estas componentes se reducen notablemente.

Adicionalmente, se identificó la frecuencia pico en cada segmento, evidenciando un desplazamiento hacia valores más bajos con el paso del tiempo. Este comportamiento es consistente con la aparición de fatiga muscular.

```python
f_pico = freqs_s[np.argmax(X_seg)]
```
Estos resultados reflejan cambios fisiológicos en el músculo, como la disminución en la velocidad de conducción de las fibras musculares y la fatiga de las unidades motoras, lo cual provoca una redistribución del contenido espectral hacia frecuencias más bajas.

En conjunto, el análisis mediante FFT confirma que el estudio en el dominio de la frecuencia es una herramienta eficaz para detectar y caracterizar la fatiga muscular en señales electromiográficas.

<p align="center">
<img width="889" height="650" alt="image" src="https://github.com/user-attachments/assets/152a0590-d05f-4898-8665-7e8b1db76e07" />
</p>
<p align="center">
  <em>Desplazamiento del pico espectral</em></p
                                              
La gráfica muestra la variación de la frecuencia pico de la señal electromiográfica en tres momentos representativos de la contracción muscular: inicio, mitad y final. Se observa una disminución marcada desde aproximadamente 83.7 Hz al inicio hasta valores cercanos a 47 Hz en la mitad y el final de la contracción.

Este comportamiento evidencia un desplazamiento significativo del contenido espectral hacia frecuencias más bajas, especialmente en la primera mitad del esfuerzo. La caída abrupta entre el inicio y la mitad sugiere que el proceso de fatiga muscular comienza de forma temprana durante la contracción sostenida.

Entre la mitad y el final, la frecuencia pico se mantiene relativamente estable (47.3 Hz a 47.0 Hz), lo que indica que el músculo ya se encuentra en un estado de fatiga más avanzado, donde los cambios en la actividad eléctrica son menos pronunciados.

Desde el punto de vista fisiológico, este fenómeno se asocia con:

- La disminución en la velocidad de conducción de las fibras musculares.
- La fatiga de las unidades motoras activas.
- Una mayor sincronización de las fibras, lo que favorece componentes de menor frecuencia.

En conjunto, la gráfica confirma que la frecuencia pico es un indicador sensible de la fatiga muscular, mostrando una reducción clara a medida que progresa la contracción. Además, complementa los resultados obtenidos con la frecuencia media y mediana, reforzando la evidencia del desplazamiento espectral hacia bajas frecuencias.

---
### Preguntas para la discusión

#### 1. ¿Cambian los valores de frecuencia media y mediana a medida que el músculo se acerca a la fatiga? ¿A qué podría atribuirse este cambio?

Sí, los valores de frecuencia media y frecuencia mediana presentan una disminución progresiva a medida que el músculo se acerca a la fatiga. Este comportamiento se evidencia en las gráficas obtenidas, donde ambas frecuencias muestran una tendencia descendente a lo largo del tiempo.

Este cambio se atribuye principalmente a factores fisiológicos asociados al proceso de fatiga muscular, entre los que se destacan:

- La disminución en la velocidad de conducción de las fibras musculares.
- La fatiga de las unidades motoras, que reduce su capacidad de activación eficiente.
- La acumulación de metabolitos (como el lactato), que afecta el funcionamiento del músculo.
- Cambios en el reclutamiento y sincronización de las fibras musculares.

Como consecuencia, el contenido espectral de la señal electromiográfica se desplaza hacia frecuencias más bajas, lo que se refleja directamente en la reducción de la frecuencia media y mediana.


---

#### 2. ¿Cómo justifica el uso de herramientas como la transformada de Fourier en escenarios como, por ejemplo, terapias de rehabilitación?

El uso de herramientas como la Transformada de Fourier se justifica porque permite analizar la señal en el dominio de la frecuencia, proporcionando información que no es fácilmente observable en el dominio del tiempo.

En el contexto de terapias de rehabilitación, este tipo de análisis resulta especialmente útil, ya que permite detectar la fatiga muscular de forma objetiva mediante el estudio de cambios en las frecuencias de la señal. Asimismo, facilita el seguimiento del progreso del paciente al evaluar cómo varía la respuesta muscular a lo largo del tiempo.

De igual manera, este análisis contribuye a ajustar la intensidad y duración de los ejercicios, evitando sobrecargas o posibles lesiones. Además, brinda información sobre la activación muscular, lo que resulta útil para diseñar programas de rehabilitación más personalizados y eficientes.

En este sentido, la Transformada de Fourier se convierte en una herramienta clave dentro del procesamiento de señales biomédicas, ya que permite extraer características relevantes que apoyan la toma de decisiones clínicas.

---

## Discusión y analisis de resultados

El análisis de la señal electromiográfica permitió evidenciar cambios significativos tanto en el dominio del tiempo como en el dominio de la frecuencia durante una contracción muscular sostenida hasta la fatiga. En el dominio temporal se observaron variaciones en la amplitud de la señal asociadas a la actividad muscular, mientras que en el dominio frecuencial se identificaron transformaciones relevantes en la distribución de energía.

Los resultados obtenidos a partir del cálculo de la frecuencia media y la frecuencia mediana muestran una tendencia decreciente a lo largo del tiempo. Esta disminución indica un desplazamiento progresivo del contenido espectral hacia frecuencias más bajas, lo cual es un comportamiento característico de la fatiga muscular. La pendiente negativa observada en ambas curvas confirma esta tendencia, siendo más pronunciada en la frecuencia mediana, lo que sugiere una mayor sensibilidad de este parámetro frente a los cambios fisiológicos del músculo.

El análisis por segmentos refuerza este comportamiento. Al comparar el espectro en diferentes momentos de la contracción (inicio, mitad y final), se evidencia una reducción del contenido de altas frecuencias y un cambio en la distribución espectral hacia componentes de menor frecuencia. Este fenómeno se confirma mediante el desplazamiento de la frecuencia pico, que presenta una disminución considerable desde el inicio hasta la mitad del ejercicio, estabilizándose posteriormente en valores más bajos. Esto sugiere que la fatiga comienza a manifestarse de manera temprana durante la contracción sostenida.

Desde el punto de vista fisiológico, estos resultados pueden explicarse por la disminución en la velocidad de conducción de las fibras musculares, la fatiga progresiva de las unidades motoras y la acumulación de metabolitos que afectan el desempeño muscular. Estos factores provocan una modificación en la señal EMG, reflejada en la pérdida de componentes de alta frecuencia.

A pesar de la tendencia general observada, también se presentan fluctuaciones en los valores de frecuencia, las cuales pueden atribuirse a variaciones en la activación muscular, presencia de ruido en la señal, pequeñas alteraciones en la posición de los electrodos y limitaciones propias del proceso de segmentación y análisis.

El uso de herramientas como la Transformada de Fourier se justifica porque permite analizar la señal en el dominio de la frecuencia, proporcionando información que no es fácilmente observable en el dominio del tiempo. En el contexto de terapias de rehabilitación, este tipo de análisis resulta especialmente útil, ya que permite detectar la fatiga muscular de forma objetiva mediante el estudio de cambios en las frecuencias de la señal. Asimismo, facilita el seguimiento del progreso del paciente al evaluar cómo varía la respuesta muscular a lo largo del tiempo.

De igual manera, este análisis contribuye a ajustar la intensidad y duración de los ejercicios, evitando sobrecargas o posibles lesiones. Además, brinda información sobre la activación muscular, lo que resulta útil para diseñar programas de rehabilitación más personalizados y eficientes. En este sentido, la Transformada de Fourier se convierte en una herramienta clave dentro del procesamiento de señales biomédicas, ya que permite extraer características relevantes que apoyan la toma de decisiones clínicas.

En conjunto, los resultados obtenidos demuestran que el análisis espectral de señales EMG es una herramienta eficaz para la identificación y caracterización de la fatiga muscular, integrando de manera coherente los hallazgos observados en las diferentes representaciones y parámetros analizados.

---

## Conclusiones

- El análisis de la señal electromiográfica permitió evidenciar de manera clara la presencia de fatiga muscular durante una contracción sostenida, manifestada a través de la disminución progresiva de la frecuencia media y la frecuencia mediana.

- El desplazamiento del contenido espectral hacia frecuencias más bajas, así como la reducción de la frecuencia pico, confirman que los parámetros en el dominio de la frecuencia son indicadores sensibles y confiables de la fatiga muscular.

- La Transformada de Fourier demostró ser una herramienta fundamental para el análisis de señales biomédicas, al permitir identificar características que no son evidentes en el dominio del tiempo.

- Los resultados obtenidos resaltan la utilidad del análisis espectral en aplicaciones clínicas, especialmente en el monitoreo de la actividad muscular y el diseño de estrategias de rehabilitación más precisas y seguras.

