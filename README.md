# Cryptography---19CS412-classical-techqniques


# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To develop a simple C program to implement Caeser Cipher.

## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
def caesar_cipher(text, shift):
    encrypted_text = ""
    for char in text:
        if char.isalpha():  # Check if the character is alphabet
            shifted = ord(char) + shift
            if char.islower():
                if shifted > ord('z'):
                    shifted -= 26
                elif shifted < ord('a'):
                    shifted += 26
            elif char.isupper():
                if shifted > ord('Z'):
                    shifted -= 26
                elif shifted < ord('A'):
                    shifted += 26
            encrypted_text += chr(shifted)
        else:
            encrypted_text += char
    return encrypted_text
text = "GOWTHAM"
shift = 4

# Encrypting the text
encrypted_text = caesar_cipher(text, shift)
print("Original Text:", text)
print("Encrypted Text:", encrypted_text)


def caesar_decrypt(encrypted_text, shift):
    decrypted_text = ""
    for char in encrypted_text:
        if char.isalpha():  # Check if the character is alphabet
            shifted = ord(char) - shift
            if char.islower():
                if shifted > ord('z'):
                    shifted -= 26
                elif shifted < ord('a'):
                    shifted += 26
            elif char.isupper():
                if shifted > ord('Z'):
                    shifted -= 26
                elif shifted < ord('A'):
                    shifted += 26
            decrypted_text += chr(shifted)
        else:
            decrypted_text += char
    return decrypted_text

# Example usage:
text = "GOWTHAM"
shift = 4

decrypted_text = caesar_decrypt(encrypted_text, shift)
print("Decrypted Text:", decrypted_text)
```

## OUTPUT:
![Screenshot 2024-02-29 203600](https://github.com/krithygowthamn/Cryptography---19CS412-classical-techqniques/assets/122247810/8f0d12cf-ae2c-4a95-9b22-54cb74e31eaa)

## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To develop a simple C program to implement PlayFair Cipher.

## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
def prepare_key(key):
    key = key.replace(" ", "").upper().replace("J", "I")
    key_without_duplicates = ""
    for letter in key:
        if letter not in key_without_duplicates:
            key_without_duplicates += letter
    alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"
    for letter in alphabet:
        if letter not in key_without_duplicates:
            key_without_duplicates += letter
    return key_without_duplicates


def generate_matrix(key):
    matrix = [[0] * 5 for _ in range(5)]
    key = prepare_key(key)
    index = 0
    for i in range(5):
        for j in range(5):
            matrix[i][j] = key[index]
            index += 1
    return matrix


def find_positions(matrix, char):
    for i in range(5):
        for j in range(5):
            if matrix[i][j] == char:
                return i, j


def encrypt(plaintext, key):
    plaintext = plaintext.upper().replace(" ", "").replace("J", "I")
    matrix = generate_matrix(key)
    ciphertext = ""
    for i in range(0, len(plaintext), 2):
        char1 = plaintext[i]
        if i + 1 < len(plaintext):
            char2 = plaintext[i + 1]
        else:
            char2 = "X"
        row1, col1 = find_positions(matrix, char1)
        row2, col2 = find_positions(matrix, char2)
        if row1 == row2:
            ciphertext += matrix[row1][(col1 + 1) % 5]
            ciphertext += matrix[row2][(col2 + 1) % 5]
        elif col1 == col2:
            ciphertext += matrix[(row1 + 1) % 5][col1]
            ciphertext += matrix[(row2 + 1) % 5][col2]
        else:
            ciphertext += matrix[row1][col2]
            ciphertext += matrix[row2][col1]
    return ciphertext


def decrypt(ciphertext, key):
    matrix = generate_matrix(key)
    plaintext = ""
    for i in range(0, len(ciphertext), 2):
        char1 = ciphertext[i]
        char2 = ciphertext[i + 1]
        row1, col1 = find_positions(matrix, char1)
        row2, col2 = find_positions(matrix, char2)
        if row1 == row2:
            plaintext += matrix[row1][(col1 - 1) % 5]
            plaintext += matrix[row2][(col2 - 1) % 5]
        elif col1 == col2:
            plaintext += matrix[(row1 - 1) % 5][col1]
            plaintext += matrix[(row2 - 1) % 5][col2]
        else:
            plaintext += matrix[row1][col2]
            plaintext += matrix[row2][col1]
    return plaintext


# Example usage:
key = "LOVE"
plaintext = "NGOWTHAM"
encrypted_text = encrypt(plaintext, key)
print("Encrypted:", encrypted_text)
decrypted_text = decrypt(encrypted_text, key)
print("Decrypted:", decrypted_text)

```
## OUTPUT:
![Screenshot 2024-02-29 203728](https://github.com/krithygowthamn/Cryptography---19CS412-classical-techqniques/assets/122247810/4d275bca-d3ed-4b56-9747-bc9899a90de5)

## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
import numpy as np

def string_to_matrix(text, n):
    # Convert the string to uppercase and remove spaces
    text = text.upper().replace(" ", "")
    # Pad the string with 'X' if its length is not a multiple of n
    if len(text) % n != 0:
        text += "X" * (n - len(text) % n)
    # Convert the string to a list of ASCII values
    ascii_values = [ord(char) - ord("A") for char in text]
    # Reshape the list into a matrix of size (len(text)//n) x n
    matrix = np.array(ascii_values).reshape(-1, n)
    return matrix

def matrix_to_string(matrix):
    # Convert the matrix to a string by converting each ASCII value to its corresponding character
    text = ""
    for row in matrix:
        for value in row:
            text += chr(value + ord("A"))
    return text

def encrypt(plaintext, key):
    n = len(key)
    # Convert plaintext and key to matrices
    plaintext_matrix = string_to_matrix(plaintext, n)
    key_matrix = np.array(key)
    # Calculate the encrypted matrix: C = P * K (mod 26)
    encrypted_matrix = np.dot(plaintext_matrix, key_matrix) % 26
    # Convert the encrypted matrix back to a string
    ciphertext = matrix_to_string(encrypted_matrix)
    return ciphertext

def decrypt(ciphertext, key):
    n = len(key)
    # Convert ciphertext and key to matrices
    ciphertext_matrix = string_to_matrix(ciphertext, n)
    key_matrix = np.array(key)
    # Calculate the inverse of the key matrix
    key_inverse = np.linalg.inv(key_matrix)
    key_inverse = np.round(key_inverse * np.linalg.det(key_matrix)).astype(int) % 26
    # Calculate the decrypted matrix: P = C * K^-1 (mod 26)
    decrypted_matrix = np.dot(ciphertext_matrix, key_inverse) % 26
    # Convert the decrypted matrix back to a string
    plaintext = matrix_to_string(decrypted_matrix)
    return plaintext

# Example usage:
plaintext = "GOWTHAM"
key = [[6, 24, 1], [13, 16, 10], [20, 17, 15]]  # Example key matrix
ciphertext = encrypt(plaintext, key)
print("Encrypted:", ciphertext)
decrypted_text = decrypt(ciphertext, key)
print("Decrypted:", decrypted_text)
```
## OUTPUT:
![Screenshot 2024-02-29 203804](https://github.com/krithygowthamn/Cryptography---19CS412-classical-techqniques/assets/122247810/e9b21f15-0e1e-473b-9c56-be713bcffe6a)

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
def vigenere_encrypt(plaintext, key):
    ciphertext = ""
    key_length = len(key)
    for i in range(len(plaintext)):
        # Calculate shift value for the current character in plaintext
        shift = ord(key[i % key_length]) - ord('A')
        # Encrypt the current character using the shift value
        if plaintext[i].isalpha():
            if plaintext[i].islower():
                encrypted_char = chr((ord(plaintext[i]) - ord('a') + shift) % 26 + ord('a'))
            else:
                encrypted_char = chr((ord(plaintext[i]) - ord('A') + shift) % 26 + ord('A'))
            ciphertext += encrypted_char
        else:
            # Non-alphabetic characters remain unchanged
            ciphertext += plaintext[i]
    return ciphertext

def vigenere_decrypt(ciphertext, key):
    plaintext = ""
    key_length = len(key)
    for i in range(len(ciphertext)):
        # Calculate shift value for the current character in ciphertext
        shift = ord(key[i % key_length]) - ord('A')
        # Decrypt the current character using the shift value
        if ciphertext[i].isalpha():
            if ciphertext[i].islower():
                decrypted_char = chr((ord(ciphertext[i]) - ord('a') - shift + 26) % 26 + ord('a'))
            else:
                decrypted_char = chr((ord(ciphertext[i]) - ord('A') - shift + 26) % 26 + ord('A'))
            plaintext += decrypted_char
        else:
            # Non-alphabetic characters remain unchanged
            plaintext += ciphertext[i]
    return plaintext

# Example usage:
plaintext = "GOWTHAM"
key = "LOVE"
encrypted_text = vigenere_encrypt(plaintext, key)
print("Encrypted:", encrypted_text)
decrypted_text = vigenere_decrypt(encrypted_text, key)
print("Decrypted:", decrypted_text)


```
## OUTPUT:
![Screenshot 2024-02-29 203847](https://github.com/krithygowthamn/Cryptography---19CS412-classical-techqniques/assets/122247810/4c657333-406d-4307-8788-d99456bec36a)

## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
def rail_fence_encrypt(plaintext, key):
    # Initialize list for each rail
    rails = [''] * key
    # Initialize direction and start rail index
    direction = -1
    rail_index = 0
    # Fill the rails with characters from plaintext
    for char in plaintext:
        rails[rail_index] += char
        if rail_index == 0 or rail_index == key - 1:
            direction *= -1
        rail_index += direction
    # Concatenate the rails to form the ciphertext
    ciphertext = ''.join(rails)
    return ciphertext

def rail_fence_decrypt(ciphertext, key):
    # Initialize list for each rail with placeholder characters
    rails = [''] * key
    # Initialize direction and start rail index
    direction = -1
    rail_index = 0
    # Determine the length of each rail
    rail_lengths = [0] * key
    for i in range(len(ciphertext)):
        rail_lengths[rail_index] += 1
        if rail_index == 0 or rail_index == key - 1:
            direction *= -1
        rail_index += direction
    # Fill the rails with placeholder characters
    index = 0
    for i in range(key):
        for j in range(rail_lengths[i]):
            rails[i] += '*'
    # Fill the placeholder characters with characters from ciphertext
    for i in range(key):
        for j in range(rail_lengths[i]):
            rails[i] = rails[i][:j] + ciphertext[index] + rails[i][j+1:]
            index += 1
    # Initialize variables for constructing plaintext
    direction = -1
    rail_index = 0
    plaintext = ''
    # Construct plaintext by traversing the rails
    for i in range(len(ciphertext)):
        plaintext += rails[rail_index][0]
        rails[rail_index] = rails[rail_index][1:]
        if rail_index == 0 or rail_index == key - 1:
            direction *= -1
        rail_index += direction
    return plaintext

# Example usage:
plaintext = "GOWTHAM"
key = 4
encrypted_text = rail_fence_encrypt(plaintext, key)
print("Encrypted:", encrypted_text)
decrypted_text = rail_fence_decrypt(encrypted_text, key)
print("Decrypted:", decrypted_text)

```
## OUTPUT:
![Screenshot 2024-02-29 203947](https://github.com/krithygowthamn/Cryptography---19CS412-classical-techqniques/assets/122247810/feb6cb0d-132e-4565-9300-73b841b5468e)

## RESULT:
The program is executed successfully
