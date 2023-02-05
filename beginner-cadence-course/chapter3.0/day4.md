# Chapter 3 - Day 4

1.
Resource interface can be used to:
- allow to only expose specific parts of the resource
- define the requirements that the resource must implement

2.
<img width="557" alt="image" src="https://user-images.githubusercontent.com/16209859/216820906-6a0e3a4f-549e-4c6f-ab1f-e7190d2d1218.png">


3.
add
`pub var favouriteFruit: String`
below
`pub var greeting: String`

add
`self.favouriteFruit = "Mango"`
below
`self.greeting = "Hello!"`

for the last one:
 - either:
 remove the `{ITest}` from `let test: Test{ITest} = Test()`
 - or:
 add `pub fun changeGreeting(newGreeting: String): String` in the interface definition
