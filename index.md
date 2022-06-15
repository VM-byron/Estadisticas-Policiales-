---
title: "Estadísticas Policiales 2021"
author: "Byron Vargas Montero"
date: '2022-06-15'
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
# Cargas de Paquetes
library(readr)
library(dplyr)
library(ggplot2)
library(plotly)
library(DT)
library(sf)
library(leaflet)
```


# Lectura de datos

```
estadisticas <-
  readxl::read_excel("estadisticaspoliciales2021.xls")
```


# Tabla 


```{r tabla}

estadisticas %>%
dplyr::select(Delito, Fecha, Victima, Edad, Género, Provincia, Cantón)
DT::datatable(colnames = c("Delito", "Fecha", "Víctima", "Edad", "Género", "Provincia", "Cantón"), options = list(pageLength = 5,
language = list(url = '//cdn.datatables.net/plug-ins/1.10.11/i18n/Spanish.json')
)
) 

```





 
