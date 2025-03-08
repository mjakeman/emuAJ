Unpacking binary data with the struct module
Most languages come with some sort of notation for representing collections of typed data. In C, it’s struct. In Pascal, it’s record. It’s an efficient way of structuring information, especially as you can order the compiler (if there is one) to pack the structure in such a way that you have complete control over the layout of that structure, bit-for-bit, in memory and on disk. That’s a useful property when you want to represent collections of bytes, like we need to with the cartridge’s header metadata.

You can do this in myriad ways in Python. The problem, however, is that binary structures like this one requires an eye for precision: you need to not only read out the information byte-by-byte, but also take into account things like:

Endianness, or the direction in which you read a sequence of bytes
Big and little endian systems interpret byte structures differently. The Z80 was a big endian CPU, and yours is probably little endian.

Type sys.byteorder in your Python interpreter to tell for sure.

Signed vs Unsigned integers
Unsigned integers are positive integers only. Signed, on the other hand, is both negative and positive. The representation you pick will determine the value held in the byte string.

Strings
Is it a C-style string or a Pascal-style? The former terminates a string with a NUL character to indicate the end is reached. But Pascal strings prefix theirs with the byte size of the string ahead of it.

Size
Are you reading an 8-bit number or a 16-bit number? Perhaps an even larger one?

And the list goes on and on. In other words, the bits and bytes that make up our data is a matter of representation. Get it wrong, and you’ll read in garbage or, worse, it’ll work with some values but not others!

Luckily the struct module that ships with Python is equipped to deal all of these issues. Using a little mini-language, not unlike the one you’d use for format strings, you can tell Python how to interpret a stream of binary data.

Big and Little Endian
Let’s briefly talk about endianness and what it is. It plays a prominent role in how we read and represent information. It’s the order in which you read a sequence of bytes of data.

A term borrowed from the book, Gulliver’s Travels, of all places.

So consider the following hexadecimal string in Python: