# Chapter 3 - Day 2

1.
```
pub contract Profiles {
  pub var id: UInt16;
  pub let arrayOfProfiles: @[Profile]
  pub var dictOfProfiles: @{UInt16: Profile}`

  pub resource Profile {
      pub let name: String
      pub let id: UInt16
      init(_name: String) {
          self.name = _name
          self.id = Profiles.id
          Profiles.id = Profiles.id + 1
      }
  }

  pub fun createIt(name: String): @Profile {
    return <- create Profile(_name: name)
  }

  //STRUCTS
  pub fun addToDict(rsc: @Profile) {
    let key: UInt16 = rsc.id
    let oldOne <- self.dictOfProfiles[key] <- rsc
    destroy oldOne
  }
  pub fun removeFromDict(id: UInt16): @Profile {
    let rsc: @Profile <- self.dictOfProfiles.remove(key: id) ?? panic("oops! not here ")
    return <- rsc
  }

  // ARRAYS
  pub fun addToArray(profile: @Profile) {
      self.arrayOfProfiles.append(<- profile)
  }
  pub fun removeFromArray(index: Int): @Profile {
    return <- self.arrayOfProfiles.remove(at: index)
  }

  init(){
    self.id = 0
    self.arrayOfProfiles <- []
    self.dictOfProfiles <- {}
  }
}
```
