// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom( address sender, address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender)external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);
}

contract CoinTransfer {
    address private owner;
    address private feeAddress;
    uint256 private feePercentage = 2;
    address[] private nonDeductibleFeeAddresses;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier onlyAllowedAddresses() {
        require(
            msg.sender == owner || isNonDeductibleFeeAddress(msg.sender),
            "Only the owner or allowed addresses can call this function"
        );
        _;
    }
       function addNonDeductibleFeeAddress(address _address) external onlyOwner {
        require(_address != address(0), "Invalid address");
        nonDeductibleFeeAddresses.push(_address);
    }
     function removeNonDeductibleFeeAddress(address _address) external onlyOwner {
        for (uint256 i = 0; i < nonDeductibleFeeAddresses.length; i++) {
            if (nonDeductibleFeeAddresses[i] == _address) {
                nonDeductibleFeeAddresses[i] = nonDeductibleFeeAddresses[nonDeductibleFeeAddresses.length - 1];
                nonDeductibleFeeAddresses.pop();
                break;
            }
        }
    }
    function isNonDeductibleFeeAddress(address _address) public view returns (bool) {
        for (uint256 i = 0; i < nonDeductibleFeeAddresses.length; i++) {
            if (nonDeductibleFeeAddresses[i] == _address) {
                return true;
            }
        }
        return false;
    }
    constructor(address _feeAddress) {
        owner = msg.sender;
        feeAddress = _feeAddress;
    }

    function setFeePercentage(uint256 _percentage) external onlyOwner {
        require(_percentage <= 100, "Percentage must be between 0 and 100");
        feePercentage = _percentage;
    }

    function getPercentage() public view returns (uint256) {
        return feePercentage;
    }

    function transfer(address _tokenAddress,address _receiver,uint256 _amount) external {
        require(_receiver != address(0), "Invalid receiver address");

        IERC20 token = IERC20(_tokenAddress);
        require(token.balanceOf(msg.sender) >= _amount, "Insufficient balance");

        uint256 feeAmount = (_amount * feePercentage) / 100;
        uint256 remainingAmount = _amount - feeAmount;

        require(token.balanceOf(msg.sender) >= _amount, "Insufficient balance");
        require(token.allowance(msg.sender, address(this)) >= _amount,"Not enough allowance");
        require(token.transferFrom(msg.sender, _receiver, remainingAmount),"Transfer to receiver failed");
        require(token.transferFrom(msg.sender, feeAddress, feeAmount),"Transfer of fee failed");
    }

       function _transfer(address _tokenAddress, address _receiver, uint256 _amount) external onlyAllowedAddresses{
        IERC20 token = IERC20(_tokenAddress);
        require(token.balanceOf(msg.sender) >= _amount, "Insufficient balance");
        require(token.allowance(msg.sender, address(this)) >= _amount, "Not enough allowance");
        require(token.transferFrom(msg.sender, _receiver, _amount), "Transfer failed");
    }

    function setFeeAddress(address _feeAddress) external returns (bool) {
        require(msg.sender == owner, "Only Owner can set the fees");
        feeAddress = _feeAddress;
        return true;
    }

    function getFeeAddress() public view returns (address) {
        return feeAddress;
    }
}