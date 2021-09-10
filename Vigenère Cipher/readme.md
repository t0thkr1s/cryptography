# Vigenère Cipher

> The Vigenère cipher is a method of encrypting alphabetic text by using a series of interwoven Caesar ciphers, based on the letters of a keyword. It is a form of polyalphabetic substitution.

### How does it work?

Let’s say we have a word "secret" and we want to encrypt with the key "key". The length of the key (3) is obviously shorter than the length of the message (6). In this case, we repeat the key until it reaches the length of the plain message. The encryption key will be "keykey". Our message starts with the letter "s", which has an index of 18. __Remember, counting starts from 0!__ Now, the letter "k" has an index of 10. If you add these two numbers, you’ll get a larger number than the length of the alphabet. To prevent this from happening, we can use the modulo operator, which gives the remainder of a division. The result is the index of the first encrypted letter in the alphabet.

> (18 + 10) % 26 = 2 = letter "C"

***Plain text message:***

> SECRET

***Encrypted message:***

> CIABIR

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

    for i in range(len(message)):
        message_letter_index = ALPHABET.index(message[i])
        key_letter_index = ALPHABET.index(key[(i % len(key))].upper())
        index = (message_letter_index + key_letter_index) % len(ALPHABET)
        encrypted_message += ALPHABET[index]

    return encrypted_message
```

First, I removed the usual special characters. Then, I created a loop with the length of the message. I calculated the index of the i-th letter in the plain message by using the *index()* method that takes a char and returns the corresponding index in a string. Next, I needed the index of the i-th key letter. This time, I divided the i index with the modulo operator to prevent too large indexes and converted the letter to upper case.

## Decryption

The fun part is the calculation of the encrypted letter index. I did it exactly as I explained, added the two index and divided the result with the modulo operator to get the remainder. Finally, I appended the character from the alphabet to the encrypted message.

```python
def decrypt(message, key):
    message = message.replace(':', '')
    message = message.replace(';', '')
    message = message.replace(',', '')
    message = message.replace('.', '')
    message = message.replace('?', '')
    message = message.replace('!', '')

    message = message.upper()

    decrypted_message = ''

    for i in range(len(message)):
        message_letter_index = ALPHABET.index(message[i])
        key_letter_index = ALPHABET.index(key[(i % len(key))].upper())
        index = (message_letter_index - key_letter_index) % len(ALPHABET)
        decrypted_message += ALPHABET[index]

    return decrypted_message
```

The *decrypt(message, key)* method only differs in one thing and that is the subtraction instead of the addition. So, in order to decrypt the message we have to subtract the key letter index from the message letter index to get the index of the letter in the alphabet. I really hope it’s understandable and not too confusing.

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

    for i in range(len(message)):
        message_letter_index = ALPHABET.index(message[i])
        key_letter_index = ALPHABET.index(key[(i % len(key))].upper())
        index = (message_letter_index + key_letter_index) % len(ALPHABET)
        encrypted_message += ALPHABET[index]

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

    for i in range(len(message)):
        message_letter_index = ALPHABET.index(message[i])
        key_letter_index = ALPHABET.index(key[(i % len(key))].upper())
        index = (message_letter_index - key_letter_index) % len(ALPHABET)
        decrypted_message += ALPHABET[index]

    return decrypted_message


def main():
    usage = "python vigenere_cipher.py -e -m \"secret\"\nor\n \ " \
            "python vigenere_cipher.py -d -m \"CIABIR\""
    description = "Command line program for demonstrating the Vigenere cipher."
    parser = argparse.ArgumentParser(usage=usage, description=description)

    parser.add_argument('-v', '--version', action='version', version='%(prog)s 1.0')

    group = parser.add_mutually_exclusive_group(required=True)
    group.add_argument("-e", "--encrypt", action="store_true", dest="encrypt", help="Encrypts the plain text message.")
    group.add_argument("-d", "--decrypt", action="store_true", dest="decrypt", help="Decrypts the encrypted message.")

    parser.add_argument("-m", "--message", action="store", required=True,
                        dest="message", help="You need to specify the message. Like this: -m \"secret\"")

    parser.add_argument("-k", "--key", action="store", required=True, dest="key", help="You need to specify the key "
                                                                                       "as a positive number.")

    options = parser.parse_args()

    if options.encrypt:
        print("The encrypted message is: " + encrypt(options.message, options.key))
    elif options.decrypt:
        print("The decrypted message is: " + decrypt(options.message, options.key))


if __name__ == '__main__':
    main()
```
