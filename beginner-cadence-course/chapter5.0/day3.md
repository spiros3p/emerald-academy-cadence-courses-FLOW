# Chapter 5 - Day 3

1. What does "force casting" with as! do? Why is it useful in our Collection?

When using force casting with 'as!' we can be sure that this particular @NonFungibleToken.NFT will be our @NFT type, not some other @NonFungibleToken.NFT from other Collection. So for example using force casting in our deposit function we can be sure anyone can only deposit in our Collection only "right" NFTs.

2. What does auth do? When do we use it?

It is used to get authorized reference. So for example if our NFT have some specific fields besides 'id', we should downcast it using 'as!' to be &NFT type, because &NonFungibleToken.NFT only have that 'id' member. But to downcast it, we need to get authorized reference with 'auth'.

3. This last quest will be your most difficult yet. Take this contract:
