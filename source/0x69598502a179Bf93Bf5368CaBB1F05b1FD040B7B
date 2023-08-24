// SPDX-License-Identifier: MIT


pragma solidity 0.8.19;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

contract Storeyourtextforever is Context{
    mapping (address => string) private _saved_text;
    string save;

    function storeText(string memory _text) public 
    {
        _saved_text[_msgSender()] = _text;
    }

    function getText(address address_of_client) public view returns (string memory)
    {
        return  _saved_text[address_of_client];
    }
}