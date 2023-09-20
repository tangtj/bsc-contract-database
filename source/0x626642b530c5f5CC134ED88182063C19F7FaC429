// SPDX-License-Identifier: SimPL-2.0

pragma solidity ^0.8.0;

interface IERC20 {
    // 返回存在的代币数量
    function totalSupply() external view returns (uint256);

    // 返回 account 拥有的代币数量
    function balanceOf(address account) external view returns (uint256);

    // 将 amount 代币从调用者账户移动到 recipient
    // 返回一个布尔值表示操作是否成功
    // 发出 {Transfer} 事件
    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    // 返回 spender 允许 owner 通过 {transferFrom}消费剩余的代币数量
    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    // 调用者设置 spender 消费自己amount数量的代币
    function approve(address spender, uint256 amount) external returns (bool);

    // 将amount数量的代币从 sender 移动到 recipient ，从调用者的账户扣除 amount
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);
    function toBurn(uint _amount) external;
    // 当value数量的代币从一个form账户移动到另一个to账户
    event Transfer(address indexed from, address indexed to, uint256 value);
    // 当调用{approve}时，触发该事件
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}




contract LRLockDes{

    address public owner;
    address public usdt = 0xA579fda72b2F56BE94805b27dE316acf5CF3Fb8d;
    address public collected = 0x98B902De0181f0EFd062A0a49999EdC04974599D;

    mapping(uint => uint) public configLists;


    event userLockDesposite(address user,uint256 amounts,uint typeIndex);

    constructor() payable{
        owner = msg.sender;

    }

    receive() external payable {}

    modifier isOnwer(){
        require(msg.sender == owner);
        _;
    }

    function setOwner(address own) public isOnwer{
        owner = own;
    }

    function setUsdtAddress(address adr) public isOnwer{
        usdt = adr;
    }

    function setCollectedAddress(address adr) public isOnwer{
        collected = adr;
    }

    function setConfigLists(uint typeIndex,uint amount) public isOnwer{
        configLists[typeIndex] = amount;
    }

    function desposite(uint256 amounts,uint typeIndex) public {
        require(configLists[typeIndex]>0,"this type is not allowed");
        require(amounts>configLists[typeIndex],"amount is too low");
        IERC20(usdt).transferFrom(msg.sender,collected,amounts);      
        emit userLockDesposite(msg.sender,amounts,typeIndex);
    }

}