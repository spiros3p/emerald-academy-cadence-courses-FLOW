# Chapter 5 - Day 3

1. What does "force casting" with as! do? Why is it useful in our Collection?

When using force casting with 'as!' we can be sure that this particular @NonFungibleToken.NFT will be our @NFT type, not some other @NonFungibleToken.NFT from other Collection. So for example using force casting in our deposit function we can be sure anyone can only deposit in our Collection only "right" NFTs.

2. What does auth do? When do we use it?

It is used to get authorized reference. So for example if our NFT have some specific fields besides 'id', we should downcast it using 'as!' to be &NFT type, because &NonFungibleToken.NFT only have that 'id' member. But to downcast it, we need to get authorized reference with 'auth'.

3. This last quest will be your most difficult yet. Take this contract:

Smart Contract: just the changes
```cadence
import NonFungibleToken from 0x02

pub contract CryptoPoops: NonFungibleToken {
// other stuff

    pub resource interface CollectionPublicAuth {
        pub fun borrowAuthNFT(id: UInt64): &NFT
    }

// other stuff

    pub resource Collection: CollectionPublicAuth, NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic {

// other stuff

        pub fun borrowAuthNFT(id: UInt64): &NFT {
            let ref = (&self.ownedNFTs[id] as auth &NonFungibleToken.NFT?)!
            return ref as! &NFT
        }
// other stuff
    }

}
// other stuff
// other stuff
// other stuff
```

Transaction: create empty collection
```cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

transaction {
    prepare(acct: AuthAccount) {
        acct.save( <- CryptoPoops.createEmptyCollection(), to: /storage/MyCollection)
        acct.link<&CryptoPoops.Collection{NonFungibleToken.CollectionPublic, CryptoPoops.CollectionPublicAuth}>(/public/MyCollection, target: /storage/MyCollection)
    }
    execute {
        log("Created a new collection")
    }
}
```

Transaction: mint NFT 
```cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

transaction(recipient: Address, _name: String) {

  // Let's assume the `signer` was the one who deployed the contract, since only they have the `Minter` resource
  prepare(signer: AuthAccount) {
    // Get a reference to the `Minter`
    let minter = signer.borrow<&CryptoPoops.Minter>(from: /storage/Minter)
                    ?? panic("This signer is not the one who deployed the contract.")

    // Get a reference to the `recipient`s public Collection
    let recipientsCollection = getAccount(recipient).getCapability(/public/MyCollection)
                                  .borrow<&CryptoPoops.Collection{NonFungibleToken.CollectionPublic}>()
                                  ?? panic("The recipient does not have a Collection.")

    // mint the NFT using the reference to the `Minter`
    let nft <- minter.createNft(name: _name)

    // deposit the NFT in the recipient's Collection
    recipientsCollection.deposit(token: <- nft)
  }

  execute{
    log("NFT MINTED")
  }
}
```


Script: read NFT data
```cadence
import CryptoPoops from 0x01

pub fun main(address: Address, nftId: UInt64): &CryptoPoops.NFT {
  let publicCollection = getAccount(address).getCapability(/public/MyCollection)
              .borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublicAuth}>()
              ?? panic("The address does not have a Collection.")
  
  let refNft = publicCollection.borrowAuthNFT(id: nftId)
  log(refNft.name)

  return refNft
}
```

SCREENSHOTS

<img width="697" alt="image" src="https://github.com/spiros3p/emerald-academy-cadence-courses-FLOW/assets/16209859/6c895da2-2a4d-45c8-a1d1-341718f92868">


<img width="624" alt="image" src="https://github.com/spiros3p/emerald-academy-cadence-courses-FLOW/assets/16209859/3e58e77a-1b28-406a-b643-c93c1053a171">
