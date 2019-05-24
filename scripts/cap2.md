Capítulo 2 - Statistical Learning
================

## 2.3 Lab: Introduction to R

### Comandos básicos

#### Dando input usando \<-

``` r
x <- c(1,3,2,5)
x 
```

    ## [1] 1 3 2 5

#### Dando input usando =

``` r
x = c(1,6,2)
x
```

    ## [1] 1 6 2

``` r
y = c(1,4,3)
y
```

    ## [1] 1 4 3

#### Somando os objetos:

``` r
x + y
```

    ## [1]  2 10  5

#### Listando e removendo os objetos criados:

``` r
ls()
```

    ## [1] "x" "y"

``` r
rm(x, y)
ls()
```

    ## character(0)

#### Pedindo ajuda do pacote **matrix**

``` r
?matrix
```

***Comentário**: Acho que criar matrizes é mais interessante usando a
função *frame\_matrix()* do pacote *tibble* do *Tidyverse*. A criação da
mesma matriz da página 44 do livro se daria de uma forma mais legível:*

``` r
x <- tibble::frame_matrix(
  ~",1", ~",2",
      1,     3,
      2,     4
  )
```

#### Funções estatísticas:

``` r
sqrt(x) 
```

    ##            ,1       ,2
    ## [1,] 1.000000 1.732051
    ## [2,] 1.414214 2.000000

``` r
x^2 
```

    ##      ,1 ,2
    ## [1,]  1  9
    ## [2,]  4 16

``` r
x <-  rnorm(50) 
y <-  x + rnorm(50, mean = 50, sd = 0.1) 
cor(x,y)
```

    ## [1] 0.996212

``` r
y <-  rnorm(100)
mean(y)
```

    ## [1] -0.05493279

``` r
var(y)
```

    ## [1] 0.9619593

``` r
sqrt(var(y))
```

    ## [1] 0.9807953

``` r
sd(y)
```

    ## [1] 0.9807953

### Criando gráficos com o pacote *graphics* (r base)

Para fazer gráficos, acho o \*ggplot\*\* mais intuitivo, apesar de que,
nesse caso do livro, é mais fácil digitar *plot(x,y)*:

``` r
set.seed(666)
x <-  rnorm(100)
y <- rnorm(100)
plot(x,y, xlab = "Eixo X", ylab = "Eixo Y", main = "Título")
```

![](cap2_files/figure-gfm/plot-1.png)<!-- -->

Criando um gráfico de contorno e um heatmap:

``` r
x <- seq(-pi, pi, length=50)
y <-  x 
f <-  outer(x, y, function(x, y)cos(y)/1+x^2)
contour(x, y, f)
contour(x, y, f, nlevels=45, add = T)
```

![](cap2_files/figure-gfm/contour-1.png)<!-- -->

``` r
fa <- (f-t(f))/2
contour(x, y, fa, nlevels = 15)
```

![](cap2_files/figure-gfm/contour-2.png)<!-- -->

``` r
image(x,y,fa)
```

![](cap2_files/figure-gfm/contour-3.png)<!-- -->

``` r
persp(x, y, fa, theta = 30, phi = 40)
```

![](cap2_files/figure-gfm/contour-4.png)<!-- -->

### Indexação de dados:

Selecionando, na segunda linha, o terceiro elemento de uma matriz:

``` r
A <- matrix(1:16, 4, 4)
A
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14
    ## [3,]    3    7   11   15
    ## [4,]    4    8   12   16

``` r
A[2,3]
```

    ## [1] 10

Lembrar que dentro de \[ \] o primeiro número se refere à linha e o
segundo à coluna Também é possível selecionar um conjunto de itens

``` r
A[c(1,3), c(2,4)]
```

    ##      [,1] [,2]
    ## [1,]    5   13
    ## [2,]    7   15

``` r
A[1:3, 2:4]
```

    ##      [,1] [,2] [,3]
    ## [1,]    5    9   13
    ## [2,]    6   10   14
    ## [3,]    7   11   15

``` r
A[1:2, ]
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14

``` r
A[, 1:2]
```

    ##      [,1] [,2]
    ## [1,]    1    5
    ## [2,]    2    6
    ## [3,]    3    7
    ## [4,]    4    8

### Bancos

#### O dataframe “Auto”

O livro sugere o *read.table*, mas o pacote *rio* permite importação de
vários formatos sem grandes esforços. Ao invés de *fix* para ver o
banco, o comando *View()* do RStudio é mais atual.

``` r
Auto <- read.table("Auto.data", header = T, na.strings = "?")
head(Auto)
```

    ##   mpg cylinders displacement horsepower weight acceleration year origin
    ## 1  18         8          307        130   3504         12.0   70      1
    ## 2  15         8          350        165   3693         11.5   70      1
    ## 3  18         8          318        150   3436         11.0   70      1
    ## 4  16         8          304        150   3433         12.0   70      1
    ## 5  17         8          302        140   3449         10.5   70      1
    ## 6  15         8          429        198   4341         10.0   70      1
    ##                        name
    ## 1 chevrolet chevelle malibu
    ## 2         buick skylark 320
    ## 3        plymouth satellite
    ## 4             amc rebel sst
    ## 5               ford torino
    ## 6          ford galaxie 500

``` r
dim(Auto)
```

    ## [1] 397   9

``` r
Auto[1:4, ]
```

    ##   mpg cylinders displacement horsepower weight acceleration year origin
    ## 1  18         8          307        130   3504         12.0   70      1
    ## 2  15         8          350        165   3693         11.5   70      1
    ## 3  18         8          318        150   3436         11.0   70      1
    ## 4  16         8          304        150   3433         12.0   70      1
    ##                        name
    ## 1 chevrolet chevelle malibu
    ## 2         buick skylark 320
    ## 3        plymouth satellite
    ## 4             amc rebel sst

``` r
Auto <- na.omit(Auto)
dim(Auto)
```

    ## [1] 392   9

``` r
names(Auto)
```

    ## [1] "mpg"          "cylinders"    "displacement" "horsepower"  
    ## [5] "weight"       "acceleration" "year"         "origin"      
    ## [9] "name"

Plotando:

``` r
plot(Auto$cylinders, Auto$mpg)
```

![](cap2_files/figure-gfm/plotauto-1.png)<!-- -->

``` r
Auto$cylinders <- as.factor(Auto$cylinders)
plot(Auto$cylinders, Auto$mpg, col = "red", varwidth = T, xlab = "Cilindros", ylab = "Milhas por galão")
```

![](cap2_files/figure-gfm/plotauto-2.png)<!-- -->

``` r
hist(Auto$mpg, col = 2, breaks = 15)
```

![](cap2_files/figure-gfm/plotauto-3.png)<!-- -->

``` r
pairs(~Auto$mpg + Auto$displacement + Auto$horsepower + Auto$weight + Auto$acceleration, Auto)
```

![](cap2_files/figure-gfm/plotauto-4.png)<!-- -->

Função *summary*:

``` r
summary(Auto)
```

    ##       mpg        cylinders  displacement     horsepower        weight    
    ##  Min.   : 9.00   3:  4     Min.   : 68.0   Min.   : 46.0   Min.   :1613  
    ##  1st Qu.:17.00   4:199     1st Qu.:105.0   1st Qu.: 75.0   1st Qu.:2225  
    ##  Median :22.75   5:  3     Median :151.0   Median : 93.5   Median :2804  
    ##  Mean   :23.45   6: 83     Mean   :194.4   Mean   :104.5   Mean   :2978  
    ##  3rd Qu.:29.00   8:103     3rd Qu.:275.8   3rd Qu.:126.0   3rd Qu.:3615  
    ##  Max.   :46.60             Max.   :455.0   Max.   :230.0   Max.   :5140  
    ##                                                                          
    ##   acceleration        year           origin                      name    
    ##  Min.   : 8.00   Min.   :70.00   Min.   :1.000   amc matador       :  5  
    ##  1st Qu.:13.78   1st Qu.:73.00   1st Qu.:1.000   ford pinto        :  5  
    ##  Median :15.50   Median :76.00   Median :1.000   toyota corolla    :  5  
    ##  Mean   :15.54   Mean   :75.98   Mean   :1.577   amc gremlin       :  4  
    ##  3rd Qu.:17.02   3rd Qu.:79.00   3rd Qu.:2.000   amc hornet        :  4  
    ##  Max.   :24.80   Max.   :82.00   Max.   :3.000   chevrolet chevette:  4  
    ##                                                  (Other)           :365

``` r
summary(Auto$mpg)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    9.00   17.00   22.75   23.45   29.00   46.60

## Exercícios

### Conceituais

1.  Método flexível de aprendizagem: melhor ou pior?

<!-- end list -->

  - Melhor, pois com muitas observações e poucas variáveis limitamos o
    modelo.
  - Pior, pois com muitas variáveis (preditores) e poucas observações, a
    variância seria enorme, gerando o que ele chamou de *overfitting*
    (p. 22)
  - Melhor, pois quando é linear, muitos pressupostos são requeridos, o
    que torna o modelo muito restrito e pouco flexível.
  - Pior, pois uma variância enorme gera o mesmo problema observado em
    b.

<!-- end list -->

2.  Explicar se fazemos: uma regressão ou classificação, uma predição ou
    inferência, e dizer *n* (tamanho da amostra) e *p* (número de
    preditores), em cada caso.

<!-- end list -->

  - Regressão. Inferência. *n* = 500, *p* = 3.
  - Classificação. Predição. *n* = 20, *p* = 13.
  - Regressão. Predição. *n* = 52, *p* = 3.

<!-- end list -->

3.  Depois.

4.  Aplicações na vida real:

<!-- end list -->

  - Sucesso ou falha em projetos de crowdfunding na internet
      - Preditores: tipo de projeto (produto, serviço, etc); tempo para
        somar os recursos; valor mínimo e máximo; variedade de valores;
      - Ambos. Entender o que define o sucesso de um projeto de
        crowdfunding permite planejar melhor os próximos, podendo prever
        a situação de novos possíveis projetos
  - Reeleição de incumbente para o governo do estado
      - Preditores: pertencer ao partido do presidente; taxa de
        desemprego no estado; diminuição de impostos no período anterior
      - Predição
  - Sugestão de filmes da Netflix
      - Preditores: filmes assistidos anteriormente; idade e sexo do
        usuário
      - Ambos.

<!-- end list -->

5.  Modelos mais flexíveis x modelos menos flexíveis:

<!-- end list -->

  - Vantagem: Se adapta melhor aos dados
  - Desvantagem: Dificuldade de interpretar

<!-- end list -->

6.  Nos modelos paramétricos, a forma dos dados é assumida, e um modelo
    escolhido para tal forma. Nos não-paramétricos, tal forma não é
    assumida.

7.  Tabela pro KNN:

<!-- end list -->

``` r
table <- tibble::tibble("Obs" = c(1, 2, 3, 4, 5, 6,0), "X1" = c(0,2,0,0,-1,1,0), "X2" = c(3,0,1,1,0,1,0), "X3" = c(0,0,3,2,1,1,0), "Y" = c("Red","Red","Red", "Green", "Green", "Red", " "))
print(table)
```

    ## # A tibble: 7 x 5
    ##     Obs    X1    X2    X3 Y    
    ##   <dbl> <dbl> <dbl> <dbl> <chr>
    ## 1     1     0     3     0 Red  
    ## 2     2     2     0     0 Red  
    ## 3     3     0     1     3 Red  
    ## 4     4     0     1     2 Green
    ## 5     5    -1     0     1 Green
    ## 6     6     1     1     1 Red  
    ## 7     0     0     0     0 " "

Calculando a distância euclidiana:

``` r
eucdist <- function(x, y) {
  sqrt(sum((x - y) ^ 2))
}
ob0 <- c(0,0,0)
ob1 <- c(0,3,0)
ob2 <- c(2,0,0)
ob3 <- c(0,1,3)
ob4 <- c(0,1,2)
ob5 <- c(-1,0,1)
ob6 <- c(1,1,1)

eucdist(ob1, ob0)
```

    ## [1] 3

``` r
eucdist(ob2, ob0)
```

    ## [1] 2

``` r
eucdist(ob3, ob0)
```

    ## [1] 3.162278

``` r
eucdist(ob4, ob0)
```

    ## [1] 2.236068

``` r
eucdist(ob5, ob0)
```

    ## [1] 1.414214

``` r
eucdist(ob6, ob0)
```

    ## [1] 1.732051
