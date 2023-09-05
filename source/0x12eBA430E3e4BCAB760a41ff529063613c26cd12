// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom( address sender, address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender)external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _setOwner(_msgSender());
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract CoinTransfer is  Ownable {
    address private feeAddress;
    uint256 private feePercentage = 2;
     address[] private nonDeductibleFeeAddresses;
    event Transfer(address indexed from, address indexed to, uint256 value);

    modifier onlyAllowedAddresses() {
        require(
            msg.sender == owner() || isNonDeductibleFeeAddress(msg.sender),
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
        emit Transfer(msg.sender, _receiver, _amount);
    }

       function _transfer(address _tokenAddress, address _receiver, uint256 _amount) external onlyAllowedAddresses{
        IERC20 token = IERC20(_tokenAddress);
        require(token.balanceOf(msg.sender) >= _amount, "Insufficient balance");
        require(token.allowance(msg.sender, address(this)) >= _amount, "Not enough allowance");
        require(token.transferFrom(msg.sender, _receiver, _amount), "Transfer failed");
         emit Transfer(msg.sender, _receiver, _amount);
    }

    function setFeeAddress(address _feeAddress) external returns (bool) {
        require(msg.sender == owner(), "Only Owner can set the fees");
        feeAddress = _feeAddress;
        return true;
    }

    function getFeeAddress() public view returns (address) {
        return feeAddress;
    }
}