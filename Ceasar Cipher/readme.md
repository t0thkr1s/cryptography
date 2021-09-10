# Ceasar Cipher

> The Caesar cipher, also known as the shift cipher, is a type of substitution cipher. Each letter in the plaintext is replaced with a letter a fixed number of positions to the left or right of that letter in the alphabet.

### How does it work?

In the following example, I’m going to encrypt a message with a key of 2. This means that in order to decrypt a message, you have to shift the alphabet left or right. The key specifies how much you have to shift. Let’s take right shift decryption, for example, C becomes A and G becomes E. I think there is not much to explain the example is pretty straightforward.

***Encrypted message:***

> UGETGV

***Decrypted message:***

> SECRET

## Encryption

If you’re not a programmer or new to programming, don’t worry I'll try to explain how the code works.

```python
def encrypt(message, key):
    message = message.replace(':', '')
    message = message.replace(';', '')
    message = message.replace(',', '')
    message = message.replace('.', '')
    message = message.replace('?', '')
    message = message.replace('!', '')

    message = message.upper()

    encrypted_message = ''

    for c in message:
        if c not in ALPHABET:
            encrypted_message += c
            continue

        index_in_alphabet = ALPHABET.find(c)
        shifted_index = ((index_in_alphabet + key) % len(ALPHABET))
        encrypted_message += ALPHABET[shifted_index]

    return encrypted_message
```

The *encrypt(message, key)* method takes two parameters. One is your message, the other is the key. I tried to eliminate some special characters from the supplied message, but it’s far from complete. Next, the loop goes through the message character by character and if the current character is not in the alphabet, then simply append the char to the encrypted message without tampering it. I needed the index of the character in the alphabet. I used it to calculate the shifted index by adding the key to the index and using the modulo operator which gives the remainder of the division with the length of the alphabet.

> C (index 2) + 3 (key) % 26 (length of the alphabet) = 5

## Decryption

```python3
def decrypt(message, key):

    message = message.replace(':', '')
    message = message.replace(';', '')
    message = message.replace(',', '')
    message = message.replace('.', '')
    message = message.replace('?', '')
    message = message.replace('!', '')

    message = message.upper()

    decrypted_message = ''

    for c in message:
        if c not in ALPHABET:
            decrypted_message += c
            continue

        index_in_alphabet = ALPHABET.find(c)
        shifted_index = ((index_in_alphabet - key) % len(ALPHABET))
        decrypted_message += ALPHABET[shifted_index]

    return decrypted_message
```

As you can see the *decrypt(message, key)* method has only one difference compared to the *encrypt(message, key)* method and that is the key subtraction from the letter index in the alphabet. Everything is else is exactly the same.

## The Full Source Code

```python
import argparse

ALPHABET = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'


def encrypt(message, key):
    message = message.replace(':', '')
    message = message.replace(';', '')
    message = message.replace(',', '')
    message = message.replace('.', '')
    message = message.replace('?', '')
    message = message.replace('!', '')

    message = message.upper()

    encrypted_message = ''

    for c in message:
        if c not in ALPHABET:
            encrypted_message += c
            continue

        index_in_alphabet = ALPHABET.find(c)
        shifted_index = ((index_in_alphabet + key) % len(ALPHABET))
        encrypted_message += ALPHABET[shifted_index]

    return encrypted_message


def decrypt(message, key):

    message = message.replace(':', '')
    message = message.replace(';', '')
    message = message.replace(',', '')
    message = message.replace('.', '')
    message = message.replace('?', '')
    message = message.replace('!', '')

    message = message.upper()

    decrypted_message = ''

    for c in message:
        if c not in ALPHABET:
            decrypted_message += c
            continue

        index_in_alphabet = ALPHABET.find(c)
        shifted_index = ((index_in_alphabet - key) % len(ALPHABET))
        decrypted_message += ALPHABET[shifted_index]

    return decrypted_message


def main():
    usage = "python caesar_cipher.py -e -m \"secret\"\nor\n \ " \
            "python caesar_cipher.py -d -m \"TFDSFU\""
    description = "Command line program for demonstrating the Caesar cipher."
    parser = argparse.ArgumentParser(usage=usage, description=description)

    parser.add_argument('-v', '--version', action='version', version='%(prog)s 1.0')

    group = parser.add_mutually_exclusive_group(required=True)
    group.add_argument("-e", "--encrypt", action="store_true", dest="encrypt", help="Encrypts the plain text message.")
    group.add_argument("-d", "--decrypt", action="store_true", dest="decrypt", help="Decrypts the encrypted message.")

    parser.add_argument("-m", "--message", action="store", required=True,
                        dest="message", help="You need to specify the message. Like this: -m \"secret\"")

    parser.add_argument("-k", "--key", action="store", required=True, dest="key", type=int,
                        help="You need to specify the key as a number.")

    options = parser.parse_args()

    if options.encrypt:
        print("The encrypted message is: " + encrypt(options.message, options.key))
    elif options.decrypt:
        print("The decrypted message is: " + decrypt(options.message, options.key))


if __name__ == '__main__':
    main()
```
