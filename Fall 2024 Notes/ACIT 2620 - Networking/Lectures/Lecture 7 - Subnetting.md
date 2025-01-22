### Subnetting Example
Required number of hosts:
50 => 64
40 => 64
30 => 32
20 => 32
20 => 32
10 => 16
10 => 16

total requirement: 256

starting ip - 192.168.56.0/24

1. 192.168.56.0 /26 - 192.168.56.63 (64)
2. 192.168.56.64 /26 - 192.168.56.127 (64)
3. 192.168.56.128 /27 - 192.168.56.159 (32)
4. 192.168.56.160 /27 - 192.168.56.191 (32)
5. 192.168.56.192 /27 - 192.168.56.223 (32)
6. 192.168.56.224 /28 - 192.168.56.239 (16)
7. 192.168.56.240 /28 - 192.168.56.256 (16)

### Supernetting 
- works in the revserse direction in relation to subnetting
- route aggregation: subnets that share the same prefix are aggregated into a larger network
- might want to use supernetting if you have too many subnets and want to combine some when restructuring 
- smaller routing tables 
	- sometimes you can consolidate some of the routes on the table using supernetting 