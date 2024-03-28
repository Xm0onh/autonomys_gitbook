# What can be built with Auto ID

## What can be built with Auto ID?

#### Demonstrating Provenance for AI Generated Content

Auto ID allows honest users who publish content online to demonstrate if and how the content was AI generated, in a manner that anyone can verify, with a high degree of certainty. Lets explore an example with our trusty friends Alice and Bob. Note that most of the below workflow woud be automated in any actual implementation.

Alice routinely publishes content on X (formerly Twitter) and uses OpenAI’s ChatGPT to help write her tweets and generate associated images. Alice has nothing to hide and would be happy to show the world that she produces content with the help of AI, but how can she do this? First, Alice creates her own Auto ID and links to her OpenAI account. OpenAI also creates its own Auto ID and uses it to issue IDs to each AI service it exposes, such as GPT-4 and DALLE. Alice and OpenAI then jointly issue an Auto ID to her own instance of ChatGPT.

Now every time Alice generates content using her GPT she can produce a digital signature of the content that is linked to her Auto ID. She could sign the content directly (or more likely, its cryptographic hash) but then if she changes the content even slightly (as most users do) it will have a completely different hash, and no one will be able to verify its origin. Instead, she can use a different OpenAI service to generate a short embedding, or semantic fingerprint of her content. She can go one step further and transform the embedding into a tiny semantic hash, using a Locality Sensitive Hashing (LSH) technique, which she can then sign using her Auto ID. If she does this for both the original content from her GPT and the modified form that she edits, then these two semantic hashes will be almost exactly the same, as they both refer to text with the same semantic meaning.

Now when Alice publishes here content to X, she can include the content along with a short _proof-of-provenance_ which includes the joint signature of the original semantic hash, and her signature of the final semantic hash. The X platform may then verify this proof, using the public keys for each Auto ID in the registry, and compare the similarity between the two hashes. If they fall within a pre-defined range then X may render a seal, perhaps a green smiling robot icon, that demonstrates this is verified AI content. Now Bob, who follows Alice religously on X, can then inspect the content and see further details such as the AI model, provider, and timestamp for the signature and be assured that it was AI generated and know that Alice honestly volunteered this information. Note that this not only applies to text, but any form of AI content which a semantic embedding may be computed over, including images, audio, and videos.

#### Automated Detection of AI Generated Content

Auto ID also provides a framework that allows anyone to detect the liklihood that a malicous user publishes AI generated content on any platform, in a way that may be linked back to the AI provider, and even the malicous users account.

Let’s now turn to Eve who also uses OpenAI to routinely publish fake news and deepfake photographs on X to spread disinformation. How can Bob, X, and OpenAI know if this is happening? Let’s assume that Eve does not have her own Auto ID, but that OpenAI has decided to create surrogate Auto IDs for every user on its platform, and then jointly issues Auto IDs for each user based on their chosen AI model, similar to the setup above with Alice. This means that every time Eve generates content on OpenAI, they are still producing semantic hashes and signing using the joint Auto ID, internal to their platform. As one additional step, OpenAI adds every semantic hash to a hash table, for fast lookups, and then exposes this over its public API.

Now X decides that it would like to start verifying the provenance of all content uploaded to its platform, which is not already self-provided (as was the case with Alice). Every time a new tweet or image is submitted, X computes the embedding and semantic hash in the same manner as before, but it now checks OpenAI’s provenance API, receiving a response if there are any hashes in their system which lie within a certain similarity range. If X receives a positive match it may then flag the content as likely AI generated, and render a seal, perhaps a red horned robot icon, that alerts honest users like Bob to the fact that is content is likely AI generated.

X may then combine this with its existing internal methods for detecting misinformation and if they determine the content is malicous they can send an automated notification to OpenAI that the user who produced this hash may be spreading misinformation. This can then be evaluated internally by OpenAI, likely in an automated fashion, to flag or even suspend Eve’s account. Note that even if Eve were to modify the output text or post-process the images, semantic hashing technqiues would sitll return a near match with high-probability.

#### Prove that you are a Human Being Online

Auto ID also provides a framework that allows honest humans to prove to existing apps and platforms that they probably actually are a human, and not a bot or AI account. This would allow users to more easily authenticate online by removing cumbersome sign-up and sign-on forms, as well as the increasingly difficult captcha puzzles we have all grown to hate.

Let’s say Alice is an honest human who would simply like to create an account on an a new platform. This platform is inundated with bots and fake users so it employs a series of captchas to deter attackers, but at the cost of exhausting honest users. How can Alice use her Auto ID to signup with a single click? Let’s assume Alice has several existing online identities through well-known identity providers (such as Google or Twitter) who can attest that she has a verified email and phone number, and she has already linked these accounts to her Auto ID. If the new platform implements sign-on with Auto ID, then it may easily see that Alice does have verifiable accounts with many well-known providers.

The more providers Alice has linked to her Auto ID, the more certainty the platform can have that she actually is a human, and even that she actually is Alice, should Alice choose to make her identity public. Alice can even link emerging proof-of-personhood systems to her Auto ID, such as World ID, which leverages biometrics to register unique human identities in a privacy preserving manner. We can even quanity the notion of how likely an Auto ID is to belong to a human as its _human score_, as a probability between 0 and 1. We could say that any Auto ID with a sufficiently high human score is an _implicitly_ _verified_ Auto ID. Any platform that integrates with Auto ID could then require that valid accounts have a certain human score, potentially present if a user holds an Auto verified account on their profile, and even allow the user to expose their actual human identity to promote further trust in their profile.

We now turn to Eve, who runs a bot farm and tries to generate as many fake accounts as possible on platforms for a variety of reasons. Before, platforms could only stop Even with captchas. Now Eve must generate or purchase mutliple verified accounts across several well-known providers to obtain a resonable human score, before she has any chance of even being able to create a new account, significantly raising the cost of sybil attacks.