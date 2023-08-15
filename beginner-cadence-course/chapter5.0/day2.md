# Chapter 5 - Day 2

1. Explain why standards can be beneficial to the Flow ecosystem.

Standards are beneficial for a smart contract because they act as 'Rules'.
Different contracts with similar case, like an NFT one, can follow some standard rules, so that each contract can be easily consumed by other Dapps with no extra effort for each different NFT contract

2. What is YOUR favourite food?   


any recipe with tortellini

4. Please fix this code (Hint: There are two things wrong):

FIXED
```
//FIX
import ITest from someAccount
//

//FIX
pub contract Test: ITest {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    // this is a logical error, we could change it to
    // self.number = newNumber
    self.number = 5
  }

  pub resource interface IStuff {
    pub var favouriteActivity: String
  }

  //FIX
  pub resource Stuff: ITest.IStuff {
    pub var favouriteActivity: String

    init() {
      self.favouriteActivity = "Playing League of Legends."
    }
  }

  init() {
    self.number = 0
  }
}
```
