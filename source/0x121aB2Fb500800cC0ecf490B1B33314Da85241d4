//SPDX-License-Identifier: MIT

/**


    Welcome to the immersive and 
    action-packed world of Mastery of Monsters, 
    an innovative game that offers players the opportunity 
    to become true Masters of Monsters. In this 
    whitepaper, we will explore in detail all 
    aspects of this universe, from the game's 
    overview to the economy, technology, 
    game mechanics and community interaction.


    Mastery of Monsters is more than just 
    a game, it's an immersive experience that 
    combines strategy, action and the 
    possibility of making real profits through 
    trading valuable digital assets. Embark on this 
    fascinating journey where the 
    line between virtual achievements and 
    real-world profits blurs.

    https://masteryofmonsters.com/
    https://mastery-of-monsters.gitbook.io/whitepaper-en/
    https://masteryofmonsters.com/whitepaper-en
    https://twitter.com/MasteryMonsters
    https://t.me/MasteryOfMonsters



*/

pragma solidity 0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
}



interface IUniswapV2Router02 is IUniswapV2Router01 {

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}


interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function createPair(address tokenA, address tokenB) external returns (address pair);
}


interface ITeamWallet {
    function setDistribution() external;
}


abstract contract Ownable is Context {
    address private _owner;

    /**
     * @dev The caller account is not authorized to perform an operation.
     */
    error OwnableUnauthorizedAccount(address account);

    /**
     * @dev The owner is not a valid owner account. (eg. `address(0)`)
     */
    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the address provided by the deployer as the initial owner.
     */
    constructor(address initialOwner) {
        _transferOwnership(initialOwner);
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
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


interface IERC20Metadata is IERC20 {

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

}


contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) internal _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 internal _totalSupply;

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

   function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        unchecked {
            _approve(sender, _msgSender(), currentAllowance - amount);
        }

        return true;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);

    }

    function _create(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: create to the zero address");

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);

    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

}



contract TeamDistribution is ERC20, Ownable  {

    address private addressUSDT     = address(0x55d398326f99059fF775485246999027B3197955);

    address private addressMOM      = address(0xA7d9D54A98aDC16E6aA980e845C740053b5B7771);

    address public ceoWallet        = address(0xdB05404e403967632Fba2AfDe8Da687189435Cc7);
    address public devWallet0       = address(0x0226161f7860a80367D21f680533977Be58bFF34);
    address public devWallet1       = address(0x5dE2114cd2b1a70256e544EF9e4E97965bdB388c);
    address public devWallet2       = address(0x1335C22b96f18C6Bd958D140704357DC59DC8A58);
    address public devWallet3       = address(0x47eE6D53A771407735ADbe40539df62aB8F75601);

    uint256 public ceoPercent = 419;
    uint256 public dev0Percent = 419;
    uint256 public dev1Percent = 419;
    uint256 public dev2Percent = 1000;
    uint256 public dev3Percent = 838;
    uint256 public totalPercent = 3095;

    mapping(address => bool) public mappingAuth;
    address[] allAddressesAuth;
    
    event SettedAuthWallet(address indexed account, bool boolean);

    constructor() ERC20("Team Address Distributor", "TeamDistributor 2") Ownable(_msgSender()) {
        mappingAuth[owner()] = true;

        if (addressMOM != address(0)) {
            mappingAuth[addressMOM] = true;
        }

        _create(owner(), 1 * (10 ** 18));
    }

    receive() external payable {}
    
    modifier onlyAuth() {
        require(_msgSender() == owner() || mappingAuth[_msgSender()], "Without permission");
        _;
    }
    
    function getAllAddressesAuthLength() external view returns (uint256) {
        return allAddressesAuth.length;
    }

    function getAllAddressesAuth() external view returns (address[] memory) {
        return allAddressesAuth;
    }

    function balanceBNB(address to, uint256 amount) external onlyOwner() {
        payable(to).transfer(amount);
    }

    function balanceERC20 (address token, address to, uint256 amount) external onlyOwner() {
        IERC20(token).transfer(to, amount);
    }

    function setDevsWallets(
        address _ceoWallet,
        address _devWallet0,
        address _devWallet1,
        address _devWallet2,
        address _devWallet3

        ) external onlyOwner() {

        ceoWallet      = _ceoWallet;
        devWallet0     = _devWallet0;
        devWallet1     = _devWallet1;
        devWallet2     = _devWallet2;
        devWallet3     = _devWallet3;

    }

    
    function setDistributionPercent(
        uint256 _ceoPercent,
        uint256 _dev0Percent,
        uint256 _dev1Percent,
        uint256 _dev2Percent,
        uint256 _dev3Percent,
        uint256 _totalPercent

        ) external onlyOwner() {

        ceoPercent  = _ceoPercent;
        dev0Percent = _dev0Percent;
        dev1Percent = _dev1Percent;
        dev2Percent = _dev2Percent;
        dev3Percent = _dev3Percent;
        totalPercent = _totalPercent;

    }

    function setMappingAuth(address account, bool boolean) external onlyOwner() {
        mappingAuth[account] = boolean;

        // Stores all addresses that at some point have already been set as Auth
        if (boolean)
        allAddressesAuth.push(account);

        emit SettedAuthWallet(account,boolean);
        
    }

    function setDistribution() external onlyAuth {
        uint256 fundsToTeam = IERC20(addressUSDT).balanceOf(address(this));

        IERC20(addressUSDT).transfer(ceoWallet, fundsToTeam * ceoPercent / totalPercent);
        IERC20(addressUSDT).transfer(devWallet0, fundsToTeam * dev0Percent / totalPercent);
        IERC20(addressUSDT).transfer(devWallet1, fundsToTeam * dev1Percent / totalPercent);
        IERC20(addressUSDT).transfer(devWallet2, fundsToTeam * dev2Percent / totalPercent);
        IERC20(addressUSDT).transfer(devWallet3, IERC20(addressUSDT).balanceOf(address(this)));

    }

}