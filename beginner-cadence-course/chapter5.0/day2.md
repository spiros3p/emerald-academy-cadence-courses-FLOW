# Chapter 5 - Day 2

1. Explain why standards can be beneficial to the Flow ecosystem.


2. What is YOUR favourite food?
any recipe with tortellini

4. Please fix this code (Hint: There are two things wrong):

FIXED
```
//FIX
import ITest from someAccount
//

pub contract Test {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
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
