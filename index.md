
Title slide

# Terminology

---

## Unicode

| Ordinal | Description | Example |
| --: | :-- | :--: |
| U+0041 | `LATIN CAPITAL LETTER A` | A |
| U+00C9 | `LATIN CAPITAL LETTER E WITH ACUTE` | É |
| U+516b | `CJK UNIFIED IDEOGRAPH-5168` | 八 |
| U+1f40d | `SNAKE` | 🐍 |

---

pic of 0&1s

---

## Codec

Coder-decoder

Encoder + Decoder

---

### Decoder

Restitutes some data in intelligible form

some data ⇒ decoder ⇒ Unicode data

---

### Encoder

Takes intelligible data into some other format

Unicode ⇒ codec => some data

---

| Name | Scope |
| --: | :-- |
| `ascii` | English alphanumerics and punctuation |
| `latin-1` | Western Europe |
| `utf-8` | All of Unicode |
| `utf-16` | All of Unicode |

<small>(See the docs for `codecs`)</small>

---

## Recap

* All your characters are belong to Unicode
* Ethernet wire doesn't speak Unicode, need codec
* export = encode, import = decode
* UTF-8 and UTF-16 are codecs that handles all of Unicode

---

PIC You didn't want theory


# What Python has

---

`bytes` and `unicode`

---

`str`: bytes on Python 2

`str`: unicode on Python 3

---

    (bytes).decode(codec)

    (text).encode(codec)

---

Things that you can use text with:

- `json`
- `requests`
- `io.open`

---

## Recap

- Two types: for bytes, for text
- Python 2: `str`, ``unicode`
- Python 3: `bytes`, `str`
- `decode` and `encode` methods
- Some objects and functions do the conversions for you


# Commmon issues

---

Trying to decode/encode at random

    >>> u'Élodie'.decode('utf-8')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'str' object has no attribute 'decode'

---

Trying to decode/encode at random on Python 2

    >>> u'Élodie'.decode('utf-8')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "/usr/lib/python2.7/encodings/utf_8.py", line 16, in decode
        return codecs.utf_8_decode(input, errors, True)
    UnicodeEncodeError: 'ascii' codec can't encode character u'\xc9' in position 0: ordinal not in range(128)

---

Trying to decode/encode at random on Python 2 with ASCII data

    >>> u'Quick brown fox'.decode('utf-8')
    u'Quick brown fox'

---

Mixing unicode and bytes

    >>> u'Élodie' + ' et ' + 'Étienne'
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    UnicodeDecodeError: 'ascii' codec can't decode byte 0xc3 in position 0: ordinal not in range(128)

---

### Solution: The Unicode sandswich

| . |
| :--: |
| Decode when you acquire data |
| Unicode throguhout your app |
| Encode when you send data out |

---

Your keystrokes are also subject to encoding, and so is displaying the text.

    >>> len(u'É')
    1
    >>> len('É')
    2
    >>> 'É'
    '\xc3\x89'

<small>(This is most obvious in Python 2, but the same happens in 3)</small>


---

Python 3 tries to display unicode in your terminal

    >>> '\N{SNOWMAN}'
    '☃'

---

    $ PYTHONIOENCODING=latin1 python3
    >>> print('\N{SNOWMAN}')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    UnicodeEncodeError: 'latin-1' codec can't encode character '\u2603' in position 0: ordinal not in range(256)
    >>> '\N{SNOWMAN}'
    '\u2603'
    >>> '\xc9tienne'
    'ienne'
    >>> print(ascii('\xc9tienne'))
    '\xc9tienne'


## Recap

- Python 2 tries to autoconvert. This hides mistakes until it is too late.
- The Unicode sandwich:
  - Decode data as you get it
  - Encode data before you hand it off to IO
- Python 2: Don't mix string types

---


