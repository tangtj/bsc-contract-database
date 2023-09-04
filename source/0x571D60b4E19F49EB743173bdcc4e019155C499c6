// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}



interface IERC20 {
   
   
    function totalSupply() external view returns (uint256);


    function balanceOf(address account) external view returns (uint256);


    function transfer(address recipient, uint256 amount) external returns (bool);


    function allowance(address owner, address spender) external view returns (uint256);


    function approve(address spender, uint256 amount) external returns (bool);


    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);


    event Transfer(address indexed from, address indexed to, uint256 value);


    event Approval(address indexed owner, address indexed spender, uint256 value);
}



pragma solidity ^0.8.0;


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


pragma solidity ^0.8.0;

contract Swap is Ownable{

    address public token;

    address public usdt;

    uint256 public priceFee;

    address public funAddr;

    event Exchange(address indexed owner, uint256 amount);


    constructor(address _token,address _usdt,address _collection,uint256 _priceFee){
        token = _token;
        funAddr = _collection;
        priceFee = _priceFee;
        usdt = _usdt;
    }



    function setFee(uint256 _price) onlyOwner external {
        priceFee = _price;
    }

    function setCollecAddr(address _value) onlyOwner external {
        funAddr =  _value;
    }


    function setToken(address _budAddr) onlyOwner external {
        token = _budAddr;
    }


    function exchange(uint256 amount) payable external {
        require(amount > 0,"amount");
        require(msg.value >= priceFee, "value");
        IERC20(token).transferFrom(msg.sender,address(this),amount);
        IERC20(usdt).transfer(msg.sender,amount);
        payable(funAddr).transfer(priceFee);
        emit Exchange(msg.sender, amount);
    }


    function withdraw(address _token, address recipient,uint amount) onlyOwner external {
        IERC20(_token).transfer(recipient, amount);
    }

    function withdrawBNB() onlyOwner external {
        payable(owner()).transfer(address(this).balance);
    }


}