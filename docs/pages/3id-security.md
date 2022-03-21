# 3ID Security

The 3ID Connect system allows blockchain accounts to be used as an authentication method to the keys used for the users 3ID. In order to authenticate a user signs a message with their wallet. The entropy is used to generate a [Key DID](../docs/advanced/standards/accounts/key-did.md) which in turn is used to decrypt the seed used for the 3ID.

The main security concern here is that the seed used for the 3ID is stored within the 3ID iframe. While this restricts the app using 3ID Connect from doing whatever it wants with the seed, a malicious browser plugin could easily pick up the seed from the web browser and take full control over the users 3ID. While are multiple paths that can be used to mitigate this issue, it's currently considered to be a fair tradeoff to improve UX for users. In contrast to a users crypto wallet, the 3ID doesn't actually hold or control any cryptocurrency; it only controls data read/writes. This means that the incentive for a hacker to execute an attack is much smaller since there is no immediate financial reward. Also note that this security issue is not inherent to 3ID itself, it's just related to how 3ID is used within 3ID connect.

## Possible security improvements

### External 3ID controller

The 3ID DID document has different public keys used for writing and controlling the DID document itself. Currently however, these keys are generated from the same seed and this seed is stored in the iframe as mentioned above. This means that if someone gets access to this seed and rotates keys using the controller key they will own the 3ID forever.

One possible mitigation strategy is to generate the day-to-day public keys and the controller public key from different seeds, and then make sure to store the controller key separately in a more secure manner (not in the iframe). This would make it possible to recover from a scenario where the day-to-day writing public keys are compromised (seed is stolen from the iframe) because the seed of the controller key would still be in control of the user. An example solution here could be to store the seed of the controller key in a MetaMask Snap plugin.

### Don't store keys in web context

The long term solution is of course to not store any keys of the 3ID in the web browser context. This could be achieved though wallets supporting 3ID directly, or an Object-based capability system rooted in blockchain accounts which would allow applications to request specific permissions from wallets.
