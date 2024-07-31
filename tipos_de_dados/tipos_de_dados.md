# Tipos de Dados no Prometheus

O Prometheus é um sistema de monitoramento e alerta que coleta métricas em tempo real e armazena essas informações em um banco de dados de séries temporais. Para organizar e representar esses dados, o Prometheus utiliza quatro tipos principais de dados:

## 1. **Counter (Contador)**

Um `Counter` é uma métrica que apenas aumenta ou é redefinida para zero. É utilizado para medir a quantidade total de eventos ou ocorrências, como o número de solicitações de HTTP recebidas ou a quantidade de erros que ocorreram.

### Exemplo

```plaintext
http_requests_total{method="post"} 1027
```

Nesse exemplo, o contador http_requests_total mede o total de solicitações HTTP recebidas, filtrado pelo método POST.

## 2. Gauge (Indicador)

Um Gauge é uma métrica que pode tanto aumentar quanto diminuir. Ele é utilizado para medir valores que podem variar, como a temperatura de uma CPU, a utilização de memória ou a quantidade de usuários conectados.

### Exemplo
```plaintext
memory_usage_bytes{process="nginx"} 2.3e+09
```

Aqui, memory_usage_bytes indica o uso de memória em bytes pelo processo nginx.

## 3. Histogram (Histograma)

Um Histogram é uma métrica que amostra observações (como solicitações HTTP ou tamanhos de pacotes) e as organiza em buckets de tamanho pré-definido. Ele também fornece o total de observações e a soma de todos os valores observados, útil para calcular a média.

### Exemplo

```plaintext
http_request_duration_seconds_bucket{le="0.05"} 24054
http_request_duration_seconds_sum 53423
http_request_duration_seconds_count 144320
```

Neste exemplo, http_request_duration_seconds mede a duração das requisições HTTP, com diferentes buckets para as durações observadas.

# 4. Summary (Resumo)

Um Summary é semelhante a um Histogram, mas com diferenças nas suas características. Ele fornece o total de observações, a soma de todos os valores observados e quantis configuráveis (como percentis 50, 90, 99). Ele é útil para medir latências e outras métricas onde os quantis são mais relevantes do que a distribuição de valores.

### Exemplo

```plaintext
http_request_duration_seconds{quantile="0.5"} 0.23
http_request_duration_seconds{quantile="0.9"} 0.42
http_request_duration_seconds_sum 53423
http_request_duration_seconds_count 144320
```go


Essa página explica os quatro tipos de dados suportados pelo Prometheus: `Counter`, `Gauge`, `Histogram` e `Summary`, com exemplos e explicações sobre como e quando usá-los.







Aqui, o Summary http_request_duration_seconds mede a duração das requisições HTTP, fornecendo quantis como 50% e 90%.

## Considerações Finais

Cada um desses tipos de dados é projetado para capturar informações específicas sobre o comportamento e o desempenho do sistema monitorado. A escolha do tipo de dado adequado é crucial para garantir a precisão e a utilidade das métricas coletadas pelo Prometheus.

Para mais detalhes sobre os tipos de dados e como utilizá-los, consulte a documentação oficial do Prometheus.