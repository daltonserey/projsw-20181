class: center, middle
# O Protocolo HTTP
### ©2018 Dalton Serey
<!-- .center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)] -->

---
# Objetivos de aprendizagem

Ao final da aula, **você deve ser capaz de**...
--
count: false

1. explicar o que é o protocolo HTTP e quais suas principais
características;
--
count: false

1. explicar o que são _requests_ e _responses_ (a que chamaremos
de requisições e respostas);
--
count: false

1. descrever o formato das mensagens HTTP;
--
count: false

1. escrever e compreender mensagens simples escritas
de acordo com o protocolo;
--
count: false

1. interagir com servidores HTTP/1.1 diretamente sobre uma
conexão FTP (via `netcat` ou `telnet`);
--
count: false

1. escrever código que faça requisições e processe respostas
para/de um servidor HTTP;
--
count: false

1. entender os conceitos de recurso e uri.

---
# Protocolo HTTP

- protocolo de nível de aplicação
- conecta sistemas hipermídia colaborativos e distribuídos
- criado por Tim Berners-Lee
- RFCs 2068, 2616 e 7230...7235
- é simples: baseia-se em mensagens textuais humanamente legíveis
- interação baseada em: request e response
- o uso de cabeçalhos torna as mensagens e sua semântica
  muito extensível e ajustável, permitindo experimentos e usos
  inovadores
- interação _stateless_: não há relação entre dois requests
  sucessivos que usem a mesma conexão;
  - cookies adicionam a possibilidade de sessões
- http é _connectionless_, delegando isso pra camada de
  transporte, em particular, pra TCP; embora uma conexão não
  seja estritamente necessária; só requer que o canal seja
  confiável
- há estudos pra um serviço de transporte mais adequado para HTTP
  do que TCP; por ora, apenas estudos

  https://developer.mozilla.org/en-US/docs/Web/HTTP

---
# Aspectos importantes pro programador

- HTTP é _connectionless_
- HTTP é _stateless_
- HTTP é _cacheable_

---
# Mensagens HTTP

Formato textual simples:

1. uma linha inicial, concluída com um `'\n'`
2. _headers_ (cabeçalhos), concluídos com dois `'\n\n'`
3. _body_ (o corpo), o restante da mensagem até o `EOF`

A linha

Tanto _requests_, quanto _responses_ têm o mesmo formato.

---
# Request

```
<método> <uri> <protocolo>
<nome>: <valor>
<nome>: <valor>
<nome>: <valor>

<body>
```
- método: um _comando_ para o servidor
- uri: a indicação de um _recurso_
- protocolo: a versão do protocolo a usar

---
# Response

```
<protocolo> <status>
<nome>: <valor>
<nome>: <valor>
<nome>: <valor>

<body>
```

---
# exemplos de requests

```
GET / HTTP/1.0

```
--
count: false

```
GET / HTTP/1.1
Host: www.dsc.ufcg.edu.br

```
--
count: false
```
PUT /api/contagem HTTP/1.1
Host: 150.165.15.17:10006

850
```
--
count: false
```
POST /api/alunos HTTP/1.1
Host: www.seila.com.br:9000

Fulano da Silva
```
--
count: false
```
DELETE /api/alunos/6263123 HTTP/1.1
Host: www.seila.com.br:9000

```

---
# exemplos de responses (1/3)

```http
HTTP/1.1 200 OK
Date: Mon, 16 Apr 2018 16:28:06 GMT
Server: Apache/2.4.20 (Unix) PHP/5.6.23

<html>
<head>
  <title>homepage</title>
</head>
<body>
  <h1>Minha homepage.</p>
</body>
</html>
```


---
# exemplos de responses (2/3)

```http
HTTP/1.1 404 Not Found
X-Powered-By: Express
Content-Security-Policy: default-src 'self'
X-Content-Type-Options: nosniff
Content-Type: text/html; charset=utf-8
Content-Length: 155
Date: Mon, 16 Apr 2018 16:33:29 GMT
Connection: close

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Error</title>
</head>
<body>
<pre>Cannot GET /api/dadoqualquer</pre>
</body>
</html>
```

---
# exemplos de responses (3/3)

```http
HTTP/1.1 405 Method Not Allowed
Date: Mon, 16 Apr 2018 16:30:20 GMT
Server: Apache/2.4.20 (Unix) PHP/5.6.23
Allow: OPTIONS,GET,HEAD,POST,TRACE
Content-Length: 242
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>405 Method Not Allowed</title>
</head><body>
<h1>Method Not Allowed</h1>
<p>The requested method DELETE is not allowed for the URL /~dalton/index.html.</p>
</body></html>
```

---
# Cadeia cliente servidor

<img src="client-server-chain.png" style="width:90%;">

- entidades usam a camada de transporte (TCP)

---
# user-agent
  - browser, dispositivos, etc
  - é quem **sempre** inicia as conversas, com um request
  - recentemente, algumas situações invertidas são previstas

---
# origin sever
  - servidor com os dados de interesse do user-agent
  - pode servir arquivos prontos ou dados produzidos
  - pode armazenar dados recebidos pelo request
  - dados produzidos são hipertextos (ou hipermídia)
  - conceitualmente uma máquina, na prática podem ser várias
    - load balancing
    - memcache
    - data base servers
    - sensores
  - importante: servidor != máquina
    - em muitos casos, uma máquina = vários servers
    - em outros, um servidor = várias máquinas
    - podem compartilhar IPs ou não

---
# proxys
  - atuam como gateways, bridges, caches, etc
  - eles fazem _relay_ de mensagens HTTP
  - importante: alguns são parte do protocolo HTTP, e não são
    meramente parte das camadas de transporte/rede
  - proxies atuam na camada de aplicação
  - alguns são transparentes (repassam mensagens iguais), outros
    não
  - fazem: cache, filtragem, balanceamento de carga,
    autenticação, log, etc, etc;
    

---
# URI

Universal Resource Identifier

- location (URL) 
- e resource (URN)

Formato:

```
<protocolo>:/<host>[:porta][?<query-strings>][#<fragmentos>]
```

---
# O que podemos aprender do exemplo?
--
count: false

### Precisamos nos perguntar e responder...
--
count: false

- que decisões foram tomadas (_design_, _projeto_)?
--
count: false

- quais são as decisões estruturantes (_arquitetura_)?
--

count: false

- e quais as consequências dessas decisões?

--
count: false
<br>
Ao longo desta e das próximas aulas, discutiremos
respostas para estas questões. E também como podemos
aproveitar essas respostas ao construírmos outros sistemas.

---
# Lições aprendidas

- o servidor de uma aplicação web é um programa que responde a
  requisições enviadas pela web, usando o protocolo HTTP;

- o servidor ora responde com dados em html, JavaScript, css ora
  responde com dados estruturados em formato json; a esta parte
  chamei (embora sem definir) de API REST da aplicação;

- escrevemos o servidor em JavaScript, mas pode ser escrito em
  qualquer linguagem, desde que responda como indicado acima;

- o cliente de uma aplicação web é um programa tipicamente
  escrito em JavaScript que executa em um _browser_;

- o cliente é lido do servidor, no início da sessão do usuário;

- o cliente faz requisições à API REST do backend e usa os dados
  recebidos para atualizar o DOM e, portanto, a _view_ da
  aplicação;

---
class: center, middle
## Como programas em máquinas diferentes interagem?

#### Ou, como funciona a internet?
---
class: center, middle
### conexões host a host (desenho?)
<img src="host_to_host_dalton.jpg" style="width:90%;">

---
class: center, middle
## Suíte de Protocolos Internet
<img src="protocolos_tcp_ip_osi.jpg" style="width:90%;">

---
# Sugestões de estudo

Faça uma busca por materiais (bem) introdutórios sobre a
arquitetura e o funcionamento da Internet (posts ou vídeos são
suficientes). Você deve entender o design sobre o qual se apoia a
Internet e, em particular, os protocolos TCP e IP. Tenha em mente
que este não é um curso de redes, mas de desenvolvimento de
software. Logo, não se preocupe com detalhes e pormenores por
ora. Desenvolva um modelo mental que lhe permita explicar como
programas operam sobre a Internet, usando o protocolo TCP e as
garantias que oferece.

**Assista aos vídeos**. Apresenta o conceito central de uma rede de
pacotes no nível IP e de conexões seguras com TCP. O próprio Vint
Cerf fala no vídeo. O terceiro vídeo é bem interessante também,
embora mais monótono.

- [The Internet: IP Addresses & DNS](https://www.youtube.com/watch?v=5o8CwafCxnU)
- [The Internet: Packets, Routing & Reliability](https://www.youtube.com/watch?v=AYdF7b3nMto)
- [Introduction to TCP/IP](https://www.youtube.com/watch?v=b9HafRqtVWc)

Depois de assisti-los, ache alguns amigos leigos interessados e
explique a eles como funciona a internet! Melhore sua explicação
a cada oportunidade. Desenhe os elementos de rede envolvidos.

---
# Responda

- Quais os principais componentes de uma aplicação web moderna?
- Uma aplicação web moderna é um sistema distribuído? Por quê?
- Podemos dizer que é uma aplicação cliente-servidor?
- Como uma mensagem é enviada pela internet até seu destino?
- Por que o protocolo IP é dito ser _connectionless_ (sem conexão)?
- O que quer dizer a frase “TCP é um protocolo orientado a conexão”?
- O que quer dizer a frase “TCP oferece um serviço fim a fim confiável”?
- O que é um _stream_ TCP?
- O que são sockets?
