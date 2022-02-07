# Scrypt Step 1

We are first going to walk through the PBKDF2(HMAC-SHA-256) with the test vector in IETF Scrypt Specification section 11.  This shows how non-zero length passphrase and salts would work.   The full test vector from section 12 has zero-length strings for passphrase and salt, so we don't want to skip how this would work.

So let's walk through how to derive a key with PBKDF2(HMAC-SHA-256) with inputs: 
(P="passwd", S="salt",  c=1, dkLen=64).
HMAC-SHA-256 is our Pseudo-Random Function.
SHA-256 has a block size of 64 bytes and an output (hlen) of 32 bytes.

From https://en.wikipedia.org/wiki/PBKDF2,

DK = PBKDF2(PRF, Password, Salt, c, dkLen)
DK = T1 + T2 + ⋯ + Tdklen/hlen
dkLen/hlen = 64/32 = 2 2.  So we will get our DK from T1 + T2
Ti = F(Password, Salt, c, i)
So we will do this function twice, as
T1 = F(Password, Salt, c, 1) and T2 = F(Password, Salt, c, 2), and we will concatenate the result.

F(Password, Salt, c, i) = U1 ^ U2 ^ ⋯ ^ Uc
For us c = 1 so we only have to do the first.  (sorry, we don't have an example yet for c>1)
U1 = PRF(Password, Salt + INT_32_BE(i)) where PRF is HMAC-SHA-256

So let's take a look at how we get these values.   

From https://en.wikipedia.org/wiki/HMAC,

We are using HMAC(K,m) where K = our password ("passwd") and m = our salt ("salt").
H is SHA-256 which is calculated manually like from this: https://emn178.github.io/online-tools/sha256.html
And our block size is 64 bytes as this is the block size for SHA-256.
(see SHA-256 entry to understand what a block size is for a hash function).

Our key ("passwd") is only 6 bytes, so we pad it up to 64 bytes with the inner pad and outer pad.  Let's go to the excel sheet, 'General' tab.

Starting in cells A3:B4, we have the hex representation of "salt" and "password".
Jumping to column 'I' and beyond (way beyond), we have space to work bit by bit up to 64 bytes.  The formulas for key and message only go out for 8 bytes but you can extend these if you want or need to.  But the zero padded msg and zero padded key show the binary representation of our msg and key all the way out for 512 bits or 64 bytes… the full block size.  
We also have rows for the Inner Key Pad and Outer Key Pad.  For Inner Key Pad, we take the full 8 bit representation of '0x36', which is '00110110', and repeat it for the full block size.   Similarly for the Outer Key Pad, we take the full 8 bit representation of '0x5c', which is '01011100' and repeat it likewise.  This allows us to show a bitwise XOR with the "key" in rows 7 and 8.

Rows 10 - 16 and 17-22 show us getting these XOR'd values back to hexadecimal.  Here's how this goes:  For the XOR'd inner pad, cells D11:G11 have the first 4 bytes, and cells D12:G12 have the second 4 bytes.  We convert these to hexadecimal in cells D13:G14.   Notice bytes 7 and 8 (Cells F14 and G14) are both '36'.   Since our key is only 6 bytes long, bytes 7 and beyond will be '36'.   On row 15, I cheated a bit and hard coded the repeating 8 bits that make up the rest of the block size.  We see that 36 will repeat throughout.
Same thing for the outer pad in rows 17-22.

This is how we get the values in cells B25 and B29.  Once we understand this, the rest is easy.
One last important point from https://en.wikipedia.org/wiki/PBKDF2,
From above, because our value of 'c' is 1, we only have to calculate U1 for each of our T1 and T2.
U1 = PRF(Password, Salt + INT_32_BE(i))
For i = 1 and i = 2.  in the spreadsheet, you will see us appending '00000001' and '00000002' to our message to follow this spec.
Now we are ready to calculate these functions:
T1 = F(Password, Salt, c, 1) and T2 = F(Password, Salt, c, 2)
Each iteration follows the functions below, where hash is SHA-256 (done manually and pasted into the spreadsheet.
Return hash(o_key_pad ∥ hash(i_key_pad ∥ message))

The final value is in cell B45, which matches the first test vector  section 11.


The first test vector in section 12 (the whole scrypt process) is even simpler for the first PBKDF2 (HMAC-SHA-256) step.
Our inputs are:
scrypt (P="", S="", N=16, r=1, p=1, dklen=64)
From the scrypt Wikipedia entry, the first step is:
[B0...Bp−1] ← PBKDF2HMAC-SHA256(Passphrase, Salt, 1, blockSize*ParallelizationFactor)

The value of '1' for 'c' is the same as above.   blockSize*ParallelizationFactor becomes 128 bytes instead of the 64 bytes above.  And with empty values for "Passphrase" and "Salt".  This makes it very easy to find i key pad and o key pad, simply repeat the values '36' and '5c' (in hex) to reach the full block length.

Because the dkLen = 128, we will repeat the process 4 times (instead of 2) above.
In the 'Scrypt 1' tab we follow these 4 steps, similar to the previous example.

The final answer from this step is shown in cell B37, and matches the input from the IETF Spec for the RO Mix step seen in section 10.

