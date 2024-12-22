mieszkania <- read.csv("apartments_pl_2024_05.csv")
View(mieszkania)

install.packages("naniar")
library(naniar)
install.packages("visdat")
vis_miss(mieszkania)
install.packages("finalfit")
library(finalfit)
missing_pattern(mieszkania)

install.packages("tidyverse")
install.packages("dlookr")
install.packages("editrules")
install.packages("VIM")
install.packages("validate")
library(tidyverse)
library(dlookr)
library(editrules)
library(VIM)
library(validate)

mieszkania[mieszkania == " "] <- NA
View(mieszkania)

mieszkania[mieszkania == "  "] <- NA
View(mieszkania)

mieszkania[mieszkania == ""] <- NA
View(mieszkania)

mieszkania[localizeErrors(reguly,mieszkania)$adapt] <- NA

#reguły
reguly <- editset(c(
  "price>0",
  "squareMeters>0",
  "rooms>0",
  "floor>0",
  "floorCount>0"
  ))

summary(violatedEdits(reguly, mieszkania))
bledy <- violatedEdits(reguly, mieszkania)
plot(bledy)

#imputacje braków danych
czyste_mieszkania <- hotdeck(mieszkania)
View(czyste_mieszkania)

install.packages("ggplot2")
install.packages("hrbrthemes")
install.packages("plotly")
install.packages("ISLR")
library(ggplot2)
library(hrbrthemes)
library(plotly)
library(ISLR)

ggplot(czyste_mieszkania, aes(x=price,fill=condition)) + geom_histogram(binwidth=100000) + labs(title="ceny mieszkań na sprzedaż w Polsce", x="cena", y="ilość") + theme_ipsum()
  
ggplot

ggplot(czyste_mieszkania, aes(x=price,fill=hasParkingSpace)) + geom_histogram(binwidth=100000) + labs(title="ceny mieszkań na sprzedaż w Polsce", x="cena", y="ilość") + theme_ipsum()

ggplot(czyste_mieszkania, aes(x=price,fill=type)) + geom_histogram(binwidth=100000) + labs(title="ceny mieszkań na sprzedaż w Polsce", x="cena", y="ilość") + theme_ipsum()

ggplot(czyste_mieszkania, aes(x=price,fill=buildingMaterial)) + geom_histogram(binwidth=100000) + labs(title="ceny mieszkań na sprzedaż w Polsce", x="cena", y="ilość") + theme_ipsum()

