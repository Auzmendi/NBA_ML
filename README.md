# NBA_ML
Entrenamiento de modelo de ML para la predicción de resultados de partidos de la NBA



#### I. Introducción

* Contexto del problema a resolver y justificación de la necesidad
  
Es bien sabido que en algunos deportes es posible extraer una gran cantidad de datos a muchas escalas. Ya sean datos sobre: encuentros ,temporadas, histórico de un club, cómo es el desempeño de un jugador en concreto... En particular la NBA, genera día a día una gran cantidad de datos. Datos que pueden ser utilizados en modelos predictivos con una gran variedad de objetivos: prevención de lesiones, fichaje de jugadores que mejor vayan a jugar con la plantilla actual, entre muchos muchos otros.

* Objetivos y alcance del proyecto

El objetivo del presente proyecto es utilizar dichos datos para entrenar un modelo de machine learning, en adelante ML, que tenga capacidad predictiva sobre el ganador de un encuentro.  En esta emoria se explicará paso a paso, cómo ha sido la ingesta de datos, su tratamiento y el entrenamiento del modelo final, así cómo un ejemplo del desempeño del modelo sobre algunos partidos de la temporada 23/24.

* Planificación inicial
  
La tabla predictora estará compuesta por toda información posible relativa al partido sobre el que va a realizar la predicción. Nos encontramos con el problema de que aunque este modelo intente predecir "el futuro", no puede tratarse como un problema de time series.

Para la resolución de este proyecto, se ha decidido dividir la tabla predictora en 2 tipos de variables:

    - Generales: Contendrá información relativa a la valoración de los equipos por agencias externas en función de su capacidad ofensiva, defensiva, etc.
    - Temporales: Contendrá información relativa al desempeño de los equipos durante los últimos partidos. Será una medida del "estado actual" del equipo.
  
 Por otro lado, cada fila deberá tener información sobre ambos contricantes. Por lo que de inicio, se duplicará la cantidad de features y ya se estudiará en el feature selection si se reduce el nº. 

#### II. Dataset

* Descripción de los datasets

Se emplean varios datasets.

**Datasets con información general**: 

 * BPI:Basketball Power Index that measures team’s true strength on net points scale; expected point margin vs average opponent on neutral court.
 * BPI RK:Basketball Power Index Rank vs all NBA teams
 * OFF:Offensive component of BPI. Offensive contribution to expected point margin vs average opponent on neutral court.
 * DEF:Defensive component of BPI. Defensive contribution to expected point margin vs average opponent on neutral court.
 * PBPI:Playoff BPI. Average BPI value throughout the course of the playoffs. Includes past, currently-scheduled, and hypothetical future playoff games
 * OVR = Overall
 * INS = Inside Scoring (Layup, Standing Dunk, Driving Dunk, Post Hook, Post Fade, Post Control, Draw Foul, and Hands)
 * OUT = Outside Scoring (Close Shot, Mid-Range Shot, Three-Point Shot, Free Throw, Shot IQ, and Offensive Consistency)
 * ATH = Athleticism (Speed, Acceleration, Strength, Vertical, Stamina, Hustle, and Overall Durability)
 * PLA = Playmaking (Pass Accuracy, Ball Handle, Speed with Ball, Pass IQ, and Pass Vision)
 * DEF = Defending (Interior Defense, Perimeter Defense, Steal, Block, Lateral Quickness, Help Defense IQ, Pass Perception, and * Defensive Consistency)
 * REB = Rebounding (Offensive Rebound and Defensive Rebound)
 * INT = Intangibles: the rate of a player's ability to sink a shot in the clutch.
 * POT = Potential: the rate in how fast a player's overall goes up during a season/off season (factoring age and injuries)



**Datasets con información de partidos**: 

  ##### STATS GENERALES
* **season_id** : hay una cifra(2,3,etc) y seguido el año de inicio de la temporada.
* **team_id_home**, un id para idenatificar a los equipos. 
* **team_abbreviation_home**, abreviatura del nombre del equipo
* **team_name_home**, nombre del equipo
* **game_id**, identificador del partido al que referencian las estadisiticas de esta fila
* **game_date**, fecha de dicho partido
* **matchup_home**, muestra quienes son los equipos que juegan con sus abrreviaturas y vs en el medio
* **wl_home**,  -----no he encontrado nada------
* **min**, minutos jugados

## STATS POR EQUIPO
## EQUIPO LOCAL
### TIROS DE CAMPO
* **fgm_home**, field goals made por el equipo local. canastas de 2 ptos HECHAS!
* **fga_home**, field goals attempted por el equipo local. canastas de 2 ptos INTENTADAS!
* **fg_pct_home**,   field goals percentage
* 
### TIROS DE 3 PUNTOS
* **fg3m_home**, tiros de 3 convertidos
* **fg3a_home**, tiros de 3 intentados
* **fg3_pct_home**, porcentaje de acierto en tiros de 3
* 
* ### TIROS LIBRES
* **ftm_home**, tiros libres convertidos
* **fta_home**,tiros libres intentados
* **ft_pct_home**, porcentaje de acierto en tiros libres

## REBOTES / ASISTENCIAS / ROBOS / TAPONES / PERDIDAS / FALTAS / PUNTOS
* **oreb_home**, rebotes ofensivos
* **dreb_home**,rebotes defensivos
* **reb_home**,rebotes totales
* **ast_home**,asistencias
* **stl_home**,robos
* **blk_home**,tapones
* **tov_home**, pérdidas de balón
* **pf_home**, faltas
* **pts_home**, puntos
* **plus_minus_home**, valoración
* **video_available_home**, video disponible.

## EQUIPO VISITANTE
## STATS GENERALES
* **team_id_away**, un id para idenatificar a los equipos.
* **team_abbreviation_away**,abreviatura del nombre del equipo
* **team_name_away**, nombre del equipo
* **matchup_away**,muestra quienes son los equipos que juegan con sus abrreviaturas y vs en el medio
* **wl_away**, -----no he encontrado nada------

### TIROS DE CAMPO
* **fgm_away**, field goals made por el equipo local. canastas de 2 ptos HECHAS!
* **fga_away**, field goals attempted por el equipo local. canastas de 2 ptos INTENTADAS!
* **fg_pct_away**,   field goals percentage

### TIROS DE 3 PUNTOS 
* **fg3m_away**, tiros de 3 convertidos
* **fg3a_away**, tiros de 3 intentados
* **fg3_pct_away**, porcentaje de acierto en tiros de 3

### TIROS LIBRES
* **ftm_away**, tiros libres convertidos
* **fta_away**, tiros libres intentados
* **ft_pct_away**, porcentaje de acierto en tiros libres

## REBOTES / ASISTENCIAS / ROBOS / TAPONES / PERDIDAS / FALTAS / PUNTOS
* **oreb_away**, rebotes ofencsivos
* **dreb_away**, rebotes defensivos
* **reb_away**, rebotes totales
* **ast_away**, asistencias
* **stl_away**, robos
* **blk_away**, tapones
* **tov_away**, pérdidas de balón
* **pf_away**, faltas
* **pts_away**, puntos
* **plus_minus_away**, valoración
* **video_available_away**, video disponible

## INDICADOR DE TIPO DE PARTIDO (PLAYOFF, TEMPORADA REGULAR, ALL STAR)
* **season_type** , indicador del tipo de partido. 3 opciones , 'Regular Season', 'Playoffs', 'All Star'



#### III. Preprocesamiento de los datos

* Verificación de la calidad de los datos

En proyecto anterior EDA ya se revisó la calidad de los datos.

* Decisiones, imputaciones y transformación de variables

  Transformación de variables. Las variables generales de los equipos, se agregan directamente de los datasets previamente mencionados. Por otro lado las variables temporales se han hecho creando una función que trabaja de la siguiente manera:

  - Teniendo como punto de partida un dataset con nombres de ambos equipos y la fecha del encuentro,  primero ordena la filas por las fechas del encuentro, después filtra el df con el nombre del uno de los equipos participantes. Y se queda únicamente con las 3 filas anteriores a la fecha en cuestión. Para finalizar calcula la media de todas las variables numéricas arriba descritas y las añade al dataset destino con el sufijo: "_local" o "_visitante" respectivamente.
  - En pasos posteriores, se ha decidido reducir el numero de variables de la tabla predictora uniendo las columnas de local y visitante mediante un resta, de forma que los valores para el equipo local queden positivos y los del visitante negativos, ya que el target es una clasificación binaria sobre si el equipo local ganará o no. Con esto se consiguen 2 cosas, se reduce la complejidad del modelo y se reduce el nº de features manteniendo la información.
  - Tambien en las feature relativas al TIER al que pertence cada equipo, se ha asignado un valor de 3 al TIER1, 2 al TIER2 y 1 al TIER3, y posteriormente también en este caso se ha hecho la diferencia entre estos valores para requipo local y visitante reduciendo así de 6 features a 1.
- Tambien se han eliminado algunas features que no aprotaban nada a los modelos. relacionadas con bloqueos, tapones, robos, recuperaciones, etc.

#### IV. Modelado

* Entrenamiento de modelos supervisados/no supervisados

Para el entrenamiento del modelo se han seguido una serie de pasos. Para agilizar el proceso de selección de features, se ha empleado un decision tree repetidamente, con el objetivo de identificar aquellas features que mas correlación tuvieran con el target y para comprobar la efectividad de la transformación mencionada anteriormente de restar los valores locales menos los visitantes. 

Finalmente se han entrenado dos modelos en paralelo para comprobar cuál predecía mejor. Rand forest y Xgboost. 

La parametrización de los modelos que mejores métricas ha dado han sido:
  - RandomForestClassifier(n_estimators=300, random_state=42,max_depth=4) --> Accuracy score: 0.706
  - XGBClassifier(learning_rate=0.1,n_estimators=100,max_depth=2) --> Accuracy score: 0.693

* Evaluación de los diferentes modelos e iteraciones.

A lo largo de iterar diferentes parametrizaciones, ha quedado claro que ambos modelos caían fácilmente en overfitting.

#### V. Predicción y resultados finales

* Descripción de la solución final y su impacto en el negocio

Los valores obtenidos con mabos modelos, puedn considerarse aceptables aunque en realidad no generan un impacto demasiado relevante frente a la capacidad predictiva de una persona con eperiencia en el campo. Por tanto, podría utilizarse como otra herramienta, aunque con la precaución que requiere ese accuracy de 0.7.

