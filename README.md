# Escrowcontract
Typical Escrow smart contract for Ethereum blockchain

```
pragma solidity ^0.4.11;
contract Escrow {


  address public buyer;
  address public seller;
  address public arbiter;

  function Escrow (address _seller, address _arbiter) payable {
    buyer = msg.sender;
    seller = _seller;
    arbiter = _arbiter;
  }

  function payoutToSeller () {
    if (msg.sender == buyer || msg.sender == arbiter) {
      seller.send(this.balance);
    }
  }

  function refundToBuyer () {
    if (msg.sender == seller || msg.sender == arbiter) {
      buyer.send(this.balance);
    }
  }

  function getBalance() constant returns (uint) {
    return this.balance;
  }
}

```
## Deployment setup with ethereum testrpc

```

var solc = require("solc")

var source = `contract Escrow{...}`

var compiled = solc.compile(source)

var abi  = JSON.parse(compiled.contracts[':Escrow'].interface)  
var bytecode = compiled.contracts[':Escrow'].bytecode

var EscrowContract = web3.eth.contract(abi)


var seller = web3.eth.accounts[0];
var buyer = web3.eth.accounts[1];
var arbiter= web3.eth.accounts[2];


var deployed =EscrowContract.new(seller,arbiter, {
from:buyer,
data:bytecode,
gas:47000000,
gasPrice:5,
value: web3.toWei(5, 'ether')
}, (error,contract) => {})   
```

5 ether will be sent to the contract upon creation







