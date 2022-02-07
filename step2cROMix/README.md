# RO Mix
From the Scrypt Wikipedia page, we have this definition of 'RO Mix':

Function ROMix(Block, Iterations)
Create Iterations copies of XX ← Block
   for i ← 0 to Iterations−1 do
      Vi ← X
      X ← BlockMix(X)

for i ← 0 to Iterations−1 do
      j ← Integerify(X) mod Iterations 
      X ← BlockMix(X xor Vj)
return X

Our RO Mix excel sheet has 16 tabs, each named 'i=n'.  Each sheet is divided into a top half and bottom half by black shading on row 70.
In the first 'for' loop above, we will fill out V0, V1, … V15 on the top half of our sheet.
In tab i=0, for V0, we paste in our input of 128 bytes in 4-byte(hex) words.  These go in cells A3 thru A34.  We then Block Mix these (dividing this into two chunks, the first being A3 thru A18, the second being A19 thru A34.  The Block Mix Result is pasted into cells A37 thru A68.
Now we jump to tab i=1.   We reference back to i=0!A37-A68 for our values for V1.  We block mix again, paste the values into i=1!A37-68, and keep going until we have the top half of all sheets, and all 16 values for V defined.  We are now done with the first 'for' loop, and our working data set for X is now V15.

We now go to the second 'for' loop.  We have to know what 'Integerify' means.  From Wikipedia:
Where RFC 7914 defines Integerify(X) as the result of interpreting the last 64 bytes of X as a little-endian integer A1.
So we get the last 64 bytes of X (which is V15 on tab i=0, or the final result from the previous tab on tabs i=1 thru i=15.
We reference these 64 bytes in cells A72 thru A87 on each tab.
We then, laboriously, flip every byte to create the little-endian hex representation that Integerify wants.  This is in cell C73.  To leave a trail, I copied / paste values into Cell C74.  Then I used an online hex to decimal converter to get the decimal integer in Cell C75.   
Long.
I then went to Wolfram Alpha to get this big integer mod 16.  This result is typed into cell C76.
I should have kept reading.  Wikipedia continues:
Since Iterations equals 2 to the power of N, only the first Ceiling(N / 8) bytes among the last 64 bytes of X, interpreted as a little-endian integer A2, are actually needed to compute Integerify(X) mod Iterations = A1 mod Iterations = A2 mod Iterations.
Cells C78, C79 and C80 show the results of this wise shortcut, which match the values with the hard way I started with.

So the whole point of this Integerify is to randomly select a value between 0 and 16.  We then pick our 128 byte block of data Vn, where n = this random value.  This is XOR'd to our current value of X to go into a Block Mix function.

This gives a view into why the parameter 'n' = Cost Factor is useful in these key derivation function.  Most of what we are doing is linear… that is once we are done processing a chunk of data, we can forget it.  But during this instance of RO Mix, we need to retain 16 blocks of 128 bytes of data throughout the processing in memory.  Not a big deal here, but we can see if the value of n grows and the block size grows, we get a significant memory requirement to process this.

The result of each Block Mix is at the bottom of each sheet, in cells A193 thru A224.
When we get to the last sheet, i=15, this result is the output of the full RO Mix Function.
