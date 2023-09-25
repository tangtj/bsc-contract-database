// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// ERC-20标准接口
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

contract GRINCHToken is IERC20 {
    string public name = "GRINCH";            // 代币名称
    string public symbol = "GRINCH";          // 代币符号
    uint8 public decimals = 18;               // 小数位数（默认为18）
    uint256 private _totalSupply;            // 总供应量

    address public usdtContractAddress = 0x85A0380C810782b9E76deFd8AE0f011c89BBcAba; // USDT合约地址

    address private _owner; // 合约拥有者
 uint256 public constant usdtToGrinchRate = 1000; // 1 USDT = 1000 GRINCH

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    modifier onlyOwner() {
        require(msg.sender == _owner, "Only the owner can call this function");
        _;
    }

  

    constructor() {
        _owner = msg.sender;
        _totalSupply = 1111111000 * 10 ** uint256(decimals); // 设置总供应量
        _balances[address(this)] = _totalSupply; // 将代币放入合约地址
        emit Transfer(address(0), address(this), _totalSupply);
    }

    // 返回总供应量
    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    // 返回指定地址的余额
    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    // 转账代币
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(address(this), recipient, amount);
        return true;
    }

    // 授权他人代币转账
    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(address(this), spender, amount);
        return true;
    }

    // 返回授权额度
    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    // 代币转账
    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender] - amount);
        return true;
    }

    // 内部方法：代币转账
    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "Transfer from the zero address");
        require(recipient != address(0), "Transfer to the zero address");
        require(_balances[sender] >= amount, "Insufficient balance");
        
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    // 内部方法：授权他人代币转账
    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "Approval from the zero address");
        require(spender != address(0), "Approval to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

 
 // 放弃管理员权限
    function renounceAdmin() public onlyOwner {
        _owner = address(0); // 设置 _owner 为零地址
    }
  
    // 接收USDT并返回相应数量的GRINCH
    function receiveUSDTAndSendGRINCH(uint256 usdtAmount) external {
        require(msg.sender != address(0), "Sender address must not be zero");
        require(usdtAmount > 0, "USDT amount must be greater than zero");
        
        // 使用外部合约调用来转移USDT到合约的地址
        (bool usdtTransferSuccess, ) = usdtContractAddress.call(
            abi.encodeWithSignature("transferFrom(address,address,uint256)", msg.sender, address(this), usdtAmount)
        );

        require(usdtTransferSuccess, "USDT transfer failed");
        
        // 计算要发送的GRINCH数量
        uint256 grinAmount = usdtAmount * usdtToGrinchRate;
        
        // 发送GRINCH到发送者地址
        _transfer(address(this), msg.sender, grinAmount);
    }
}