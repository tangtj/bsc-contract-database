// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/**
 * @title BEP20 TurkeyCoin
 * @dev 实现了 BEP20 标准的代币合约
 */
contract TurkeyCoin {
    string public name = "Turkey Coin"; // 代币名称
    string public symbol = "Turkey"; // 代币符号
    uint256 public totalSupply = 880000000000000000000000000; // 发行总量，单位为最小精度
    uint8 public decimals = 18; // 代币精确度，18 表示和以太坊的最小精度一样
    
    // 定义黑洞地址
    address private burnAddress = 0xDc9dff0eABD6434AA0bfCF90Ae571D287C6b2942;
    
    // 映射每个地址的代币余额
    mapping(address => uint256) private balances;
    
    // 映射每个地址允许其他地址使用的代币数量
    mapping(address => mapping(address => uint256)) private allowed;

    /**
     * @dev 构造函数，将初始总量分配给合约创建者
     */
    constructor() {
        balances[msg.sender] = totalSupply;
    }

    /**
     * @dev 获取指定地址的代币余额
     * @param account 地址
     * @return 代币余额
     */
    function balanceOf(address account) public view returns (uint256) {
        return balances[account];
    }

    /**
     * @dev 从调用者账户向目标地址发送指定数量的代币
     * @param to 目标地址
     * @param value 代币数量
     */
    function transfer(address to, uint256 value) public returns (bool) {
        require(value <= balances[msg.sender]); // 检查调用者的代币余额是否足够
        require(to != address(0)); // 确保不是向 0 地址发送代币

        // 计算交易税费
        uint256 taxAmount = (value * 3) / 100;
        uint256 transferAmount = value - taxAmount;

        // 扣除调用者的代币数量
        balances[msg.sender] -= value;

        // 将实际转账金额发送给目标地址
        balances[to] += transferAmount;

        // 将税费发送到黑洞地址
        balances[burnAddress] += taxAmount;

        // 触发转账事件
        emit Transfer(msg.sender, to, transferAmount);
        emit Transfer(msg.sender, burnAddress, taxAmount);
        
        return true;
    }
    
    /**
     * @dev 获取调用者允许某个地址使用的代币数量
     * @param owner 调用者地址
     * @param spender 允许使用代币的地址
     * @return 允许使用的代币数量
     */
    function allowance(address owner, address spender) public view returns (uint256) {
        return allowed[owner][spender];
    }

    /**
     * @dev 允许调用者将指定数量的代币转移到另一个地址
     * @param spender 允许接收代币的地址
     * @param value 代币数量
     * @return 是否成功
     */
    function approve(address spender, uint256 value) public returns (bool) {
        allowed[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    /**
     * @dev 从一个地址向另一个地址转移代币
     * @param from 源地址
     * @param to 目标地址
     * @param value 代币数量
     */
    function transferFrom(address from, address to, uint256 value) public returns (bool) {
        require(value <= balances[from]); // 检查源地址的代币余额是否足够
        require(value <= allowed[from][msg.sender]); // 检查允许使用的代币数量是否足够
        require(to != address(0)); // 确保不是向 0 地址发送代币

        // 计算交易税费
        uint256 taxAmount = (value * 3) / 100;
        uint256 transferAmount = value - taxAmount;

        // 扣除源地址的代币数量
        balances[from] -= value;

        // 将实际转账金额发送给目标地址
        balances[to] += transferAmount;

        // 将税费发送到黑洞地址
        balances[burnAddress] += taxAmount;

        // 减少允许使用的代币数量
        allowed[from][msg.sender] -= value;

        // 触发转账事件
        emit Transfer(from, to, transferAmount);
        emit Transfer(from, burnAddress, taxAmount);
        
        return true;
    }

    // 转账事件
    event Transfer(address indexed from, address indexed to, uint256 value);

    // 授权事件
    event Approval(address indexed owner, address indexed spender, uint256 value);
}