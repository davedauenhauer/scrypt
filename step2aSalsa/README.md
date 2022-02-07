# Salsa

Helpful references:  From the author:  https://cr.yp.to/salsa20.html
	Very helpful visualization of the 'rounds':  http://www.crypto-it.net/eng/symmetric/salsa20.html
	Wikipedia:  https://en.wikipedia.org/wiki/Salsa20
	
Salsa 20 mixes a 64 byte string to another 64 byte string. The '/8' means it goes through 8 mixing iterations:  4 'column rounds' and 4 'row rounds'.  The crypto-it.net page above was very helpful to me to understand how these rounds work.

The bulk of the complexity is in each column round and row round tab.  

The 'front' tab has the input, the output, and works simply left-to-right.

Input 4 byte strings into column A in hex format.
Column B 'reflects' the order of each byte (the littleendian' function described in crypto-it.net above.
Column C changes to decimal.  This is the format that each 'round' tab will ingest.
Column D shows the output after 8 rounds.
Column E brings this back to Decimal
Column F shows the addition of columns C and E mod 2^32.
Column G takes the result back to hex format.
Column I shows the result, after 'reflecting' the endianness again.

