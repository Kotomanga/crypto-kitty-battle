# WIP Crypto Kitties Battleground

Crypto Kitties Battleground is an extension to the popular crypto kitties smart contracts. Those who have purchased or bred crypto kitties may deposit their cats into the battle arena contract. Once deposited kitty owners can train their crypto warriors by calling the `level()` function. The more ether an owner has the higher they may pump their kitties battle stats.

Currently all cats enter the arena with a base power rating that can be increased by training the kitty. The base power rating is determined randomly based upon the genetics of the cat. Factors such as a lower generation number and faster birthing cool down will give the kitty a higher chance of having a high base power. This base power is the multiplication factor of the power increase gained from calling the level() function. Cats with a higher base power rating will be able to achieve the highest power in the arena.

Entering a battle is accomplished by checking the battle registry to find an open battle request in your kitties power range, or by creating an open battle request with a specified power range and ether wager for win/lose. Each dueling kitties stats are compared in a non-verified contract that incorporates some randomness into the victory decision.

#### Hold the arena!

The kitty in the gym with the highest power will collect a percentage of all arena earnings via level ups and wagers while they retain the highest power.

## Rinkeby Testnet Deployment

Do to the complexity of the CK contract the test version of CK Battle Arena is deployed on the rinkeby/kovan test network where the block gas limit is > 6000000

Kitty 0 (genesis kitty) genes are hard coded to the underflow of `uint256 (0 - 1 = 2**256 - 1)`

A testnet version of the arena based on the open and verified crypto kitties mainnet deployment can be found at the following addresses.

### How to test battle

1. Get a test kitty. 

Using MEW load the kitty core ABI and contract address below. Access the `createPromoKitty()` function. Grab a genetic sequence from your favorite real crypto kitty or enter a random 128bit integer string as the genes parameter and the ethereum address of the owner of the new kitteh.

2. Enter your kitteh in the arena

On the KittyCore contract now access the ERC721 `approve()` function and approve the arena address to transfer your test kitteh. Depositing kitties into the arena in two transactions is a limit of tokens that do not implement an `approveAndCall()` method for contracts that need permission to transfer tokens internally. 

Open a new instance of MEW and load the Arena ABI and address listed below. Access the `enterArena()` function with the kitty ID of the kitteh you have just approved and send the transaction from the owner account that was passed in during kitteh creation. You now have a kitteh ready to battle and you can check its base states by accessing the `battleKitties()` storage mapping.

3. Battle a kitteh

In the current version there is no array of open battle to iterate through, it is left to an application layer to access the logs and reference the mapping storing open battles. You can deposit a second cat to test battling or create a battle with another tester.

3 a) Create a battle

Access the `createBattle()` function on the arena contract and send a transaction specifying 

- which cat you wish to enter into battle 
- the wager price you have must send to battle this kitteh, 0 if game mode 2 or 3
- the game mode: 
  - 1 for wager battle
  - 2 for owner transfer battle
  - 3 for pride battle (no wager)
  - 4 to the death (coming soon, losing kitteh will be sent to a burn address)
- power range, supply the acceptable range of power a challenger cat may have compared to your entered kitty. (ex if your kitty is base power 6 and you enter 2 on kittehs in the range of 4-8 will be able to battle)

3 b) Challenge an open battle

Access the `fight()` function and send a transaction from the owner of the cat you wish to battle with providing the ERC721 ID of the kitteh you are battling and the one you are fighting with and any ether wager payment game mode 2

4. Review battle outcome

The battle winner is chosen at random weighted by your cats base power rating. You can check your trainer stats by accessing `trainers()` with your ethereum address or by checking your kittehs stats. Each tallies the wins and loses. If you played game mode 1,2, or 4 you should see transfers of the ERC721 tokens and Eth tokens accordingly.

Arena Address:  `0x1932bd525d341643123fc4a19d9c8ab2742610eb`

Arena ABI:

```
[
    {
      "constant": false,
      "inputs": [
        {
          "name": "_tokenId",
          "type": "uint256"
        }
      ],
      "name": "level",
      "outputs": [],
      "payable": true,
      "stateMutability": "payable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "score2",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_tokenId",
          "type": "uint256"
        },
        {
          "name": "_wagerPrice",
          "type": "uint256"
        },
        {
          "name": "_gameMode",
          "type": "uint8"
        },
        {
          "name": "_powerRange",
          "type": "uint64"
        }
      ],
      "name": "createBattle",
      "outputs": [],
      "payable": true,
      "stateMutability": "payable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "name": "trainers",
      "outputs": [
        {
          "name": "numKittiesInArena",
          "type": "uint64"
        },
        {
          "name": "wins",
          "type": "uint64"
        },
        {
          "name": "losses",
          "type": "uint64"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "rand1",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [],
      "name": "unpause",
      "outputs": [
        {
          "name": "",
          "type": "bool"
        }
      ],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_tokenId",
          "type": "uint256"
        }
      ],
      "name": "leaveArena",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_tokenId",
          "type": "uint256"
        }
      ],
      "name": "cancelBattle",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "championCut",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "name": "battleKitties",
      "outputs": [
        {
          "name": "basePower",
          "type": "uint128"
        },
        {
          "name": "wins",
          "type": "uint64"
        },
        {
          "name": "losses",
          "type": "uint64"
        },
        {
          "name": "level",
          "type": "uint8"
        },
        {
          "name": "coolDown",
          "type": "uint64"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "paused",
      "outputs": [
        {
          "name": "",
          "type": "bool"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [],
      "name": "withdrawBalance",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "_tokenId",
          "type": "uint256"
        }
      ],
      "name": "getBattle",
      "outputs": [
        {
          "name": "initiator",
          "type": "address"
        },
        {
          "name": "initiatorCatID",
          "type": "uint256"
        },
        {
          "name": "wagerPrice",
          "type": "uint128"
        },
        {
          "name": "gameMode",
          "type": "uint8"
        },
        {
          "name": "startedAt",
          "type": "uint64"
        },
        {
          "name": "powerRange",
          "type": "uint64"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "name": "fighterIndexToOwner",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "ownerCut",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [],
      "name": "pause",
      "outputs": [
        {
          "name": "",
          "type": "bool"
        }
      ],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "owner",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "score1",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "name": "tokenIdToBattle",
      "outputs": [
        {
          "name": "initiator",
          "type": "address"
        },
        {
          "name": "initiatorCatID",
          "type": "uint256"
        },
        {
          "name": "wagerPrice",
          "type": "uint128"
        },
        {
          "name": "gameMode",
          "type": "uint8"
        },
        {
          "name": "startedAt",
          "type": "uint64"
        },
        {
          "name": "powerRange",
          "type": "uint64"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_address",
          "type": "address"
        }
      ],
      "name": "setPowerScienceAddress",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "championId",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_tokenId",
          "type": "uint256"
        }
      ],
      "name": "enterArena",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_tokenId",
          "type": "uint256"
        },
        {
          "name": "_initiatorTokenId",
          "type": "uint256"
        }
      ],
      "name": "fight",
      "outputs": [],
      "payable": true,
      "stateMutability": "payable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "nonFungibleContract",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "rand2",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "newOwner",
          "type": "address"
        }
      ],
      "name": "transferOwnership",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "powerScience",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "inputs": [
        {
          "name": "_nftAddress",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "constructor"
    },
    {
      "anonymous": false,
      "inputs": [],
      "name": "Pause",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [],
      "name": "Unpause",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "name": "tokenId",
          "type": "uint256"
        },
        {
          "indexed": false,
          "name": "wagerPrice",
          "type": "uint256"
        },
        {
          "indexed": false,
          "name": "duration",
          "type": "uint256"
        }
      ],
      "name": "BattleCreated",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "name": "tokenId",
          "type": "uint256"
        }
      ],
      "name": "BattleCancelled",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "name": "_tokenId",
          "type": "uint256"
        },
        {
          "indexed": false,
          "name": "price",
          "type": "uint256"
        },
        {
          "indexed": false,
          "name": "winner",
          "type": "address"
        }
      ],
      "name": "BattleSuccessful",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "name": "tokenId",
          "type": "uint256"
        }
      ],
      "name": "ArenaEntered",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "name": "tokenId",
          "type": "uint256"
        },
        {
          "indexed": false,
          "name": "level",
          "type": "uint8"
        }
      ],
      "name": "LevelUp",
      "type": "event"
    }
  ]
```

KittyCore: `0x95ef2833688ee675dfc1350394619ae22b7667df`

Kitty Core ABI:

```
[
    {
      "constant": true,
      "inputs": [
        {
          "name": "_interfaceID",
          "type": "bytes4"
        }
      ],
      "name": "supportsInterface",
      "outputs": [
        {
          "name": "",
          "type": "bool"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "cfoAddress",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "_tokenId",
          "type": "uint256"
        },
        {
          "name": "_preferredTransport",
          "type": "string"
        }
      ],
      "name": "tokenMetadata",
      "outputs": [
        {
          "name": "infoUrl",
          "type": "string"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "promoCreatedCount",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "name",
      "outputs": [
        {
          "name": "",
          "type": "string"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_to",
          "type": "address"
        },
        {
          "name": "_tokenId",
          "type": "uint256"
        }
      ],
      "name": "approve",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "ceoAddress",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "GEN0_STARTING_PRICE",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_address",
          "type": "address"
        }
      ],
      "name": "setSiringAuctionAddress",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "totalSupply",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "pregnantKitties",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "_kittyId",
          "type": "uint256"
        }
      ],
      "name": "isPregnant",
      "outputs": [
        {
          "name": "",
          "type": "bool"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "GEN0_AUCTION_DURATION",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "siringAuction",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_from",
          "type": "address"
        },
        {
          "name": "_to",
          "type": "address"
        },
        {
          "name": "_tokenId",
          "type": "uint256"
        }
      ],
      "name": "transferFrom",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_address",
          "type": "address"
        }
      ],
      "name": "setGeneScienceAddress",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_newCEO",
          "type": "address"
        }
      ],
      "name": "setCEO",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_newCOO",
          "type": "address"
        }
      ],
      "name": "setCOO",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_kittyId",
          "type": "uint256"
        },
        {
          "name": "_startingPrice",
          "type": "uint256"
        },
        {
          "name": "_endingPrice",
          "type": "uint256"
        },
        {
          "name": "_duration",
          "type": "uint256"
        }
      ],
      "name": "createSaleAuction",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [],
      "name": "unpause",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "name": "sireAllowedToAddress",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "_matronId",
          "type": "uint256"
        },
        {
          "name": "_sireId",
          "type": "uint256"
        }
      ],
      "name": "canBreedWith",
      "outputs": [
        {
          "name": "",
          "type": "bool"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "name": "kittyIndexToApproved",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_kittyId",
          "type": "uint256"
        },
        {
          "name": "_startingPrice",
          "type": "uint256"
        },
        {
          "name": "_endingPrice",
          "type": "uint256"
        },
        {
          "name": "_duration",
          "type": "uint256"
        }
      ],
      "name": "createSiringAuction",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "val",
          "type": "uint256"
        }
      ],
      "name": "setAutoBirthFee",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_addr",
          "type": "address"
        },
        {
          "name": "_sireId",
          "type": "uint256"
        }
      ],
      "name": "approveSiring",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_newCFO",
          "type": "address"
        }
      ],
      "name": "setCFO",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_genes",
          "type": "uint256"
        },
        {
          "name": "_owner",
          "type": "address"
        }
      ],
      "name": "createPromoKitty",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "secs",
          "type": "uint256"
        }
      ],
      "name": "setSecondsPerBlock",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "paused",
      "outputs": [
        {
          "name": "",
          "type": "bool"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [],
      "name": "withdrawBalance",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "_tokenId",
          "type": "uint256"
        }
      ],
      "name": "ownerOf",
      "outputs": [
        {
          "name": "owner",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "GEN0_CREATION_LIMIT",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "newContractAddress",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_address",
          "type": "address"
        }
      ],
      "name": "setSaleAuctionAddress",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "_owner",
          "type": "address"
        }
      ],
      "name": "balanceOf",
      "outputs": [
        {
          "name": "count",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_v2Address",
          "type": "address"
        }
      ],
      "name": "setNewAddress",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "secondsPerBlock",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [],
      "name": "pause",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "_owner",
          "type": "address"
        }
      ],
      "name": "tokensOfOwner",
      "outputs": [
        {
          "name": "ownerTokens",
          "type": "uint256[]"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_matronId",
          "type": "uint256"
        }
      ],
      "name": "giveBirth",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [],
      "name": "withdrawAuctionBalances",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "symbol",
      "outputs": [
        {
          "name": "",
          "type": "string"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "name": "cooldowns",
      "outputs": [
        {
          "name": "",
          "type": "uint32"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "name": "kittyIndexToOwner",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_to",
          "type": "address"
        },
        {
          "name": "_tokenId",
          "type": "uint256"
        }
      ],
      "name": "transfer",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "cooAddress",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "autoBirthFee",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "erc721Metadata",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_genes",
          "type": "uint256"
        }
      ],
      "name": "createGen0Auction",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "_kittyId",
          "type": "uint256"
        }
      ],
      "name": "isReadyToBreed",
      "outputs": [
        {
          "name": "",
          "type": "bool"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "PROMO_CREATION_LIMIT",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_contractAddress",
          "type": "address"
        }
      ],
      "name": "setMetadataAddress",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "saleAuction",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "_id",
          "type": "uint256"
        }
      ],
      "name": "getKitty",
      "outputs": [
        {
          "name": "isGestating",
          "type": "bool"
        },
        {
          "name": "isReady",
          "type": "bool"
        },
        {
          "name": "cooldownIndex",
          "type": "uint256"
        },
        {
          "name": "nextActionAt",
          "type": "uint256"
        },
        {
          "name": "siringWithId",
          "type": "uint256"
        },
        {
          "name": "birthTime",
          "type": "uint256"
        },
        {
          "name": "matronId",
          "type": "uint256"
        },
        {
          "name": "sireId",
          "type": "uint256"
        },
        {
          "name": "generation",
          "type": "uint256"
        },
        {
          "name": "genes",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_sireId",
          "type": "uint256"
        },
        {
          "name": "_matronId",
          "type": "uint256"
        }
      ],
      "name": "bidOnSiringAuction",
      "outputs": [],
      "payable": true,
      "stateMutability": "payable",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "gen0CreatedCount",
      "outputs": [
        {
          "name": "",
          "type": "uint256"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "geneScience",
      "outputs": [
        {
          "name": "",
          "type": "address"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function"
    },
    {
      "constant": false,
      "inputs": [
        {
          "name": "_matronId",
          "type": "uint256"
        },
        {
          "name": "_sireId",
          "type": "uint256"
        }
      ],
      "name": "breedWithAuto",
      "outputs": [],
      "payable": true,
      "stateMutability": "payable",
      "type": "function"
    },
    {
      "inputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "constructor"
    },
    {
      "payable": true,
      "stateMutability": "payable",
      "type": "fallback"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "name": "owner",
          "type": "address"
        },
        {
          "indexed": false,
          "name": "matronId",
          "type": "uint256"
        },
        {
          "indexed": false,
          "name": "sireId",
          "type": "uint256"
        },
        {
          "indexed": false,
          "name": "cooldownEndBlock",
          "type": "uint256"
        }
      ],
      "name": "Pregnant",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "name": "from",
          "type": "address"
        },
        {
          "indexed": false,
          "name": "to",
          "type": "address"
        },
        {
          "indexed": false,
          "name": "tokenId",
          "type": "uint256"
        }
      ],
      "name": "Transfer",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "name": "owner",
          "type": "address"
        },
        {
          "indexed": false,
          "name": "approved",
          "type": "address"
        },
        {
          "indexed": false,
          "name": "tokenId",
          "type": "uint256"
        }
      ],
      "name": "Approval",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "name": "owner",
          "type": "address"
        },
        {
          "indexed": false,
          "name": "kittyId",
          "type": "uint256"
        },
        {
          "indexed": false,
          "name": "matronId",
          "type": "uint256"
        },
        {
          "indexed": false,
          "name": "sireId",
          "type": "uint256"
        },
        {
          "indexed": false,
          "name": "genes",
          "type": "uint256"
        }
      ],
      "name": "Birth",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "name": "newContract",
          "type": "address"
        }
      ],
      "name": "ContractUpgrade",
      "type": "event"
    }
  ]
```



### Deployment process notes

- Deploy Kitteh core
- Deploy Sale Auction Contract 
- Set Kitteh auction with sale auction by calling `setSaleAuctionAddress(address _address)`
- Deploy Siring Auction Contract
- Set Kitteh auction with siring auction by calling `setSiringAuctionAddress(address _address)`
- unpause()
- generate promo kitties for testing (TODO create a kitteh faucet)
- Deploy PowerScience contract
- Deploy Arena and link powerscience
- enter kittehs in the arena to train/battle.

#### Gene Science Contract

The geneScience contract controls the mixing algorithm that defines the outcome genetics of new crypto kitties. The code is not verified. Gene Mixing is currently not tested and all genetic code is provided from the COO ability to generate clock and promo cats. 
