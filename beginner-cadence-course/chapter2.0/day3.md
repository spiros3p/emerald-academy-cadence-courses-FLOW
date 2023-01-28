# Chapter 2 - Day 3

1.
<img width="460" alt="image" src="https://user-images.githubusercontent.com/16209859/215290182-f59ffb05-d749-4c47-9446-da4e5ecf05b4.png">

2.
<img width="894" alt="image" src="https://user-images.githubusercontent.com/16209859/215290321-924b9a11-bf05-4116-ac40-92bee2aa0db9.png">

3.
The force-unwrap operator basically "unwraps" an optional type in cadence.
a simple example would be:
`var myNumber: Number? = 6`
`var yourNum: Number = myNumber!`

4.
- the error means that the function is expecting to return a String but it is returning an String? (optional - nil or String)
- because cadence can not be sure that the dictionary contains a "0x03" key, thus it could be nil or String the value
- 2 ways. a) using the force-unwrap operator (line 3. `return thing[0x03]`) b) change function return type to String?  
