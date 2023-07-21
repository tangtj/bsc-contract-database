// SPDX-License-Identifier: None

pragma solidity ^0.8.0;

interface BEP20 {
    function totalSupply() external view returns (uint theTotalSupply);
    function balanceOf(address _owner) external view returns (uint balance);
    function transfer(address _to, uint _value) external returns (bool success);
    function transferFrom(address _from, address _to, uint _value) external returns (bool success);
    function approve(address _spender, uint _value) external returns (bool success);
    function allowance(address _owner, address _spender) external view returns (uint remaining);
    event Transfer(address indexed _from, address indexed _to, uint _value);
    event Approval(address indexed _owner, address indexed _spender, uint _value);
    
}

contract ArthaMultiSender {

   address public owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        owner = msg.sender;
    }

    function sendMultiple(address payable[] memory _addresses, uint[] memory _amounts) external payable {
        require(msg.sender == owner, "Only Owner");
        require(_addresses.length == _amounts.length, "The length of addresses and amounts must be the same");

        for(uint i = 0; i < _addresses.length; i++) {
            _addresses[i].transfer(_amounts[i]);
        }
    }

  function getBalances(BEP20 token, address[] calldata addresses) external view returns (uint256[] memory, uint256[] memory) {
    uint256[]  memory tokenBalances = new uint256[](addresses.length);
    uint256[] memory bnbBalances = new uint256[](addresses.length);

    uint256 length = addresses.length;
    tokenBalances = new uint256[] (length);
    bnbBalances = new uint256[] (length);

    for (uint256 i = 0; i < addresses.length; i++) {
        tokenBalances[i] = token.balanceOf(addresses[i]);
        bnbBalances[i] = addresses[i].balance;
    }

    return (tokenBalances, bnbBalances);
}


    function transferOwnership(address _to) external {
        require(msg.sender == owner, "Only owner");
        address oldOwner  = owner;
        owner = _to;
        emit OwnershipTransferred(oldOwner,_to);
    }

    // Only owner can withdraw token 
    function withdrawToken(address tokenAddress, address to, uint amount) public returns(bool) {
        require(msg.sender == owner, "Only owner");
        require(to != address(0), "Cannot send to zero address");
        BEP20 _token = BEP20(tokenAddress);
        _token.transfer(to, amount);
        return true;
    }
    
    // Owner BNB Withdraw
    // Only owner can withdraw BNB from contract
    function withdrawBNB(address payable to, uint amount) public returns(bool) {
        require(msg.sender == owner, "Only owner");
        require(to != address(0), "Cannot send to zero address");
        to.transfer(amount);
        return true;
    }


}