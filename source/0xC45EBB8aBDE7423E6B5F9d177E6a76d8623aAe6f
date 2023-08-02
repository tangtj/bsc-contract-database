//SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

interface IBEP20 {
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

interface IBEP20Metadata is IBEP20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

contract BEP20 is Context, IBEP20, IBEP20Metadata {
    mapping(address => uint256) internal _balances;

    mapping(address => mapping(address => uint256)) internal _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "BEP20: transfer amount exceeds allowance");
        _approve(sender, _msgSender(), currentAllowance - amount);

        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "BEP20: decreased allowance below zero");
        _approve(_msgSender(), spender, currentAllowance - subtractedValue);

        return true;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "BEP20: transfer amount exceeds balance");
        _balances[sender] = senderBalance - amount;
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }

    function _tokengeneration(address account, uint256 amount) internal virtual {
        _totalSupply = amount;
        _balances[account] = amount;
        emit Transfer(address(0), account, amount);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}

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

interface IFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Router02 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
}

contract XShiba is BEP20, Ownable {
    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;
    bool public tradingEnabled = false;
   
    //whitelist function, used temporary by locks or presale
    mapping(address => bool) public whitelist;

   //Contract Update Information
    string public constant Contract_Version = "0.8.19";
    string public constant Contract_Dev_By = "Team ANOOP SAFU DEV || NFA,DYOR";
    string public constant Contract_Edition = "CA For Presale";
    address public constant deadWallet = 0x000000000000000000000000000000000000dEaD;
    
    // Events
    event TradingOpenUpdated();
    event ETHBalanceRecovered();
    event ERC20TokensBurned();
    event ERC20TokensRecovered(uint256 indexed _amount);
    event ExcludeFromFeeUpdated(address indexed account);
    event includeFromFeeUpdated(address indexed account);
    
    constructor() BEP20("XShiba", "XShiba") {
        _tokengeneration(msg.sender,  100_000_000 * 10**decimals());
            if (block.chainid == 56){
     uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E); // PCS BSC Mainnet Router
     }
      else if(block.chainid == 1 || block.chainid == 4 || block.chainid == 3 || block.chainid == 5){
          uniswapV2Router = IUniswapV2Router02(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);  // Uniswap ETH Mainnet Router
      }
      else if(block.chainid == 42161){
           uniswapV2Router = IUniswapV2Router02(0x1b02dA8Cb0d097eB8D57A175b88c7D8b47997506); // Sushi Arbitrum Mainnet Router
      }
     else  if (block.chainid == 97){
     uniswapV2Router = IUniswapV2Router02(0xD99D1c33F9fC3444f8101754aBC46c52416550D1); // PCS BSC Testnet Router
     }
    else  if (block.chainid == 8453){
     uniswapV2Router = IUniswapV2Router02(0xfCD3842f85ed87ba2889b4D35893403796e67FF1); // BaseChian LeetSwap Router
     }
    else {
         revert("Wrong Chain Id");
        }
    uniswapV2Pair = IFactory(uniswapV2Router.factory()).createPair(address(this), uniswapV2Router.WETH());

        whitelist[msg.sender] = true;
        whitelist[address(this)] = true;
        whitelist[deadWallet] = true;
        whitelist[0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE] = true; // BSC PinkSale Lock
        whitelist[0x5E5b9bE5fd939c578ABE5800a90C566eeEbA44a5] = true; // Tesnet PinkSale Lock
        whitelist[0xeBb415084Ce323338CFD3174162964CC23753dFD] = true; // Arbitrum PinkSale Lock
        whitelist[0x71B5759d73262FBb223956913ecF4ecC51057641] = true; // ETH PinkSale Lock
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "BEP20: transfer amount exceeds allowance");
        _approve(sender, _msgSender(), currentAllowance - amount);

        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        override
        returns (bool)
    {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "BEP20: decreased allowance below zero");
        _approve(_msgSender(), spender, currentAllowance - subtractedValue);

        return true;
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal override {
        require(amount > 0, "Transfer amount must be greater than zero");

        if (!whitelist[sender] && !whitelist[recipient]) {
            require(tradingEnabled, "Trading not enabled");
        }
       
        super._transfer(sender, recipient, amount);
    }

    function go_live() external onlyOwner {
        require(!tradingEnabled, "Trading is already enabled");
        tradingEnabled = true;
       emit TradingOpenUpdated();
    }
   
    function excludeFromFee(address _address) external onlyOwner {
        require(whitelist[_address] != true,"Account is already excluded");
        whitelist[_address] = true;
       emit ExcludeFromFeeUpdated(_address);
    }
   
    function includeFromFee(address _address) external onlyOwner {
        require(whitelist[_address] != false,"Account is already included");
        whitelist[_address] = false;
       emit includeFromFeeUpdated(_address);
    }

   function rescueETH() external {
        uint256 contractETHBalance = address(this).balance;
        require(contractETHBalance > 0, "Amount should be greater than zero");
        payable(owner()).transfer(contractETHBalance);
      emit ETHBalanceRecovered();
    }

   function rescueERC20(address _tokenAddy, uint256 _amount) external {
        require(_tokenAddy != address(this), "Owner can't claim contract's balance of its own tokens");
        require(_amount > 0, "Amount should be greater than zero");
        require(_amount <= IBEP20(_tokenAddy).balanceOf(address(this)), "Insufficient Amount");
        IBEP20(_tokenAddy).transfer(owner(), _amount);
      emit ERC20TokensRecovered(_amount); 
    }

    function burnERC20(address _tokenAdd, uint256 _amount) external onlyOwner {
       require(_amount > 0, "Amount should be greater than zero");
        IBEP20(_tokenAdd).transfer(deadWallet, _amount);
      emit ERC20TokensBurned();
    }

    // fallbacks
    receive() external payable {}
}