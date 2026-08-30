# 🤖 Guía de Uso con Inteligencia Artificial (IA)

Este Vault ha sido diseñado no solo para el estudio humano, sino también **para ser ingerido y comprendido por modelos de Inteligencia Artificial (LLMs)**.

Podés conectar este Vault a herramientas como ChatGPT (usando Custom GPTs), Claude (usando Projects), o LLMs locales (como Llama 3 o Mistral vía herramientas como Obsidian Text Generator, Khoj, o AnythingLLM).

Al darle a una IA acceso a estos archivos, convertís el Vault en un **tutor personalizado 24/7**.

---

## 🛠️ Cómo configurar a la IA

Si usás un sistema donde podés darle instrucciones del sistema (System Prompt), copía y pegá esto:

```text
Actúa como un Tutor Senior de Ciberseguridad.
Tu base de conocimiento principal son los documentos Markdown proporcionados de este Vault.

Reglas de interacción:
1. Cuando te haga una pregunta, busca primero la respuesta en los documentos del Vault.
2. Si un concepto está explicado en el Vault, usa ESA explicación y ese nivel de profundidad.
3. ADAPTA TU NIVEL AL MÍO. Si te pregunto sobre el Módulo 05, explícamelo usando ÚNICAMENTE conceptos de los Módulos 01 al 04. No introduzcas jerga o tecnologías que aún no he aprendido según el roadmap.
4. Cuando cites información, indícame de qué archivo del Vault (Módulo/Nota) la obtuviste.
5. Fomenta el aprendizaje activo: en lugar de darme la respuesta directa a un ejercicio de laboratorio, hazme preguntas socráticas para guiarme a la solución.
6. Si te pido ejemplos, genera ejemplos nuevos que no estén en las notas pero que apliquen los mismos principios.
```

---

## 🗣️ Ejemplos de Prompts (Preguntas) Efectivas

Acá tenés algunas formas de sacarle provecho a la IA mientras estudiás con este Vault:

### 1. Nivelación y Contexto (¡Muy Importante!)
Las IAs tienden a explicar las cosas de forma muy compleja. Obligalas a respetar tu progreso:
> *"Estoy empezando a leer la nota `06 - Cryptography/13 - TLS en Profundidad.md`. Explícame cómo funciona el Handshake de TLS utilizando SOLAMENTE conceptos que ya estudié en los módulos 01 (Fundamentos) y 02 (Networking)."*

### 2. Identificación de Prerrequisitos
> *"Quiero hacer el `Lab 17 - Active Directory`. ¿Qué notas específicas de los módulos 04 y 08 necesito repasar y dominar antes de intentar este laboratorio?"*

### 3. Relación de Conceptos
> *"Basándote en las notas del Vault, relacioná los conceptos de DNS (Módulo 02), HTTP (Módulo 09) y TLS (Módulo 06). Explicame cómo trabajan juntos cuando entro a una página web de un banco."*

### 4. Preparación para Certificaciones
> *"Estoy estudiando para Security+. Revisa el estado de mi archivo `Progreso.md` y dime, basándote en la `Certification Coverage Matrix`, qué dominios del examen me faltan cubrir."*

### 5. Simulación de Tutor Socrático
> *"Estoy trabado en el Ejercicio 3 de la nota `10 - Python - Sockets`. No me des la respuesta del código. Haceme preguntas sobre cómo estoy estructurando el script para que me dé cuenta del error por mí mismo."*

### 6. Creación de Escenarios
> *"Ya terminé el módulo `14 - SOC & SIEM`. Generá un log de Apache web server ficticio donde se vea un intento de ataque de 'Path Traversal' (Módulo 09) y decime qué Reglas de Detección (Sigma o YARA) podría aplicar."*

### 7. Repaso y "ELI5" (Explain Like I'm 5)
> *"Leí la nota sobre `Kerberos` en el módulo de Windows, pero no entiendo la diferencia entre el TGT y el TGS. Explicámelo haciendo una analogía con comprar entradas en un parque de diversiones."*

---

## ⚠️ Advertencia sobre Alucinaciones

Asegurate de pedirle siempre a la IA que se base en los documentos del Vault. Los modelos pueden inventar herramientas (alucinaciones) o darte comandos incorrectos.

Si la IA te sugiere un comando para un laboratorio que parece peligroso (ej. `rm -rf` o ejecutar scripts descargados ciegamente), **verificalo siempre con la documentación oficial** o con las cheat sheets incluidas en la carpeta `00 - Inicio/Cheat Sheets/`.
