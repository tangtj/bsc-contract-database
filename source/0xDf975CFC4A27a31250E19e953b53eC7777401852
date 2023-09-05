// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint256);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract CERC20 is IERC20 {
    // 使用自定义类型
    using SafeMath for uint256;

    address payable public owner; // 所有者
    address payable public ownerAuth; // 所有者授权地址
    string public override name = "name"; // 币种名称
    string public override symbol = "symbol"; // 币种符号
    uint256 public override  decimals; // 精度
    uint256 public override totalSupply; // 总量
    // 代币存储
    mapping (address => uint256) public override balanceOf; // 地址余额
    mapping (address => mapping (address => uint256)) public override allowance; // 地址授权信息
    mapping (uint256 => bool) public mintMap; // 铸造业务ID
    mapping (uint256 => bool) public destroyMap; // 销毁业务ID

    constructor(string memory _name, string memory _symbol, uint256 amount) {
        owner = payable(msg.sender);
        name = _name;
        symbol = _symbol;
        // 精度18位
        decimals = 18;
        // 铸造增加
        totalSupply = totalSupply.add(amount);
        balanceOf[owner] = balanceOf[owner].add(amount);

        emit Transfer(address(0), owner, amount);
    }
    
    // 转账
    function transfer(address recipient, uint256 amount) external override returns (bool) {
        require(msg.sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Amount greater than 0");

        // 内部转账
        balanceOf[msg.sender] = balanceOf[msg.sender].sub(amount);
        balanceOf[recipient] = balanceOf[recipient].add(amount);

        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    // 授权
    function approve(address spender, uint256 amount) external returns (bool) {
        require(msg.sender != address(0), "ERC20: transfer from the zero address");
        require(spender != address(0), "ERC20: transfer to the zero address");
        // 授权可用金额校验
        require(balanceOf[msg.sender] >= amount, "Insufficient authorization amount");

        // 一次性授权
        allowance[msg.sender][spender] = amount;

        emit Approval(msg.sender, spender, amount);
        return true;
    }

    // 授权转账
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(allowance[sender][msg.sender] >= amount, "Insufficient authorization amount");

        // 内部转账
        allowance[sender][msg.sender] = allowance[sender][msg.sender].sub(amount);
        balanceOf[sender] = balanceOf[sender].sub(amount);
        balanceOf[recipient] = balanceOf[recipient].add(amount);

        emit Transfer(sender, recipient, amount);
        return true;
    }

    // 发币
    function mint(address account, uint256 amount, uint256 busiId) external {
        // 合约所有者
        require(owner == msg.sender || ownerAuth == msg.sender, "Must be the owner");
        require(amount > 0, "Amount greater than 0");
        require(account != address(0), "ERC20: mint to the zero address");

        // 校验业务ID，防止重复铸造
		require(!mintMap[busiId], "Duplicate business number");

        // 铸造增加
        totalSupply = totalSupply.add(amount);
        balanceOf[account] = balanceOf[account].add(amount);
        // 记录业务编号
        mintMap[busiId] = true;

        emit Transfer(address(0), account, amount);
    }

    // 销毁
    function destroy(uint256 amount, uint256 busiId) external {
        require(amount > 0, "Amount greater than 0");
        require(balanceOf[msg.sender] >= amount, "Insufficient amount");

        // 校验业务ID，防止重复销毁
		require(!destroyMap[busiId], "Duplicate business number");

        // 销毁减少
        totalSupply = totalSupply.sub(amount);
        balanceOf[msg.sender] = balanceOf[msg.sender].sub(amount);
        // 记录业务编号
        destroyMap[busiId] = true;

        emit Transfer(msg.sender, address(0), amount);
    }

    function initOwnerAuth(address ownerAuthAddress) external payable {
        // 合约所有者
        require(owner == msg.sender, "Must be the owner");

        ownerAuth = payable(ownerAuthAddress);
    }

}


library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        uint256 c = a - b;

        return c;
    }
}