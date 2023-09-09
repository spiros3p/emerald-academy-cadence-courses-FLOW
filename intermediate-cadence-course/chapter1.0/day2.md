# Chapter 1 - Day 2

1. _In the very last transaction of the Using Private Capabilities section in today's lesson, there was this line:_
```cadence
// Borrow the capability
let minter = &ExampleNFT.NFTMinter = minterCapability.borrow() ?? panic("The capability is no longer valid.")
```
It would panic if the owner of the @NFTMinter has unlinked the private capability 

2. _Explain two reasons why passing around a private capability to a resource you own is different from simply giving users a resource to store in their account._

- You can always revoke (unlink) the private capability, while you do not have access to someone else /storage/ path
- Giving someone the minting resource, in our example, it would require to coordinate 2 signers for 1 transaction, which is not as easy as giving the account a capability in its existing resource.

3. _Write (in words) a scenario where you would use private capabilities. It cannot be the same NFT example we saw today._

We have a contract that settles offers for NFTs. For Buyers, in order to create a valid offer, it would require for them to give a private cap to their token's vault. 
So when a seller accepts the offer, the contract would have access to pull the required amount from the buyer's vault and send it to the seller.

4. _Architect and implement your idea in #3. Show:_

- at least one example of you linking a private capability
```cadence
import FlowToken from 0x01

transaction() {
  prepare(signer: AuthAccount) {
    // Link the Flow Vault to a private path
    signer.link<&FlowToken.Vault{FungibleToken.Provider}>(/private/flowTokenVault, target: /storage/flowTokenVault)
  }
}
```

- at least one example of you storing the private capability inside a resource
```cadence
import Offers from 0x01
import FlowToken from 0x01

transaction(amount: UFix64, itemId: UInt64) {
  // signer - the account with the NFTMinter resource
  prepare(signer: AuthAccount) {
    // Link the Flow Vault to a private path
    signer.link<&FlowToken.Vault{FungibleToken.Provider}>(/private/flowTokenVault, target: /storage/flowTokenVault)

    // Get the private capability 
    let vaultPrivCap: Capability<&FlowToken.Vault{FungibleToken.Provider}> = signer.getCapability<&FlowToken.Vault{FungibleToken.Provider}>(/private/flowTokenVault)

    // create offer and give private cap to the contract
    Offers.createOffer(itemId: itemId, amount: amount, vaultCap: vaultPrivCap, receiver: signer.address)
  }
}
```
```cadence
pub contract ExampleNFT {

  pub let offers: {UInt64: SomeStruct}
  // SomeStruct that stores the receiver address, amount of the offer, and vaultCap
  
  pub fun createOffer(itemId: UInt64, amount: UFix64, vaultCap: Capability<&FlowToken.Vault>, receiver: Address) {
    offers[itemId] = SomeStruct(amount: amount, vaultCap: vaultCap, receiver: receover)
  }

}
```

- an example of you revoking (unlinking) the capability and showing the capability is now invalid
```cadence
transaction() {
  prepare(signer: AuthAccount) {
    signer.unlink(/private/flowTokenVault)
    // or better to cancel some specific offer from the contract
  }
}
```
