# Chapter 5 - Day 1

1. Describe what an event is, and why it might be useful to a client.

An event is a way for a smart contract to communicate with the outside world. It is useful to a client, because the clients gets notified by the emitted event instead of constantly quering the smart contract for changes.

2. Describe what an event is, and why it might be useful to a client.

```cadence
pub contract Test {
  
  pub event ContractDeployed()

  init(){
    emit ContractDeployed()
  }
}
```

3. Using the contract in step 2), add some pre conditions and post conditions to your contract to get used to writing them out.

```cadence
pub contract Test {
  
  pub event ContractDeployed()
  pub event ItemAdded()
  pub event ItemRemoved()

  pub let letters: [String]

  pub fun addLetter(letter: String) {
    pre {
      letter.length == 1 : "only 1 character is allowed"
      ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"].contains(letter) : "Can not be a number"
    }

    self.letters.append(letter)
  }

  pub fun removeLetter(letter: String) {
    post {
      before(self.letters.length) == self.letters.length + 1: "nothing removed"
    }

    let index = self.letters.firstIndex(of: letter) ?? panic("not found")
    self.letters.remove(at: index)
  }

  init(){
    emit ContractDeployed()

    self.letters = []
  }
}
```

4. For each of the functions below (numberOne, numberTwo, numberThree), follow the instructions.

complete
```cadence
pub contract Test {

  // TODO
  // Tell me whether or not this function will log the name.
  // name: 'Jacob'
  // ANSWER: it will log the name
  pub fun numberOne(name: String) {
    pre {
      name.length == 5: "This name is not cool enough."
    }
    log(name)
  }

  // TODO
  // Tell me whether or not this function will return a value.
  // name: 'Jacob'
  // ANSWER: it will return "Jacob Tucker"
  pub fun numberTwo(name: String): String {
    pre {
      name.length >= 0: "You must input a valid name."
    }
    post {
      result == "Jacob Tucker"
    }
    return name.concat(" Tucker")
  }

  pub resource TestResource {
    pub var number: Int

    // TODO
    // Tell me whether or not this function will log the updated number.
    // Also, tell me the value of `self.number` after it's run.
    // ANSWER: there is no log in the function, it will not run, as the post
    // condition will always fail.
    // Also, self.number will always stay 0
    pub fun numberThree(): Int {
      post {
        before(self.number) == result + 1
      }
      self.number = self.number + 1
      return self.number
    }

    init() {
      self.number = 0
    }

  }

}
```
