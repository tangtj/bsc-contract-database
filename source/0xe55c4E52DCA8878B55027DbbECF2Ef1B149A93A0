// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract ReceiveEther {

    receive() external payable {}
    fallback() external payable {}

    function getBalance() public view returns (uint) {
        return address(this).balance;
    }

    function withdraw(address _to) public payable{
        payable(address(_to)).transfer(address(this).balance);
    }
}

contract SendEther {

    function sendViaCall(address payable _to) public payable {
        // Call returns a boolean value indicating success or failure.
        // This is the current recommended method to use.
        (bool sent,) = _to.call{value: msg.value}("");
        require(sent, "Failed to send Ether");
    }
}

interface With {
    function getBalance() external view returns(uint);
    function withdraw(address _to) external payable;
}

contract WithrawAmtFromAnotherContract{

    function getBalance(address _address) public view returns(uint){
        return With(_address).getBalance();
    }

    function stealAmntFromContract_method1(address _address) public {
        With(_address).withdraw(msg.sender);
    }
}