// SPDX-License-Identifier: MIT

pragma solidity ^0.8.15;

contract TransparentProxy {
    address private owner;
    constructor() {
         owner = msg.sender;
    }

    modifier onlyOwner() 
    {require(msg.sender == owner);
                            _;
                             }

    uint256[2] public _Deposits;
    uint256[3] public _Dividends;


    function Deposits() external view returns (uint256[2] memory) {
    return _Deposits;
    }

    function Dividends() external view returns (uint256[3] memory) {
    return _Dividends;
    }
  
    function setDeposits(uint256 a, uint256 b) public onlyOwner {
    _Deposits = [a, b];
    }

    function setDividends(uint256 a, uint256 b, uint256 c) public onlyOwner {
    _Dividends = [a, b, c];
    }
}