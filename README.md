# 3er Corte - Segunda Actividad
Fecha: 31/03/2026
Autor: Cristian David Medina Hernández
Código: 90108
Asignatura: Electiva II
Docente: Diego Fernando Zárate Pineda

## Descripción

Base SvelteKit con arquitectura `feature-first/domain-first` para integrar la API de Rick and Morty.

# Proyecto de Prueba CI/CD

Este repositorio contiene una aplicación configurada con un pipeline de **Integración Continua y Despliegue Continuo (CI/CD)** a través de **GitHub Actions**.

## Características del Pipeline
- **Automatización:** Se ejecuta en cada `push` a la rama `main`.
- **Pruebas automáticas:** Valida la calidad y funcionamiento del código antes de cualquier despliegue.
- **Despliegue automático:** Implementa los cambios de forma continua para agilizar el ciclo de vida del software.

## Cómo usar este repositorio
1. Realiza cambios en el código.
2. Sube los cambios con `git push origin main`.
3. Observa el estado de la construcción en la pestaña **Actions** de GitHub.

---

## Run

```bash
npm install
npm run dev
```

## Check

```bash
npm run check
npm run build
```

## Estructura

- `src/lib/core`: cliente HTTP, configuracion y adaptadores.
- `src/lib/entities`: tipos y mapeos de dominio.
- `src/lib/features`: features por caso de uso.
- `src/routes`: composicion de rutas y carga de datos.
