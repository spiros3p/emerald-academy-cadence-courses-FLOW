# Chapter 4 - Day 1


1. Explain what lives inside of an account.

account storage and contracts

2. What is the difference between the /storage/, /public/, and /private/ paths?

- storage: accessed by the owner (AuthAccount)
- public: accessed by everyone
- private: accessed by the owner and whoever has been given access

3. What does .save() do? What does .load() do? What does .borrow() do?

- .save(): moves resource into specified path of the accounts's storage
- .load(): moves resource out of -//- -//- (same above)
- .borrow(): gets (fetches) a reference of a resource in the specified path of someones's account

4. Explain why we couldn't save something to our account storage inside of a script.

scripts dont mutate chain state
saving something into account storage is chain state mutation

5. Explain why I couldn't save something to your account.

because that something is stored in /storage/ and only owner has access to storage


6. Define a contract that returns a resource that has at least 1 field in it. Then, write 2 transactions:

(dont mind the profile thing with address, was trying to implement your intermediate chap 4 day 1, I think)

i. A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it.

<img width="688" alt="image" src="https://user-images.githubusercontent.com/16209859/218279357-6b56e51d-f801-44b7-8fa0-2abeee60b479.png">


ii. A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.

<img width="677" alt="Screenshot 2023-02-11 at 9 11 11 PM" src="https://user-images.githubusercontent.com/16209859/218279216-f294ba62-661d-49ed-b619-d991fe134fb0.png">
