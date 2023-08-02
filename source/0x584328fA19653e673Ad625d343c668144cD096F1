// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address _who) external view returns (uint256);

    function transfer(address _to, uint256 _value) external;

    function allowance(address _owner, address _spender)
        external
        view
        returns (uint256);

    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    ) external;

    function approve(address _spender, uint256 _value) external;

    function burnFrom(address _from, uint256 _value) external;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

contract ICCHelp {
    IERC20 public _USDT; // USDT Token address

    address public _owner;

    address public walletAddress;

    // USDT-0x55d398326f99059fF775485246999027B3197955
    constructor(IERC20 USDT) {
        _USDT = USDT;
        _owner = msg.sender;
        walletAddress = address(this);
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, "Permission denied");
        _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0));
        _owner = newOwner;
    }

    function transferWalletAddress(address newWalletAddress) public onlyOwner {
        require(newWalletAddress != address(0));
        walletAddress = newWalletAddress;
    }

    function rechargeUSDT(uint256 _amount) public {
        _USDT.transferFrom(msg.sender, walletAddress, _amount);
    }

    function rechargeICC(uint256 _amount) public {}

    function withdrawUSDT(address _account, uint256 _amount) public onlyOwner {
        _USDT.transfer(_account, _amount);
    }
}