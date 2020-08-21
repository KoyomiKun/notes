## 2. Relation 

### 2.1 Key

1. Super key: values for K is sufficient to identify a unique tuple of R

2. Candidate key: minimal superkey 

3. Primary Key: user defined candidate key

4. Foreign Key: check K is null or value of the key reference from

### 2.2 Relational Algebra

#### 2.2.1 Basic Operations

***Select*** : 

$\sigma_{p}(r)$ : p is called `selection predicate` 

Ex:$\sigma_{A=\beta }(r)$

***Project*** :

$\Pi_{A, C}(r)$ : Projct A, C from table with tuple(A, B, C)

***Union*** :

$r \cup s$ : 

1. r, s must have the same arity
2. The attribute domains must be compatible

***Set difference*** 

$r-s=\{t|t\in r \ and\  t\notin s\}$ :elements in r but not in s

***Cartesian-Product*** :

$r\times s = \{{t q}| t \in r \ and \ q \in s\}$ 

***Rename*** :

$\rho_{X(A1, A2, ...)}(E)$ 

#### 2.2.2 Additional Operation

***Set Intersection*** :

$r \cap s =\{t| t\in r \ and \ t \in s  \}$ 

$r\cap s = r-(r-s)$ 




