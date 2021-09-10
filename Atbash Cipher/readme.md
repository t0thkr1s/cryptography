# Atbash Cipher

> The Atbash cipher is another type of substitution cipher. It entails replacing each letter in the plaintext with the corresponding letter if the alphabet were reversed.

### How does it work?

The Atbash cipher does not use a key and this is the reason why it doesn’t provide any communication security. The idea here is simple, just reverse the alphabet and substitute the letters. A becomes Z, B becomes Y, and so on… I think there is not much to explain the example is pretty straightforward.

***Encrypted message:***

> HVXIVG

***Decrypted message:***

> SECRET

## Encryption

If you’re not a programmer or new to programming, don’t worry I'll try to explain how the code works.

```python
def encrypt(message):
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
        shifted_index = len(ALPHABET) - 1 - index_in_alphabet
        encrypted_message += ALPHABET[shifted_index]

    return encrypted_message
```

First, I removed the usual special characters. Next, the loops go through the message character by character. If the current character is not in the alphabet, then simply append the character to the encrypted message and move on. I got the shifted index by subtracting the index of the character from the length of the alphabet. Finally, I appended the letter at the shifted index to the encrypted message variable.

## Decryption

```python
def decrypt(message):
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
        shifted_index = len(ALPHABET) - 1 - index_in_alphabet
        decrypted_message += ALPHABET[shifted_index]

    return decrypted_message
```

Obviously, the *decrypt(message)* method is exactly the same as the *encrypt(message)* method.

## The Full Source Code

```python
import argparse

ALPHABET = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'


def encrypt(message):
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
        shifted_index = len(ALPHABET) - 1 - index_in_alphabet
        encrypted_message += ALPHABET[shifted_index]

    return encrypted_message


def decrypt(message):
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
        shifted_index = len(ALPHABET) - 1 - index_in_alphabet
        decrypted_message += ALPHABET[shifted_index]

    return decrypted_message


def main():
    usage = "python atbash_cipher.py -e -m \"secret\"\nor\n \ " \
            "python atbash_cipher.py -d -m \"HVXIVG\""
    description = "Command line program for demonstrating the Atbash cipher."
    parser = argparse.ArgumentParser(usage=usage, description=description)

    parser.add_argument('-v', '--version', action='version', version='%(prog)s 1.0')

    group = parser.add_mutually_exclusive_group(required=True)
    group.add_argument("-e", "--encrypt", action="store_true", dest="encrypt", help="Encrypts the plain text message.")
    group.add_argument("-d", "--decrypt", action="store_true", dest="decrypt", help="Decrypts the encrypted message.")

    parser.add_argument("-m", "--message", action="store", required=True,
                        dest="message", help="You need to specify the message. Like this: -m \"secret\"")

    options = parser.parse_args()

    if options.encrypt:
        print("The encrypted message is: " + encrypt(options.message))
    elif options.decrypt:
        print("The decrypted message is: " + decrypt(options.message))


if __name__ == '__main__':
    main()
```
