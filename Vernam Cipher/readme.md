# Vernam Cipher

> The Vernam Cipher is based on the principle that each plaintext character from a message is ‘mixed’ with one character from a key stream. If a truly random key stream is used, the result will be a truly ‘random’ ciphertext which bears no relation to the original plaintext.
>
> Source: https://www.cryptomuseum.com/crypto/vernam.htm

### How does it work?

Let’s say we have a word "secret" and we want to encrypt with the key "key". The length of the key (3) is obviously shorter than the length of the message (6). In this case, we repeat the key until it reaches the length of the plain message. The encryption key will be "keykey".

Next, we have to convert both the plain text and the key to binary.

***Plain text message in binary:***

> 011100110110010101100011011100100110010101110100

***The key in binary:***

> 011010110110010101111001011010110110010101111001

Finally, we have to XOR these two binary values to get the encrypted message.

> 0 ⊕ 0 = 0
> 
> 0 ⊕ 1 = 1
> 
> 1 ⊕ 0 = 1
> 
> 1 ⊕ 1 = 0

***Encrypted message:***

> 000110000000000000011010000110010000000000001101

## Encryption

The encrypt method takes 2 parameters a key and a message, which are required for the encryption. I initialized an index variable with a value of 0. It’s used for keeping track of the key length. The loop goes through the message character by character. In the loop, I calculate the ASCII code for the current character both in the message and key, then perform a XOR operation on those values. I convert this back to a single char (might be a gibberish value) and append it to the `encrypted_message` variable. After this, the index needs to be incremented. If the index equals with the length of the key, then I set the index back to 0 to prevent possible index out of range exceptions.

```python
def encrypt(key, message):
    encrypted_message = ""

    index = 0
    for char in message:
        encrypted_message = encrypted_message + chr(ord(char) ^ ord(key[index]))
        index = index + 1
        if index == len(key):
            index = 0

    return bin(int(binascii.hexlify(encrypted_message), 16))
```

## Decryption

As for the decryption, the procedure is almost exactly the same. The only thing that differs from the encryption is that I converted the encrypted message to binary, which is required for binary operations.

```python
def decrypt(key, message):
    message = binascii.unhexlify("%x" % int(message, 2))

    decrypted_message = ""

    index = 0
    for char in message:
        decrypted_message = decrypted_message + chr(ord(char) ^ ord(key[index]))
        index = index + 1
        if index == len(key):
            index = 0

    return decrypted_message
```

## The Full Source Code

```python
import argparse
import binascii


def encrypt(key, message):
    encrypted_message = ""

    index = 0
    for char in message:
        encrypted_message = encrypted_message + chr(ord(char) ^ ord(key[index]))
        index = index + 1
        if index == len(key):
            index = 0

    return bin(int(binascii.hexlify(encrypted_message), 16))


def decrypt(key, message):
    message = binascii.unhexlify("%x" % int(message, 2))

    decrypted_message = ""

    index = 0
    for char in message:
        decrypted_message = decrypted_message + chr(ord(char) ^ ord(key[index]))
        index = index + 1
        if index == len(key):
            index = 0

    return decrypted_message


def main():
    usage = "python vernam_cipher.py -e -m secret -k key\nor\n \ " \
            "python vernam_cipher.py -d -m 000110000000000000011010000110010000000000001101 -k key"
    description = "Command line program for demonstrating the Vernam cipher."
    parser = argparse.ArgumentParser(usage=usage, description=description)

    parser.add_argument("-v", "--version", action="version", version="%(prog)s 1.0")

    group = parser.add_mutually_exclusive_group(required=True)
    group.add_argument("-e", "--encrypt", action="store_true", dest="encrypt",
                       help="Encrypts the plain text message.")
    group.add_argument("-d", "--decrypt", action="store_true", dest="decrypt",
                       help="Decrypts the encrypted message.")

    parser.add_argument("-m", "--message", action="store", required=True, dest="message",
                        help="You need to specify the message. Like this: -m \"secret\"")

    parser.add_argument("-k", "--key", action="store", required=True, dest="key",
                        help="You need to specify the key.")

    options = parser.parse_args()

    if options.encrypt:
        print("The encrypted message is: " + encrypt(options.key, options.message))
    elif options.decrypt:
        print("The decrypted message is: " + decrypt(options.key, options.message))


if __name__ == "__main__":
    main()
```
