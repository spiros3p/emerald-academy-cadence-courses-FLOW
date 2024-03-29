# Chapter 4 - Day 2

1. What does .link() do?

.link() links a resource to /public or /private path

2. In your own words (no code), explain how we can use resource interfaces to only expose certain things to the /public/ path.

after we create a resource that has a certain resource interface 
we also link that specific interface to the public path

3. Deploy a contract that contains a resource that implements a resource interface. Then, do the following:


i. In a transaction, save the resource to storage and link it to the public with the restrictive interface.

<img width="581" alt="image" src="https://user-images.githubusercontent.com/16209859/218349335-4846c58b-2e2b-4e9d-9969-0e86a5206850.png">


ii. Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.

<img width="935" alt="image" src="https://user-images.githubusercontent.com/16209859/218349319-0b67ece4-0b18-4aa9-b521-ea065a9cae34.png">


iii. Run the script and access something you CAN read from. Return it from the script.

<img width="915" alt="image" src="https://user-images.githubusercontent.com/16209859/218349464-b13dd9a4-e972-4c20-bcfc-4826b4b13388.png">
