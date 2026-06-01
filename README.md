# 🌦️ Conversor Inteligente de Monedas con n8n y Telegram

**Asignatura:** N8n sistemas automatizados

**Integrantes:** Oscar Abel Bonilla García y MAnuel Isaac Camaño Díaz

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

En un mundo cada vez más conectado, las fronteras económicas son cada vez menos visibles. Millones de personas realizan compras en tiendas internacionales, trabajan para empresas extranjeras, invierten en mercados internacionales o planean viajes a otros países. Sin embargo, el valor de las monedas cambia constantemente debido a factores económicos, políticos y financieros, lo que hace difícil conocer con precisión cuánto dinero representa una cantidad determinada en pesos colombianos.

Imaginemos a una persona que desea comprar un producto en Estados Unidos, enviar dinero a un familiar en Europa o simplemente conocer cuánto valen sus ahorros en otra divisa. Para tomar decisiones acertadas, necesita acceder a información actualizada sobre las tasas de cambio. Realizar esta consulta manualmente varias veces al día puede resultar tedioso, consumir tiempo y aumentar el riesgo de cometer errores.

Con el fin de solucionar este problema, se plantea el desarrollo de un workflow automatizado en n8n capaz de consultar periódicamente el valor actual de una moneda extranjera mediante una API pública de tasas de cambio. El sistema obtendrá la información en tiempo real, realizará automáticamente la conversión a pesos colombianos (COP) y enviará el resultado a través de plataformas de comunicación como Telegram

---

## 🎯 Objetivos

- **Objetivo General:** Crear un workflow que permita conocer a tiempo real la tasa de cambio de divisas en cualquier momnento y lugar del mundo garacias a la ayuda de una API la cual tiene actualizadas las tasas de cada divisa.
- **Objetivos Específicos:**
- Integrar múltiples nodos tecnológicos en n8n para procesar datos en lote (Multtiples divisas y tasas de cambio).
- Implementar scripts en JavaScript para manipular arreglos de datos y estructurar el formato de los mensajes.
- Diseñar una lógica de bifurcación condicional anidada para mostrar las tasas de cambio.
- Garantizar la resiliencia del sistema ante fallos de conexión de red externos.

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

- **API de tasa de cambio (https://open.er-api.com/v6/latest/USD):** Usado esta api para buscar y actualizar la tasa de valores en el chat bot
- **API de Mensajería (Discord):** Utilizada mediante el nodo nativo de discrod en n8n, ejecutando el recurso `Message` y la operación `SendMessage`.

---

## 🛠️ Desarrollo Paso a Paso

### Explicación Detallada del Workflow

El flujo de trabajo se ejecuta segun la hora que se le disponga

<a href="https://ibb.co/DgCx4FYy"><img src="https://i.ibb.co/wrW9yq7X/Captura-de-pantalla-2026-05-31-224051.png" alt="Captura de pantalla 2026 05 31 224051" border="0"></a>

### Explicación de Cada Nodo y Códigos JavaScript

#### 1. Schedule Trigger

Iniciador del flujo. Permite programar la ejecución automática en intervalos de tiempo específicos (por ejemplo, todas las mañanas) para recibir el reporte temprano.

#### 2. Datos de ciudades (Nodo JavaScript 1)

En este nodo definimos las tasas de monea que deseamos monitoriar. Retorna un arreglo de objetos JSON estructurados.

```javascript
[
  {
    result: "success",
    provider: "https://www.exchangerate-api.com",
    documentation: "https://www.exchangerate-api.com/docs/free",
    terms_of_use: "https://www.exchangerate-api.com/terms",
    time_last_update_unix: 1780272151,
    time_last_update_utc: "Mon, 01 Jun 2026 00:02:31 +0000",
    time_next_update_unix: 1780358941,
    time_next_update_utc: "Tue, 02 Jun 2026 00:09:01 +0000",
    time_eol_unix: 0,
    base_code: "USD",
    rates: {
      USD: 1,
      AED: 3.6725,
      AFN: 63.51083,
      ALL: 82.026436,
      AMD: 368.055721,
      ANG: 1.79,
      AOA: 925.048399,
      ARS: 1410.2904,
      AUD: 1.393181,
      AWG: 1.79,
      AZN: 1.700524,
      BAM: 1.678357,
      BBD: 2,
      BDT: 122.701645,
      BGN: 1.678357,
      BHD: 0.376,
      BIF: 2984.971747,
      BMD: 1,
      BND: 1.277152,
      BOB: 6.897538,
      BRL: 5.040062,
      BSD: 1,
      BTN: 95.076107,
      BWP: 13.448962,
      BYN: 2.744732,
      BZD: 2,
      CAD: 1.379493,
      CDF: 2261.890027,
      CHF: 0.781767,
      CLF: 0.022566,
      CLP: 891.936007,
      CNH: 6.764055,
      CNY: 6.772555,
      COP: 3649.090201,
      CRC: 452.48822,
      CUP: 24,
      CVE: 94.621729,
      CZK: 20.839003,
      DJF: 177.721,
      DKK: 6.402599,
      DOP: 58.966245,
      DZD: 133.067926,
      EGP: 52.223915,
      ERN: 15,
      ETB: 160.237592,
      EUR: 0.858131,
      FJD: 2.208015,
      FKP: 0.743525,
      FOK: 6.402096,
      GBP: 0.743525,
      GEL: 2.666303,
      GGP: 0.743525,
      GHS: 11.720299,
      GIP: 0.743525,
      GMD: 74.17729,
      GNF: 8760.681158,
      GTQ: 7.621894,
      GYD: 208.986244,
      HKD: 7.836719,
      HNL: 26.574606,
      HRK: 6.465582,
      HTG: 130.609015,
      HUF: 303.814558,
      IDR: 17858.514994,
      ILS: 2.810135,
      IMP: 0.743525,
      INR: 95.076394,
      IQD: 1308.753381,
      IRR: 1122743.150955,
      ISK: 123.084451,
      JEP: 0.743525,
      JMD: 157.155383,
      JOD: 0.709,
      JPY: 159.338809,
      KES: 129.492092,
      KGS: 87.489145,
      KHR: 4016.628634,
      KID: 1.393179,
      KMF: 422.172394,
      KRW: 1506.827898,
      KWD: 0.309437,
      KYD: 0.833333,
      KZT: 486.481425,
      LAK: 21828.67875,
      LBP: 89500,
      LKR: 329.126281,
      LRD: 181.856413,
      LSL: 16.249695,
      LYD: 6.347308,
      MAD: 9.184028,
      MDL: 17.301285,
      MGA: 4199.144671,
      MKD: 53.042515,
      MMK: 2100.298182,
      MNT: 3589.036361,
      MOP: 8.07182,
      MRU: 39.937543,
      MUR: 47.339018,
      MVR: 15.432626,
      MWK: 1744.784906,
      MXN: 17.351063,
      MYR: 3.965041,
      MZN: 63.604013,
      NAD: 16.249695,
      NGN: 1371.646088,
      NIO: 36.751168,
      NOK: 9.249348,
      NPR: 152.121772,
      NZD: 1.672389,
      OMR: 0.384497,
      PAB: 1,
      PEN: 3.402089,
      PGK: 4.35201,
      PHP: 61.554879,
      PKR: 278.40944,
      PLN: 3.630679,
      PYG: 6141.711003,
      QAR: 3.64,
      RON: 4.51045,
      RSD: 100.854012,
      RUB: 71.069547,
      RWF: 1468.257909,
      SAR: 3.75,
      SBD: 7.920617,
      SCR: 14.23099,
      SDG: 590.611385,
      SEK: 9.245104,
      SGD: 1.277152,
      SHP: 0.743525,
      SLE: 24.581085,
      SLL: 24581.085213,
      SOS: 570.947178,
      SRD: 37.137516,
      SSP: 4693.52707,
      STN: 21.02419,
      SYP: 110.791113,
      SZL: 16.249695,
      THB: 32.550692,
      TJS: 9.233282,
      TMT: 3.49957,
      TND: 2.904306,
      TOP: 2.383021,
      TRY: 45.897832,
      TTD: 6.780099,
      TVD: 1.393179,
      TWD: 31.475628,
      TZS: 2619.566712,
      UAH: 44.285157,
      UGX: 3744.285671,
      UYU: 39.937905,
      UZS: 12001.488345,
      VES: 554.4258,
      VND: 26307.488628,
      VUV: 117.874944,
      WST: 2.685445,
      XAF: 562.896526,
      XCD: 2.7,
      XCG: 1.79,
      XDR: 0.731213,
      XOF: 562.896526,
      XPF: 102.402396,
      YER: 238.703393,
      ZAR: 16.249718,
      ZMW: 18.473771,
      ZWG: 26.9181,
      ZWL: 26.9181,
    },
  },
];
```

#### 3. Consulta del valor de cada devisa (HTTP Request)

Realiza la consulta dinámica con el uso de la api https://open.er-api.com/v6/latest/USD.

```
https://open.er-api.com/v6/latest/USD

```

<a href="https://ibb.co/kVr26zj3"><img src="https://i.ibb.co/Ngk2nf51/Captura-de-pantalla-2026-05-31-224659.png" alt="Captura de pantalla 2026 05 31 224659" border="0"></a>

#### 4. Organizacion del json con los datos del las divisas que estamos monitoriando (organizacion de divisas)

Al recibir la respuesta del valor de las divisas el modulo de code da el array de las devias en javascript

```// Datos que vienen de la API
const data = $input.first().json;

// Tasas de cambio (respecto a USD)
const rates = data.rates;

// Monedas que queremos monitorear
const monedasObjetivo = ['EUR', 'MXN', 'USD', 'JPY'];

// Tasa COP
const cop = rates['COP'];

// Construimos el resultado para cada moneda
const resultados = monedasObjetivo.map(moneda => {
  const tasaMoneda = rates[moneda];

  // 1 unidad de la moneda extranjera cuántos COP vale
  // Lógica: 1 USD = X moneda, 1 USD = Y COP → 1 moneda = Y/X COP
  const valorEnCOP = cop / tasaMoneda;

  return {
    moneda: moneda,
    tasaVsUSD: tasaMoneda,
    valorEnCOP: Math.round(valorEnCOP),
    fecha: data.time_last_update_utc
  };
});

return resultados.map(r => ({ json: r }));

```

#### 5. Muestra de las devisas del dia anterior (Valor anterior de la devisas)

LA api utiliza valores del dia anterior de las devisas, pero para eso necesitariamos hacer mas complejo el flujo y para eso usamos un registro con base de datos pero para este ejercicio practico hacemos uso de unos datos predefinidos.

```javascript
const item = $input.item.json;

// Valor actual
const valorActual = item.valorEnCOP;

// Simulamos un valor anterior (en producción vendría de una BD o archivo)
// Para la demo usamos un valor fijo de referencia
const valoresReferencia = {
  EUR: 4238.1,
  GBP: 4884.1,
  JPY: 22.83,
};

const valorAnterior = valoresReferencia[item.moneda] || valorActual;

// Calcular cambio
const diferencia = valorActual - valorAnterior;
const porcentaje = ((diferencia / valorAnterior) * 100).toFixed(2);
const tendencia =
  diferencia > 0 ? "📈 Subió" : diferencia < 0 ? "📉 Bajó" : "➡️ Igual";

return {
  json: {
    ...item,
    valorAnterior: valorAnterior,
    diferencia: Math.round(diferencia),
    porcentaje: porcentaje,
    tendencia: tendencia,
  },
};
```

#### 6. Muestra del formato de presentacion de los datos

En este apartado se mostrara como llegara el mensaje al bot de discord

```const items = $input.all();

const fecha = items[0].json.fecha;

let mensaje = `💱 *TASAS DE CAMBIO A COP*\n`;
mensaje += `📅 ${fecha}\n`;
mensaje += `━━━━━━━━━━━━━━━━━━━━\n\n`;

for (const item of items) {
  const d = item.json;
  mensaje += `*${d.moneda}* → COP\n`;
  mensaje += `💰 Valor: $${d.valorEnCOP.toLocaleString('es-CO')} COP\n`;
  mensaje += `${d.tendencia} ${d.porcentaje}%\n`;
  mensaje += `\n`;
}

mensaje += `━━━━━━━━━━━━━━━━━━━━\n`;
mensaje += `🤖 Reporte automático diario`;

return [{ json: { mensaje: mensaje } }]

```

<a href="https://ibb.co/8nwX0W20"><img src="https://i.ibb.co/Q7VXf2nf/Captura-de-pantalla-2026-05-31-225743.png" alt="Captura de pantalla 2026 05 31 225743" border="0"></a>

## 📊 Resultados Obtenidos

El sitema muestra los datos esperados sobre el valor de las devisias que estamos interesados en monitoriar u estar al pendiente.

## <a href="https://ibb.co/PG8cMwSw"><img src="https://i.ibb.co/YBGcXprp/Captura-de-pantalla-2026-05-31-230044.png" alt="Captura de pantalla 2026 05 31 230044" border="0"></a>

## 🛠️ Dificultades Encontradas y Soluciones Aplicadas

> 🚨 **Problema de Timeout en discord:** > No detectava la moneda de cop pesos colombianos y detenia el flujo.
> 💡 **Solución Aplicada:** >

---

## 🏁 Conclusiones Finales

- n8n demostró ser una plataforma robusta y extremadamente flexible para la orquestación de datos, acortando significativamente los tiempos de desarrollo de software en comparación con un desarrollo desde cero.
- La correcta manipulación de objetos y estructuras JSON es una habilidad crítica en tecnología, ya que permite transformar respuestas crudas de servidores externos en información útil e interactiva para el usuario.
- La inclusión de lógicas de resiliencia (reintentos automatizados) separa un prototipo de laboratorio de un sistema de automatización verdaderamente apto para entornos de producción reales.
