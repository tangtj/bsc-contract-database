// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// واجهة IBEP20 المعتادة للتوافق مع معايير BEP20
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

// العقد الرئيسي
contract AdvancedTaxableBEP20 is IBEP20 {
    // المتغيرات العامة للرمز
    string public name = "gogo";
    string public symbol = "gogo";
    uint8 public decimals = 18;

    // تعريف المتغيرات المتعلقة بالضرائب والمحفظة
    uint256 private _totalSupply = 1000000 * 10**18; 
    uint256 public buyTaxRate = 10; // ضريبة الشراء
    uint256 public sellTaxRate = 10; // ضريبة البيع
    address public taxWallet; // عنوان محفظة الضرائب
    address private _owner; // مالك العقد

    // الخرائط لتخزين الرصيد والتفويضات
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    // حدث لتسجيل تغييرات الضرائب
    event UpdatedTaxRates(uint256 buyTaxRate, uint256 sellTaxRate);

    // البناء
    constructor(address _taxWallet) {
        _owner = msg.sender; // تعيين المرسل كمالك
        _balances[_owner] = _totalSupply; // منح المالك الكمية الإجمالية من الرموز
        setTaxWallet(_taxWallet); // تعيين محفظة الضرائب
    }

    // تعريف المعدّل
    modifier onlyOwner() {
        require(_owner == msg.sender, "Not the contract owner");
        _;
    }

    // وظيفة لتعيين نسب الضرائب
    function setTaxRates(uint256 buyRate, uint256 sellRate) external onlyOwner {
        buyTaxRate = buyRate;
        sellTaxRate = sellRate;
        emit UpdatedTaxRates(buyRate, sellRate);
    }

    // وظيفة لتعيين محفظة الضرائب
    function setTaxWallet(address newTaxWallet) public onlyOwner {
        require(newTaxWallet != address(0), "Cannot set tax wallet to the zero address");
        taxWallet = newTaxWallet;
    }

    // الوظائف الأساسية لمعيار BEP20
    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender] - amount);
        return true;
    }

    // الوظيفة الداخلية لنقل الرموز
    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        uint256 taxAmount = 0;
        if (recipient == address(this)) {
            taxAmount = amount * buyTaxRate / 100;
        } else if (sender == address(this)) {
            taxAmount = amount * sellTaxRate / 100;
        }

        _balances[sender] -= amount;
        _balances[recipient] += amount - taxAmount;
        _balances[taxWallet] += taxAmount;

        emit Transfer(sender, recipient, amount - taxAmount);
        if (taxAmount > 0) {
            emit Transfer(sender, taxWallet, taxAmount);
        }
    }

    // الوظيفة الداخلية للموافقة
    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}