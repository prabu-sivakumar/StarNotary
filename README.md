## Star Notary

## Task 1

Add a name and a symbol for your starNotary tokens.

```Java
string  public  constant name = "Udacity";

string  public  constant symbol = "UDC";
```
Add a function ***lookUptokenIdToStarInfo***, that looks up the stars using the Token ID, and then returns the name of the star.

```Java
function  lookUptokenIdToStarInfo (uint  _tokenId) public  view  returns (string  memory) {
	return tokenIdToStarInfo[_tokenId].name;
}
```
Add a function called ***exchangeStars***, so 2 users can exchange their star tokens

```Java
function  exchangeStars(uint256  _tokenId1, uint256  _tokenId2) public {
	require(ownerOf(_tokenId1) == msg.sender || ownerOf(_tokenId2) == msg.sender, "Sender does not own either of the two tokens");
	address owner1Address = ownerOf(_tokenId1);
	address owner2Address = ownerOf(_tokenId2);
	_transferFrom(owner1Address, owner2Address, _tokenId1);
	_transferFrom(owner2Address, owner1Address, _tokenId2);
}
```
Write a function to ***Transfer*** a Star. The function should transfer a star from the address of the caller. The function should accept 2 arguments, the address to transfer the star to, and the token ID of the star
```Java
function  transferStar(address  _to1, uint256  _tokenId) public {
	require(ownerOf(_tokenId) == msg.sender);
	_transferFrom(msg.sender, _to1, _tokenId);
}
```

## Task 2
Add supporting unit tests, to test the following:

The token name and token symbol are added properly.
```Java
it('can add the star name and star symbol properly', async() => {
	let  instance = await  StarNotary.deployed();
	let  user1 = accounts[1];
	let  starId = 6;
	await  instance.createStar('Blockchain Developer', starId, {from:  user1});
	let  name = await  instance.name.call();
	let  symbol = await  instance.symbol.call();
	assert.equal(name, 'Udacity');
	assert.equal(symbol, 'UDC');
});
```
2 users can exchange their stars
```Java
it('lets 2 users exchange stars', async() => {
	let  instance = await  StarNotary.deployed();
	let  user1 = accounts[1];
	let  user2 = accounts[2];
	let  starId1 = 7;
	let  starId2 = 8;
	await  instance.createStar('Python Developer', starId1, {from:  user1});
	await  instance.createStar('Java Developer', starId2, {from:  user2});
	await  instance.exchangeStars(starId1, starId2, {from:  user1});
	assert.equal(await  instance.ownerOf.call(starId1), user2);
	assert.equal(await  instance.ownerOf.call(starId2), user1);
});
```
Stars Tokens can be transferred from one address to another
```Java
it('lets a user transfer a star', async() => {
	let  instance = await  StarNotary.deployed();
	let  user1 = accounts[1];
	let  user2 = accounts[2];
	let  starId = 9;
	await  instance.createStar('Golang Developer', starId, {from:user1});
	await  instance.transferStar(user2, starId, {from:user1});
	assert.equal(await  instance.ownerOf.call(starId), user2);
});
```
Lookup TokenId to Star Info. 
```Java
it('lookUptokenIdToStarInfo test', async() => {
	let  instance = await  StarNotary.deployed();
	let  user1 = accounts[1];
	let  starId = 10;
	await  instance.createStar('Solidity Developer', starId, {from:  user1});
	let  name = await  instance.lookUptokenIdToStarInfo.call(starId);
	assert.equal(name, 'Solidity Developer');
});
```

## Task 3

Deploy your contract to Rinkeby Test Network

Edit the truffle.config file to add settings to deploy your contract to the Rinkeby Public Network.

```Javascript
var  HDWalletProvider = require("truffle-hdwallet-provider");
const  fs = require('fs');
const  mnemonic = fs.readFileSync(".secret").toString().trim();

module.exports = {
	networks: {
		development: {
			host:  "127.0.0.1",
			port:  9545,
			network_id:  "*"
		},
		rinkeby: {
			provider:  function() {
				return  new  HDWalletProvider(mnemonic, "https://rinkeby.infura.io/v3/4db5346c3f2e45f4b7d3feee19595f83");
			},
			network_id:  4,
			gas:  4500000,
			gasPrice:  10000000000,

		}
	}
};
```
Commands used to deploy Smart Contract

    truffle migrate --reset --network rinkeby

Metamask seed is included in the .secret file. 

### Infura Project
|Attribute|Value  |
|--|--|
|Project Name  |prabusivakumar  |
|Project ID  |4db5346c3f2e45f4b7d3feee19595f83  |

### Faucet
To request ethers for Rinkeby Network, the Chainlink faucet is used.
https://faucets.chain.link/rinkeby
**Note**: I'm unable to request tokens from the following recommnded faucets:
https://faucet.metamask.io/ 
```JSON
Error: 500 {"error":"[ethjs-query] while formatting outputs from RPC '{\"value\":{\"code\":-32000,\"message\":\"replacement transaction underpriced\"}}'"}
```
I've included the Address in the Facebook and Twitter posts; included the post URL in the https://faucet.rinkeby.io/, the address could not be resolved. 

## Task 4
Modify the front end of the DAPP to achieve the following:
Lookup a star by ID using tokenIdToStarInfo() (you will have to add code for this in your index.html and index.js files)

```Javascript
lookUp:  async  function (){
	const { lookUptokenIdToStarInfo } = this.meta.methods;
	const  id = document.getElementById("lookid").value;
	let  name = await  lookUptokenIdToStarInfo(id).call();
	App.setStatus("Star name for Token ID "+ id + " is "+ name);
}
```
## Project Submission 
Truffle Version 
```
Truffle v5.4.27 - a development framework for Ethereum
```
Openzeppelin Solidity Version
```
openzeppelin-solidity@2.3
```
### ERC-721 Token Information
|Attribute|Value  |
|--|--|
|Name  |Udacity  |
|Symbol  |UDC  |

### Token Address in Rinkeby Network
https://rinkeby.etherscan.io/address/0x1D4a709a58502BD810669738e99Cd4cAeEfc8EFC

**Transaction Hash**
*0x4050460db03456aa0c1cdb5c1f666f056a2d932a2ef67663812a7baf89d0f4dc*

