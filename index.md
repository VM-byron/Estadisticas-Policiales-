---
title: "Estadisticas policiales"
author: "Byron Vargas Montero"
date: '14-06-2022'
output:
  html_document:
    code_folding: hide
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


# Preparativos



## Carga de paquetes

```{r carga-paquetes, message=FALSE}
library(readr)
library(dplyr)
library(tidyr)
library(lubridate)
library(ggplot2)
library(plotly)
```

``` {r estadisticas-policiales, message=FALSE}
```


# Lectura de datos

```{r lectura-datos, message=FALSE}

estadísticas <-
  read_delim(
    readxl::read_excel("estadisticaspoliciales2021.xls"),
    delim = ";",
    col_select = c("Delito", "Fecha", "Edad", "Género", "Provincia", "Cantón")
  )
  
```


# Transformaciones

```{r transformaciones-datos}
estadísticas <-
  bioestadísticas %>%
  mutate(fecha = as.Date(fecha, format = "%Y-%m-%d")) %>%
  arrange(fecha)
```

# Tabla de datos

```
estadísticas <-
  datasets::estadisticas_policiales %>%
  select(Delito, Fecha, Edad, Género, Provincia, Cantón)


estadísticas %>%
  datatable(options = list(
    pageLength = 5,
    language = list(url = '//cdn.datatables.net/plug-ins/1.10.11/i18n/Spanish.json')
  ))
```

 

