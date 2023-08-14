# Chapter 4 - Day 4

1. Take our NFT contract so far and add comments to every single resource or function explaining what it's doing in your own words.
2. 

```cadence
pub contract CryptoPoops {
    pub var totalSupply: UInt64

    // this is a resource called NFT that represents an NFT and has some attributes that can be defined when created:
    // the id and name.
    // NFTs are stored inside the collection resource of an account
    pub resource NFT {
        pub let id: UInt64

        pub let name: String

        init(_name: String) {
            self.id = CryptoPoops.totalSupply
            self.name = _name
            CryptoPoops.totalSupply = CryptoPoops.totalSupply + 1
        }
    }

    // this is a resource interface that is used to expose the defined functions to the public 
    // for the resource name Collection
    pub resource interface CollectionPublic {
        pub fun deposit(token: @NFT)
        pub fun getIDs(): [UInt64]
        pub fun borrowNFT(id: UInt64): &NFT
    }

    // this is a resource named Collection and is saved in the account's storage 
    // it is used as the place to store the NFTs created by this smart contract
    pub resource Collection: CollectionPublic {

        pub var ownedNFTs: @{UInt64: NFT}

        // this function can be called by the public cap by anyone and is used to deposit an NFT resource
        // in this Collection resource
        pub fun deposit(token: @NFT){
            self.ownedNFTs[token.id] <-! token
        }

        // this function is only used by the Collection's owner and is used to move an NFT out of the Collection
        pub fun withdraw(id: UInt64): @NFT {
            let token <- self.ownedNFTs.remove(key: id) 
                    ?? panic("Collection does not contain NFT with that id!")
            return <- token
        }
     
        // public function that returns a reference of an NFT from this Collection
        pub fun borrowNFT(id: UInt64): &NFT {
            return (&self.ownedNFTs[id] as &NFT?)!
        }

        // public function that returns all NFT ids from this Collection
        pub fun getIDs(): [UInt64] {
            return self.ownedNFTs.keys
        }

        init() {
            self.ownedNFTs <- {}
        }

        // manually destroy nested resoruces when Collection resource is destroyed
        destroy(){
            destroy self.ownedNFTs
        }
    }

    // public function, can be called by anyone, returns a Collection resource
    pub fun createEmptyCollection(): @Collection{
        return <- create Collection()
    }

    // this resource is the only entity that can create a new NFT resource
    pub resource Minter {

        // the function that creates a NFT resource
        pub fun createNft(name: String): @NFT {
            return <- create NFT(_name: name)
        }
    }

    init(){
        self.totalSupply = 0
        self.account.save(<- create Minter(), to: /storage/Minter)
    }
}
```
