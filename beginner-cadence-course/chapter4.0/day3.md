# Chapter 4 - Day 3

1. Why did we add a Collection to this contract? List the two main reasons.

- because an account can store only one resource at a certain path, hence, we store a collection there,
and use the collection to store the nfts in it.
- we can use interfaces on a collection to manage access to the nested resources however we want

2. What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")

- you must declare a Destroy function to destroy the nested resources manualy

3. Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.

- we need to create new resource, that will be more like a role for the wallet that owns it, like an admin, 
that will be the only one able to mint/create NFTs
- create a public interface for the NFT to borrow the reference instead of the actual resource in order to read its information
