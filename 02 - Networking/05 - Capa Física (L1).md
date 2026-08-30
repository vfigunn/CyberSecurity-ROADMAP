# 05 - Capa Física (Capa 1 / L1)

## 🎯 Objetivos
- Entender el propósito de la Capa 1 del Modelo OSI.
- Conocer los tres tipos de medios de transmisión físicos.
- Comprender las vulnerabilidades de seguridad a nivel físico.

---

## 🧠 Concepto

La **Capa Física (L1)** es la capa más baja del Modelo OSI. Su trabajo es extremadamente simple pero fundamental: **transmitir bits crudos (ceros y unos) a través de un medio físico.**

La Capa 1 no sabe qué es una dirección IP, no sabe qué es una página web, ni siquiera sabe qué es una letra. Solo entiende voltajes eléctricos, pulsos de luz o señales de radiofrecuencia. Su unidad de datos (PDU) es el **Bit**.

---

## 🔌 Medios de Transmisión (Media)

Para que los bits viajen de A hacia B, necesitan un camino. Existen tres categorías principales de medios:

### 1. Cables de Cobre (Señales Eléctricas)
- **Cables de Par Trenzado (Twisted Pair):** Es el clásico cable de red (UTP/STP) que usas para conectar tu PC al router (cables Cat5e, Cat6, etc.). Transmite datos enviando pulsos eléctricos de bajo voltaje.
  - *Ventaja:* Barato y fácil de instalar.
  - *Desventaja:* Sensible a la Interferencia Electromagnética (EMI). Si pasas un cable UTP cerca de un motor industrial o luces fluorescentes, los datos pueden corromperse. Distancia máxima típica: 100 metros.

### 2. Fibra Óptica (Pulsos de Luz)
- Hilos de vidrio o plástico extremadamente finos que transmiten luz láser o LED.
  - *Ventaja:* Inmune a interferencias electromagnéticas (EMI), no emite radiación, soporta velocidades altísimas (Gbps/Tbps) y cubre distancias gigantescas (kilómetros o cruza océanos).
  - *Desventaja:* Caro, frágil, y requiere equipos especializados para empalmar (cortar/unir) los cables.

### 3. Inalámbrico / Wireless (Ondas de Radio)
- Transmite los bits mediante radiofrecuencia o microondas por el aire (Wi-Fi, Bluetooth, 4G/5G).
  - *Ventaja:* Movilidad absoluta. No requiere cables.
  - *Desventaja:* Es el medio menos seguro por defecto (cualquiera en el rango puede "escuchar" la señal). Sensible a obstáculos (paredes) y congestión del espectro (muchas redes Wi-Fi compitiendo).

---

## ❓ ¿Por qué importa en Seguridad?

Aunque la mayoría del trabajo de ciberseguridad ocurre en capas más altas (L3, L4, L7), los atacantes no ignoran la Capa 1. Si comprometés la Capa 1, comprometés todas las demás capas por encima de ella.

### Amenazas en Capa 1:
- **Wiretapping (Intervención de cables):** Un atacante físico puede conectar un dispositivo espía a un cable de cobre UTP usando inducción magnética sin siquiera cortar el cable, capturando todo el tráfico (Sniffing pasivo).
- **Corte de Cables (Sabotaje):** Un atacante con una tijera o una retroexcavadora cortando un cable de fibra óptica causa una falla inmediata de la Disponibilidad. (Los incidentes donde tiburones o barcos cortan cables submarinos de internet son reales).
- **Jamming (Interferencia intencional):** En redes inalámbricas, un atacante puede usar un dispositivo que emite "ruido" de radiofrecuencia muy fuerte para ahogar la señal Wi-Fi legítima, causando un ataque de Denegación de Servicio (DoS).
- **Data Emanation / TEMPEST:** Los cables de cobre y monitores emiten radiación electromagnética (fugas) que espías avanzados pueden captar a distancia con antenas direccionales para reconstruir la información que viaja por el cable.

### Controles de Seguridad en Capa 1:
- **Controles Físicos:** Cerraduras en los armarios de telecomunicaciones (Racks), cámaras de seguridad, tubos blindados para pasar los cables.
- **Elección del Medio:** La fibra óptica es intrínsecamente más segura que el cobre, ya que no emite radiación electromagnética (es muy difícil hacerle un "wiretap" sin cortar la luz y disparar alarmas).

---

## 📌 Must Know (Imprescindible)
- La PDU de la Capa 1 es el **Bit**.
- Los tres medios físicos: Cobre, Fibra, Inalámbrico.
- Las redes inalámbricas son las más susceptibles a interceptación en L1.

## 💡 Good to Know (Bueno saberlo)
- Los **Hubs** (concentradores) son dispositivos de Capa 1 antiguos. Todo lo que entraba por un puerto salía por todos los demás (como un repetidor bruto). Fueron reemplazados por los Switches (Capa 2) por razones de eficiencia y seguridad.

---

## 🔄 Preguntas de repaso
1. Una organización militar necesita conectar dos edificios y está muy preocupada por el espionaje electromagnético (Wiretapping) de los cables entre los edificios. ¿Qué tipo de medio físico deberían usar?
2. Un atacante utiliza un inhibidor de señal en el estacionamiento para que las cámaras Wi-Fi pierdan la conexión. ¿A qué capa del modelo OSI y a qué pilar de la Tríada CIA está atacando?
3. ¿Cuál es la "PDU" (Unidad de Datos del Protocolo) en la Capa 1?

**➡️ Siguiente nota:** [[06 - Capa de Enlace de Datos y MAC (L2)]]
