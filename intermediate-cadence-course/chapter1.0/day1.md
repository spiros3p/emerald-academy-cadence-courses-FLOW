# Chapter 1 - Day 1

1. Write a transaction to save a @Record.Collection to the signer's account, making sure to link the appropriate interfaces to the public path.

```cadence
import Record from somewhere

transaction {
    prepare(acct: AuthAccount) {
      acct.save( <- Record.createEmptyCollection(), to: Record.CollectionStoragePath)
      acct.link<&Record.Collection{Record.CollectionPublic}>(Record.CollectionPublicPath, target: Record.CollectionStoragePath) 
    }
}
```

2. Write a transaction to mint some @Record.NFTs to the user's @Record.Collection

```cadence
import Record from 0x01

transaction(recipient: Address, _songName: String) {
    prepare(acct: AuthAccount) {
        let recordNFT <- Record.createRecord(songName: _songName)

        let recCollectionRef = getAccount(recipient)
                .getCapability(Record.CollectionPublicPath)
                .borrow<&Record.Collection{Record.CollectionPublic}>()
                ?? panic("No public cap found for the collection")

        recCollectionRef.deposit(token: <- recordNFT)
    }
}
```

3. Write a script to return an array of all the user's &Record.NFT? in their @Record.Collection

```cadence
import Record from 0x01

pub fun main(address: Address): [&Record.NFT] {
  let publicCollection = getAccount(address)
                    .getCapability(Record.CollectionPublicPath)
                    .borrow<&Record.Collection{Record.CollectionPublic}>()
                    ?? panic("The address does not have a Collection.")
  
  let ids = publicCollection.getIDs()
  let array: [&Record.NFT] = []
  for id in ids {
    array.append( publicCollection.borrowRecordNFT(id: id)! )
  }

  return array
}
```

4. Write a transaction to save a @Artist.Profile to the signer's account, making sure to link it to the public so we can read it

```cadence
import Artist from 0x01
import Record from 0x01

transaction(artistName: String) {
    prepare(acct: AuthAccount) {

        let cap = acct.getCapability<&Record.Collection{Record.CollectionPublic}>(Record.CollectionPublicPath)
        assert(cap.check(), message: "This capability is invalid!")

        acct.save( <- Artist.createProfile(
            name: artistName,
            recordCollection: cap
        ), to: /storage/artist)
        acct.link<&Artist.Profile>(/public/artist, target: /storage/artist) 
    }
}
```

5. Write a script to fetch a user's &Artist.Profile, borrow their recordCollection, and return an array of all the user's &Record.NFT? in their @Record.Collection from the recordCollection

```cadence
import Record from 0x01
import Artist from 0x01

pub fun main(address: Address): [&Record.NFT] {
    let profileRef = getAccount(address)
                    .getCapability(/public/artist)
                    .borrow<&Artist.Profile>()
                    ?? panic("The address does not have a Profile.")
  
    let array: [&Record.NFT] = []
    let ids = profileRef.recordCollection.borrow()!.getIDs()
    for id in ids {
        array.append( profileRef.recordCollection.borrow()!.borrowRecordNFT(id: id)! )
    } 
  return array
}
```

6. Write a transaction to unlink a user's @Record.Collection from the public path

```cadence
import Record from 0x01

transaction {
    prepare(acct: AuthAccount) {
      acct.unlink(Record.CollectionPublicPath)
    }
}
```

7. Explain why the recordCollection inside the user's @Artist.Profile is now invalid

Because the link of the resource stored in the storage path /storage/RecordCollection to the public path 
/public/RecordCollection has been destroyed (unlink).
So now, only the owner of the resource and through a transaction (not a script) has access to the storage path 
/storage/RecordCollection.

8. Write a script that proves why your answer to #7 is true by trying to borrow a user's recordCollection from their &Artist.Profile

```adence
import Record from 0x01
import Artist from 0x01

pub fun main(address: Address): Void {
    let profileRef = getAccount(address)
                .getCapability(/public/artist)
                .borrow<&Artist.Profile>()
                ?? panic("The address does not have a Profile.")

    assert(profileRef.recordCollection.check(), message: "This capability is invalid!")
} 
```
