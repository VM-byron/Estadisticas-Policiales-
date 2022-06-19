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



# Descripción 

Esta es la segunda tarea del curso de Procesamientos de Datos Geográficos (GF-0604) la cual tiene como objetivo que cada estudiante realice a partir de las estadisticas policiales del 2021 tablas y gráficos interactivos para detallar ciertos aspectos requeridos por el docente en dichas tablas y gráficos. 
Estos datos fueron extraidos de la siguiente página del [Organismo de Investigación Judicial (OIJ)] (https://sitiooij.poder-judicial.go.cr/index.php/ayuda/servicios-policiales/servicios-a-organizaciones/indice-de-transparencia-del-sector-publico-costarricense/datos-abiertos) 

  
## Carga de paquetes a emplear 

``` 
library(ggplot2)
library(readxl)
library(plotly)
library(dplyr)
library(sf)
library(DT)
```


```
estadisticas <-
  read_excel("estadisticaspoliciales2021.xls")
estadisticas = subset(
  estadisticas,
  select = c(
    "Delito",
    "Fecha",
    "Victima",
    "Edad",
    "Genero",
    "Provincia",
    "Canton"
  )
)
```  

## **Tabla de datos**  

``` 
estadisticas$Fecha = as.Date(estadisticas$Fecha)
colnames(estadisticas) = c("Delito",
                             "Fecha",
                             "Víctima",
                             "Edad",
                             "Género",
                             "Provincia",
                             "Cantón")
estadisticas %>%
  datatable(options = list(
    pageLength = 5,
    language = list(url = '//cdn.datatables.net/plug-ins/1.10.11/i18n/Spanish.json')
  ))
```  


## Gráfico de cantidad y tipo de delitos  


Grafics <-
  estadisticas %>%
  count(Delito) %>%
  ggplot(aes(x = order(n, Delito), y = Delito)) +
  geom_bar(stat = "identity") +
  coord_flip() +
  ggtitle("Cantidad de delitos según sea el tipo de delito en Costa Rica") +
  xlab("Cantidad delitos") +
  ylab("Delitos") +
  theme_linedraw()
  
ggplotly(Grafics) %>% 
  config(locale = 'es')
```  

## Gráfico del total de delitos por número de mes 

```{r, Segundo gráfico, message=FALSE, warning=FALSE}  
Grafics_2 <-
  estadisticas %>% mutate(meses = lubridate::month(Fecha))
número_meses <-
  c(
    "1",
    "2",
    "3",
    "4",
    "5",
    "6",
    "7",
    "8",
    "9",
    "10",
    "11"
  )
Grafics_2 <-
  Grafics_2 %>%
  count(meses) %>%
  ggplot(level = levelorder, (aes(
    x = reorder(número_meses, meses), y = n
  ))) +
  geom_bar(stat = "identity") +
  ggtitle("Total de delitos al mes") +
  xlab("Número de Meses") +
  ylab("Total de delitos") +
  theme_linedraw()
  
ggplotly (Grafics_2) %>% 
  config(locale = 'es')
```  

## Gráfico detallando la proporción de delitos según sea el mes   

```{r, Tercer gráfico, message=FALSE, warning=FALSE}  
Grafics_3 <-
  estadisticas %>%
  ggplot(aes(x = Delito, fill = Género)) +
  geom_bar(position = "fill") +
  coord_flip() +
  labs(fill = "Género") +
  ggtitle("Proporción de delitos según sea su diferente género") +
  xlab("Modalidad de delito") +
  ylab("Proporción")
  
  
ggplotly(Grafics_3) %>% 
  config(locale = 'es')
```  

## Gráfico de cantones con la cantidad de delitos  

```{r, Cuarto gráfico, message=FALSE, warning=FALSE}  

grafics_4 <-
  estadisticas %>%
  count(Cantón) %>%
  filter(Cantón == "SAN JOSE" |
           Cantón == "ALAJUELA" |
           Cantón == "CARTAGO" | 
           Cantón == "HEREDIA") %>%
  ggplot(aes(x = reorder(Cantón, n), y = n)) +
  coord_flip() +
  geom_bar(stat = "identity") +
  ggtitle("Delitos por cantón en San José, Alajuela, Cartago, Heredia") +
  xlab("Cantón") +
  ylab("total") +
  theme_linedraw()
  
ggplotly(grafics_4) %>% 
  config(locale = 'es')
```



 
