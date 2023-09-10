# Chapter 1 - Day 4

1. Explain what a resource identifier is.

A resource identifier consists of the 
contract address that the type was defined + name of the contract + name of the type
and can be used to check the underlying type of a resource.


2. Is it possible for two different resources, that have the same type, to have the same identifier?

Yes it is possible, since each resource is unique, hence each one is different than any other, 
there can be 2 different resources from the same contract of the same type.

3. Is it possible for two different resources, that have a different type, to have the same identifier?

No it is not possible to have the same identifier, as the identifier contains the type of the resource.

4. Based on the included comments:

- What is wrong with the following script?
The capability is not required to be of some certain type, it is of &AnyResource

- Explain in detail how someone hack this script to always return true.
I could create my own contract and my own collection resource in it,
that implements the interface "NonFungibleToken.CollectionPublic" and is linked with a public cap in /public/Collection.
Then mint 6 NFTs from my own contract in this collection.
That would return true

- Then, what are two ways we could fix this script to make sure it is safe?
either add the type of the resource when borrowing it.
or add an assertion to check the resource identifier of the collection, to match the desired type

5. Let's say we have the following script on Mainnet...

```cadence
import FungibleToken from 0xf233dcee88fe0abe

pub fun main(user: Address): UFix64 {
  let vault = getAccount(user).getCapability(/public/Vault)
              .borrow<&{FungibleToken.Receiver}>()
              ?? panic("Your Vault is not set up correctly.")

  assert(
    vault.getType().identifier == "A.1654653399040a61.FlowToken.Vault",
    message: "This is not the correct type. No hacking me today!"
  )
  
  return vault.balance
}
```
