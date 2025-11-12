#  Tutorial: Teste de Carga com Apache Benchmark (ab) em Aplicação React no Nginx

Este guia mostra como **instalar o Apache Benchmark (ab)** e realizar **testes de desempenho e carga** em uma aplicação **React servida pelo Nginx** (como no container configurado anteriormente).

---

##  1. Instalando o Apache Benchmark

Atualize os pacotes do sistema e instale o utilitário `apache2-utils`, que contém o comando `ab`:

```bash
sudo apt update && sudo apt install apache2-utils -y
```
Verifique se foi instalado corretamente:
```bash
ab -V
```
---
## 2. Executando um Teste de Carga
```bash
ab -n <NÚMERO_DE_REQUISIÇÕES> -c <CONCORRÊNCIA> <URL>
```
 Parâmetros:

| Parâmetro | Descrição |
|-----------|-----------|
| `-n` | Número total de requisições a serem enviadas |
| `-c` | Número de requisições simultâneas (nível de concorrência) |
| `<URL>` | Endereço da aplicação que será testada |
---
## 3. Exemplo Prático
```bash
ab -n 10000 -c 1000 http://localhost/
```
`-n 10000` → Envia 1000 requisições no total.

`-c 1000`→ Mantém 100 requisições simultâneas (concorrência).

`http://localhost/` → URL da aplicação que está sendo servida pelo Nginx.

---
## 4.Saída do Teste
```bash
This is ApacheBench, Version 2.3 <$Revision: 1903618 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests


Server Software:        nginx/1.29.3
Server Hostname:        localhost
Server Port:            80

Document Path:          /
Document Length:        384 bytes

Concurrency Level:      1000
Time taken for tests:   0.994 seconds
Complete requests:      10000
Failed requests:        0
Total transferred:      6170000 bytes
HTML transferred:       3840000 bytes
Requests per second:    10060.30 [#/sec] (mean)
Time per request:       99.401 [ms] (mean)
Time per request:       0.099 [ms] (mean, across all concurrent requests)
Transfer rate:          6061.72 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0   40  13.6     37     104
Processing:    13   51  14.2     48     111
Waiting:        1   36  12.6     34      75
Total:         64   91  17.4     89     209

Percentage of the requests served within a certain time (ms)
  50%     89
  66%     95
  75%    100
  80%    102
  90%    111
  95%    123
  98%    142
  99%    153
 100%    209 (longest request)
```
---
## 5.Interpretação de Resultados do Apache Benchmark
| Métrica | Descrição | Interpretação | 
|---------|-----------|---------------|
| **Requests per second** | Número de requisições processadas por segundo | Valores baixos podem indicar gargalos no servidor |
| **Time per request** | Tempo médio por requisição | Valores altos (> 500 ms) indicam servidor sobrecarregado | 
| **Failed requests** | Número de requisições com falha | Se > 0, investigar logs do servidor | 
| **Percentis (ex: 99%)** | Tempo máximo para X% das requisições | Se 99% ≫ média, há requisições esporádicas lentas | 


