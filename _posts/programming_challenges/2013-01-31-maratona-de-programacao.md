---
layout: post
category : maratona
tags : [maratona, challenges, programming]
---
{% include JB/setup %}

Aqui eu coloco algumas dicas sobre Maratonas de Programação, uma das atividades que considero mais importantes para quem estuda computação.

## Background

Entrei na UFRGS em 2008. Como em todo semestre há maratonas de programação na semana acadêmica, participei já no primeiro. 

Em 2009, montamos um time: eu, Kauê Silveira e Bruno Fiss. Nosso time chegou a final nacional. Infelizmente, não participei tanto pois estava mais focado em escrever um artigo para a bolsa :/

Fiz a cadeira de Desafios de Programação em 2012/1. A cadeira é bem básica, mas fundamental para entender os principais fundamentos cobrados em maratonas.

Em 2012, formamos outro time: eu, André Ferreira e Leonardo Chatain. Duas semanas antes da regional, começamos a treinas pelas regionais passadas. Na regional em Pelotas/RS ficamos em primeiros colocados. Porém não ficamos contentes com nosso desempenho: eu travei num problema (que resolvi sem muito esforço mais tarde), todos não leram todos os problemas com cuidado (havia um fácil que foi mal interpretado) e num problema deveriamos saber sobre Euler Totient. :/

Continuamos praticando para a final. Alguns tópicos praticados foram: Suffix Array, LCP, Segment Tree, Rolling Hash, ... A final foi em Londrina/PR, ficamos em 12º. Entre os problemas, fiz um de Segment Tree rapidamente, porém por descuido eviei sem verificar alguns casos, tomamos uma penalidade. Em 2h de competição estavamos travados, resolvemos fazer um de grafos. Demoramos o resto da competição para fazer, sendo o André quem mais puxou nesse problema. Passamos nos últimos 3 minutos. Como nos últimos minutos de competição não dá para saber os resultados, só ficamos sabendo quando o placar foi anunciado no final da competição. 

## O que estudar

Aqui segue uma lista de tópicos que são fundamentais para se preparar para uma maratona.

### Data Strucutres

* Stacks/Queues/Heaps/...
* Segment Tree
* BitVec

### Graph

* DFS
* BFS
* Topological Sort
* Kruskal
* Prim
* Dijkstra (with previous nodes)
* Bellman Ford
* Floyd-Warshall (minimax, maximin, safest path, transitive hull)

### Dynamic Programming (DP)

* Knaspack
* LIS/LDS (n^2 e n log n)
* LCS
* Counting Change

### Math and Number Theory

* Base Convertion
* GCD/LCM
* Primes, sieve
* powMod
* Fibonacci Mod
* Polar coordinate system
* LatlongDistance
* Modular arithmetics
* Combinatorics
* Totient
* Carmichael Number
* Catalan Formula
* Recursion as matrix exponentiation

### Geometry
* Point, distance, triangle area, collinear, counter-clockwise
* Line, parallel
* Convex Hull: Graham Scan

### Strings

* Suffix array
* LCP

**\* A lista está incompleta, ainda deve ser finalizada!!**

## Livros e Material

#### Art of Programming Contest, Ahmed Shamsul Arefin

[Link para o livro](http://acm.uva.es/problemset/Art_of_Programming_Contest_SE_for_uva.pdf)

Livro bem básico e rápido. Problemas simples (e também antigos). Ótimo para começar.

#### The Hitchhiker’s Guide to the Programming Contests, Nite Nimajneb

Livro mais avançado, possuí vários conceitos básicos, mas que não caem diretamente em competições. Ler, entender e **levar para competições**.

#### Competitive Programming, Steven Halin

Livro da cadeira de Desafios e Programação. Os problemas desse livro são dividos em nível de dificuldade e podem ser encontrados no [uHunt](http://uhunt.felix-halim.net/).


#### CS 97SI: Introduction to Competitive Programming Contests

Curso da Universidade de Stanford. Na página há slides com muito conteúdo para estudar: [http://www.stanford.edu/class/cs97si/](http://www.stanford.edu/class/cs97si/)

Há também a cola usada pela equipe de Stanford: [http://stanford.edu/~liszt90/acm/notebook.html](http://stanford.edu/~liszt90/acm/notebook.html).

#### Curso: Algorithms, Robert Sedgewick, Kevin Wayne

Curso online da Universidade de Princeton. Baseado no livro [Algorithms (4th Edition)](http://www.amazon.com/gp/product/032157351X). Curso impotante para solidificar o conhecimento em algorítimos.

* [Parte I do curso](https://www.coursera.org/course/algs4partI)
* [Parte II do curso](https://www.coursera.org/course/algs4partII)

#### TopCoder Algorithm Tutorials

[https://community.topcoder.com/tc?module=Static&d1=tutorials&d2=alg_index](https://community.topcoder.com/tc?module=Static&d1=tutorials&d2=alg_index)

Lista com artigos sobre os principais algoritimos e técnicas cobrados em competições de programação. Alguns são mais avaçados. O interessante é que os artigos são inteiramente focados em competições.

## Sites de desafios

Alguns sites com desafios para praticar são:

* [uHunt](http://uhunt.felix-halim.net/id/52491) - Usa o banco de problemas do [UVa](http://uva.onlinejudge.org/) e sugere outros problemas para serem resolvidos
* [Topcoder](https://www.topcoder.com/)
* [Codeforces](http://codeforces.com/)
* [Hacker Rank Challenges](http://www.hackerrank.com) - Tem problemas interessantes e bem desafiadores. Algumas empresas usam esse site para recrutamento.
* [Hacker.org](http://www.hacker.org/)
* [Project Euler](https://projecteuler.net/) - Desafios matemáticos

Algumas sites de competições são:

* [Google Code Jam](https://code.google.com/codejam/)
* [Facebook Hackercup](https://www.facebook.com/hackercup)
* [Internet Problem Solving Contest - IPSC](http://ipsc.ksp.sk/)
* [Challenge 24](http://ch24.org/)
