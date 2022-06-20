# Tarea-datos-policiales
---
title: "Analisis de Estadisticas Policial 2021"
author: "Christopher Cadena Calvo"
date: '2022-06-19'
output:
  html_document:
    theme: readable    
    toc: true
    toc_depth: 5
    toc_float:
      collapsed: false
      smooth_scroll: false
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

#Tarea 2. Instrucciones

## Entradas
### Carga de paquetes de R

```{r carga de paquetes, message=FALSE}
library(DT)
library(ggplot2)
library(plotly)
library(dplyr)
library(leaflet)
library(sf)
library(readxl)
library(tidyverse)
library(ggthemes)

```

```{r, message=FALSE}
Estadisticas_Policiales <- 
  readxl::read_excel("C:/tarea_2_procesamiento/estadisticaspoliciales2021.xls")

Estadisticas_Policiales$Fecha <- as.Date(Estadisticas_Policiales$Fecha, format = "%Y-%m-%d")

```

#Desarrollo
## Tabla DT

```{r, message=FALSE}
# Tabla interactiva del paquete DT

Estadisticas_Policiales %>%
  dplyr::select(Delito,
                Fecha,
                Victima,
                Edad,
                Genero,
                Provincia,
                Canton) %>%
  mutate(Fecha = as.Date(Fecha, format = "%d/%m/%Y")) %>%
  datatable(
    options = list(
      pageLength = 15,
      language = list(url = '//cdn.datatables.net/plug-ins/1.10.11/i18n/Spanish.json')
    ),
    colnames = c(
      # encabezados de las columnas
      "Delito",
      "Fecha",
      "Víctima",
      "Edad",
      "Género",
      "Provincia",
      "Cantón"
    )
  )
```

## Gráfico de barras simples de Cantidad de Delitos por tipo de Delito

```{r}
Cantidad_de_delito_por_tipo <-
  Estadisticas_Policiales %>%
  count(Delito) %>%
  ggplot(aes(x = reorder(Delito, n), y = n)) +
  geom_bar(stat = "identity") +
  ggtitle("Cantidad de delitos por tipo de delito") +
  xlab("Tipo de Delito") +
  ylab("Cantidad") +
  coord_flip() +
  theme_hc()

Cantidad_de_delito_por_tipo %>%
  ggplotly() %>%
  config(locale = "es")
```

## Gráfico simple de Cantidad de Delitos por mes

```{r}
#FALTA
```

## Gráfico de barras apiladas de proporción de Delitos por Género

```{r}
Proporcion_de_Delito_por_Genero <-
  Estadisticas_Policiales %>%
  ggplot(aes(x = Genero, fill = Delito)) +
  geom_bar(position = "fill") +
  ggtitle("Proporciones de Delito por tipo de Género") +
  xlab("Género") +
  ylab("Proporción") +
  labs(fill = "Delito") +
  theme_minimal()

ggplotly(Proporcion_de_Delito_por_Genero) %>% config(locale = 'es')
```

## Gráfico simple de Cantidad de Delitos en San José, Alajuela, Cartago y Heredia 

```{r}

grafico <- filter(Estadisticas_Policiales, grepl('HEREDIA|ALAJUELA|CARTAGO|SAN JOSE', Canton))

Delitos_SanJose_Alajuela_Cartago_Heredia <-
  ggplot(data= grafico, aes(x=Canton)) +
  geom_bar( ) +
  ggtitle("Delitos en San Jose Alajuela Cartago Heredia") +
  xlab("Cantones") +
  ylab("Cantidad de Delitos") +
  theme_minimal()

ggplotly(Delitos_SanJose_Alajuela_Cartago_Heredia) %>% config(locale = 'es')

```


