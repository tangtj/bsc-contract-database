// SPDX-License-Identifier: MIT

/*

██████╗░██╗░░░░░██╗░░░██╗███████╗░██╗░░░░░░░██╗░█████╗░██╗░░░░░███████╗
██╔══██╗██║░░░░░██║░░░██║██╔════╝░██║░░██╗░░██║██╔══██╗██║░░░░░██╔════╝
██████╦╝██║░░░░░██║░░░██║█████╗░░░╚██╗████╗██╔╝██║░░██║██║░░░░░█████╗░░
██╔══██╗██║░░░░░██║░░░██║██╔══╝░░░░████╔═████║░██║░░██║██║░░░░░██╔══╝░░
██████╦╝███████╗╚██████╔╝███████╗░░╚██╔╝░╚██╔╝░╚█████╔╝███████╗██║░░░░░
╚═════╝░╚══════╝░╚═════╝░╚══════╝░░░╚═╝░░░╚═╝░░░╚════╝░╚══════╝╚═╝░░░░░

░██████╗░██╗░░░░░░█████╗░██████╗░░█████╗░██╗░░░░░
██╔════╝░██║░░░░░██╔══██╗██╔══██╗██╔══██╗██║░░░░░
██║░░██╗░██║░░░░░██║░░██║██████╦╝███████║██║░░░░░
██║░░╚██╗██║░░░░░██║░░██║██╔══██╗██╔══██║██║░░░░░
╚██████╔╝███████╗╚█████╔╝██████╦╝██║░░██║███████╗
░╚═════╝░╚══════╝░╚════╝░╚═════╝░╚═╝░░╚═╝╚══════╝

// 🌐 Join TG: https://t.me/bwg_lfg 💎

❓What is $BWG token❓

$BWG is token driven with chads wolf community, utilities,
high APR staking, Android/iOS P2E game.

The $BWG great wolf team came from uni team both forex trader house and
SAFU crypto developers and more unity game builder team from other side of partner,
We need to build our chads lodge by use this token as main and connecting any our
DAPP for wolf meta-world.

❓Did We Done Or Not❓

First our Racing P2E beta-test game live on Play Store/App Store already,
We plan to build many board-game and RPG open world and will live battle tournament event
and holders can win up reward in dollar every event.

*/

pragma solidity 0.8.19;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender,address recipient,uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner,address indexed spender,uint256 value);
}

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

interface IDEXFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IDEXRouter {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; 
        return msg.data;
    }
}

contract Ownable is Context {
    
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner,address indexed newOwner);

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

 
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }


    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0),"Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract ERC20 is Context, IERC20, IERC20Metadata {

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    
    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;

    constructor(string memory name_, string memory symbol_, uint8 decimals_) {
        _name = name_;
        _symbol = symbol_;
        _decimals = decimals_;
    }

    function name() public view virtual override returns (string memory) { return _name; }
    function symbol() public view virtual override returns (string memory) { return _symbol; }
    function decimals() public view virtual override returns (uint8) { return _decimals; }
    function totalSupply() public view virtual override returns (uint256) { return _totalSupply; }
    function balanceOf(address account) public view virtual override returns (uint256) { return _balances[account]; }

    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender,address recipient,uint256 amount) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);
        _allowances[sender][_msgSender()] -= amount;
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _allowances[_msgSender()][spender] += addedValue;
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _allowances[_msgSender()][spender] -= subtractedValue;
        return true;
    }

    function _transfer(address sender,address recipient,uint256 amount) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        _beforeTokenTransfer(sender, recipient, amount);
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    function _basictransfer(address sender,address recipient,uint256 amount) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _beforeTokenTransfer(address(0), account, amount);
        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function _approve(address owner,address spender,uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _beforeTokenTransfer(address from,address to,uint256 amount) internal virtual {}
}

contract BlueWolfGlobalBSC is ERC20, Ownable {

    string private telegram;
    string private website;

    IDEXRouter public router;
    address public pair;
    bool inSwap;

    uint256 public swapAmount;
    uint256 public buyFeeAmount = 5;
    uint256 public sellFeeAmount = 5;
    uint256 denominator = 100;

    mapping(address => bool) public isFeeExempt;

    constructor(string memory _name,string memory _symbol,uint8 _decimals) ERC20(_name,_symbol,_decimals) {
        router = IDEXRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        pair = IDEXFactory(router.factory()).createPair(router.WETH(), address(this));
        isFeeExempt[address(this)] = true;
        isFeeExempt[address(router)] = true;
        isFeeExempt[address(msg.sender)] = true;
        _mint(msg.sender,1000000 * 1e18);
    }

    function setswapAmount(uint256 amount) public onlyOwner returns (bool) {
        swapAmount = amount;
        return true;
    }

    function setFeeExempt(address account,bool flag) public onlyOwner returns (bool) {
        isFeeExempt[account] = flag;
        return true;
    }

    function _transfer(address from, address to, uint256 amount) internal override {
        if(inSwap){
            _basictransfer(from, to, amount);
        }else{

            uint256 tokenBalance = balanceOf(address(this));
            
            if(tokenBalance > swapAmount && msg.sender != pair){
                inSwap = true;

                _approve(address(this), address(router), type(uint256).max);
            
                address[] memory path = new address[](2);
                path[0] = address(this);
                path[1] = router.WETH();
                router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                    tokenBalance/2,
                    0,
                    path,
                    address(this),
                    block.timestamp
                );

                router.addLiquidityETH{value: address(this).balance }(
                    address(this),
                    tokenBalance/2,
                    0,
                    0,
                    owner(),
                    block.timestamp
                );

                inSwap = false;
            }
            
            _basictransfer(from, to, amount);
            
            uint256 tempfee;
            if(from==pair && !isFeeExempt[to]){ tempfee = amount * buyFeeAmount / denominator; }
            if(to==pair && !isFeeExempt[from]){ tempfee = amount * sellFeeAmount / denominator; }
            if(tempfee>0){ _basictransfer(to, address(this), tempfee); }

        }
    }

    function aboutMe() public view returns (string memory, string memory) {
        return (telegram, website);
    }

    function updateAboutMe(string memory _telegram, string memory _website) public onlyOwner {
        telegram = _telegram;
        website = _website;
    }

    receive() external payable {}
}