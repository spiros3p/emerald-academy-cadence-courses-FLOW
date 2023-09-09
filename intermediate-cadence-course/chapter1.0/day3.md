# Chapter 1 - Day 3

1. Explain the two most common mistakes Cadence developers make regarding poor capability links.

- forgetting an interface
When linking the public cap of a resource, for example NFT collection, forgetting to add any of 
the "standard" interfaces like NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, MetadataViews.ResolverCollection.
This can result in trouble with third party integrations and making a developer's life harder.

- Using &AnyResource
When the reference type is not specified during linking of a capability.
Hence, the base resource type is not linked to the public, and this can result in trouble when trying to borrow the collection
using that capability.
Why? because we do not really check the type of the resource that could be stored in that path, so it could be any resource.

2. Rewrite this script from the NBATopShot official repo to link users' collections properly.

```cadence
import NonFungibleToken from 0xNFTADDRESS
import TopShot from 0xTOPSHOTADDRESS
import MetadataViews from 0xMETADATAVIEWSADDRESS

// This transaction sets up an account to use Top Shot
// by storing an empty moment collection and creating
// a public capability for it

transaction {

    prepare(acct: AuthAccount) {

        // First, check to see if a moment collection already exists
        if acct.borrow<&TopShot.Collection>(from: /storage/MomentCollection) == nil {

            // create a new TopShot Collection
            let collection <- TopShot.createEmptyCollection() as! @TopShot.Collection

            // Put the new Collection in storage
            acct.save(<-collection, to: /storage/MomentCollection)

            // create a public capability for the collection
            acct.link<&TopShot.Collection{NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, TopShot.MomentCollectionPublic, MetadataViews.ResolverCollection}>(/public/MomentCollection, target: /storage/MomentCollection)
        }
    }
}
```

3. Let's say this script fails when trying to read a user's NFTs:
```cadence
import ExampleNFT from 0x01
import NonFungibleToken from 0x02

pub fun main(user: Address): [UInt64] {
  let collection = getAccount(user).getCapability(/public/Collection)
              .borrow<&ExampleNFT.Collection{NonFungibleToken.CollectionPublic}>()
              ?? panic("Your TopShot Collection is not set up correctly.")

  return collection.getIDs()
}
```
...but this script works:
```cadence
import ExampleNFT from 0x01

pub fun main(user: Address): [UInt64] {
  let collection = getAccount(user).getCapability(/public/Collection)
              .borrow<&ExampleNFT.Collection{ExampleNFT.CollectionPublic}>()
              ?? panic("Your TopShot Collection is not set up correctly.")

  return collection.getIDs()
}
```
What do you think the transaction looked like to set up this user's collection?
**ANSWER**
```cadence
import ExampleNFT from 0x01
import NonFungibleToken from 0x02

transaction {
    prepare(acct: AuthAccount) {
      let collection <- ExampleNFT.createEmptyCollection()
      acct.save(<-collection, to: /storage/Collection)
  
      acct.link<&ExampleNFT.Collection{ExampleNFT.CollectionPublic}>(/public/Collection, target: /storage/Collection)
    }
}
```

4. Let's say this script fails when trying to read a user's NFTs:
```cadence
import ExampleNFT from 0x01
import NonFungibleToken from 0x02

pub fun main(user: Address): [UInt64] {
  let collection = getAccount(user).getCapability(/public/Collection)
              .borrow<&ExampleNFT.Collection{NonFungibleToken.CollectionPublic}>()
              ?? panic("Your TopShot Collection is not set up correctly.")

  return collection.getIDs()
}
```
...but this script works:
```cadence
import NonFungibleToken from 0x02

pub fun main(user: Address): [UInt64] {
  let collection = getAccount(user).getCapability(/public/Collection)
              .borrow<&{NonFungibleToken.CollectionPublic}>()
              ?? panic("Your TopShot Collection is not set up correctly.")

  return collection.getIDs()
}
```
What do you think the transaction looked like to set up this user's collection?
**ANSWER**
```cadence
import ExampleNFT from 0x01
import NonFungibleToken from 0x02

transaction {
    prepare(acct: AuthAccount) {
      let collection <- ExampleNFT.createEmptyCollection()
      acct.save(<-collection, to: /storage/Collection)
  
      acct.link<&{NonFungibleToken.CollectionPublic}>(/public/Collection, target: /storage/Collection)
    }
}
```

