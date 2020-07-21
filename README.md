# Vendas_Por_Semana
Qual a Probabilidade de Ter Um Número Específico de Vendas Por Semana?






# Distribuição de Probabilidade Para Variável Discreta - Qual a Probabilidade de Ter Um Número Específico de Vendas Por Semana?


######----------------- Distribuição Poisson -----------------------######

# Na teoria das probabilidades e estatística, a Distribuição de Poisson, nomeada em homenagem ao matemático francês 
# Siméon Denis Poisson, é uma distribuição de probabilidade discreta que expressa a probabilidade de um determinado número 
# de eventos ocorrendo em um intervalo fixo de tempo ou espaço, se esses eventos ocorrerem com uma taxa média constante 
# conhecida e independentemente do tempo desde o último evento. 

# A Distribuição Poisson também pode ser usada para o número de eventos em outros intervalos especificados, como distância, 
# área ou volume.

# A Distribuição de Poisson é a distribuição de probabilidade de ocorrências de eventos independentes em um 
# intervalo fixo de tempo ou espaço.

# A função R dpois (x, lambda) é a probabilidade de x sucessos em um período em que o número esperado de eventos é lambda. 
# A função R ppois (q, lambda, lower.tail) é a probabilidade cumulativa, sendo 
# (lower.tail = TRUE para a cauda esquerda, lower.tail = FALSE para a cauda direita) menor ou igual a q sucessos.

# Qual é a probabilidade de realizar de 2 a 4 vendas em uma semana se a taxa média de vendas for de 3 por semana?

# Usando a probabilidade exata
?dpois
dpois(x = 2, lambda = 3) + dpois(x = 3, lambda = 3) + dpois(x = 4, lambda = 3)

# Usando probabilidade acumulada (por que a quarta opção é a correta?)

ppois(q = 4, lambda = 3, lower.tail = TRUE) - ppois(q = 1, lambda = 3, lower.tail = TRUE)

# Vamos plotar todas as possibilidades
x <- ppois(q = 0:10, lambda = 3,  lower.tail = TRUE)
barplot(x, names.arg = 0:10, space = 0)

# Qual a probabilidade de qualquer número de vendas em uma semana se a taxa média de vendas for de 3 por semana?
# Número esperado de vendas = lambda = 3

# Pacotes
library(ggplot2)

# Formata valores decimais
options(scipen = 999, digits = 2) 

# Eventos possíveis (número de vendas)
eventos <- 0:10

# Calcula as probabilidades para todos os eventos, ou seja, a distribuição de probabilidades 
# para a variável aleatória.
probs <- dpois(x = eventos, lambda = 3)

# Calcula as probabilidades acumuladas para todos os eventos.
prob_acumulada <- ppois(q = eventos, lambda = 3, lower.tail = TRUE)

# Consolidade tudo em um dataframe
df <- data.frame(eventos, probs, prob_acumulada)
df

# Plot (cuidado com a escala do plot)

# Sem probabilidade acumulada
ggplot(df, aes(x = factor(eventos), y = probs)) +
        geom_col() +
        geom_text(aes(label = round(probs,2), y = probs + 0.01), position = position_dodge(0.9), size = 3, vjust = 0) +
        labs(title = "Distribuição Poisson Para Calcular a Probabilidade de Vendas Por Semana", 
             x = "Evento (Número de Vendas)", 
             y = "Probabilidade") 

# Com probabilidade acumulada
ggplot(df, aes(x = factor(eventos), y = probs)) +
        geom_col() +
        geom_text(aes(label = round(probs,2), y = probs + 0.01), position = position_dodge(0.9), size = 3, vjust = 0) +
        labs(title = "Distribuição Poisson Para Calcular a Probabilidade de Vendas Por Semana", 
             x = "Evento (Número de Vendas)", 
             y = "Probabilidade") +
        geom_line(data = df, aes(x = eventos, y = prob_acumulada))



