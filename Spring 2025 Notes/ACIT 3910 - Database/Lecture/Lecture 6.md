- InnoDB better than MyISAM
	- new default
	- may only run into MyISAM in legacy databases and tables
![[Pasted image 20250213135753.png]]

- innoDB has row locking
- MyISAM has table locking 

### ACID Transactions
- Atomic
	- indivisible - all or nothing
- Consistent
	- always valid - obeys constraints
- Isolated
	- no interference
- Durable
	- robust - written in stone 