#  Hash Length Extension Attack Lab

---

## Introdução

O objetivo deste laboratório é demonstrar uma vulnerabilidade crítica na forma como alguns sistemas calculam Message Authentication Codes (MACs). Quando um MAC é calculado simplesmente concatenando uma chave secreta com uma mensagem e aplicando uma função de hash (`MAC = SHA256(key:message)`), torna-se vulnerável ao **Length Extension Attack**.

Este ataque explora a natureza iterativa das funções de hash como SHA-256, permitindo que um atacante estenda uma mensagem autenticada e calcule um MAC válido para a mensagem estendida, sem conhecer a chave secreta.

### Ambiente do Laboratório

- **Sistema Operativo:** SEED Ubuntu 22.04 ARM64
- **Virtualização:** VMware Fusion (Apple Silicon M3)
- **Ferramentas:** Docker, Python 3, GCC, OpenSSL
- **Servidor:** Container Docker com servidor web Flask

---

## Setup do Ambiente

### 1. Configuração Inicial

Após descarregar e descompactar o ficheiro `Labsetup-arm.zip`, configurámos o ambiente Docker:
```bash
cd ~/labs/Labsetup-arm
docker-compose build
docker-compose up -d
docker ps
```

**[11.png]** 

### 2. Configuração do /etc/hosts

Adicionámos a seguinte entrada ao ficheiro `/etc/hosts` para mapear o hostname do servidor:
```
10.9.0.80 www.seedlab-hashlen.com
```

### 3. Obtenção das Chaves

Acedemos ao container para visualizar as chaves disponíveis:
```bash
docker exec -it fe3 /bin/bash
cat /app/LabHome/key.txt
```

Escolhemos:
- **UID:** 1001
- **Chave:** 123456
- **Nome:** SofiaCoutinho

---

## Tarefa 1: Enviar Pedido para Listar Ficheiros

### Objetivo

Familiarizar-nos com o sistema de autenticação MAC e enviar pedidos autenticados legítimos ao servidor.

### Procedimento

#### 1.1 Cálculo do MAC para listar ficheiros

Calculámos o MAC usando SHA-256 sobre a concatenação `key:message`:
```bash
echo -n "123456:myname=SofiaCoutinho&uid=1001&lstcmd=1" | sha256sum
```

**Resultado:**
```
bdf6f4262fab93d2a689eafc9cf17ed91a8205250a5655ddfce15dd8e94c43d6
```

**[22.png]** 

#### 1.2 Teste do pedido lstcmd

Construímos o URL:
```
http://www.seedlab-hashlen.com/?myname=SofiaCoutinho&uid=1001&lstcmd=1&mac=bdf6f4262fab93d2a689eafc9cf17ed91a8205250a5655ddfce15dd8e94c43d6
```

**[33.png]** 

#### 1.3 Cálculo do MAC para download

Calculámos o MAC incluindo o comando de download:
```bash
echo -n "123456:myname=SofiaCoutinho&uid=1001&lstcmd=1&download=secret.txt" | sha256sum
```

**Resultado:**
```
62f21ce7b74b644953d72a00bc8d4013be14aa83775df999857c929602a84628
```

**[44.png]** 

#### 1.4 Teste do pedido download

URL construído:
```
http://www.seedlab-hashlen.com/?myname=SofiaCoutinho&uid=1001&lstcmd=1&download=secret.txt&mac=62f21ce7b74b644953d72a00bc8d4013be14aa83775df999857c929602a84628
```

**[55.png]** 

### Análise

Nesta tarefa, demonstrámos que com conhecimento da chave secreta, podemos calcular MACs válidos para qualquer mensagem. O servidor verifica a integridade calculando `SHA256(key:parametros_URL)` e comparando com o MAC fornecido.

---

## Tarefa 2: Criar Padding

### Objetivo

Calcular o padding correto que o SHA-256 adiciona à mensagem original para completar um bloco de 64 bytes.

### Estrutura do Padding SHA-256

O SHA-256 processa mensagens em blocos de 64 bytes. O padding consiste em:
1. **1 byte:** `\x80` (bit 1 seguido de zeros)
2. **N bytes de zeros:** Para preencher
3. **8 bytes:** Length field (tamanho da mensagem em bits, Big-Endian)

### Implementação

Criámos um script Python (`padding_calculator.py`) para calcular o padding:

**[66.png]** 

### Execução e Resultados
```bash
python3 padding_calculator.py
```

**[77.png]** 
- Tamanho da mensagem: **45 bytes**
- Tamanho em bits: **360 bits (0x168)**
- Padding: **19 bytes** (1 + 10 zeros + 8 length field)
- Tamanho total: **64 bytes** ✓
- Padding para URL: `%80%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%01%68`

### Análise

O padding é crucial porque:
1. Garante que a mensagem + padding = múltiplo de 64 bytes
2. O length field contém o tamanho **original** da mensagem (antes do padding)
3. Este padding será incluído no nosso URL de ataque na Tarefa 3

**Fórmula do padding:**
```
Mensagem (45 bytes) + \x80 (1 byte) + Zeros (10 bytes) + Length (8 bytes) = 64 bytes
```

---

## Tarefa 3: The Length Extension Attack

### Objetivo

Explorar a vulnerabilidade do MAC baseado em concatenação simples, forjando um MAC válido para uma mensagem modificada **sem conhecer a chave secreta**.

### Teoria do Ataque

O SHA-256 é uma função iterativa que processa blocos de 64 bytes. O estado interno após processar um bloco torna-se a entrada para o próximo bloco. 

**Insight chave:** O MAC da mensagem original é exatamente o estado interno do SHA-256 após processar `key:message:padding`. Podemos usar este estado para continuar o cálculo e adicionar uma nova mensagem.

### Implementação

#### 3.1 Código C para Length Extension

Criámos o ficheiro `length_ext.c`:

**[88.png]** 

**Pontos importantes do código:**
- Dividimos o MAC original em 8 partes de 32 bits cada
- Usamos `htole32()` para converter para little-endian (ARM)
- Simulamos o processamento de 64 bytes (mensagem original + padding)
- Adicionamos a mensagem maliciosa: `&download=secret.txt` (**20 bytes**)

#### 3.2 Compilação e Execução
```bash
gcc length_ext.c -o length_ext -lcrypto
./length_ext
```

**[99.png]** 
```
4bf5f436d78aa1b1406df984ed14e80db47fce9547bcc9d82c88e2748fa1a1f8
```

#### 3.3 Construção do URL Malicioso

O URL final contém:
1. Pedido original da Tarefa 1: `myname=SofiaCoutinho&uid=1001&lstcmd=1`
2. Padding calculado na Tarefa 2: `%80%00%00...%01%68`
3. Comando malicioso: `&download=secret.txt`
4. MAC forjado: `4bf5f436d78aa1b1406df984ed14e80db47fce9547bcc9d82c88e2748fa1a1f8`

**URL completo:**
```
http://www.seedlab-hashlen.com/?myname=SofiaCoutinho&uid=1001&lstcmd=1%80%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%01%68&download=secret.txt&mac=4bf5f436d78aa1b1406df984ed14e80db47fce9547bcc9d82c88e2748fa1a1f8
```

#### 3.4 Resultado do Ataque

**[1010.png]** 
```
Yes, your MAC is valid
File Content
TOP SECRET.
DO NOT DISCLOSE.
```

### Análise Técnica

**Por que o ataque funcionou:**

1. O servidor calcula: `SHA256(key:myname=...&lstcmd=1:padding:&download=secret.txt)`
2. Nós calculámos: `SHA256_continue(MAC_original, &download=secret.txt)`
3. Como o MAC original já representa o estado interno após processar `key:message:padding`, continuar o hash produz o mesmo resultado!



**Limitação importante:** Precisamos de conhecer:
- O tamanho da chave (para calcular padding correto);
- O MAC da mensagem original;
- A estrutura do sistema de autenticação.

---

## Conclusões

### Vulnerabilidade Demonstrada

Demonstrámos com sucesso que MACs calculados como `MAC = SHA256(key:message)` são **fundamentalmente inseguros**. Um atacante que:
1. Conhece um par (mensagem, MAC) válido;
2. Conhece o tamanho da chave;
3. Pode calcular MACs válidos para mensagens estendidas **sem a chave**.

### Por que o Ataque Funciona

O SHA-256  processa dados em blocos. O estado interno após processar um bloco depende apenas:
- Do estado anterior;
- Do bloco atual.

**Não há forma** de o servidor distinguir entre:
- Um hash calculado desde o início com a chave
- Um hash continuado a partir de um MAC conhecido

### Mitigação: HMAC

O ataque **não funciona** contra HMAC porque:


HMAC usa **duas passagens** da função de hash com chaves derivadas diferentes. Mesmo conhecendo `HMAC(K, m1)`, não conseguimos calcular `HMAC(K, m1 || m2)` porque:
1. O estado interno da primeira passagem não é o HMAC final;
2. A segunda passagem usa uma chave diferente.

### Lições Aprendidas

1. **Nunca implemente criptografia "à mão"** - use primitivas padrão (HMAC, não concatenação);
2. **Entender a estrutura interna** das funções criptográficas é essencial para as usar corretamente;
3. **Length extension attacks** são um exemplo clássico de vulnerabilidade em construções "óbvias" mas incorretas;
4. **Standards existem por uma razão** - HMAC (RFC 2104) foi criado exatamente para prevenir este ataque.

---

