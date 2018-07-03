# BitcoinCase_-
Bitcoin Development
Fist we have to make a Bitcon block. It consists with title and transaction parts.

What we have to have in TITLE:

  HASH - SHA-256 title hash. It's a random value, calculating time can be predicted. Hash calculates without transaction hash.
  VER - version of block sheme.
  PREV-BLOCK - hash of previous block in chain. Depends on previous block hash.
  MRKL_ROOT - Mercle root - list of transactions hashes. First calculate hash of transaction, next is hash of block.
  TIME(uint32_t) - time of block creation. Maximum is Year 2106(if it is Bitcoin). Our time can be different a could be more less than number of Bitcoins.
  BITS - short form of target block hash. We can count number of created block in some time and compare with generated block hash. If it's value less than target hash - our block we can consider 'Validate'.
  NONCE - it's a count that incremets every time when has was calculated. Has of previous blok could be updated when someone will generate a new block. In our system it will be actual, because if we will think that our token will be attach with information that user give us(specifications about car and other values), many users will be able to create a new blocks at short period of time.
  N_TX - nubers of transaction in block.
  SIZE - byte size of block.
  
 
 What we have to have in TRANSACTION:
 
 
  HASH - hash of full transaction. Hashing twice. First, when we calculate Transaction hash. Second, when we calculate block hash. Each block refers to previous block has, each transaction refers to previous transaction hash.
  VER - version of transaction.
  VIN_SZ - number of previous transaction, out of which refers to new addresses where our Token was spent. One or more.
  VOUT_SZ - number of addresses where we send our Token. One or more.
  LOCK_TIME - a value, that can help us to put our transaction to any other block that will be generated after.
  SIZE - byte transaction size.
  IN - list of transactions that refers to our transaction:
    {
       HASH - hash of previous transaction.
       N - number of exit from where we take token.
       SCRIPTSIG - public key of person that have new transaction, because previous transaction refers to our new transaction, and ECDSA signature that was made with person's private key.
    }
   OUT - List of addresses where we refer. For whom we are going to send Token.
    {
      VALUE - count of Token that we are going to send. To not to loose Tokens we have to write value of outgoing tokens and value of residue.
      SCRIPTPUBKEY - Specific script language in Bitcoin and hash code of recipient's public key and ECDSA signature that was made with recipient's private key.
    }
  
First we create structure with that fields that includes main infromation about block and transactions.
The next step is to create a Wallet with out tokens inside.

Wallet:

  The simplest wallet is a program which performs all three functions: it generates private keys, derives the corresponding public keys, helps distribute those public keys as necessary, monitors for outputs spent to those public keys, creates and signs transactions spending those outputs, and broadcasts the signed transactions.
  [Create Parens Private Key] -> [Derive Parent Public Keys] -> [Derive Child Public Keys] -> [Distribute Public Keys] -> [Monitor For Outputs] -> [Create Unsigned Txes] -> [Sign Txes] -> [Broadcast Txes].
  Parent Private and Public keys create child private and public keys. Bitcoin wallets at their core are a collection of private keys. We can make the same.
  Private keys are what are used to unlock satoshis from a particular address. In Bitcoin, a private key in standard format is simply a 256-bit number, between the values:

0x01 and 0xFFFF FFFF FFFF FFFF FFFF FFFF FFFF FFFE BAAE DCE6 AF48 A03B BFD2 5E8C D036 4140, representing nearly the entire range of 2256-1 values. The range is governed by the secp256k1 ECDSA encryption standard used by Bitcoin. We can mae the same.

  In order to make copying of private keys less prone to error, Wallet Import Format may be utilized. WIF uses base58Check encoding on an private key, greatly decreasing the chance of copying error, much like standard Bitcoin addresses.

    Take a private key.

    Add a 0x80 byte in front of it for our network addresses or 0xef for other net addresses.

    Append a 0x01 byte after it if it should be used with compressed public keys (described in a later subsection). Nothing is appended if it is used with uncompressed public keys.

    Perform a SHA-256 hash on the extended key.

    Perform a SHA-256 hash on result of SHA-256 hash.

    Take the first four bytes of the second SHA-256 hash; this is the checksum.

    Add the four checksum bytes from point 5 at the end of the extended key from point 2.

    Convert the result from a byte string into a Base58 string using Base58Check encoding.

The process is easily reversible, using the Base58 decoding function, and removing the padding.
  Data about our tokens we can have inside of some DataBase with numbers of all tokens that we create. Wallet have an object(Token) with each coins inside.

Payment Processing:

  [New Order] -> [Price Order in our Token] -> [Verify Payment and Fulfill Order] -> [Issue Refund (if we need that)] -> [Diburse Income (if we want to take money from the differences with courses)]


Finally it looks like we a making Tokens inside our network, generating orders to pay for someone or take money from someone, then we make a transaction inside wallet( it have all information about numers of Tokens, it cost and other information), put this order to transaction, then to block and send to address, after confirm payment or take back our tokens. All parts of cryptography will be inside wallets of users. 
