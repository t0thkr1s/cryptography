# Null Cipher

> The null cipher adds noise to the plaintext, with the goal of making the plaintext hard to isolate.

### How does it work?
In the following example, I’m going to encrypt a message with a key of 3. This means that the third letter of every word in the encrypted text will be part of the plain text. It’s important to mention that counting starts from 0, so check the fourth letter in every word. I think there is not much to explain the example is pretty straightforward.

***Encrypted message:***

> KUHSF URTEZ WEFCP HBVRM OJKEQ XCSTY

***Decrypted message:***

> SECRET

## Encryption
If you’re not a programmer or new to programming, don’t worry I'll try to explain how the code works.

```python
def encrypt(message, key):
    message = message.replace(":", "")
    message = message.replace(";", "")
    message = message.replace(",", "")
    message = message.replace(".", "")
    message = message.replace("?", "")
    message = message.replace("!", "")

    message = message.upper()

    word_substitutes = []

    for c in message:
        if c not in ALPHABET:
            continue

        index_of_plaintext = key % 5

        word_substitute = ""
        for i in range(0, 5):
            if i == index_of_plaintext:
                word_substitute += c
            else:
                random_letter_index = random.randint(0, len(ALPHABET) - 1)
                word_substitute += ALPHABET[random_letter_index]

        word_substitutes.append(word_substitute)

    return ' '.join(word_substitutes)
```

The *encrypt(message, key)* method requires 2 parameter. One is your message, the other is key. I tried to eliminate some special characters from the supplied message, but it’s far from complete. Next, there’s a loop which goes through the message letter by letter and checks if the character is in the alphabet. If the letter is not in the alphabet, then just simply skip it. I calculated the position of the letter from the message by using the modulo operator (%) which gives the remainder of a division. In other words, the key of 3 modulo 5 gives 2. So, the index of the letter will be 2.

> Keep in mind, counting starts from 0!

I used another loop to add some random letters to the plain text. If the loop reaches the index of the plain text, then we append the letter otherwise just pick a random letter from the alphabet. Finally, I appended these small words and created a single space separated string.

## Decryption

```python
def decrypt(message, key):
    message = message.replace(":", "")
    message = message.replace(";", "")
    message = message.replace(",", "")
    message = message.replace(".", "")
    message = message.replace("?", "")
    message = message.replace("!", "")

    message = message.upper()

    words_in_message = message.split()

    decrypted_message = ""

    for word in words_in_message:
        decrypted_message += word[key % 5]

    return decrypted_message
```

The *decrypt(message, key)* method is very simple. Again, I removed some special characters and split the encrypted string. This way, I could easily loop through the words and extract the characters from the correct position. This position is the decryption key.

## The Full Source Code

```python

import argparse
import random

ALPHABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"


def encrypt(message, key):
    message = message.replace(":", "")
    message = message.replace(";", "")
    message = message.replace(",", "")
    message = message.replace(".", "")
    message = message.replace("?", "")
    message = message.replace("!", "")

    message = message.upper()

    word_substitutes = []

    for c in message:
        if c not in ALPHABET:
            continue

        index_of_plaintext = key % 5

        word_substitute = ""
        for i in range(0, 5):
            if i == index_of_plaintext:
                word_substitute += c
            else:
                random_letter_index = random.randint(0, len(ALPHABET) - 1)
                word_substitute += ALPHABET[random_letter_index]

        word_substitutes.append(word_substitute)

    return ' '.join(word_substitutes)


def decrypt(message, key):
    message = message.replace(":", "")
    message = message.replace(";", "")
    message = message.replace(",", "")
    message = message.replace(".", "")
    message = message.replace("?", "")
    message = message.replace("!", "")

    message = message.upper()

    words_in_message = message.split()

    decrypted_message = ""

    for word in words_in_message:
        decrypted_message += word[key % 5]

    return decrypted_message


def main():
    usage = "python null_cipher.py -e -m \"secret\"\nor\n \ " \
            "python null_cipher.py -d -m \"WESYL ZREFM VSCNJ HIRLG CQEKK CQTXX\""
    description = "Command line program for demonstrating the Null cipher."
    parser = argparse.ArgumentParser(usage=usage, description=description)

    parser.add_argument('-v', '--version', action='version', version='%(prog)s 1.0')

    group = parser.add_mutually_exclusive_group(required=True)
    group.add_argument("-e", "--encrypt", action="store_true", dest="encrypt", help="Encrypts the plain text message.")
    group.add_argument("-d", "--decrypt", action="store_true", dest="decrypt", help="Decrypts the encrypted message.")

    parser.add_argument("-m", "--message", action="store", required=True,
                        dest="message", help="You need to specify the message. Like this: -m \"secret\"")

    parser.add_argument("-k", "--key", action="store", required=True, dest="key", type=int,
                        help="You need to specify the key as a positive number.")

    options = parser.parse_args()

    if options.encrypt:
        print("The encrypted message is: " + encrypt(options.message, options.key))
    elif options.decrypt:
        print("The decrypted message is: " + decrypt(options.message, options.key))


if __name__ == '__main__':
    main()

```
