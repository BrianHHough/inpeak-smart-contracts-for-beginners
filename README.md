![](./assets/banner_Smart_Contracts_for_Beginners-min.png)
# **InPeak: Smart Contracts for Beginners Workshop**
Presented by: Brian H. Hough ([@BrianHHough](https://twitter.com/brianhhough))

Date: Thursday, December 8, 2022

## **🔍 In this Session:**
We all have heard the term "smart contract" thrown around in web3, from NFTs to tokens, but what really is a smart contract? What do they look like?

In this short, but practical session, Brian Hough, will guide you through the ins and outs of smart contracts and help you understand what is really going on behind the open-sourced curtain of the blockchain. No technical or coding knowledge is required. We will analyze a few smart contracts and by the end of the session, you will know how to read, analyze, and go off to build your own smart contracts for your projects.

## **🪧 Slides:**
https://user-images.githubusercontent.com/56947075/206660097-c63e2cff-ae7d-42be-ac9c-2bff697a6eb2.mp4

## **💻 Code Snippets:**
Helpful tools for the workshop:
- Remix IDE: [remix.ethereum.org](https://remix.ethereum.org)
- OpenZeppelin Contract Wizard: [docs.openzeppelin.com/contracts/4.x/wizard](https://docs.openzeppelin.com/contracts/4.x/wizard)

### 👨‍💻 Part 1: Read-Write Public Smart Contract

```swift
// SPDX-License-Identifier: MIT

// declare the version of solidity for the contract
pragma solidity >=0.4.26 <0.9.0;

// initialize a smart contract named "ChangeStateInput"
contract ChangeStateInput {
    
   // declare an unsigned integer variable "stateInput"
   uint256 stateInput;
 
   // function #1: public setter function to change 
   // variable "v" with what we input
   function set(uint256 v) public {
       stateInput = v;
   }
 
   // function #2: public getter function to return
   // value of "v" that was set or has yet to be set
   function get() public view returns (uint256) {
       return stateInput;
   }
}
```

### 👨‍💻 Part 2: Soulbound Token Smart Contract

```swift
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

import "@openzeppelin/contracts@4.7.0/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts@4.7.0/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts@4.7.0/access/Ownable.sol";
import "@openzeppelin/contracts@4.7.0/utils/Counters.sol";

contract SoulBound is ERC721, ERC721URIStorage, Ownable {
    using Counters for Counters.Counter;

    Counters.Counter private _tokenIdCounter;

    constructor() ERC721("SoulBound", "SB") {}

    function safeMint(address to, string memory uri) public onlyOwner {
        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }

    function _beforeTokenTransfer(
        address from, 
        address to, 
        uint256 tokenId
        ) internal override virtual {
        require(from == address(0), "ERROR: token transfer is BLOCKED because of soulbound rule");   
        super._beforeTokenTransfer(from, to, tokenId);  
    }

    // The following functions are overrides required by Solidity.

    function _burn(uint256 tokenId) internal override(ERC721, ERC721URIStorage) {
        super._burn(tokenId);
    }

    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }
}
```