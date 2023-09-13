//SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    constructor() {
        _transferOwnership(_msgSender());
    }
    modifier onlyOwner() {
        _checkOwner();
        _;
    }
    function owner() public view virtual returns (address) {
        return _owner;
    }
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract aexnglobal is Ownable {
  
    IERC20 public token;
    address public subOwner;

    event Multisended(uint256 total, address tokenAddress);

    constructor(IERC20 _token,address _subOwner) {
        token = _token;
        subOwner = _subOwner;
    }

    modifier onlySubOwner() {
        require(msg.sender == subOwner || msg.sender == owner(), "Only Call by subowner");
        _;
    }

    function sell(uint256 _amount)  
    external 
    {   require(_amount > 0,"invalid amount");
        require(token.transferFrom(msg.sender,address(this),_amount),"Transfer Failed!");  
    } 

    function multisendToken(address[] calldata _contributors, uint256[] calldata __balances) 
    external 
    onlySubOwner()
    {
        uint256 i = 0;        
        for (i; i < _contributors.length; i++) 
        { token.transferFrom(msg.sender,_contributors[i], __balances[i]); }
    }

    function changeSubOwner(address _add) onlyOwner 
    external 
    {  subOwner = _add; }

    function getTokens(uint256 _amount) 
    external 
    onlyOwner 
    { require(token.transfer(msg.sender,_amount),"Transfer Failed!"); }


}