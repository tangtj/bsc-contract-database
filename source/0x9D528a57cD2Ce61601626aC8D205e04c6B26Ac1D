// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

/**
 * The People's Token! By the People For The People!
 * Dedicated to the OG Day 1 FOUNDRz of the $EMOT Family!!
 * Special thanks to Positivelife90
 */

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a / b;
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

abstract contract Pausable is Context {
    event Paused(address account);

    event Unpaused(address account);

    bool private _paused;

    constructor() {
        _paused = false;
    }

    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }

    modifier whenPaused() {
        _requirePaused();
        _;
    }

    function paused() public view virtual returns (bool) {
        return _paused;
    }

    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
    }

    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}

interface IERC20 {
    function approve(address spender, uint256 amount) external returns (bool);

    function balanceOf(address account) external view returns (uint256);
}

interface IPancakeV2Factory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface IPancakeV2Router02 {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    )
        external
        payable
        returns (uint amountToken, uint amountETH, uint liquidity);
}

contract EMOTTOKEN is Pausable, Ownable {
    using SafeMath for uint256;

    uint8 private _decimals;

    uint256 private _totalSupply;
    uint256 public WhaleTaxAvail;
    uint256 public WhaleTaxTotal;
    uint256 public tooMuch;
    uint256 private taxFee;
    uint256 private minBalance;
    uint256 private holderPerk;

    uint256[4] public TaxDistribution;
    uint256[2] public WhaleTaxDistribution;
    uint256 public _taxSwapThreshold = 0 * 10 ** _decimals;

    address public pancakeV2Pair;

    address[4] public wallets;
    address[] private listedHolders;
    address payable private _taxWallet;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private tokenBlacklist;
    mapping(address => bool) private tokenTaxFreeList;
    mapping(address => bool) public isListedHolder;
    mapping(address => bool) public removedListedHolder;
    mapping(address => bool) private isDexAddress;

    string private _name;
    string private _symbol;

    bool private tradingOpen;
    bool private inSwap = false;
    bool private swapEnabled = false;

    IPancakeV2Router02 public pancakeV2Router;

    event Blacklist(address indexed blackListed, bool value);
    event TaxFreeList(address indexed taxFreeListed, bool value);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor(
        string memory _tname,
        string memory _tsymbol,
        uint8 _decimal,
        uint256 _supply,
        address _tokenOwner,
        address[4] memory _wallets,
        uint256[4] memory _taxDistribution,
        uint256[4] memory _distribution,
        address _pancakeV2Router,
        uint256 _tax,
        uint256 _TMDivider
    ) {
        _name = _tname;
        _symbol = _tsymbol;
        wallets = _wallets;
        _decimals = _decimal;
        _mint(_tokenOwner, _supply * 10 ** _decimal);
        pancakeV2Router = IPancakeV2Router02(_pancakeV2Router);
        taxFee = _tax;
        tooMuch = (_supply * 10 ** _decimal) / _TMDivider;
        TaxDistribution = _taxDistribution;
        WhaleTaxDistribution[0] = _distribution[0];
        WhaleTaxDistribution[1] = _distribution[1];
        minBalance = _distribution[2];
        holderPerk = _distribution[3];
        _taxWallet = payable(msg.sender);
    }

    function updateDexContract(
        address _pair,
        address _router
    ) public onlyOwner {
        pancakeV2Pair = _pair;
        pancakeV2Router = IPancakeV2Router02(_router);
    }

    function updateTSThreshold(uint256 _newTST) public onlyOwner {
        _taxSwapThreshold = _newTST;
    }

    function setTaxWallet(address payable _tWallet) public {
        {
            require(
                _msgSender() == owner() || _msgSender() == _taxWallet,
                "Not owner or tax wallet"
            );
            _taxWallet = _tWallet;
        }
    }

    function updateListedHolder(
        address[] memory _holders,
        bool[] memory _mode
    ) public onlyOwner {
        require(_holders.length == _mode.length, "length not match");
        for (uint256 i = 0; i < _holders.length; i++) {
            address hol = _holders[i];
            bool _m = _mode[i];
            if (_m == true) {
                isListedHolder[hol] = _m;
            } else if (_m == false) {
                removedListedHolder[hol] = _m;
                isListedHolder[hol] = _m;
            }
        }
    }

    function sendTokenToPrevHolder(
        address _tokenAddress,
        address[] memory _holders
    ) public onlyOwner {
        IERC20 tokenContract = IERC20(_tokenAddress);
        for (uint256 i = 0; i < _holders.length; i++) {
            uint256 hBal = tokenContract.balanceOf(_holders[i]);
            _transferStandard(msg.sender, _holders[i], hBal);
        }
    }

    function manualSwap() public {
        require(_msgSender() == _taxWallet);
        uint256 tokenBalance = balanceOf(address(this));
        if (tokenBalance > 0) {
            swapTokensForEth(tokenBalance);
        }
        uint256 ethBalance = address(this).balance;
        if (ethBalance > 0) {
            sendETHToFee(ethBalance);
        }
    }

    function blackListAddress(
        address listAddress,
        bool isBlackListed
    ) public whenNotPaused onlyOwner returns (bool success) {
        return _blackList(listAddress, isBlackListed);
    }

    function taxFreeListAddress(
        address listAddress,
        bool istaxFreeListed
    ) public whenNotPaused onlyOwner returns (bool success) {
        return _taxFreeList(listAddress, istaxFreeListed);
    }

    function setTooMuch(uint256 _amount) public onlyOwner {
        tooMuch = _totalSupply / _amount;
    }

    function setWallets(
        address[4] memory _wallets,
        uint256[4] memory _taxDistribution
    ) public onlyOwner {
        wallets = _wallets;
        TaxDistribution = _taxDistribution;
    }

    function updateWTD(uint256[2] memory _WTD) public onlyOwner {
        WhaleTaxDistribution = _WTD;
    }

    function updateMH(
        uint256 _newMBalance,
        uint256 _newMHperk
    ) public onlyOwner {
        minBalance = _newMBalance;
        holderPerk = _newMHperk;
    }

    function addDexAddress(
        address[] memory _dexs,
        bool[] memory _modes
    ) public onlyOwner {
        require(_dexs.length == _modes.length, "length");
        for (uint256 i = 0; i < _dexs.length; i++) {
            isDexAddress[_dexs[i]] = _modes[i];
        }
    }

    function pause() public onlyOwner {
        _pause();
    }

    function unpause() public onlyOwner {
        _unpause();
    }

    receive() external payable {}

    function setSwapEnabled() public onlyOwner {
        swapEnabled = !swapEnabled;
    }

    function addLiquidity() external onlyOwner {
        _approve(address(this), address(pancakeV2Router), _totalSupply);
        if (tradingOpen == false) {
            pancakeV2Pair = IPancakeV2Factory(pancakeV2Router.factory())
                .createPair(address(this), pancakeV2Router.WETH());
        }

        pancakeV2Router.addLiquidityETH{value: address(this).balance}(
            address(this),
            balanceOf(address(this)),
            0,
            0,
            owner(),
            block.timestamp
        );

        tradingOpen = true;
        swapEnabled = true;
        IERC20(pancakeV2Pair).approve(address(pancakeV2Router), type(uint).max);
        isDexAddress[pancakeV2Pair] = true;
        isDexAddress[address(pancakeV2Router)] = true;
    }

    function transfer(
        address _to,
        uint256 _value
    ) public whenNotPaused returns (bool) {
        require(tokenBlacklist[msg.sender] == false);
        _transfer(msg.sender, _to, _value);
        return true;
    }

    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    ) public returns (bool) {
        require(tokenBlacklist[_from] == false);
        _transfer(_from, _to, _value);
        _spendAllowance(_from, msg.sender, _value);
        return true;
    }

    function _transfer(
        address _from,
        address _to,
        uint256 _value
    ) internal returns (bool) {
        require(_value <= _balances[_from]);
        uint256 _Tax;
        uint256 _WTax;
        if (!isListedHolder[_to] && !removedListedHolder[_to]) {
            if (!isDexAddress[_to]) {
                isListedHolder[_to] = true;
                listedHolders.push(_to);
            }
        }
        if (tokenTaxFreeList[_from] == true) {
            _transferStandard(_from, _to, _value);
            return true;
        } else if (!isDexAddress[_from] && !isDexAddress[_to]) {
            _transferStandard(_from, _to, _value);
            return true;
        } else if (_from == address(this)) {
            _transferStandard(_from, _to, _value);
            return true;
        } else if (_value <= tooMuch) {
            _Tax = _value.mul(taxFee).div(100);
            _value = _value.sub(_Tax);
            _transferStandard(_from, _to, _value);
            _distributeTax(_from, _Tax);
            // return true;
        } else if (_value > tooMuch) {
            if (_from != pancakeV2Pair) {
                _transferStandard(_from, address(this), _value);
                uint256 _whaleTax_PayOuts;
                _WTax = _value.mul(WhaleTaxDistribution[0]).div(100);
                _value = _value.sub(_WTax);
                _Tax = _WTax.mul(WhaleTaxDistribution[1]).div(100);
                _whaleTax_PayOuts = _WTax.sub(_Tax);
                WhaleTaxTotal += _whaleTax_PayOuts;
                WhaleTaxAvail = _whaleTax_PayOuts;
                _distributeTax(address(this), _Tax);
                _transferStandard(address(this), _to, _value);
                _disperseWhaleTax(address(this), _whaleTax_PayOuts);

                //   return true;
            } else {
                _Tax = _value.mul(taxFee).div(100);
                _value = _value.sub(_Tax);
                _transferStandard(_from, _to, _value);
                _distributeTax(_from, _Tax);
                return true;
            }
        }
         uint256 contractTokenBalance = balanceOf(address(this));
        if (
            !inSwap &&
            _from != pancakeV2Pair &&
            swapEnabled &&
            contractTokenBalance > _taxSwapThreshold
        ) {
            uint256 tokenBalance = balanceOf(address(this));
            if (tokenBalance > 0) {
                swapTokensForEth(tokenBalance);
            }
            uint256 ethBalance = address(this).balance;
            if (ethBalance > 0) {
                sendETHToFee(ethBalance);
            }
        }
        return true;
    }

    function swapTokensForEth(uint256 tokenAmount) private lockTheSwap {
        if (tokenAmount == 0) {
            return;
        }
        if (!tradingOpen) {
            return;
        }
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = pancakeV2Router.WETH();
        _approve(address(this), address(pancakeV2Router), tokenAmount);
        pancakeV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp + 5 minutes
        );
    }

    function sendETHToFee(uint256 amount) private {
        _taxWallet.transfer(amount);
    }

    function _transferStandard(
        address from,
        address to,
        uint256 amount
    ) internal {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        uint256 fromBalance = _balances[from];
        require(
            fromBalance >= amount,
            "ERC20: transfer amount exceeds balance"
        );
        unchecked {
            _balances[from] = fromBalance - amount;
            _balances[to] += amount;
        }

        emit Transfer(from, to, amount);
    }

    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(
                currentAllowance >= amount,
                "ERC20: insufficient allowance"
            );
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    function _distributeTax(address _from, uint256 taxAmount) internal {
        address[4] memory _wallets = wallets;
        uint256[4] memory _distribution = TaxDistribution;
        for (uint256 i = 0; i < _wallets.length; i++) {
            uint256 walletShare = taxAmount.mul(_distribution[i]).div(100);
            address wAddress = _wallets[i];

            if (wAddress == 0x000000000000000000000000000000000000dEaD) {
                _burn(_from, walletShare);
            } else {
                _transferStandard(_from, wAddress, walletShare);
            }
        }
    }

    function _disperseWhaleTax(address _from, uint256 WTamount) internal {
        address[] memory walHODLrz = listedHolders;
        uint256 totalWalDist = walHODLrz.length;
        uint256 HODLrzPerk = holderPerk;
        uint256 mBalance = minBalance;
        uint256 HODLrzPerkWals;

        for (uint256 i = 0; i < walHODLrz.length; i++) {
            address wallet = walHODLrz[i];
            uint256 bal = balanceOf(wallet);
            if (bal >= holderPerk) {
                HODLrzPerkWals++;
            }
        }
        uint256 HODLrzPerkWalDist = HODLrzPerkWals.mul(20).div(100);
        uint256 t = totalWalDist.add(HODLrzPerkWalDist);
        uint256 WhaleWalletShare = WTamount.div(t);

        for (uint256 i = 0; i < totalWalDist; i++) {
            address wallet = walHODLrz[i];
            if (
                wallet != address(0) &&
                wallet != 0x000000000000000000000000000000000000dEaD &&
                isListedHolder[wallet] &&
                !removedListedHolder[wallet]
            ) {
                uint256 walletBalance = balanceOf(wallet);
                if (
                    !tokenBlacklist[wallet] &&
                    wallet != owner() &&
                    wallet != address(this) &&
                    walletBalance >= mBalance &&
                    WhaleWalletShare != 0
                ) {
                    if (walletBalance >= HODLrzPerk) {
                        if (
                            balanceOf(_from) >=
                            WhaleWalletShare.add(
                                WhaleWalletShare.mul(20).div(100)
                            )
                        ) {
                            _transferStandard(
                                _from,
                                wallet,
                                WhaleWalletShare.add(
                                    WhaleWalletShare.mul(20).div(100)
                                )
                            );
                        }
                    } else {
                        if (balanceOf(_from) >= WhaleWalletShare) {
                            _transferStandard(_from, wallet, WhaleWalletShare);
                        }
                    }
                }
            }
        }
    }

    function withdrawStuckETH() external onlyOwner {
        (bool success, ) = address(msg.sender).call{
            value: address(this).balance
        }("");
        require(success, "Withdraw_Failed");
        _transferStandard(address(this), msg.sender, balanceOf(address(this)));
    }

    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: burn from the zero address");
        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            _totalSupply -= amount;
        }

        emit Transfer(account, address(0), amount);
    }

    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: mint to the zero address");
        _totalSupply += amount;
        unchecked {
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);
    }

    function _blackList(
        address _address,
        bool _isBlackListed
    ) internal returns (bool) {
        require(tokenBlacklist[_address] != _isBlackListed);
        tokenBlacklist[_address] = _isBlackListed;
        emit Blacklist(_address, _isBlackListed);
        return true;
    }

    function _taxFreeList(
        address _address,
        bool _istaxFreeListed
    ) internal returns (bool) {
        require(tokenTaxFreeList[_address] != _istaxFreeListed);
        tokenTaxFreeList[_address] = _istaxFreeListed;
        emit TaxFreeList(_address, _istaxFreeListed);
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public whenNotPaused returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(
        address spender,
        uint256 subtractedValue
    ) public whenNotPaused returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    function approve(
        address spender,
        uint256 amount
    ) public whenNotPaused returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function allowance(
        address owner,
        address spender
    ) public view returns (uint256) {
        return _allowances[owner][spender];
    }
}
/**
*Cryptoneer1, Jeciera, & Clambo
*2023 to Infinity!!!
*/