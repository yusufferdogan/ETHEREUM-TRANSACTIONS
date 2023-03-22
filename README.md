![image](https://user-images.githubusercontent.com/45846424/227036749-a12d3f9c-e85b-4331-bb6f-9d71493402f4.png)

# ETHEREUM TRANSACTIONS

An Ethereum transaction is a message sent from one Ethereum account to another Ethereum account that includes instructions to execute a specific action, such as transferring Ether or invoking a smart contract function. Transactions must be signed by the account owner using their private key and are broadcast to the Ethereum network for validation and inclusion in the blockchain. Once a transaction is confirmed and added to a block in the blockchain, it becomes immutable and permanent.

Transactions are signed messages originated by an externally owned account, transmitted by the Ethereum network, and recorded on the Ethereum blockchain. **Contracts don’t run on their own. Ethereum doesn’t run autonomously. Everything starts with a transaction.**

## The Structure of a Transaction

An Ethereum transaction consists of several components that together form its structure:

1. Nonce: A unique value used to prevent replay attacks and to ensure that the transaction is only processed once.
2. Gas price: The amount of Ether the sender is willing to pay for each unit of gas used to execute the transaction.
3. Gas limit: The maximum amount of gas that can be used to execute the transaction.
4. To address: The address of the account or smart contract that will receive the transaction.
5. Value: The amount of Ether or other tokens being transferred in the transaction.
6. Data: Optional additional data or instructions to be executed as part of the transaction, typically used when invoking a smart contract function.
7. Signature (v,r,s): The digital signature created by the sender using their private key to prove ownership of the account and authorize the transaction.
    
    The transaction message’s structure is serialized using the Recursive Length Prefix (RLP) encoding scheme, which was created specifically for simple, byte-perfect data serialization in Ethereum. All numbers in Ethereum are encoded as big-endian integers, of lengths that are multiples of 8 bits.
    

you may notice there is no “from” data in the address identifying the originator EOA. the "from" address is derived from the signature used to sign the transaction, therefore, implicit in the transaction structure rather than being included as a separate field.

## The Transaction Nonce

A nonce is a unique value that is included in each transaction, and it is incremented by 1 each time a new transaction is created from the same account. The nonce ensures that each transaction is unique and can only be executed once. If two transactions with the same nonce are submitted, only the first one will be processed, and the second one will be rejected as a duplicate.

Replay attacks are a type of attack where an attacker resubmits a previously valid transaction to the network in an attempt to execute the same action multiple times. By including a nonce in each transaction, the Ethereum network can differentiate between new transactions and repeated transactions and prevent replay attacks from occurring.

In addition, the nonce also helps ensure that transactions are processed in the correct order. Since the Ethereum network processes transactions in order based on their nonce value, including a nonce in each transaction helps ensure that they are executed in the correct sequence.

### Why Transaction Nonce is important

1. Prevents Replay Attacks:

Let's say Alice wants to transfer 1 ETH to Bob. She creates a transaction with a nonce of 1 and broadcasts it to the network. The transaction is confirmed and added to the blockchain. Later, an attacker intercepts the transaction and tries to replay it to transfer the same 1 ETH to another account. However, since the attacker uses the same nonce (1), the network recognizes it as a duplicate transaction and rejects it, preventing the attacker from stealing Alice's funds.

1. Ensures Transaction Order:

Let's say Alice wants to execute two transactions in a specific order. She first creates a transaction with a nonce of 1 to transfer 1 ETH to Bob. She then creates a second transaction with a nonce of 2 to execute a smart contract function. If she accidentally submits the second transaction before the first one, the network will reject it because the nonce (2) is higher than the next expected nonce (1). This ensures that the first transaction is executed before the second one, as intended by Alice.

Because nonce value is included in transaction data, every single transaction becomes unique.

### Why the use of nonce is vital for an account-based protocol in contrast to the “Unspent Transaction Output” (UTXO) mechanism of the Bitcoin protocol?

UTXO stands for Unspent Transaction Output and refers to the output of a transaction that has not yet been used as an input for another transaction. In other words, UTXO is a record of how much cryptocurrency is owned by a particular address on the blockchain.

In a blockchain system, when a transaction is made, the output of that transaction becomes a new UTXO that can be used as an input for a future transaction. The UTXO model is used in Bitcoin and other blockchain-based cryptocurrencies to keep track of ownership of digital assets.

When a user wants to spend their cryptocurrency, they need to create a new transaction that references one or more UTXOs that they own as inputs. The output of this transaction then becomes a new UTXO that can be spent in the future.

In an account-based protocol like Ethereum, keeping track of a nonce for each account is important to prevent replay attacks. A nonce is a number that is incremented each time a transaction is sent from an account. The nonce ensures that each transaction is unique and can only be processed once by the network. This prevents attackers from replaying a transaction multiple times to drain an account of its funds.

In contrast, the UTXO mechanism used in Bitcoin does not require the use of nonces, since each UTXO is uniquely identified by its transaction hash and index. When a UTXO is spent, it is consumed as an input to a new transaction, and a new set of UTXOs is created as outputs. Since each UTXO can only be spent once, there is no need for a nonce to prevent replay attacks.

However, in an account-based protocol like Ethereum, the state of each account can be updated by multiple transactions, and the order in which these transactions are processed can affect the final state of the account. Therefore, a nonce is used to ensure that transactions are processed in the correct order and prevent attackers from manipulating the state of an account.

Overall, the use of nonces in an account-based protocol like Ethereum helps to ensure the integrity and security of the network, while the UTXO mechanism used in Bitcoin provides a simpler and more efficient way to process transactions.

### Let’s Check Nonce on Ethers.js

Go to the ethers playground.

![image](https://user-images.githubusercontent.com/45846424/227037049-07a8b8f5-14e0-44d3-89a4-58b901492208.png)

`apikey = "YOUR_API_KEY"`

Enter your alchemy API key.

`provider = new ethers.providers.AlchemyProvider("goerli",apikey);`

Paste This and press enter. Now you have a provider object and we can connect to the network.
In this example, we will use Alchemy but you can use others (infura, QuickNote, etc ..)

`provider.getTransactionCount("YOUR_ETHEREUM_ADDRESS")`

Enter your address as a parameter to this method and press enter. And check Output.

![image](https://user-images.githubusercontent.com/45846424/227037201-0a8a7a12-08f9-4545-be44-ebf4f70f00a3.png)

As you see it was 26 after creating a transaction it increased. This is how to check nonce on ethers.js

### Gap Between Transactions

If there is a gap between two transactions, meaning that you have skipped a nonce value in your transaction sequence, then the blockchain network will not process the second transaction until the nonce gap is filled.

For example, if your previous transaction had a nonce value of 5, and you then submit a transaction with a nonce value of 7, the network will not process the second transaction until you submit a transaction with a nonce value of 6. This is because the network expects transactions to be submitted in sequence, with each transaction having a unique nonce value that is incremented by one from the previous transaction.

In this scenario, your second transaction will remain in a pending state until the gap is filled, and you will not be able to make any further transactions with the same account until the gap is resolved. Once you submit a transaction with the missing nonce value, the network will process the transaction with the higher nonce value (in this case, the second transaction).

It's important to keep track of your nonce values when submitting transactions to the network to avoid gaps and ensure that your transactions are processed in a timely manner.

### Duplicating Transaction with the same nonce

It will cause a conflict in the blockchain network, and only one of the transactions will be accepted by the network.

The transaction with the lower gas price will be rejected by the network, and the transaction with the higher gas price will be processed. If both transactions have the same gas price, then the transaction that is received by the network first will be processed, and the other one will be rejected.

### Transaction Replacement

Transaction Replacement is a feature in the Ethereum blockchain that allows users to replace a pending transaction with a new transaction that has the same nonce but a higher gas price. This is also sometimes referred to as *transaction bumping*.

When a user submits a transaction to the Ethereum network, If is not confirmed by the network quickly enough, the user may want to speed up the transaction by resubmitting it with a higher gas price. In this case, the user can create a new transaction with the same nonce as the original transaction, but with a higher gas price.

The Ethereum network will then see the new transaction as a replacement for the original transaction, and will only process the transaction with the highest gas price. This means that if the replacement transaction has a higher gas price, it is more likely to be included in the next block and confirmed by the network quickly.

Transaction Replacement can be a useful feature for users who need to adjust the gas price or other parameters of a transaction after it has been submitted, or when they want to speed up the confirmation time of a pending transaction. However, not all wallets and applications support transaction replacement, and it is important to understand how it works before attempting to use it.

### Transaction Gas

The Ethereum network uses gas fees to discourage network abuse and ensure that the network is not overwhelmed by resource-intensive computations. By requiring fees for all programmable computation on the network, Ethereum incentivizes users to create efficiently and well-optimized smart contracts and transactions. This helps to ensure that the network can continue to scale and remain secure over time. The concept of Turing completeness refers to the fact that the Ethereum Virtual Machine (EVM) is a complete computational system that can theoretically execute any algorithm or computation that can be expressed in code. However, because such computations can be resource-intensive, Ethereum requires fees to prevent abuse and ensure that the network can operate effectively.

Every transaction has a specific amount of gas associated with it: **gasLimit**. This is the amount of gas that is implicitly purchased from the sender’s account balance.

The purchase happens at the according gasPrice, also specified in the transaction. The transaction is considered invalid if the account balance cannot support such a purchase. It is named **gasLimit** since any unused gas at the end of the transaction is refunded (at the same rate of purchase) to the sender’s account.

Gas is only used to execute a transaction or a contract on the Ethereum network. After the transaction or contract execution is complete, any unused gas is refunded to the sender. Therefore, gas is not a permanent storage unit or asset on the Ethereum network, and it does not exist outside of the context of transaction execution.

For accounts that have trusted code associated with them, such as contracts that have been audited and verified, it may be possible to set a relatively high gas limit and leave it unchanged. This is because the trusted code has already been tested and optimized to work efficiently within the gas limit specified, so there is no need to adjust the gas limit for every transaction.

However, for accounts that have untested or unverified code associated with them, it is important to set a reasonable gas limit for each transaction to prevent the code from consuming too many network resources and causing the transaction to fail or become too expensive. Setting a reasonable gas limit ensures that the transaction can be executed successfully without causing problems for the network or other users.

### Transaction Recipient

The recipient of a transaction is specified in the field. This contains a 20-byte Ethereum address. The address can be an EOA or a contract address.

Ethereum does no further validation of this field. Any 20-byte value is considered
valid. If the 20-byte value corresponds to an address without a corresponding private
key, or without a corresponding contract, the transaction is still valid. Ethereum has
no way of knowing whether an address was correctly derived from a public key (and
therefore from a private key) in existence.

Sending a transaction to the wrong address will probably burn the ether sent, render‐ ing it forever inaccessible (unspendable) since most addresses do not have a known private key, and therefore no signature can be generated to spend it. It is assumed that validation of the address happens at the user interface level.

### Transaction Value and Data

In Ethereum, transactions are the fundamental unit of interaction with the network. When you send a transaction, you are asking the network to execute a certain piece of code or smart contract. A transaction can include two main components: the value and the data.

- Value: This refers to the amount of Ether (ETH) being transferred in the transaction. A transaction with only value and no data is considered a payment. It is simply a transfer of Ether from one account to another.
- Data: This refers to the payload of the transaction, which includes the code that the network will execute when the transaction is processed. A transaction with only data and no value is an invocation. This means that the transaction is simply calling a smart contract function or executing a piece of code on the Ethereum network.
- Both value and data: A transaction with both value and data is considered both a payment and an invocation. This means that it is transferring Ether while also executing some code on the network.
- Neither value nor data: It is possible to send a transaction with neither value nor data. This type of transaction is usually just used to initiate some sort of event on the network or to signal some sort of state change. However, it doesn't actually transfer any Ether or execute any code, so it's considered a "waste of gas". Gas is the unit of measurement for the cost of executing transactions on the Ethereum network.

In summary, the type of transaction depends on whether it includes value, data, or both. A transaction with only value is a payment, a transaction with only data is an invocation, and a transaction with both value and data is both a payment and an invocation.

### Transmitting Value to EOAs and Contracts

If you send value(ether) alongside with transaction it is the equivalent of a *payment***.** Such transactions behave differently depending on whether the ****destination address is a contract or not.

If an account is not flagged as a contract in the blockchain simply its balance is increased by the value sent with the transaction.

if the recipient is a contract account evm will try to call a function named in the data payload of your transaction. The first 4 bytes of the data field indicate the function selector of the function. If the *data* field is empty then the contract’s receive function is executed if it does not exist **payable fallback** function must exist. If neither of them exists transaction will be reverted.

### Transmitting a Data Payload to an EOA.

If you send data to an EOA, the data will be recorded on the blockchain along with the transaction, but it will not have any impact on the EOA's balance or trigger any actions on the blockchain. The data will simply be stored as part of the transaction's metadata.

Overall, sending data to an EOA is generally not useful because EOAs are not associated with any smart contract code that can process the data. Sending data is typically only useful when sending transactions to smart contract accounts, where the data can be used to trigger specific contract functions or update the contract's state.

### Let’s try sending the data alongside with transaction using the metamask wallet.

Firstly go to Settings > Advanced > Show Hex Data to turn on sending Hex Data

![image](https://user-images.githubusercontent.com/45846424/227037305-2a66f9b7-e9ba-46ef-9e39-7def4ee7294e.png)

Now you will see the hex data field while sending transactions. You can put any data in hexadecimal format. Let’s Send human-readable data to an EOA.

Go to the

Enter your message and press enter to convert the string to hex.

![image](https://user-images.githubusercontent.com/45846424/227037461-c9a86f27-1e20-4b2e-92c2-adc2ea890f4f.png)

Copy the encoded hex number and paste it to the hex data field in the metamask. But don’t forget to add `0x` to the beginning. It indicates it is in hex format.

![image](https://user-images.githubusercontent.com/45846424/227037681-018b2f68-5f1f-4414-a17e-636a159b4fdf.png)

After that, you submit the transaction you can decode this message in the etherscan. Just view the input as UTF-8 and now the message will be decoded.

![image](https://user-images.githubusercontent.com/45846424/227037745-f200b719-08de-4dc7-a9c0-dcf12785f5f5.png)

### Transmitting a Data Payload to a Contract

Let’s assume your transaction is delivering data to a contract address. In that case, the data will be interpreted by the EVM as a contract invocation. Most contracts use this data more specifically as a function invocation, calling the named function and passing any encoded arguments to the function.

The data payload sent to an ABI-compatible contract is a hex-serialized encoding of:

***A function selector:*** The first 4 bytes of the Keccak-256 hash of the function’s prototype. This allows the contract to unambiguously identify which function you wish to invoke.

***The function arguments*:** The function’s arguments are encoded according to the rules for the various elementary types defined in the ABI specification.

`//example function
function sumArray(uint256[] memory numbers) public returns(uint256);`

The Signature of a function is defined as the string containing the name of the function, followed by the data types of each of its arguments, enclosed in parentheses and separated by commas. The function name here is sumArr and it takes a single argument that is a uint256 array so the prototype of withdraw would be:

`sumArray(uint256[])`

Let’s calculate the Keccak-256 hash of this string in ethers.js:

`utils.keccak256(utils.toUtf8Bytes("sumArray(uint256[])"))
0x1e2aea0647440989b12156a7890618f4164f4d9df7ff97b5a87ca7d783a0d76f`

`0x1e2aea06` is the selector of the function.

Next, let’s calculate a value to pass as the argument numbers.

Go to the

`abi = ["function sumArray(uint256[] memory numbers) public returns(uint256)"]
iface = new utils.Interface(abi)
iface.encodeFunctionData("sumArray",[[1,2,3,4,5,6,7]])`

when you follow these steps output will be :

`0x1e2aea06000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000070000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000030000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000500000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000007`

Now if you send this data payload to a contract that has this sumArray function you will have interacted with the function.

### Special Transaction: Contract Creation

A contract creation transaction is a special type of transaction in the Ethereum blockchain that is used to deploy a new smart contract onto the network. When a contract creation transaction is executed, it contains the bytecode of the smart contract and the transaction is sent to the network. The nodes on the network then execute the bytecode to create a new contract instance on the blockchain.

The contract creation transaction includes the following:

- Gas Limit: The maximum amount of gas that the creator is willing to pay to deploy the contract.
- Gas Price: The price per unit of gas that the creator is willing to pay to deploy the contract.
- Nonce: A unique number that identifies the transaction.
- **To Address**: This field is **left blank**, as the transaction is creating a new contract instance.
- Value: This field is also left blank, as the transaction is not sending any Ether to another address.
- **Data**: The **bytecode** of the smart contract that will be executed by the nodes on the network to create the new contract instance.

Once the contract creation transaction is broadcasted to the network and confirmed, the smart contract is deployed on the Ethereum blockchain and a new contract address is created. This address can be used to interact with the contract by sending transactions to it, such as calling its functions or sending it Ether.

Create hardhat project via `npx hardhat` and enter the console via `yarn run hardhat console`

`// get signers
[owner, ...addresses] = await ethers.getSigners();
// create a simple contract and create its factory
factory = await ethers.getContractFactory("Contract");
// get its bytecode
bytecode = factory.bytecode
// create transaction 
await owner.sendTransaction({data: bytecode})`

The output will be like this.

![image](https://user-images.githubusercontent.com/45846424/227037830-55088caf-fda4-4cd4-ba29-199f81d3482b.png)

Also created contracts address is shown in creates.

### Conclusions

Transactions are the starting point of every activity in the Ethereum system. Transactions are the “inputs” that cause the Ethereum Virtual Machine to evaluate contracts, update balances, and more generally modify the state of the Ethereum blockchain. Next, we will work with smart contracts in a lot more detail and learn how to pro‐ gram in the Solidity contract-oriented language.

### References

- Mastering Ethereum Book
- [Ethereum Yellow Paper](https://ethereum.github.io/yellowpaper/paper.pdf)
- [Solidity Language Docs](https://docs.soliditylang.org/en/v0.8.19/index.html)

### Contact with me

- [Twitter](https://twitter.com/YusufEthDev)
- [LinkedIn](https://www.linkedin.com/in/yusuf--erdogan/)
