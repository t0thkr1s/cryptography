# Affine Cipher

> The affine cipher is a type of monoalphabetic substitution cipher, wherein each letter in an alphabet is mapped to its numeric equivalent, encrypted using a simple mathematical function, and converted back to a letter. The formula used means that each letter encrypts to one other letter, and back again, meaning the cipher is essentially a standard substitution cipher with a rule governing which letter goes to which. As such, it has the weaknesses of all substitution ciphers.
> 
> Source: https://en.wikipedia.org/wiki/Affine_cipher

### How does it work?

Each letter is enciphered with the function `(a * x + b) mod 26`, where b is the magnitude of the shift. Let’s say we have a word "secret" and we want to encrypt it with `a = 5` and `b = 7`. These numbers are basically the encryption key.

Now, we have to take the word letter by letter and find the position of every letter in the ABC. __Keep in mind that counting starts from 0!__ All we have to do is substitute these number into the formula. The result will be the position of the encrypted letter in the ABC.

***Plain text message:***

```
+----+---+---+----+---+----+
|  s | e | c |  r | e |  t |
+----+---+---+----+---+----+
| 18 | 4 | 2 | 17 | 4 | 19 |
+----+---+---+----+---+----+
```

***Encrypting the letters:***

```
5 * 18 + 7 mod 26 = 19
5 * 4 + 7 mod 26 = 1
5 * 2 + 7 mod 26 = 17
5 * 17 + 7 mod 26 = 14
5 * 4 + 7 mod 26 = 1
5 * 19 + 7 mod 26 = 24
```

***Encrypted message:***

```
+----+---+----+----+---+----+
| 19 | 1 | 17 | 14 | 1 | 24 |
+----+---+----+----+---+----+
|  t | b |  r |  o | b |  y |
+----+---+----+----+---+----+
```

## Encryption

The encrypt method takes 2 parameters, a coefficients list which will have 2 elements and a message. I created 2 variable for the 2 coefficient and started iterating through the plain message letter by letter. I ignored every letter that is not in the alphabet and simply continued the loop.

Next, I created a new `index` variable to store the current letter index. For better readability, I decided to store the result of the formula (the encrypted letter index) in a new variable.

Finally, I appended that specific letter from the alphabet to the `encrypted_message` variable.

```python
def encrypt(coefficients, message):
    message = message.upper()
    encrypted_message = ""

    coefficient_a = coefficients[0]
    coefficient_b = coefficients[1]

    for char in message:
        if char not in ALPHABET:
            message = message.replace(char, "")

    for char in message:
        index = ALPHABET.find(char)
        encrypted_char_index = (coefficient_a * index + coefficient_b) % len(ALPHABET)
        encrypted_message += ALPHABET[encrypted_char_index]

    return encrypted_message
```

## Decryption

As for the decryption, the procedure is a little different. The decryption formula is `a^-1 (x - b) mod 26`. For this to work, we have to find the modular multiplicative inverse of `a`. The modular multiplicative inverse of an integer `a` is an integer `x` such that the product `a * x` is congruent to 1 with respect to the modulus `m`. The multiplicative inverse of `a` only exists if `a` and `m` are coprime. Hence without the restriction on `a`, decryption might not be possible.

I started a loop, which iterates through the length of the alphabet. If `a * i % 26` equals to 1, then we found our multiplicative inverse and we can continue with the next loop. In the second loop, I calculated the `decrypted_char_index` by substituting into the formula and at the end, I appended the decrypted letter to the `decrypted_message` variable.

```python
def decrypt(coefficients, message):
    message = message.upper()
    decrypted_message = ""

    coefficient_a = coefficients[0]
    coefficient_b = coefficients[1]

    multiplicative_inverse = None

    for i in range(len(ALPHABET)):
        if coefficient_a * i % len(ALPHABET) == 1:
            multiplicative_inverse = i
            break

    if multiplicative_inverse is None:
        return "Sorry, the decryption is not possible!"

    for char in message:
        if char not in ALPHABET:
            continue
        index = ALPHABET.find(char)
        decrypted_char_index = (multiplicative_inverse * (index - coefficient_b)) % len(ALPHABET)
        decrypted_message += ALPHABET[decrypted_char_index]

    return decrypted_message
```

## The Full Source Code

```python
import argparse

ALPHABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"


def encrypt(coefficients, message):
    message = message.upper()
    encrypted_message = ""

    coefficient_a = coefficients[0]
    coefficient_b = coefficients[1]

    for char in message:
        if char not in ALPHABET:
            message = message.replace(char, "")

    for char in message:
        index = ALPHABET.find(char)
        encrypted_char_index = (coefficient_a * index + coefficient_b) % len(ALPHABET)
        encrypted_message += ALPHABET[encrypted_char_index]

    return encrypted_message


def decrypt(coefficients, message):
    message = message.upper()
    decrypted_message = ""

    coefficient_a = coefficients[0]
    coefficient_b = coefficients[1]

    multiplicative_inverse = None

    for i in range(len(ALPHABET)):
        if coefficient_a * i % len(ALPHABET) == 1:
            multiplicative_inverse = i
            break

    if multiplicative_inverse is None:
        return "Sorry, the decryption is not possible!"

    for char in message:
        if char not in ALPHABET:
            continue
        index = ALPHABET.find(char)
        decrypted_char_index = (multiplicative_inverse * (index - coefficient_b)) % len(ALPHABET)
        decrypted_message += ALPHABET[decrypted_char_index]

    return decrypted_message


def main():
    usage = "python affine_cipher.py -e -m secret -c 5 7\nor\n" \
            "python affine_cipher.py -d -m tbroby -c 5 7"

    description = "Command line program for demonstrating the Affine cipher."

    parser = argparse.ArgumentParser(usage=usage, description=description)
    parser.add_argument("-v", "--version", action="version", version="%(affine_cipher)s 1.0")

    group = parser.add_mutually_exclusive_group(required=True)

    group.add_argument("-e", "--encrypt", action="store_true", dest="encrypt",
                       help="Encrypts the plain text message.")

    group.add_argument("-d", "--decrypt", action="store_true", dest="decrypt",
                       help="Decrypts the encrypted message.")

    parser.add_argument("-m", "--message", action="store", required=True, dest="message",
                        help="You need to specify the message. Like this: -m \"secret\"")

    parser.add_argument("-c", "--coefficients", action="store", required=True, type=int,
                        nargs=2, dest="coefficients", help="You need to specify the keys.")

    options = parser.parse_args()

    if options.encrypt:
        print("The encrypted message is: " + encrypt(options.coefficients, options.message))
    elif options.decrypt:
        print("The decrypted message is: " + decrypt(options.coefficients, options.message))


if __name__ == '__main__':
    main()
```
