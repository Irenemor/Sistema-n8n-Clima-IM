# Sistema-n8n-Clima-IM

Sistema-n8n-Clima-IM:
<img width="496" height="175" alt="image" src="https://github.com/user-attachments/assets/62db56dd-fc51-44cc-8a06-5dc9fcc399d3" />


## Descripción del proyecto 
Este flujo de n8n automatiza la consulta meteorológica para una ciudad mediante peticiones HTTP, clasifica la temperatura y utiliza un modelo de Inteligencia Artificial para generar recomendaciones de vestimenta personalizadas antes de devolver la respuesta al usuario.

### Funcionamiento del Flujo
El recorrido exitoso (happy path) sigue estos pasos ordenados:

Recepción de la solicitud (Webhook: Entrada de usuario recibida): Recibe una llamada HTTP (método POST) con los datos del usuario, conteniendo el nombre de la ciudad a consultar.

Validación inicial (Validar información de entrada): Un nodo de condición comprueba que la petición incluya el parámetro esperado.

Geocodificación (HTTP Request API de geocodificación): Consulta un servicio externo de geolocalización para convertir el nombre textual de la ciudad en coordenadas geográficas (latitud y longitud).

Validación de la ciudad (Validar nombre de Ciudad): Verifica que la API de geocodificación haya encontrado coincidencias válidas.

Preparación de coordenadas (Datos City): Limpia y formatea los datos de latitud, longitud y nombre estandarizado de la ciudad.

Consulta meteorológica (HTTP Request API Coordenadas): Llama a la API del tiempo (según la nota del nodo, Open-Meteo) enviando las coordenadas obtenidas.

Verificación de respuesta (Verificar que la API devolvió resultados): Confirma que la API meteorológica respondió correctamente con datos actuales.

Procesamiento del clima (Campos meteorológicos): Extrae las variables clave (temperatura actual, sensación térmica, viento, lluvia, etc.).

Bifurcación por temperatura (SI: Temperatura > 20°C):

Rama true (Clima Cálido): Prepara un contexto o etiqueta indicando condiciones cálidas.

Rama false (Clima fresco): Prepara un contexto o etiqueta indicando condiciones frías/templadas.

Unificación (Merge): Junta ambas ramas de nuevo en un solo flujo continuo.

Generación con IA (Message a model / Recomendación de IA): Un LLM procesa los datos del clima y el contexto para redactar un mensaje amigable con consejos prácticos sobre qué prendas vestir.

Respuesta final (Éxito Webhook): Devuelve la recomendación final al cliente que originó la petición.


### Uso e Integración
Interfaz de llamada: Como indica la nota adhesiva ([https://hoppscotch.io/](https://hoppscotch.io/) con método POST), el webhook está preparado para ser consumido desde clientes API (Postman, Hoppscotch), formularios web, aplicaciones móviles o bots de mensajería (Telegram, WhatsApp).

Entrada típica (JSON):

JSON
{
  "ciudad": "Madrid"
}
Salida esperada: Un mensaje descriptivo con la temperatura actual y sugerencias concretas de vestuario (por ejemplo: recomendar manga corta e hidratación si supera los 20 °C, o abrigo y paraguas si las condiciones son frescas o lluviosas).
