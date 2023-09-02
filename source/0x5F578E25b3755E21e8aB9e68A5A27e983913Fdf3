pragma solidity ^0.8.4;


interface IERC20 
{
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Treasury 
{
    address private _cryptoFlip;
    address public owner;
    
    receive() external payable {}

    constructor() {
      
        owner = msg.sender;
    }
    function setCryptoFlip(address cryptoFlip) external {
        require(msg.sender == owner, "not owner");
        _cryptoFlip = cryptoFlip;
    }

    function transferOwnership(address newOwner) external {
        require(msg.sender == owner, "not owner");
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        owner = newOwner;
    }
    
    function transfer(address spender, address token, uint256 amount) external returns (bool) {
        require(_cryptoFlip == msg.sender, "not allow");
        bool status = false;

        if(token == address(0)) {
            if(address(this).balance>=amount) {
                payable(spender).transfer(amount);
                status = true;
            }
        }
        else {
            if(IERC20(token).balanceOf(address(this))>=amount)
            {
                IERC20(token).approve(spender, amount);
                IERC20(token).transfer(spender, amount);
                status = true;
            }
        }
        return true;
    }

    function getBalance(address token) external view returns (uint256)
    {
        if(token == address(0)) {
            return address(this).balance;
        }
        else {
            return IERC20(token).balanceOf(address(this));
        }
    }

    function widthdraw(uint256 amount, address token) external 
    {
        require(msg.sender == owner, "not owner");
        if(token == address(0)) {
            payable(msg.sender).transfer(amount);
        }
        else {
            IERC20(token).transfer(msg.sender, amount);
        }
    }
    
     
}