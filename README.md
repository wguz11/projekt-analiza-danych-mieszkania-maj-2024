mieszkania <- read.csv("apartments_pl_2024_05.csv")
View(mieszkania)


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
install.packages ("GGally")
library(GGally)
library(tidyverse)
library(dlookr)
library(editrules)
library(VIM)
library(validate)
library(dplyr)

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

ggplot(czyste_mieszkania, aes(x = price, fill = type)) +
geom_density(alpha = 0.5) +
labs(
title = "Rozkład cen mieszkań według standardu lokalu",
x = "Cena" ,
y = "Typ",
fill = "Typ"
) +
theme_minimal()
)

ggplot(czyste_mieszkania, aes(x = city, y = price)) +
geom_boxplot(fill = "pink", color = "blue") +
labs(
title = "Cena mieszkań według miasta",
x = "miasto" ,
y = "cena"
) +
theme_minimal()
theme(axis.text.x = element_text(angle = 45, hjust = 1))


ggcorr(
data = czyste_mieszkania %>% select(where(is.numeric)),
method = c("pairwise.complete.obs" , "pearson"),
label = TRUE
) +
theme(
axis.text.x = element_text(hjust = 1), 
axis.text.y = element_text(hjust = 1)
)

install.packages("summarytools")
mean(price)
    median(price)
    sd(price) #standard deviation
    var(price) #variance
    coeff_var<-sd(price)/mean(price) #coefficient of variability %
    coeff_var
    IQR(price)# difference between quartiles =Q3-Q1 
    sx<-IQR(price)/2  #interquartile deviation
    coeff_varx<-sx/median(price) #IQR coefficient of variability %
    coeff_varx
    min(price)
    max(price)
    quantile(price,probs=c(0,0.1,0.25,0.5,0.75,0.95,1),na.rm=TRUE)

install.packages("qwraps2")
install.packages ("psych")

price czyste_mieszkania$price


library(psych)
raport <-
  list("price" =
       list("Min"=  min(price),
            "Max"=  max(price),
            "Kwartyl dolny"=  quantile(price,0.25),
            "Mediana"=  round(median(price),2),
            "Kwartyl górny"=  quantile(price,0.75),
            "Średnia"=  round(mean(price),2),
            "Odch. std."=  round(sd(price),2),
            "IQR"=  round(IQR(price),2),
            "Odchylenie ćwiartkowe"=round(IQR(price)/2,2),
            "Odch. std. w %"=round((sd(price)/mean(price)),2),
            "Odch. ćwiartkowe w %"=round((IQR(price)/median(price)),2),
            "Skośność"=round(skew(price),2),
            "Kurtoza"=round(kurtosi(price),2)
            ))

table<-summary_table(czyste_mieszkania, summaries = raport, by = c("city"))



