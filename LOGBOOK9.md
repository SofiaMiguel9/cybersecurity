# LOGBOOK 9 - Secret Key Encryption

##  Task 1: Frequency Analysis

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