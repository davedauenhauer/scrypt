# Scrypt explained in Excel
The inputs to the scrypt function are:
	Passphrase: 
	Salt:  
	Cost Factor 'n':
	Block Size Factor 'r':
	Parallelization Factor 'p':
	Desired Key Length 'dkLen':
	
We will focus on the first test vector in the IETF Scrypt specification (section 12) since most of the interim test vectors follow this.  The inputs to this test vector are:
Passphrase: "", Salt: "", n: 16, r:1, p:1, dkLen: 64.
We will also follow the  PBKDF2(HMAC-SHA-256) with Passphrase: "passwd", Salt: "salt" since the blank passphrase and salt above allow us to skip some interesting steps.

The high-level steps we will follow are:
	1) Use PBKDF2(HMAC-SHA-256) to create the first data array from the input passphrase, salt , block size factor and parallelization factor.
	2) Use the RO Mix function with step 1 output to generate an "expensive salt".
	3) Use PBKDF2(HMAC-SHA-256) with this new expensive salt to generate the derived key.

Steps 1 and 3 will use one excel file each.  These are similar showing the PBKDF2 functions with the SHA-256 hashing done manually.
Step 2 has 3 excel sheets and are constructed with several manual steps.  These sheets are named:
	2A:  Salsa20/82
	2B:  Block Mix
	2C:  RO Mix
RO Mix uses Block Mix and Salsa20/8 to build the result.

## References

These sites provided guidance throughout and are used as references throughout.

| Site | URL |
| ------ | ------ |
| IETF Scrypt Specification | [https://datatracker.ietf.org/doc/html/rfc7914][PlDb] |
| Wikipedia entry on Scrypt | [https://en.wikipedia.org/wiki/Scrypt][PlGh] |
| Wikipedia entry on PBKDF2 | [https://en.wikipedia.org/wiki/PBKDF2][PlGd] |
| Wikipedia entry on HMAC | [https://en.wikipedia.org/wiki/HMAC][PlOd] |


[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://datatracker.ietf.org/doc/html/rfc7914>
   [PlGh]: <https://en.wikipedia.org/wiki/Scrypt>
   [PlGd]: <https://en.wikipedia.org/wiki/PBKDF2>
   [PlOd]: <https://en.wikipedia.org/wiki/HMAC>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>
