# Block Mix
From the Scrypt Wikipedia page, we have this definition of 'Block Mix':
Function BlockMix(B):
The block B is r 128-byte chunks (which is equivalent of 2r 64-byte chunks)r ← Length(B) / 128;

Treat B as an array of 2r 64-byte chunks[B0...B2r-1] ← B

X ← B2r−1
    for i ← 0 to 2r−1 do
        X ← Salsa20/8(X xor Bi)  // Salsa20/8 hashes from 64-bytes to 64-bytesYi ← X

return ← Y0∥Y2∥...∥Y2r−2 ∥ Y1∥Y3∥...∥Y2r−1

We have r = 1.  Block Mix breaks this into 2 64-byte chunks.
Cells with the blue/grey shading in column A are pasted in, either as input, or a result from the Salsa 20/8 function.
Our first chunk, B0, gets pasted into column A, in 4-byte words, rows 3 thru 18.
Our second chunk, B1, gets pasted into column A, in 4-byte words, rows 21 thru 36.
We paste the same chunk into column A, rows 39 thru 54, to represent 'X', the working chunk of data.   So B1 is our first working chunk.

Columns C - K take the binary representation to decimal.  This is not really used anywhere in this excel sheet, but it's a practice used to have this representation available.
Columns M - AR are the binary representation of the 4 byte word on each row.  These are either derived from the value in column A (indicated by the --> arrow in column A), or they are the result of a bitwise calculation from above.   In these cases, the value in column A is derived from the binary result, and indicated by an arrow like this: <--

So now we follow the logic from the pseudo-code.
Values in A57 through A72 show the XOR of X to B0.
We cut and paste to Salsa 20, and paste the results back in here, A76 through A91.  This is the first chunk (64 bytes) of output  to the Block Mix function.  These values are also now the working version of 'X'.
We XOR these to our B1 input.  These are shown in cells A94 thru A109.
Cut and paste these values to Salsa 20 again and paste the results back to A113 to A128.  This is the second chunk (64 bytes) of the block mix function.
