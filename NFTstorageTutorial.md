# Coding NFTs the Right Way

## With IPFS & Filecoin

### Original presentation created by: Ally Haire (Developer Advocate--Filecoin Foundation)

### Adapted to written tutorial by: Dawn Kelly (Developer Advocate--Protocol Labs)

## Introduction

This workshop will provide a brief introduction to IPFS and Filecoin. We will then dive into making an NFT using IPFS and FIlecoin to store the metadata persistently and immutably to protect the value of your digital assets over time. Before we are through, we will walk through all the code you need to create your very own immutable NFTs using NFT.storage to upload and store your metadata.

TL; DR: If you are already familiar with IPFS & Filecoin, you can jump straight to the coding demo [here](#demo)

First, let’s clear up any confusion that may be out there around a few names you’ve probably heard floating around the web3 ecosystem and how they fit together. You have probably heard of Protocol Labs, IPFS, Filecoin, and the Filecoin Foundation. Many times these names are used interchangeably and, while they are all tightly affiliated, they are all actually separate organizations.

![Infographic showing relationships between Protocol Labs, Filecoin Foundation, Filecoint, & IPFS](https://ipfs.io/ipfs/bafybeibix67vqxoama6ekiy4mmex67pdf5p63c2jwxzgss5p4sf7src7ai/PLrelationships.jpg)

### Protocol Labs

Founded in 2013 by Juan Benet, Protocol Labs is a fully distributed open-source R&D organization which builds protocols, tools, and services aimed at helping to radically improve the internet and drive breakthroughs in computing to help move humanity forward. These are some big goals for sure and, luckily, some truly powerhouse people are working to help drive this research and mission.

### IPFS (InterPlanetary File System)

[IPFS](https://docs.filecoin.io/) is more than just a cool, futuristic name. As a distributed, peer to peer network for files and folders, it was designed to be able to work even when the network is between planets! IPFS uses cryptography to address content by what it is instead of where it is. This creates the ability to retrieve content from the peer closest to you. If we were all hanging out in our shiny, new Mars colony, we could request files from each other quickly rather than waiting for signals to travel to centralized servers back on Earth and then back to us. IPFS is distributed and decentralized by design , has no central authority servers for censorship resistance, is designed to be offline first for resilience, and provides trustless verifiability of stored data through cryptographic hashes called Content IDs (CIDs).

![Infographic showing relationship between CID and data content](https://ipfs.io/ipfs/bafybeicf5damu4pubbt5du2ljixkvv4ybetmdf5nnnkhuvpityj26jf7ya/CIDcontent.jpg)

It’s worth taking a moment to talk a bit more here about Content Ids (CIDs). IPFS takes content and uses SHA-256 cryptography to create a deterministic hash representation of that data. In simple terms, changing one character or pixel of that saved data would create a completely new CID. In practice, this means you can be assured a CID match means you are getting back the data you expected and it remains immutable. You can see why this is desirable for use cases like NFTs where value is derived from authenticity and uniqueness.

### Filecoin

A peer to peer network is only useful if you have a healthy number of distributed peers to participate in storing and sharing data. Most people require an incentive to participate in this type of structure. Filecoin is designed to leverage a crypto-economic incentive model together with cryptographic proofs in order to ensure data is stored persistently, reliably, and verifiably across a distributed network. Its use of cryptographic proofs also enables smart-contract based permanence, and means that it is designed to be as permanent as you, the data owner, want it to be. Whether that permanence requirement is for 500 years or 5 minutes, you have total control over the timeframe you want for your storage and how many copies of it you want to ensure for resilience.

### Filecoin Foundation

The Filecoin [Foundation](https://fil.org/) is the steward of the Filecoin community, aspiring to put the power of humanity’s most important information back into the hands of everyone. The foundation provides resources, guidance, and fair, equitable principles to help the ecosystem flourish, and to open up the internet for a safer, stronger, and more reliable environment where everyone can thrive.

## NFT Use Case

Whether you want to use the ERC-721 token standard, or the more recently released ERC-1155 multi-token standard, a best practice approach is to leverage the community vetted, security audited, open-sourced smart contracts available through Open Zeppelin. Open Zeppelin has a range of standard implementations and utility contracts and libraries that are used widely by the community to create NFTs on any ethereum compatible chains.

![Image showing sample Open Zeppelin ERC721 contract](https://ipfs.io/ipfs/bafybeieosegiifyat7srdathtbhluvcxz7wh4zyzrh44qplugl6qzrgwre/OZERC721.jpg)

The main function we want to look at here is the mint and setTokenURI functions though. If we look at this example contract implementation, we can see that this contract creates a numbered ID GameItem for a player (with a wallet address) and takes a tokenURI (or Universal resource identifier) as a parameter. This is where it gets interesting, because the tokenURI is simply a JSON object containing the unique characteristics of the NFT - Thor’s hammer in this example.

![Image showing tokenURI JSON document](https://ipfs.io/ipfs/bafybeihc24s6y3jzixgzf7auhp22uwoalrwgylrscwaixjkl6xqtbxxosu/ThorHammer.jpg)

What’s important is that we really want to ensure that our metadata conforms to the characteristics we’re looking for in our NFT - particularly its non-fungibility and uniqueness . So where and how we save this metadata is an important consideration.

There are three main options to save this NFT metadata. 1) You can save it on-chain via the contract itself. This can get prohibitively expensive on higher gas fee chains, particularly for larger files like images, music or video files. 2) You can use a http address to fetch the NFT properties, as shown with the image address in the JSON object above. There are unfortunately multiple issues with this approach. One is that we are relying on someone to continue to pay for this storage. We are also relying on this http address always resolving to the same data. If someone wants to change this data and has access to this server, our non-fungible property will be lost. 3) This brings us to the third option. You can save this metadata as an IPFS CID and store it on Filecoin. This approach guarantees verifiable immutability of your NFT metadata to give your holders peace of mind.

## IPFS & Filecoin: DevTools

Now you know why you want to store your NFT metadata using IPFS and Filecoin but, how do you get started? Let’s take a look at some developer tools to make it easier to use IPFS and Filecoin in your projects.

### NFT.storage

[NFT.storage](https://nft.storage/docs/) was created as a public good to archive and persist NFT data. It creates an IPFS CID for your NFT metadata and then makes an automatic deal to store this content persistently on Filecoin with eight or more times redundancy. Over 25 million NFTs are currently stored on NFT.storage and it is used by both OpenSea and Magic Eden. The service is free as the team is committed to NFT metadata as a public good. Work is in progress to build renewal of Filecoin storage deals into smart contracts so there will be no need to trust a centralized entity for renewals.

### Web3.storage

[Web3.storage](https://web3.storage/docs/)  is another project built by IPFS and Filecoin and designed to give you the benefits of decentralized storage and IPFS content addressing with the frictionless experience you expect in a modern storage solution. The service is also free to use and store up to 1 TB of data and compatible with both JavaScript and Go client libraries.

### Fleek hosting

[Fleek](https://docs.fleek.co/) is a CI/CD tool you can use to deploy your apps for free as simply and easily as you would with some of the web2 tools you might be familiar with like Netlify or Vercel. Fleek uses IPFS to host your site or app and even offers ENS domain routing on their platform.

## <h2 id="demo"> NFT Demo </h2>

It’s the part we’ve all been waiting for: let’s build something! The tech stack for this demo includes:

- [Solidity](https://docs.soliditylang.org/en/v0.8.15/) (via Open Zeppelin smart contracts)
- [React](https://reactjs.org/docs/getting-started.html) (front end)
- [Ethers](https://docs.ethers.io/v5/) (React-JS client for web3)
- [Hardhat](https://hardhat.org/getting-started) (dev tool)
- [Moralis](https://docs.moralis.io/introduction/readme) (for contract calling & multi-chain deployments)
- [NFT.Storage](https://nft.storage/docs/) (IPFS CID creation and Filecoin metadata storage)
- [Fleek](https://docs.fleek.co/) (front-end deployment to the web on IPFS & Filecoin)
- [MetaMask](https://docs.metamask.io/guide/) (ALWAYS use a NEW Wallet for testing - NEVER use wallets with real assets in them)
- [Node](https://nodejs.org/en/docs/) installed

The full GitHub code repo is [here](https://github.com/DeveloperAlly/filecoin-expanded-nft-starter). You can view the ReadMe and Quickstart guide [here](https://github.com/DeveloperAlly/filecoin-expanded-nft-starter/blob/main/README.md).

For this demonstration, we’ve created a small dynamically minted NFT project with the option of using the Open Zeppelin ERC-721 or ERC-1155 smart contract and leveraging NFT.storage; a devtool which does the heavy lifting of IPFS and Filecoin for us and is designed specifically for NFTs. Dynamically minting the NFT just means that we aren’t creating/minting the NFT until the contract is called. This contract call could come from your website front end or in-game logic like a player leveling up or similar. You can deploy these contracts to any EVM-compatible chain including Polygon, AVAX, BSC or, of course, Ethereum.

If you are taking the build it from scratch approach, follow these steps:

:rocket: In your terminal, install and create either a NextJS app or React app

```npx create-next-app@latest``` or ```npx create-react-app[projectName]```

:rocket: Navigate to your app

```cd [projectName]``` and rename the README.md file (ex. README_React.md)

:rocket: Still in the terminal, install and make a Hardhat project
```npm install –save-dev hardhat```
```npx hardhat```

- Choose create a basic sample project and say yes to all the questions
- Rename the README.md file (ex. README_Hardhat.md)

:rocket: Install dotenv and create .env file
```npm install dotenv```
```touch.env```

:rocket: Create a server for the smart contracts as per the [Moralis](https://docs.moralis.io/moralis-dapp/getting-started/create-a-moralis-dapp) documentation. We’ve used Moralis so we can deploy to multiple EVM-compatible chains and can use their React hooks. Other options are Chainstack, Alchemy, Infura, and Quicknode.

:rocket: Add your .env variables as per [.env.example](https://github.com/DeveloperAlly/filecoin-expanded-nft-starter/blob/main/.env.example) We will add the deployed contract ones later so you can leave these blank for now.

:rocket: Create your [hardhat.config](https://hardhat.org/config) file per [hardhat.config.js](https://github.com/DeveloperAlly/filecoin-expanded-nft-starter/blob/main/hardhat.config.js) example.

:rocket: Time to make our contract and test it on [Remix](https://remix-ide.readthedocs.io/en/latest/)!

- In the terminal, install some more dependencies:

```npm install @openzeppelin/contracts @nomiclabs/hardhat-etherscan```

- We don’t need to write a smart contract from scratch when using EVM-compatible chains. We can use a template library provided by [OpenZeppelin](https://docs.openzeppelin.com/) which is security audited and community tested.

A note on ERC standards: The original standard template for NFTs was defined in the Ethereum Improvement Proposal ERC-721. EIPs describe the standards for the Ethereum platform like core protocol specs, client APIs and contract standards. Many chains also have their own versions of Improvement Proposals (like FIP - Filecoin Improvement Proposal). An improvement proposal is "proposed" by the community - this could be core dev's, members of the ecosystem or anyone in the world. The community then discusses this proposal and it then goes through a process of approval which generally includes an auditing review also. ERC's on the other hand are Ethereum Request for Comments. It's a weird name leftover from an original developer draft proposal, that basically means it's related to a specific category of EIPs that work on the application level (ie. it means that it's not a core function of the chain's code and doesn't need to be adopted by all participants).

ERC721 is perfectly fine for creating a basic NFT and when this demo was first created - it was the only one available. However, there's recently been a new proposal that is a multi token standard called [EIP1155](https://eips.ethereum.org/EIPS/eip-1155). This standard proposes the ability to create NFTs and fungible tokens in one contract and adds some cool new features like batch minting to NFTs. While I don't need either of these new features for this demo, I'm choosing to use OpenZeppelin's ERC1155 [template](https://docs.openzeppelin.com/contracts/3.x/erc1155) because of the gas optimisations it provides due to it's more efficient method for data storage (see [this](https://ethereum.stackexchange.com/questions/113811/does-erc-1155-contract-use-less-gas-to-mint-tokens/113868#113868) great write up on Eth StackOverflow) and also because who knows where this project might go - maybe we'll want to add fungible tokens at some point?

:rocket: create a file for a new Solidity smart contract called FilGoodNFT.sol

:rocket: Import the ERC1155 contract. We’re also going to use a debugger tool from Hardhat that will give us the ability to console.log variables and a counter utility library from OpenZeppelin that will allow us to give the NFTs individual numbers easily

![Image showing code to import contract dependencies into VS Code](<https://ipfs.io/ipfs/bafybeibsfviml2aefiq4ngzuyckjiavatbo7vlkm5yl32mv4zslsmpkvnm/importContractDepends.jpg>)

The ERC1155 contract is missing a tokenURI function out of the box so you will need to override the URI. You can read about how to do this [here](https://forum.openzeppelin.com/t/how-to-erc-1155-id-substitution-for-token-uri/3312/16) or watch a video [here](https://www.youtube.com/watch?app=desktop&v=19SSvs32m8I). You will also want to add a mapping for your TokenURIs.

:rocket: Mapping:

![Image showing code for mapping TokenURI](<https://ipfs.io/ipfs/bafybeib6kq72wkgzseeda2h4ecm2shc3hnfe2k5est4xlzxw76igrglcoq/mapping.jpg>)

:rocket: TokenURI override:

![Image showing code for overriding TokenURI](<https://ipfs.io/ipfs/bafybeicans6gytwkd4xpa3m3ku2hrkmin6ki26ajejmveix6nzysr5ania/tokenURIoverride.jpg>)

:rocket: Creating NFT Metadata with NFT.Storage

![Image showing code for creating NFT metadata with NFT.Storage](<https://ipfs.io/ipfs/bafybeia3vpbrrfwls7yvp4pnmn6nakjzfjkttjlfwbl3uhbq4ruecs4mti/saveToNFTStorage.jpg>)

:rocket: For ease of flow I suggest testing your contracts on Remix first. You can also import Solidity debuggers into your IDE of choice.

:rocket: Now we can create our deploy scripts and deploy our contracts to live networks! Let's head back to the terminal.

```
cd scripts
touch deploy.js
npx hardhat run --network NETWORK scripts/deploy.js
```

Note: NETWORK = localhost or any in the config file

:rocket: Verify our deployed contract (for Etherscan networks - ie Goerli, Ropsten, Ethereum mainnet)

```
npx hardhat verify –network NETWORK DEPLOYED_CONTRACT_ADDRESS
```

Note: NETWORK = localhost or any in the config file; DEPLOYED_CONTRACT_ADDRESS = the blockchain address of your deployed contract

## IPFS & Filecoin: Resources & Getting Involved

### Learn

- [Proto.school](https://proto.school/)
- [NFTschool.dev](https://nftschool.dev/)
- [Docs](https://docs.filecoin.io/)
- [YouTube](https://www.youtube.com/c/FilecoinProject)

### Contributions & Grants

- [Hackathons.filecoin.io](https://hackathons.filecoin.io/)
- [Grants](https://github.com/filecoin-project/devgrants) (microgrants, project grants, bounties)
- Projects and [repos](https://github.com/filecoin-project) are open source

### Join the Community

- [Ambassador](https://orbit.love/community) program (Orbit community program)
- Protocol Labs [Launchpad](https://boards.greenhouse.io/protocollabs/jobs/4305898004) (for those looking to become part of the business ecosystem)

### Get in touch

- Ally Haire: Developer Advocate [Twitter](<https://twitter.com/DeveloperAlly>)

- Dawn Kelly: Developer Advocate [Twitter](<https://twitter.com/run4pancakes>)
