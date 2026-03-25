# 🔬 Microservicio de Detección de Afirmaciones Científicas y Recuperación de Fuentes

Microservicio de NLP integrado en **FactCheckToolEC**, plataforma ecuatoriana de verificación de noticias. Detecta automáticamente si una noticia contiene discurso científico y recupera artículos académicos relevantes como evidencia de apoyo.

Desarrollado como proyecto de tesis de grado en Ingeniería en Sistemas de Información — Universidad de Guayaquil (2026).

---

## 🎯 Problema que resuelve

Los verificadores de hechos en Ecuador reciben cientos de noticias diariamente. Identificar manualmente cuáles contienen afirmaciones científicas verificables y buscar literatura académica que las respalde o refute es un proceso lento y costoso. Este microservicio automatiza ambas tareas.

---

## ⚙️ ¿Cómo funciona?

Para cada noticia analizada, el sistema ejecuta el siguiente pipeline:

```
Texto de noticia
      │
      ▼
1. Normalización del texto
   (limpieza de caracteres especiales, entidades HTML)
      │
      ▼
2. Clasificación con BETO fine-tuneado
   ┌─────────────────────────────────────────────┐
   │  claim   → ¿Contiene afirmación científica  │
   │             verificable?          (Sí / No) │
   │  context → ¿Menciona científicos,           │
   │             instituciones o investigación?  │
   │                                   (Sí / No) │
   └─────────────────────────────────────────────┘
      │
      ▼
3. Si claim=1 OR context=1 → Recuperación de fuentes
   con multilingual-e5-base (similitud semántica)
      │
      ▼
4. Top-5 documentos académicos recuperados
   (Crossref + PubMed)
      │
      ▼
5. Persistencia en MongoDB
```

---

## 📊 Métricas del modelo

| Tarea | Métrica | Resultado |
|-------|---------|-----------|
| Clasificación (BETO) | F1-Macro | **0.85** |
| Recuperación de fuentes (E5) | MRR@5 | **0.6563** |

---

## 🗂️ Dataset

El corpus fue construido y anotado manualmente desde cero a partir de noticias reales de organizaciones ecuatorianas de verificación de hechos, adaptando el framework **SciTweets** para el contexto ecuatoriano en español.

```
datasets/
└── corpus_anotado.csv     # Registros anotados con etiquetas claim/context
```

**Criterios de anotación:**
- `claim = 1` → El texto contiene una proposición fáctica de carácter científico comprobable con estudios, papers o datos de investigación.
- `context = 1` → El texto menciona científicos, instituciones, laboratorios o procesos de investigación.
- Ambas etiquetas son independientes; una noticia puede tener una, ambas o ninguna.

---

## 🖥️ Demo del sistema en producción

### Módulo de Verificación Científica — vista principal
![Vista principal del módulo](images/modulo_principal.png)

### Noticias pendientes de análisis en el modal de detalles
![Noticias pendientes](images/noticias_pendientes.png)

### Resultado del análisis en el modal de detalles (Noticias procesadas)
![Resultado del análisis](images/resultado_analisis.png)

### Etiquetado manual por el verificador
![Etiquetado manual](images/etiquetado_manual.png)

---

## 🛠️ Stack tecnológico

| Capa | Tecnología |
|------|-----------|
| Modelos NLP | BETO (fine-tuned), multilingual-e5-base |
| Backend | Python, FastAPI |
| Base de datos | MongoDB |
| Fuentes académicas | Crossref API, PubMed API |
| Contenerización | Docker |
| Orquestación | Kubernetes (Oracle Kubernetes Engine) |
| Cloud | Oracle Cloud Infrastructure (OCI) |
| Frontend | Next.js |

---

## 📁 Estructura del repositorio

```
├── datasets/
│   └── corpus_anotado.csv
├── images/
│   └── *.png                  # Capturas del sistema en producción
└── README.md
```

> ⚠️ El código fuente del microservicio forma parte del repositorio institucional del proyecto FactCheckToolEC (Universidad de Guayaquil). Los modelos fine-tuneados no están incluidos por su tamaño; pueden descargarse desde HuggingFace.

---

## 🔗 Enlaces

- 🌐 **Sistema en producción:** FactCheckToolEC: http://149.130.164.117/verificacion-cientifica

---

## 👩‍💻 Autora

**Camila Chusan Zambrano**
Ingeniera en Sistemas de Información — Universidad de Guayaquil
[LinkedIn](https://www.linkedin.com/in/camilachusan)

**Anthony Pluas Mero**
Ingeniero en Sistemas de Información — Universidad de Guayaquil
