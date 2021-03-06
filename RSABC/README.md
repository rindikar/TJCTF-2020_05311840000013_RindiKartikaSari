# TJCTF-2020_RSABC
## Description
I was just listening to some  [relaxing ASMR](https://youtu.be/J2g3lvNkAfI)  when a notification popped up with  [this](https://static.tjctf.org/68f148e8d4b5ceb8f9fa6da568db024c28b80b55891fba49880b76b35d436114_rsa.txt).
??? <br>
__Hint__ : ```It's easy as R-S-A! Wait..``` <br>
```html
n=57772961349879658023983283615621490728299498090674385733830087914838280699121 
e=65537
c=36913885366666102438288732953977798352561146298725524881805840497762448828130
```

## Solution
1. Kita cari faktorisasi dari nilai pada variabel ```n``` menggunakan _online factorize tools_ yakni ```FactorDB```<br>
![01](https://user-images.githubusercontent.com/49342639/83088275-73c86f00-a0bd-11ea-911f-86339762c88d.PNG)
	<br> Didapatkan faktorisasi ```p``` dan ```q``` dari nilai pada variabel ```n``` yakni:
	```html
	p=202049603951664548551555274464815496697
	q=285934543893985722871321330457714807993
	```
2. Lakukan pendeskripsian RSA menggunakan __RSA Script dalam bahasa Ruby__ ```RSA.rb```
	```ruby
	#!/usr/bin/env ruby

	require 'openssl'

	# From challenge

	n = 57772961349879658023983283615621490728299498090674385733830087914838280699121 
	
	e = 65537

	c = 36913885366666102438288732953977798352561146298725524881805840497762448828130

	# Factorised by factordb.com

	p = 202049603951664548551555274464815496697

	q = 285934543893985722871321330457714807993

	raise "wrong factors" unless p*q == n

	# Helper methods

	"""

	def extended_gcd(a, b)

	last_remainder, remainder = a.abs, b.abs

	x, last_x, y, last_y = 0, 1, 1, 0

	while remainder != 0

	last_remainder, (quotient, remainder) = remainder, last_remainder.divmod(remainder)

	x, last_x = last_x - quotient*x, x

	y, last_y = last_y - quotient*y, y

	end

	return last_remainder, last_x * (a < 0 ? -1 : 1)

	end

	def invmod(e, et)

	g, x = extended_gcd(e, et)

	if g != 1

	raise 'The maths are broken!'

	end

	x % et

	end

	"""

	def invmod(e, et)

	e.to_bn.mod_inverse(et)

	end

	def powmod(x, e, n)

	x.to_bn.mod_exp(e, n)

	end

	def hex2ascii(text)

	[text].pack('H*')

	end

	# Decrypt

	phi = (p-1)*(q-1)

	d = invmod(e,phi)

	plaintext = powmod(c, d, n)

	plaintext = plaintext.to_s(16)

	plaintext = hex2ascii(plaintext)

	puts plaintext
	```
	![02](https://user-images.githubusercontent.com/49342639/83088915-3cf35880-a0bf-11ea-81ed-fc1ffa2bfa33.PNG)
	<br>Jalankan ```RSA.rb``` <br>
	```html
	ruby RSA.rb
	```
	![03](https://user-images.githubusercontent.com/49342639/83089021-76c45f00-a0bf-11ea-99ac-db01cc723e88.PNG)
	
## Flag
```html
tjctf{BOLm1QMWi3c}
```
ruby..ruby..rubyliciousss boom :sparkler: <br>
![Solved19](https://user-images.githubusercontent.com/49342639/83089186-e5a1b800-a0bf-11ea-8d14-af256e9cacf9.PNG)
