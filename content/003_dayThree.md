Title: Day Three
Date: 2018-03-25 22:32
Modified: 2018-03-27 13:15
Category: Algorithms
Tags: python, programming, howto, algorithms, byte, bytearray
Slug: day-three
Authors: Christopher Tichenor
Summary: Bytes and Bytearrays.

## How to Read/Write a BMP file using Bytes and Bytearrays

As a follow-up from yesterday, apparently using inheritance isn’t much better for [removing methods from a parent](https://medium.com/@george.shuklin/why-you-cant-remove-parent-methods-in-python-d4935e6d54a4). Ah well.

Time to move on to working with some of the other array types. On the varying data side, I’m pretty familiar with lists and tuples, so I won’t waste time with those. On the single-data-type side I’m pretty familiar with strings too (though it’s tempting to play with the newly-discovered recursion aspect). That leaves Bytes and Bytearrays.

It might be a good exercise to try to [read a BMP file again](https://stackoverflow.com/questions/20276458/working-with-bmp-files-in-python-3) as I’d attempted to do at a class preceding PyBay 2017.

---

First off, we need to read the file:

```python
with open('/Users/chris/Dropbox/Experiments/eatyouralgorithms/bmptest.bmp', 'rb') as f:
    orig_bmp = bytes(f.read())
```

Here's what it looks like for reference:

![I'm actually a trained artist. Go figure.]({filename}/images/003_bmptest.png)

Now, let's decode the bytes like in the article:

```python
orig_bmp.read(2).decode()
```

Error!

```text
    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-2-7b9b1a26efec> in <module>()
    ----> 1 orig_bmp.read(2).decode()
    

    AttributeError: 'bytes' object has no attribute 'read'
```

Oops. That doesn't work. What if I try to print the whole thing?

```python
print(orig_bmp)
```

And we get...

```text
    IOPub data rate exceeded.
    The notebook server will temporarily stop sending output
    to the client in order to avoid crashing it.
    To change this limit, set the config variable
    `--NotebookApp.iopub_data_rate_limit`.
```

Yes, I'm using a Jupyter Notebook for these tests, suppose I should've guessed that would happen. 

Let's try doing exactly what the original StackOverflow article recommends against. [The Wikipedia article](https://en.wikipedia.org/wiki/BMP_file_format) says that the first 14 bytes are the header:


```python
print(orig_bmp[0:13])
```

And we get...

```text
    b'BM6v/\x00\x00\x00\x00\x006\x00\x00'
```

Ech. My hexadecimal sucks so this is effectively gibberish to me. What if we try printing decimals? (_I went through a few iterations of this before realizing where the cutoff should be._)

```python
for i,j in enumerate(orig_bmp[0:60]):
    print('{}:{}'.format(i,j))
```

This prints out:

```text
    0:66
    1:77
    2:54
    3:118
    4:47
    5:0
    6:0
    7:0
    8:0
    9:0
    10:54
    11:0
    12:0
    13:0
    14:40
    15:0
    16:0
    17:0
    18:56
    19:4
    20:0
    21:0
    22:48
    23:253
    24:255
    25:255
    26:1
    27:0
    28:32
    29:0
    30:0
    31:0
    32:0
    33:0
    34:0
    35:0
    36:0
    37:0
    38:19
    39:11
    40:0
    41:0
    42:19
    43:11
    44:0
    45:0
    46:0
    47:0
    48:0
    49:0
    50:0
    51:0
    52:0
    53:0
    54:255
    55:255
    56:255
    57:255
    58:255
    59:255
...more of 255 for a long time...
...then other numbers, then more 255s, etc....
```

It looks like at byte 54 we start going into the image data. This also isn't telling me a whole lot more than that. Let's try the Struct module, [this](https://stackoverflow.com/questions/47003833/how-to-read-bmp-file-header-in-python) seems to give better information on using it:

```python
import struct

with open('/Users/chris/Dropbox/Experiments/eatyouralgorithms/bmptest.bmp', 'rb') as f:
    print('Type:', f.read(2).decode())
    print('Size: %s' % struct.unpack('I', f.read(4)))
    print('Reserved 1: %s' % struct.unpack('H', f.read(2)))
    print('Reserved 2: %s' % struct.unpack('H', f.read(2)))
    print('Offset: %s' % struct.unpack('I', f.read(4)))
    print('DIB Header Size: %s' % struct.unpack('I', f.read(4)))
    print('Width: %s' % struct.unpack('I', f.read(4)))
    print('Height: %s' % struct.unpack('I', f.read(4)))
    print('Colour Planes: %s' % struct.unpack('H', f.read(2)))
    print('Bits per Pixel: %s' % struct.unpack('H', f.read(2)))
    print('Compression Method: %s' % struct.unpack('I', f.read(4)))
    print('Raw Image Size: %s' % struct.unpack('I', f.read(4)))
    print('Horizontal Resolution: %s' % struct.unpack('I', f.read(4)))
    print('Vertical Resolution: %s' % struct.unpack('I', f.read(4)))
    print('Number of Colours: %s' % struct.unpack('I', f.read(4)))
    print('Important Colours: %s' % struct.unpack('I', f.read(4)))
```

Which results in:

```text
    Type: BM
    Size: 3110454
    Reserved 1: 0
    Reserved 2: 0
    Offset: 54
    DIB Header Size: 40
    Width: 1080
    Height: 4294966576
    Colour Planes: 1
    Bits per Pixel: 32
    Compression Method: 0
    Raw Image Size: 0
    Horizontal Resolution: 2835
    Vertical Resolution: 2835
    Number of Colours: 0
    Important Colours: 0
```

Something's going haywire when we read the "Height" metadata, but then I think the "Colour" planes is ok, and the "Bits per Pixel" looks right. How the hell the resolution is determined beats me.

[StackOverflow to the rescue](https://stackoverflow.com/questions/12464310/getting-incorrect-width-when-decoding-bitmap#12465956) again, looks like Acorn may have put a negative value in for the height, denoting that it's top-down rather than the typical bottom-up. Let's fix that:


```python
with open('/Users/chris/Dropbox/Experiments/eatyouralgorithms/bmptest.bmp', 'rb') as f:
    print('Type:', f.read(2).decode())
    print('Size: %s' % struct.unpack('I', f.read(4)))
    print('Reserved 1: %s' % struct.unpack('H', f.read(2)))
    print('Reserved 2: %s' % struct.unpack('H', f.read(2)))
    print('Offset: %s' % struct.unpack('I', f.read(4)))
    print('DIB Header Size: %s' % struct.unpack('I', f.read(4)))
    print('Width: %s' % struct.unpack('I', f.read(4)))
    print('Height: %s' % struct.unpack('i', f.read(4)))  # <- Change to signed int
    print('Colour Planes: %s' % struct.unpack('H', f.read(2)))
    print('Bits per Pixel: %s' % struct.unpack('H', f.read(2)))
    print('Compression Method: %s' % struct.unpack('I', f.read(4)))
    print('Raw Image Size: %s' % struct.unpack('I', f.read(4)))
    print('Horizontal Resolution: %s' % struct.unpack('I', f.read(4)))
    print('Vertical Resolution: %s' % struct.unpack('I', f.read(4)))
    print('Number of Colours: %s' % struct.unpack('I', f.read(4)))
    print('Important Colours: %s' % struct.unpack('I', f.read(4)))
```

And now we get:

```text
    Type: BM
    Size: 3110454
    Reserved 1: 0
    Reserved 2: 0
    Offset: 54
    DIB Header Size: 40
    Width: 1080
    Height: -720
    Colour Planes: 1
    Bits per Pixel: 32
    Compression Method: 0
    Raw Image Size: 0
    Horizontal Resolution: 2835
    Vertical Resolution: 2835
    Number of Colours: 0
    Important Colours: 0
```

That looks right now.

Okay so after reading all of that data, does that match up with where the image data starts? Let me extract the bytes with some regexing in Sublime Text, which yields:

```text
2 4 2 2 4 4 4 4 2 2 4 4 4 4 4 4
```

I'll calculate this in Python because that's where I'm building up my fluency:

```python
nums = "2 4 2 2 4 4 4 4 2 2 4 4 4 4 4 4".split(' ')
x = 0

for i in nums:
    x += int(i)

x
# Output: 54
```

Bingo. That matches with where I saw the image data starting.

Now that we know where the pixel data starts, let's see where the pixel data is _not_ black or white:

```python
for i,j in enumerate(orig_bmp[54:]):
    if j != 255 and j != 0:
        print('{}:{}'.format(i,j))
```

```text
...blah blah blah...
    1461672:73
    1461673:73
    1461674:73
    1461676:231
    1461677:231
    1461678:231
    1462201:233
    1462202:227
    1462205:203
    1462206:191
    1462209:173
    1462210:155
    1462213:144
    1462214:118
    1462217:115
    1462218:83
    1462221:95
    1462222:58
    1462225:84
    1462226:46
...and so on and so forth...
```

At pixel 1462201 is where things start getting interesting, at least to me. This is where the pixels are not the same in all three channels, which means these are colored pixels. Acorn, bless its heart, does not use partial pixel antialiasing, so when it antialiases a transition between black and white, all of the RGB values are increased/decreased at the same time.

The file metadata also indicates that the image data is 32bpp, which means we have an alpha channel. That's consistent with this data as we're seeing a 1 byte gap between the color data.

So, pixel 1462201 is where we should start seeing the "nose". What I'd like to do is change the color of the nose from blue to red. According to Acorn, the blue that I'm using uses 255 in the Blue channel, 4 in the Red channel, and 51 in the Green channel. I should flip that around so I'm using 255 in the Red channel, 4 in the Green channel, and 51 in the Red channel (yes, I know that color perception doesn't work that way, but the values should be "close enough" for a decent, if slightly magenta, red).

I iterated through this code to figure out what color is what:

```python
mbmp = bytearray(orig_bmp[54:])

for i in range(0,len(mbmp),4):
    if mbmp[i+1] != mbmp[i+2] and mbmp[i+1] != mbmp[i+3]:
        print('Green: {}'.format(mbmp[i+1]))
        print('Red: {}'.format(mbmp[i+2]))
        print('Blue: {}'.format(mbmp[i+3]))
```

Which results in:

```text
...blah blah blah...
    Blue: 255
    Green: 51
    Red: 4
    Blue: 255
    Green: 51
    Red: 4
    Blue: 255
    Green: 51
    Red: 4
...blah blah blah...
```

Now let's put it all together:

```python
output_bytes = bytearray(orig_bmp)
for i in range(54,len(output_bytes),4):
    if output_bytes[i+1] != output_bytes[i+2] and output_bytes[i+1] != output_bytes[i+3]:
        green = output_bytes[i+1]
        red = output_bytes[i+2]
        blue = output_bytes[i+3]
        output_bytes[i+1] = red
        output_bytes[i+2] = blue
        output_bytes[i+3] = green
        
with open('out.bmp', 'wb') as outfile:
    outfile.write(output_bytes)
```

![That nose is not what I expected.]({filename}/images/003_out_magenta.png)

It works! Okay, more magenta than I would've liked, but still, I edited the raw bytes of an image!

Upon further inspection it looks like I'm not actually modifying the blue value, both blue and red are at 255, which explains the magenta-ness.

Let's rework the color channels again:

```python
for i in range(54,len(orig_bmp),4):
    if orig_bmp[i+1] != orig_bmp[i+2]:
        print('Green: {}'.format(orig_bmp[i+1]))
        print('Red: {}'.format(orig_bmp[i+2]))
        print('Blue: {}'.format(orig_bmp[i]))
```

And now we have:

```text
...blah blah blah...
    Green: 51
    Red: 4
    Blue: 255
    Green: 51
    Red: 4
    Blue: 255
    Green: 51
    Red: 4
    Blue: 255
    Green: 51
    Red: 4
    Blue: 255
...blah blah blah...
```

Messing around with this code has showed me that I mixed up the alpha with the blue channel, now to fix:

```python
output_bytes = bytearray(orig_bmp)
for i in range(54,len(output_bytes),4):
    if output_bytes[i+1] != output_bytes[i+2] and output_bytes[i+1] != output_bytes[i+3]:
        blue = output_bytes[i]
        green = output_bytes[i+1]
        red = output_bytes[i+2]
        output_bytes[i] = red
        output_bytes[i+1] = green
        output_bytes[i+2] = blue
        
with open('out.bmp', 'wb') as outfile:
    outfile.write(output_bytes)
```

![That nose isn't cold anymore!]({filename}/images/003_out.png)

And it actually works (for real this time)!
