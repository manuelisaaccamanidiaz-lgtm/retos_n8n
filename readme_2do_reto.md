# 🌦️ Sistema de Alertas Climáticas Inteligente con n8n y Telegram

**Asignatura:** Reto Práctico de Automatización

**Integrantes:** 
* Manuel Isaac Camaño Diaz
* Oscar Abel Bonilla Garcia

**Fecha:** Mayo 2026

---

## 📋 Tabla de Contenido

1. [Explicación del Problema]
2. [Objetivos]
3. [Investigación Realizada]
4. [APIs Utilizadas]
5. [Desarrollo Paso a Paso]
6. [Resultados Obtenidos]
7. [Dificultades Encontradas y Soluciones Aplicadas]
8. [Mejoras Implementadas]
9. [Relación con la Rúbrica de Evaluación]
10. [Conclusiones Finales]

---

## 🔍 Explicación del Problema

En muchas regiones de Colombia, los cambios climáticos repentinos afectan las actividades diarias de las personas, el transporte y la logística urbana. Planificar el día a día se vuelve complicado si no se cuenta con información meteorológica oportuna. Aunque existen aplicaciones climáticas, estas requieren que el usuario ingrese manualmente a revisar el pronóstico.

Identificamos la necesidad de diseñar un sistema automatizado que centralice, analice y clasifique proactivamente el riesgo de precipitaciones para las principales ciudades del país, enviando notificaciones personalizadas directamente a los dispositivos móviles de los usuarios mediante canales cotidianos como Telegram.

---

## 🎯 Objetivos

* **Objetivo General:** Construir un flujo de trabajo (workflow) en n8n que consulte automáticamente el clima de varias ciudades colombianas utilizando una API meteorológica, analice el riesgo de lluvia y envíe alertas personalizadas a Telegram según la probabilidad detectada.
* **Objetivos Específicos:**
* Integrar múltiples nodos tecnológicos en n8n para procesar datos en lote (múltiples ciudades simultáneamente).
* Implementar scripts en JavaScript para manipular arreglos de datos y estructurar el formato de los mensajes.
* Diseñar una lógica de bifurcación condicional anidada para categorizar las alertas climáticas.
* Garantizar la resiliencia del sistema ante fallos de conexión de red externos.



---

## 📚 Investigación Realizada

### ¿Qué es n8n?

Es una herramienta de automatización de flujos de trabajo basada en nodos y de código abierto. A diferencia de otras plataformas, nos permite manejar estructuras de datos complejas de manera visual y, al mismo tiempo, nos da la libertad de inyectar código JavaScript personalizado para tener control absoluto sobre la información que transita por el flujo.

### ¿Qué son las APIs?

Las Interfaces de Programación de Aplicaciones (APIs) son puentes de comunicación que permiten a dos software intercambiar datos de forma estructurada. En nuestro proyecto, el workflow actúa como "cliente" que solicita datos en tiempo real al servidor meteorológico externo.

### ¿Qué es JSON?

JavaScript Object Notation (JSON) es el formato estándar de intercambio de datos en la web. Se compone de estructuras legibles de clave-valor (objetos) y listas (arreglos). Es el lenguaje universal que utilizan los nodos de n8n para pasarse información entre sí.

### ¿Qué son los nodos IF?

Son nodos condicionales que evalúan una expresión lógica (falsa o verdadera). Permiten que el flujo de automatización tome caminos distintos según los valores que traigan los datos en tiempo real, emulando la estructura clásica `if / else` de la programación tradicional.

### ¿Qué es Telegram Bot API?

Es la interfaz gratuita provista por Telegram que permite a desarrolladores controlar cuentas automatizadas (bots). A través de peticiones HTTP seguras, nuestro flujo puede ordenar al bot que envíe textos enriquecidos con emojis a chats específicos utilizando un `Chat ID` y un token de autenticación.

### ¿Qué es Open-Meteo?

Es una API meteorológica de acceso público y gratuito para fines educativos. No requiere llaves de autenticación (API Keys) complejas y ofrece pronósticos horarios altamente detallados basados en coordenadas de latitud y longitud a nivel global.

---

## 🌐 APIs Utilizadas

* **API de Clima (Open-Meteo):** Se seleccionó por su velocidad y formato JSON limpio. El endpoint utilizado consulta las variables de pronóstico por horas, específicamente apuntando al parámetro `precipitation_probability`.
* **API de Mensajería (Telegram Bot API):** Utilizada mediante el nodo nativo de Telegram en n8n, ejecutando el recurso `Message` y la operación `SendMessage`.

---

## 🛠️ Desarrollo Paso a Paso

### Explicación Detallada del Workflow

Nuestro flujo automatizado se procesa de manera secuencial e inteligente. Inicia con un temporizador, define la matriz de ciudades en Colombia, extrae sus datos meteorológicos individuales, reasocia los nombres de las ciudades tras la consulta externa, calcula probabilísticamente el peor escenario de lluvias para el día y clasifica el resultado final en tres rutas distintas de mensajería automatizada.

<a href="https://ibb.co/bjw5PPN1"><img src="https://i.ibb.co/kVb2xxqK/Captura-de-pantalla-2026-05-30-193058.png" alt="Captura-de-pantalla-2026-05-30-193058" border="0"></a>

### Explicación de Cada Nodo y Códigos JavaScript

#### 1. Schedule Trigger

Iniciador del flujo. Permite programar la ejecución automática en intervalos de tiempo específicos (por ejemplo, todas las mañanas) para recibir el reporte temprano.

#### 2. Datos de ciudades (Nodo JavaScript 1)

En este nodo definimos las coordenadas geográficas de las ciudades colombianas que deseamos monitorear. Retorna un arreglo de objetos JSON estructurados.

```javascript
return [
  {
    json: {
      ciudad: "Bogotá",
      latitud: 4.7110,
      longitud: -74.0721
    }
  },
  {
    json: {
      ciudad: "Medellín",
      latitud: 6.2442,
      longitud: -75.5812
    }
  },
  {
    json: {
      ciudad: "Bucaramanga",
      latitud: 7.1254,
      longitud: -73.1198
    }
  }
];

```

#### 3. Datos climáticos (HTTP Request)

Realiza la consulta dinámica a Open-Meteo mapeando las expresiones de n8n `{{ $json.latitud }}` y `{{ $json.longitud }}` directamente en la URL del endpoint.

```
https://api.open-meteo.com/v1/forecast?latitude={{$json.latitud}}&longitude={{$json.longitud}}&hourly=precipitation_probability&current_weather=true

```

<a href="https://ibb.co/ks1PZ02b"><img src="https://i.ibb.co/v6YM9m4y/Captura-de-pantalla-2026-05-30-193123.png" alt="Captura-de-pantalla-2026-05-30-193123" border="0"></a>

#### 4. Organización de las ciudades dentro del JSON (Nodo JavaScript 2)

Al recibir la respuesta de Open-Meteo, el nombre original de la ciudad no viene incluido en el JSON de respuesta directa (solo regresan las coordenadas formateadas). Implementamos este script para identificar geográficamente a qué ciudad corresponde cada respuesta según rangos aproximados de latitud, asegurando que ningún dato pierda su etiqueta de origen:

```javascript
const lat = $json.latitude;

let ciudad = "Desconocida";

if (lat > 4 && lat < 5) {
  ciudad = "Bogotá";
} else if (lat > 6 && lat < 7) {
  ciudad = "Medellín";
} else if (lat > 7 && lat < 8) {
  ciudad = "Bucaramanga";
}

$json.ciudad = ciudad;

return $json;

```

#### 5. Cálculo de probabilidad máxima de que llueva (Nodo JavaScript 3)

La API nos devuelve una lista con 168 horas de predicciones de lluvia. Para enviar una alerta diaria efectiva, utilizamos la función matemática de JavaScript `Math.max` expandiendo el arreglo con el operador *spread* (`...`), encontrando así el porcentaje más alto de probabilidad de lluvia registrado para las próximas horas.

```javascript
const maxProb = Math.max(...$json.hourly.precipitation_probability);

$json.maxProbabilidad = maxProb;

return $json;

```

#### 6. Probabilidad mayor al 70% (Nodo IF 1)

Evalúa el valor de `{{ $json.maxProbabilidad }}`. Si es mayor a 70, el flujo se desvía por la rama `true`. Si es menor o igual, pasa a la rama `false` para ser revaluado.

```text
{{ $json.maxProbabilidad }} > 70

```

<a href="https://ibb.co/prGpmC8d"><img src="https://i.ibb.co/7N8F975K/Captura-de-pantalla-2026-05-30-193138.png" alt="Captura-de-pantalla-2026-05-30-193138" border="0"></a>

#### 7. Mensaje advertencia 70% (Nodo Telegram - Alerta Alta)

Se conecta al bot si la condición del IF 1 es verdadera. Imprime un reporte de color rojo indicando una alerta climática de alta preocupación, la temperatura actual y el porcentaje máximo de lluvia detectado.

<a href="https://ibb.co/7NgrvLN7"><img src="https://i.ibb.co/CsJVBDsx/Captura-de-pantalla-2026-05-30-193232.png" alt="Captura-de-pantalla-2026-05-30-193232" border="0"></a>

#### 8. Probabilidad mayor al 30% (Nodo IF 2)

Conectado a la salida `false` del primer condicional. Recibe los elementos sobrantes y evalúa si la probabilidad se mantiene al menos por encima del 30%.

```
{{ $json.maxProbabilidad }} > 30

```

<a href="https://ibb.co/gMJK6py0"><img src="https://i.ibb.co/MD7094nz/Captura-de-pantalla-2026-05-30-193149.png" alt="Captura-de-pantalla-2026-05-30-193149" border="0"></a>

#### 9. Mensaje advertencia 30% (Nodo Telegram - Alerta Moderada)

Se activa si la probabilidad está entre 31% y 70%. Envía un mensaje con una advertencia de mínima preocupación informando las condiciones estables pero con posible llovizna.

<a href="https://ibb.co/Ly24qNx"><img src="https://i.ibb.co/fWPmKDk/Captura-de-pantalla-2026-05-30-193214.png" alt="Captura-de-pantalla-2026-05-30-193214" border="0"></a>

#### 10. Mensaje de respaldo (Nodo Telegram - Sin riesgo)

Conectado a la salida `false` del segundo condicional. Si la probabilidad es de 30% o menos, el bot envía una notificación optimista indicando que el clima es ideal para actividades al aire libre.

<a href="https://ibb.co/SChy4k5"><img src="https://i.ibb.co/tr7KT0Q/Captura-de-pantalla-2026-05-30-193259.png" alt="Captura-de-pantalla-2026-05-30-193259" border="0"></a>
---

## 📊 Resultados Obtenidos

El sistema opera exitosamente en paralelo para los 3 elementos configurados. Al ejecutarse el flujo, procesa de forma individual las condiciones de cada ciudad y dispara las alertas personalizadas al canal de Telegram en tiempo real.

<a href="https://ibb.co/ZRPR8LKS"><img src="https://i.ibb.co/7x5xWYVQ/Captura-de-pantalla-2026-05-30-195357.png" alt="Captura de pantalla 2026 05 30 195357" border="0"></a>
---

## 🛠️ Dificultades Encontradas y Soluciones Aplicadas

> 🚨 **Problema de Timeout en Telegram:** > Durante las pruebas iniciales, cuando la red experimentaba intermitencias o latencia alta, las peticiones HTTP hacia la API de Telegram fallaban aleatoriamente por tiempo de espera agotado (*timeout*), deteniendo el flujo por completo y dejando ciudades sin notificar.
> 💡 **Solución Aplicada:** > Entramos a los ajustes avanzados (*Settings*) de los nodos de Telegram y activamos la propiedad **Retry On Fail**. Configuramos un margen de **3 reintentos** automáticos espaciados por un intervalo de **1000 milisegundos (1 segundos)**. Con esto, si una petición falla por un parpadeo de red, n8n reintenta el envío de forma transparente sin romper el ciclo del flujo.

---

## 🚀 Mejoras Implementadas

Como valor agregado e iniciativas propias fuera de los requerimientos base del reto, añadimos las siguientes optimizaciones:

1. **Soporte para múltiples ciudades nativo:** Diseñamos el flujo para trabajar directamente con arreglos de objetos JSON. Esto permite escalar el sistema y añadir 10 o 50 ciudades en el primer nodo sin necesidad de clonar o duplicar los nodos posteriores del flujo.
2. **Diferenciación jerárquica de alertas:** Creamos tres estados de respuesta dinámicos (Alta, Moderada y Baja) en lugar de una simple validación binaria de "llueve o no llueve", mejorando la utilidad de la información para el usuario final.
3. **Cálculo inteligente del peor escenario:** Implementamos manipulación de datos avanzada con `Math.max` para escanear todo el arreglo horario provisto por la API, garantizando que si existe riesgo de tempestad a cualquier hora del día, el sistema la detecte con anticipación.

---

## 🏁 Conclusiones Finales

* n8n demostró ser una plataforma robusta y extremadamente flexible para la orquestación de datos, acortando significativamente los tiempos de desarrollo de software en comparación con un desarrollo desde cero.
* La correcta manipulación de objetos y estructuras JSON es una habilidad crítica en tecnología, ya que permite transformar respuestas crudas de servidores externos en información útil e interactiva para el usuario.
* La inclusión de lógicas de resiliencia (reintentos automatizados) separa un prototipo de laboratorio de un sistema de automatización verdaderamente apto para entornos de producción reales.
