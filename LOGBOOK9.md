# LOGBOOK 9 - Secret Key Encryption


## Task 1 - Frequency Analysis

## 1\. Objetivo e Metodologia

O objetivo desta tarefa foi decifrar o conteúdo do ficheiro `ciphertext.txt`. O ficheiro contém uma mensagem em inglês encriptada com uma **cifra de substituição monoalfabética**, onde cada letra do texto original é mapeada para uma letra diferente (mas fixa) no texto cifrado.

Para quebrar a cifra, utilizámos a **Análise de Frequência**. Como o texto original está em inglês, podemos comparar a frequência das letras, bigramas (pares de letras) e trigramas (trios) do texto cifrado com as médias estatísticas da língua inglesa.

**Ferramentas utilizadas:**

  * **`freq.py`**: Script para calcular as frequências absolutas no texto cifrado.
  * **`tr`**: Comando de terminal para realizar a transposição de caracteres (`tr <encrypted> <original> < input > output`).
  * **Análise Manual**: Dedução baseada em contexto e padrões linguísticos.

## 2\. Processo de Decifragem

### Ponto de Partida: Trigramas

A análise começou pelo trigrama mais comum no texto cifrado: **`ytn`**.

  * Estatisticamente, o trigrama mais comum em inglês é **"THE"**.
  * O trigrama `ytn` contém dois caracteres de alta frequência (`y` e `n`) separados por um de frequência média (`t`).
  * **Hipótese inicial:** `ytn` -\> `THE`.

Comando inicial:

```bash
tr "ytn" "THE" < ciphertext.txt > message.txt
```

### Substituição Iterativa

A partir da identificação do "THE", analisámos o ficheiro `message.txt` gerado parcialmente para identificar palavras incompletas e deduzir as restantes letras.

## 3\. Execução Técnica Final

Após identificar todas as 26 letras, construímos o comando final para decifrar o texto completo:

```bash
tr "ytnvxihqbmagplurczedfsojkw" "THEAOLRSFICBDWNGMUPYVKJQXZ" < ciphertext.txt > message.txt
```

### Chave de Mapeamento

A tabela abaixo mostra a correspondência completa entre o alfabeto original e o cifrado (Chave):

| | Alfabeto |
| :--- | :--- |
| **Original** | `A B C D E F G H I J K L M N O P Q R S T U V W X Y Z` |
| **Cifrado** | `V G A P N B R T M O S I C U X E J H Q Y Z F L K D W` |

## 4\. Texto Decifrado

A mensagem final, após a aplicação da cifra de substituição, revela um texto sobre os Óscares e o movimento MeToo:

> THE OSCARS TURN  ON SUNDAY WHICH SEEMS ABOUT RIGHT AFTER THIS LONG STRANGE AWARDS TRIP THE BAGGER FEELS LIKE A NONAGENARIAN TOO
>
> THE AWARDS RACE WAS BOOKENDED BY THE DEMISE OF HARVEY WEINSTEIN AT ITS OUTSET AND THE APPARENT IMPLOSION OF HIS FILM COMPANY AT THE END AND IT WAS SHAPED BY THE EMERGENCE OF METOO TIMES UP BLACKGOWN POLITICS ARMCANDY ACTIVISM AND A NATIONAL CONVERSATION AS BRIEF AND MAD AS A FEVER DREAM ABOUT WHETHER THERE OUGHT TO BE A PRESIDENT WINFREY THE SEASON DIDNT JUST SEEM EXTRA LONG IT WAS EXTRA LONG BECAUSE THE OSCARS WERE MOVED TO THE FIRST WEEKEND IN MARCH TO AVOID CONFLICTING WITH THE CLOSING CEREMONY OF THE WINTER OLYMPICS THANKS PYEONGCHANG
>
> ONE BIG QUESTION SURROUNDING THIS YEARS ACADEMY AWARDS IS HOW OR IF THE CEREMONY WILL ADDRESS METOO ESPECIALLY AFTER THE GOLDEN GLOBES WHICH BECAME A JUBILANT COMINGOUT PARTY FOR TIMES UP THE MOVEMENT SPEARHEADED BY  POWERFUL HOLLYWOOD WOMEN WHO HELPED RAISE MILLIONS OF DOLLARS TO FIGHT SEXUAL HARASSMENT AROUND THE COUNTRY
>
> SIGNALING THEIR SUPPORT GOLDEN GLOBES ATTENDEES SWATHED THEMSELVES IN BLACK SPORTED LAPEL PINS AND SOUNDED OFF ABOUT SEXIST POWER IMBALANCES FROM THE RED CARPET AND THE STAGE ON THE AIR E WAS CALLED OUT ABOUT PAY INEQUITY AFTER ITS FORMER ANCHOR CATT SADLER QUIT ONCE SHE LEARNED THAT SHE WAS MAKING FAR LESS THAN A MALE COHOST AND DURING THE CEREMONY NATALIE PORTMAN TOOK A BLUNT AND SATISFYING DIG AT THE ALLMALE ROSTER OF NOMINATED DIRECTORS HOW COULD THAT BE TOPPED
>
> AS IT TURNS OUT AT LEAST IN TERMS OF THE OSCARS IT PROBABLY WONT BE
>
> WOMEN INVOLVED IN TIMES UP SAID THAT ALTHOUGH THE GLOBES SIGNIFIED THE INITIATIVES LAUNCH THEY NEVER INTENDED IT TO BE JUST AN AWARDS SEASON CAMPAIGN OR ONE THAT BECAME ASSOCIATED ONLY WITH REDCARPET ACTIONS INSTEAD A SPOKESWOMAN SAID THE GROUP IS WORKING BEHIND CLOSED DOORS AND HAS SINCE AMASSED  MILLION FOR ITS LEGAL DEFENSE FUND WHICH AFTER THE GLOBES WAS FLOODED WITH THOUSANDS OF DONATIONS OF  OR LESS FROM PEOPLE IN SOME  COUNTRIES
>
> NO CALL TO WEAR BLACK GOWNS WENT OUT IN ADVANCE OF THE OSCARS THOUGH THE MOVEMENT WILL ALMOST CERTAINLY BE REFERENCED BEFORE AND DURING THE CEREMONY  ESPECIALLY SINCE VOCAL METOO SUPPORTERS LIKE ASHLEY JUDD LAURA DERN AND NICOLE KIDMAN ARE SCHEDULED PRESENTERS
>
> ANOTHER FEATURE OF THIS SEASON NO ONE REALLY KNOWS WHO IS GOING TO WIN BEST PICTURE ARGUABLY THIS HAPPENS A LOT OF THE TIME INARGUABLY THE NAILBITER NARRATIVE ONLY SERVES THE AWARDS HYPE MACHINE BUT OFTEN THE PEOPLE FORECASTING THE RACE SOCALLED OSCAROLOGISTS CAN MAKE ONLY EDUCATED GUESSES
>
> THE WAY THE ACADEMY TABULATES THE BIG WINNER DOESNT HELP IN EVERY OTHER CATEGORY THE NOMINEE WITH THE MOST VOTES WINS BUT IN THE BEST PICTURE CATEGORY VOTERS ARE ASKED TO LIST THEIR TOP MOVIES IN PREFERENTIAL ORDER IF A MOVIE GETS MORE THAN  PERCENT OF THE FIRSTPLACE VOTES IT WINS WHEN NO MOVIE MANAGES THAT THE ONE WITH THE FEWEST FIRSTPLACE VOTES IS ELIMINATED AND ITS VOTES ARE REDISTRIBUTED TO THE MOVIES THAT GARNERED THE ELIMINATED BALLOTS SECONDPLACE VOTES AND THIS CONTINUES UNTIL A WINNER EMERGES
>
> IT IS ALL TERRIBLY CONFUSING BUT APPARENTLY THE CONSENSUS FAVORITE COMES OUT AHEAD IN THE END THIS MEANS THAT ENDOFSEASON AWARDS CHATTER INVARIABLY INVOLVES TORTURED SPECULATION ABOUT WHICH FILM WOULD MOST LIKELY BE VOTERS SECOND OR THIRD FAVORITE AND THEN EQUALLY TORTURED CONCLUSIONS ABOUT WHICH FILM MIGHT PREVAIL
>
> IN  IT WAS A TOSSUP BETWEEN BOYHOOD AND THE EVENTUAL WINNER BIRDMAN IN  WITH LOTS OF EXPERTS BETTING ON THE REVENANT OR THE BIG SHORT THE PRIZE WENT TO SPOTLIGHT LAST YEAR NEARLY ALL THE FORECASTERS DECLARED LA LA LAND THE PRESUMPTIVE WINNER AND FOR TWO AND A HALF MINUTES THEY WERE CORRECT BEFORE AN ENVELOPE SNAFU WAS REVEALED AND THE RIGHTFUL WINNER MOONLIGHT WAS CROWNED
>
> THIS YEAR AWARDS WATCHERS ARE UNEQUALLY DIVIDED BETWEEN THREE BILLBOARDS OUTSIDE EBBING MISSOURI THE FAVORITE AND THE SHAPE OF WATER WHICH IS THE BAGGERS PREDICTION WITH A FEW FORECASTING A HAIL MARY WIN FOR GET OUT
>
> BUT ALL OF THOSE FILMS HAVE HISTORICAL OSCARVOTING PATTERNS AGAINST THEM THE SHAPE OF WATER HAS  NOMINATIONS MORE THAN ANY OTHER FILM AND WAS ALSO NAMED THE YEARS BEST BY THE PRODUCERS AND DIRECTORS GUILDS YET IT WAS NOT NOMINATED FOR A SCREEN ACTORS GUILD AWARD FOR BEST ENSEMBLE AND NO FILM HAS WON BEST PICTURE WITHOUT PREVIOUSLY LANDING AT LEAST THE ACTORS NOMINATION SINCE BRAVEHEART IN  THIS YEAR THE BEST ENSEMBLE SAG ENDED UP GOING TO THREE BILLBOARDS WHICH IS SIGNIFICANT BECAUSE ACTORS MAKE UP THE ACADEMYS LARGEST BRANCH THAT FILM WHILE DIVISIVE ALSO WON THE BEST DRAMA GOLDEN GLOBE AND THE BAFTA BUT ITS FILMMAKER MARTIN MCDONAGH WAS NOT NOMINATED FOR BEST DIRECTOR AND APART FROM ARGO MOVIES THAT LAND BEST PICTURE WITHOUT ALSO EARNING BEST DIRECTOR NOMINATIONS ARE FEW AND FAR BETWEEN

## Task 2: Encryption Modes with OpenSSL

### 1\. Preparação

Nesta tarefa, explorámos os modos de operação **ECB**, **CBC** e **CTR** utilizando o algoritmo `AES-128` através da ferramenta de linha de comandos `openssl`.

Para evidenciar as diferenças entre os modos (especialmente o ECB), geramos um ficheiro `plaintext.txt` (1191 bytes) contendo padrões repetitivos:

```
1-ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789\n
2-ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789\n
...
```

## 2\. Cifragem (Encryption)

Para realizar a cifragem, utilizámos o comando `openssl enc`. Abaixo detalhamos as flags e a execução.

### Análise das Flags

  * **`-e`**: Define a operação de **cifragem** (encrypt).
  * **`-aes-128-[modo]`**: Algoritmo e modo (ex: `aes-128-ecb`, `aes-128-cbc`).
  * **`-in` / `-out`**: Ficheiros de entrada (texto limpo) e saída (texto cifrado).
  * **`-K [hex]`**: Chave de encriptação (32 caracteres hexadecimais = 128 bits).
  * **`-iv [hex]`**: Vetor de Inicialização (32 caracteres hexadecimais = 128 bits). **Nota:** Obrigatório para CBC e CTR; ignorado no ECB.

### Execução dos Comandos

```bash
# 1. AES-128-ECB (Electronic Code Book) - Sem IV
openssl enc -aes-128-ecb -e -in plaintext.txt -out cipher_ecb.bin \
    -K 00112233445566778899aabbccddeeff

# 2. AES-128-CBC (Cipher Block Chaining) - Com IV
openssl enc -aes-128-cbc -e -in plaintext.txt -out cipher_cbc.bin \
    -K 00112233445566778899aabbccddeeff \
    -iv 0102030405060708090a0b0c0d0e0f10

# 3. AES-128-CTR (Counter Mode) - Com IV
openssl enc -aes-128-ctr -e -in plaintext.txt -out cipher_ctr.bin \
    -K 00112233445566778899aabbccddeeff \
    -iv 0102030405060708090a0b0c0d0e0f10
```

## 3\. Comparação dos Modos de Operação

Após a cifragem, comparámos o comportamento de cada modo:

| Modo | Característica Principal | Segurança & Padrões |
| :--- | :--- | :--- |
| **ECB** | Cifra cada bloco independentemente. É determinístico (mesmo bloco de entrada = mesmo bloco de saída). | **Inseguro para dados estruturados.** Revela padrões visuais do texto original, pois blocos repetidos no `plaintext` resultam em blocos repetidos no cifrado. |
| **CBC** | Encadeia os blocos (XOR com o bloco anterior cifrado). Requer um IV aleatório. | **Oculta padrões.** Mesmo que o texto se repita, o resultado cifrado é completamente diferente. Não permite paralelismo na cifragem. |
| **CTR** | Transforma o bloco cifrador num *stream cipher* usando um contador. Faz XOR do *keystream* com o texto. | **Oculta padrões e é rápido.** Permite acesso aleatório e paralelismo total (cifragem e decifragem). |

## 4\. Decifragem (Decryption)

Para reverter o processo, as flags são ajustadas:

1.  Substituir `-e` por **`-d`** (decrypt).
2.  Trocar os ficheiros de `-in` (agora o binário cifrado) e `-out` (texto recuperado).
3.  Manter exatamente a mesma **Chave (-K)** e **IV (-iv)**.

**Exemplo de comando (ECB):**

```bash
openssl enc -aes-128-ecb -d -in cipher_ecb.bin -out decipher_ecb.txt \
    -K 00112233445566778899aabbccddeeff
```

## 5\. Análise de Erros: "Bad Decrypt" vs. Silêncio

Testámos a decifragem utilizando uma **chave incorreta** propositadamente para observar como o OpenSSL reage em diferentes modos.

**Observação:**

  * **ECB e CBC:** O comando falha e retorna o erro `bad decrypt`.
  * **CTR:** O comando executa com "sucesso", mas o ficheiro de saída contém lixo (dados corrompidos).

**Explicação Técnica:**
A diferença reside na utilização de **Padding (Preenchimento)**.

  * **ECB/CBC (Block Ciphers):** Por defeito, utilizam o padrão **PKCS\#7** para preencher o último bloco. Ao decifrar com a chave errada, o último bloco resulta em dados aleatórios. O OpenSSL verifica se o final do ficheiro contém um padding válido (ex: `0x03 0x03 0x03`). Como a chave errada gera um padding inválido, a ferramenta deteta a corrupção e lança um erro.
  * **CTR (Stream Cipher behavior):** O modo CTR não necessita de padding, pois faz apenas o XOR bit-a-bit do fluxo de chaves com o texto cifrado. Se a chave estiver errada, o XOR produz dados errados, mas o "processo" matemático é válido. O OpenSSL não tem forma de saber se o resultado é texto válido ou lixo, por isso não emite erro.

## Task 5: Error Propagation – Corrupted Cipher Text

## 1\. Objetivo e Preparação

O objetivo desta tarefa foi analisar a **propagação de erros** nos diferentes modos de cifra (ECB, CBC, CTR). Queríamos verificar o impacto na decifragem quando um único byte do ficheiro cifrado é corrompido.

**Cálculo do Byte a Corromper:**

  * Número do Grupo ($G$): **3**
  * Byte alvo: $50 \times G = 150$.
  * Índice (0-based): **149**.

**Ferramenta de Corrupção:**
Em vez de utilizar um editor hexadecimal manual (`bless`), desenvolvemos um script em Python para realizar a operação de **XOR** no byte de índice 149, garantindo uma corrupção controlada e reprodutível.

```python
import os

def patch_file_byte():
    """
    Asks for a filename, opens it, and XORs the 50th byte.
    """
    
    # 1. Get the filename from the user
    file_path = input("Enter the path of the file to modify: ").strip()

    # 2. Check if file exists
    if not os.path.exists(file_path):
        print(f"Error: The file '{file_path}' does not exist.")
        return

    # 3. Check if it's a file (not a directory)
    if not os.path.isfile(file_path):
        print(f"Error: '{file_path}' is not a valid file.")
        return

    try:
        # Open the file in binary read/write mode ('r+b')
        with open(file_path, 'r+b') as f:
            
            # 4. Check if the file is large enough (needs at least 51 bytes to have a 50th byte index 49 or 50)
            # We assume "50th byte" means index 49 (0-indexed) or index 50 depending on interpretation.
            # Usually humans mean count from 1, so index 49.
            TARGET_INDEX = 50*3-1
            
            # Move pointer to the end to get size
            f.seek(0, os.SEEK_END)
            file_size = f.tell()

            if file_size <= TARGET_INDEX:
                print(f"Error: File is too small. It only has {file_size} bytes.")
                return

            # 5. Move the file pointer to the 50th byte
            f.seek(TARGET_INDEX)

            # 6. Read the single byte
            original_byte_data = f.read(1)
            
            # Convert byte to integer
            # int.from_bytes is robust for byte objects
            original_byte_val = int.from_bytes(original_byte_data, "big")

            # 7. Perform XOR operation
            # We'll use a hardcoded key, e.g., 0xFF (255) to invert the bits, 
            # or any other integer between 1 and 255.
            XOR_KEY = 0xFF 
            new_byte_val = original_byte_val ^ XOR_KEY

            # 8. Move pointer back to the 50th byte to overwrite it
            f.seek(TARGET_INDEX)

            # 9. Write the new byte
            # Convert integer back to bytes
            f.write(new_byte_val.to_bytes(1, "big"))

            print(f"Success! Modified byte at index {TARGET_INDEX}.")
            print(f"Original: {hex(original_byte_val)} | Key: {hex(XOR_KEY)} | New: {hex(new_byte_val)}")

    except PermissionError:
        print(f"Error: Permission denied. You might need admin rights to modify '{file_path}'.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")

if __name__ == "__main__":
    patch_file_byte()
```

-----

## 2\. Execução e Análise por Modo

Para cada modo, realizámos a cifragem, corrupção, decifragem e contagem das diferenças (bytes perdidos) usando o comando `cmp`.

### A. Modo ECB (Electronic Code Book)

**Comandos Executados:**

```bash
# 1. Cifrar
openssl enc -aes-128-ecb -e -in plaintext.txt -out cipher_ecb.bin \
    -K 00112233445566778889aabbccddeeff

# 2. Corromper (Python Script)
# Input: cipher_ecb.bin -> Modifica byte 149

# 3. Decifrar o ficheiro corrompido
openssl enc -aes-128-ecb -d -in cipher_ecb.bin -out decipher_ecb.txt \
    -K 00112233445566778889aabbccddeeff

# 4. Verificar diferenças
cmp -lb plaintext.txt decipher_ecb.txt | wc -l
```

**Resultado Observado:**

  * **Bytes diferentes:** `17`
  * **Análise:** No modo ECB, os blocos são independentes. A corrupção de 1 byte no texto cifrado destrói completamente a decifragem do **bloco inteiro** (16 bytes) onde esse erro ocorreu. O valor obtido (17) corresponde aproximadamente a um bloco de dados perdido (os 16 bytes do bloco mais eventuais desalinhamentos detetados pelo `cmp`).
![imagem 19](images/ecb.png)
-----

### B. Modo CBC (Cipher Block Chaining)

**Comandos Executados:**

```bash
# 1. Cifrar
openssl enc -aes-128-cbc -e -in plaintext.txt -out cipher_cbc.bin \
    -K 00112233445566778889aabbccddeeff -iv 0102030405060708

# 2. Corromper (Python Script)
# Input: cipher_cbc.bin -> Modifica byte 149

# 3. Decifrar
openssl enc -aes-128-cbc -d -in cipher_cbc.bin -out decipher_cbc.txt \
    -K 00112233445566778889aabbccddeeff -iv 0102030405060708

# 4. Verificar diferenças
cmp -lb plaintext.txt decipher_cbc.txt | wc -l
```

**Resultado Observado:**

  * **Bytes diferentes:** `18`
  * **Análise:** No modo CBC, a decifragem de um bloco depende do bloco anterior.
    1.  O bloco onde ocorreu o erro (bloco $N$) é **totalmente corrompido** (16 bytes de lixo).
    2.  O bit específico alterado no cifrado afeta o XOR no bloco seguinte (bloco $N+1$), gerando um erro de **1 byte** (ou bit) na posição correspondente.
    <!-- end list -->
      * **Total teórico:** 16 bytes (bloco atual) + 1 byte (bloco seguinte) ≈ 17 bytes. O valor obtido (18) confirma a propagação do erro para o bloco seguinte.

![imagem 19](images/cbc.png)
-----

### C. Modo CTR (Counter Mode)

**Comandos Executados:**

```bash
# 1. Cifrar
openssl enc -aes-128-ctr -e -in plaintext.txt -out cipher_ctr.bin \
    -K 00112233445566778889aabbccddeeff -iv 0102030405060708

# 2. Corromper (Python Script)
# Input: cipher_ctr.bin -> Modifica byte 149

# 3. Decifrar
openssl enc -aes-128-ctr -d -in cipher_ctr.bin -out decipher_ctr.txt \
    -K 00112233445566778889aabbccddeeff -iv 0102030405060708

# 4. Verificar diferenças
cmp -lb plaintext.txt decipher_ctr.txt | wc -l
```

**Resultado Observado:**

  * **Bytes diferentes:** `1`
  * **Análise:** O modo CTR funciona como um *stream cipher*. A operação é um XOR bit-a-bit entre o texto cifrado e o *keystream*.
      * Se inverter um bit no cifrado, apenas **o bit correspondente** no texto limpo é invertido.
      * Não há propagação de erro para outros bytes ou blocos. A perda de informação é mínima (apenas 1 byte corrompido).

![imagem 19](images/ctr.png)
-----

## 3\. Conclusão da Tarefa

| Modo | Bytes Perdidos (Observed) | Comportamento na Corrupção |
| :--- | :---: | :--- |
| **ECB** | **17** | **Danos Locais Elevados:** O bloco inteiro (16 bytes) é perdido. |
| **CBC** | **18** | **Propagação em Cadeia:** Perde o bloco atual inteiro + o byte correspondente no bloco seguinte. |
| **CTR** | **1** | **Danos Mínimos:** Apenas o byte exato onde ocorreu o erro é afetado. |


## Desafio

### 1. Análise do Criptograma
Foi-nos fornecido um texto cifrado com a cifra de **Vigenère** e as seguintes condições:
* **Alfabeto:** A-Z e 0-9 (Total de 36 caracteres: $A=0, ..., Z=25, 0=26, ..., 9=35$).
* **Tamanho da chave:** 5 caracteres.
* **Dica:** "Fundamentos de Segurança Informática".

O texto cifrado é:
`N516MHZIFBN5OEDSVKGIY9WD7T4MD9YBP6MJDWDPY0WFOF2MAOXBWDGNX6GPH62D8K3Q4FFA4AOHZIF8T7MFTTCZVMIW66TTCLK9JBP2O1W09JZ3LF90WZ39FXZ2DIBW5DJ9QK9Z7IF8YSS6OMWXGRJ9J27P01KON4MLCJ`

### 2. Recuperação da Chave (Heurística)
Em vez de realizar uma análise de frequência bruta (que seria o método padrão sem dicas), utilizámos inferência lógica baseada nas pistas contextuais:

1.  **Prefixo da Chave:** A dica "Fundamentos de Segurança Informática" aponta fortemente para o acrónimo da unidade curricular: **FSI**.
2.  **Sufixo da Chave:** A chave tem tamanho 5, restando 2 caracteres (`FSI__`). Dado que o alfabeto inclui dígitos numéricos, formulou-se a hipótese de os últimos caracteres representarem o ano corrente.
3.  **Teste:** Testou-se a chave **`FSI25`**.

Ao aplicar a chave ao início do criptograma, o resultado foi texto legível em inglês, confirmando a hipótese.

### 3. Decifragem
Utilizámos um script simples em Python para testar a hipótese.
```python
# Vigenère Cipher Decryption Script
# Alphabet: A-Z (indices 0-25) followed by 0-9 (indices 26-35)
# Total Alphabet Size: 36

import sys

# Define the custom alphabet as requested
# A=0, ..., Z=25, 0=26, ..., 9=35
ALPHABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
ALPHABET_SIZE = len(ALPHABET)

# Create a mapping for quick lookup (Char -> Index)
CHAR_TO_INDEX = {char: index for index, char in enumerate(ALPHABET)}

def decrypt(ciphertext, key):
    """
    Decrypts a ciphertext using the Vigenère cipher with the custom 36-char alphabet.
    """
    plaintext = []
    
    # Clean inputs: remove whitespace and convert to uppercase to match our alphabet
    # Note: This implementation assumes the user wants to keep the numbers in the key/text
    # strictly within the defined alphabet.
    
    key_index = 0
    filtered_key = [k for k in key.upper() if k in ALPHABET]
    
    if not filtered_key:
        return "Error: Key must contain at least one valid character from the alphabet."

    for char in ciphertext.upper():
        if char in ALPHABET:
            # Get indices
            c_idx = CHAR_TO_INDEX[char]
            k_idx = CHAR_TO_INDEX[filtered_key[key_index % len(filtered_key)]]
            
            # Vigenère Decryption Formula: P = (C - K) mod N
            # Python's % operator handles negative numbers correctly automatically
            p_idx = (c_idx - k_idx) % ALPHABET_SIZE
            
            plaintext.append(ALPHABET[p_idx])
            
            # Move to next letter in key
            key_index += 1
        else:
            # If character is not in our alphabet (e.g., space, symbol), append as is
            # and DO NOT advance the key (standard Vigenère behavior)
            plaintext.append(char)
            
    return "".join(plaintext)

def encrypt(plaintext, key):
    """
    Helper function to encrypt, useful for verifying the decryption.
    """
    ciphertext = []
    key_index = 0
    filtered_key = [k for k in key.upper() if k in ALPHABET]
    
    if not filtered_key:
        return "Error: Key valid characters missing."

    for char in plaintext.upper():
        if char in ALPHABET:
            p_idx = CHAR_TO_INDEX[char]
            k_idx = CHAR_TO_INDEX[filtered_key[key_index % len(filtered_key)]]
            
            # Encryption: C = (P + K) mod N
            c_idx = (p_idx + k_idx) % ALPHABET_SIZE
            ciphertext.append(ALPHABET[c_idx])
            key_index += 1
        else:
            ciphertext.append(char)
            
    return "".join(ciphertext)

def main():
    print(f"--- Vigenère Cipher (Alphabet Size: {ALPHABET_SIZE}) ---")
    print(f"Alphabet: {ALPHABET}")
    print("-" * 50)

    # You can change these values to test specific cases
    mode = input("Do you want to (E)ncrypt or (D)ecrypt? ").strip().upper()
    key = input("Enter Key: ").strip()
    text = input("Enter Text: ").strip()

    if mode == 'E':
        result = encrypt(text, key)
        print(f"\nCiphertext: {result}")
    elif mode == 'D':
        result = decrypt(text, key)
        print(f"\nPlaintext:  {result}")
    else:
        print("Invalid mode selected.")

if __name__ == "__main__":
    main()
```

**Texto Decifrado:**
> INTERCHANGINGMINDCONTROLCOMELETTHEREVOLUTIONTAKEITSTOLLIFYOUCOULDFLICKASWITCHANDOPENYOUR3RDEYEYOUDSEETHATWESHOULDNEVERBEAFRAIDTODIERISEUPANDTAKETHEPOWERBACKITSTIMETHE

### 4. Resolução do Desafio
O texto decifrado corresponde à letra da música **"Uprising"** da banda Muse. O desafio coloca a questão: *"O que deve acontecer aos gatos gordos?"*.

Embora o texto decifrado corte na frase "ITS TIME THE", a continuação da letra original é:
*"It's time the **fat cats had a heart attack**."*

**Resposta:** They should have a heart attack.