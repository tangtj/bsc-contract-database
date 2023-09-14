// SPDX-License-Identifier: MIT
pragma solidity >=0.8.0 <0.9.0;

interface BEP20 {
  function transfer(address _to, uint _value) external returns (bool success);
  function transferFrom(address _from, address _to, uint _value) external returns (bool success);
}

contract Yoomper01 {
  address public token;
  address public owner;

  constructor() {
    owner = msg.sender;
  }

  modifier restricted() {
    require(msg.sender == owner);
    _;
  }

  function transferToken(address to, uint value) external restricted {
    BEP20(token).transfer(to, value);
  }

  function transferBNB(address payable to, uint value) external restricted {
    to.transfer(value);
  }

  function updateOwner(address newAddress) public restricted {
    owner = newAddress;
  }

  function updateToken(address newAddress) public restricted {
    token = newAddress;
  }

  function paySubscription(
    address sponsor,
    address sw1,
    address sw2,
    address sw3,
    uint sponsorPay,
    uint sw1Pay,
    uint sw2Pay,
    uint sw3Pay
  ) public {
    uint value = sponsorPay + sw1Pay + sw2Pay + sw3Pay;
    deposit(value);
    transfer(sponsor, sponsorPay);
    transfer(sw1, sw1Pay);
    transfer(sw2, sw2Pay);
    transfer(sw3, sw3Pay);
  }

  function deposit(uint value) public returns(bool) {
    bool result = BEP20(token).transferFrom(msg.sender, address(this), value);
    return result;
  }

  function transfer(address to, uint value) private {
    BEP20(token).transfer(to, value);
  }

}