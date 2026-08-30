# 02 - Topologías y Tipos de Redes

## 🎯 Objetivos
- Conocer los diferentes tamaños/escalas de redes (LAN, WAN, PAN, MAN).
- Entender las topologías físicas y lógicas comunes (Estrella, Malla, Bus).
- Analizar cómo la topología afecta la seguridad y resiliencia de la red.

---

## 📏 Tipos de Redes (Por Alcance Geográfico)

Las redes se clasifican comúnmente por su tamaño físico:

1. **PAN (Personal Area Network):** 
   - Red personal de muy corto alcance (unos pocos metros).
   - *Ejemplo:* Tu celular conectado a auriculares por Bluetooth o a un smartwatch.
2. **LAN (Local Area Network):** 
   - Red confinada a un área geográfica pequeña, como una casa, un piso de una oficina o un edificio. Suelen estar controladas por un único administrador (tu empresa o vos mismo).
   - *Ejemplo:* La red Wi-Fi y por cable Ethernet de tu casa.
3. **MAN (Metropolitan Area Network):**
   - Red que abarca una ciudad entera o un campus universitario grande. (Término menos usado hoy en día).
4. **WAN (Wide Area Network):**
   - Red que cubre áreas geográficas inmensas (países, continentes). Conectan múltiples redes LAN entre sí. Son administradas por múltiples proveedores de servicios (ISPs).
   - *Ejemplo:* **Internet** es la WAN más grande del mundo.

> [!important] Intranet vs Internet vs Extranet
> - **Intranet:** Una red LAN privada que pertenece a una organización (solo para empleados).
> - **Internet:** La red global, pública y accesible por todos.
> - **Extranet:** Una parte de la Intranet que la empresa expone de forma controlada a socios o proveedores externos específicos.

---

## 🕸️ Topologías de Red (La forma de la red)

La **topología** es cómo están conectados los nodos. Se divide en Topología Física (cómo van los cables reales) y Topología Lógica (cómo fluyen los datos). 

### 1. Topología de Bus (Bus)
- *Histórica.* Todas las computadoras se conectaban a un solo cable central largo (el bus). 
- *Problema:* Si el cable se rompía, toda la red se caía (Single Point of Failure). Además, todas las PCs "escuchaban" el tráfico de las demás (pesadilla de seguridad). Ya no se usa para redes LAN corporativas.

### 2. Topología de Anillo (Ring)
- Cada computadora se conecta a la siguiente formando un círculo cerrado. El mensaje da vueltas hasta encontrar a su destinatario.
- *Problema:* Lenta y difícil de escalar. Poco común hoy en día, salvo en redes backbone antiguas de fibra.

### 3. Topología de Estrella (Star) - *La más común* ⭐
- Todas las computadoras se conectan a un dispositivo central, normalmente un **Switch** (lo veremos en la [[07 - Switches y ARP|Nota 07]]). 
- *Ventaja:* Si se rompe el cable de la PC 1, el resto de la red sigue funcionando. Es fácil añadir nuevos equipos.
- *Problema (Seguridad/Resiliencia):* El dispositivo central (el Switch) es un **Punto Único de Fallo (Single Point of Failure - SPOF)**. Si el Switch se rompe (o es atacado con un DDoS), ninguna computadora de la estrella puede comunicarse.

### 4. Topología de Malla (Mesh)
- Cada computadora o router está conectado a muchos o a todos los demás dispositivos simultáneamente.
- *Ventaja (Resiliencia):* Altamente tolerante a fallos (Alta [[03 - CIA Triad|Disponibilidad]]). Si un cable se corta, el mensaje toma un camino alternativo.
- *Desventaja:* Extremadamente cara y compleja de cablear en el mundo físico.
- *Dónde se usa:* Principalmente en redes inalámbricas (Mesh Wi-Fi) y, fundamentalmente, **es la topología lógica en la que se basa Internet**. Los routers de Internet tienen múltiples caminos redundantes entre sí.

---

## ❓ ¿Por qué importa en Seguridad?

El diseño de la red define tu **[[10 - Attack Surface|Superficie de Ataque]]**.

- **El peligro del SPOF (Single Point of Failure):** Si sos un atacante y querés afectar la Disponibilidad de una red en Estrella (como un call center), no atacás las 100 computadoras; lanzás un ataque para saturar o apagar el Switch central.
- **Micro-segmentación:** Modernamente, no queremos que todas las computadoras en la LAN puedan hablar libremente entre sí, porque si un empleado descarga Ransomware, este se propagará por la estrella a todos los demás. Por eso dividimos las LANs en redes virtuales más chicas ([[23 - VLANs|VLANs]]).

---

## 📌 Must Know (Imprescindible)
- Diferencia clara entre LAN y WAN.
- Topología de Estrella: Entender qué es y por qué el dispositivo central es un punto único de fallo (SPOF).

## 💡 Good to Know (Bueno saberlo)
- Red **WLAN** (Wireless LAN): Es simplemente una LAN que usa tecnología inalámbrica (Wi-Fi) en lugar de cables.
- **Topología Híbrida:** En la vida real, las empresas usan híbridos. Por ejemplo, "Estrella extendida" (Varios Switches en estrella conectados a un Router central).

---

## 🔄 Preguntas de repaso
1. Clasificá la red que usás para conectar la sede de una empresa en Buenos Aires con su sucursal en Madrid (¿LAN, PAN o WAN?).
2. ¿Por qué la topología de Estrella es mejor que la topología de Bus si el cable de una computadora se corta?
3. En términos de la Tríada CIA, ¿qué pilar se busca maximizar al utilizar una topología de Malla (Mesh)?

**➡️ Siguiente nota:** [[03 - Modelo OSI]]
