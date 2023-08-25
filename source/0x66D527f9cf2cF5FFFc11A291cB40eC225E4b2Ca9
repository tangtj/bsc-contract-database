// SPDX-License-Identifier: MIT

pragma solidity ^0.6.7;

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; 
        return msg.data;
    }
}


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


library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

library Address {
    function isContract(address account) internal view returns (bool) {
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly { codehash := extcodehash(account) }
        return (codehash != accountHash && codehash != 0x0);
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
      return functionCall(target, data, "Address: low-level call failed");
    }


    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        return _functionCallWithValue(target, data, 0, errorMessage);
    }


    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }


    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        return _functionCallWithValue(target, data, value, errorMessage);
    }

    function _functionCallWithValue(address target, bytes memory data, uint256 weiValue, string memory errorMessage) private returns (bytes memory) {
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{ value: weiValue }(data);
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                // solhint-disable-next-line no-inline-assembly
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}


contract Ownable is Context {
    
    address private _owner;
    event OwnershipTransferred(address indexed pOwner, address indexed nOwner);

    constructor () internal {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier oOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual oOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address nOwner) public virtual oOwner {
        require(nOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, nOwner);
        _owner = nOwner;
    }
}

contract NestSwap is Context, IBEP20, Ownable {

    struct Transaction {
        bool enabled;
        address destination;
        bytes data;
    }

    Transaction[] public transactions;
    using SafeMath for uint256;
    using Address for address;

    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) private _isExcluded;
    mapping (address => uint256) private _kOwned;
    mapping (address => uint256) private _uOwned;
    address[] private _excluded;
    
    string private constant _NAME = 'NestSwap';
    string private constant _SYMBOL = 'NEST';
    uint8 private constant _DECIMALS = 9;
    uint256 private constant _MAX = ~uint256(0);
    uint256 private constant _DECIMALFORMAT = 10 ** uint256(_DECIMALS);
    uint256 private constant _GRANULARITY = 100;
    uint256 private _uTotal = 100000000 * _DECIMALFORMAT;
    uint256 private _kTotal = (_MAX - (_MAX % _uTotal));
    uint256 private _uDestroyTotal;
    uint256 private _uTaxTotal;
    uint256 private _uDestroyCycles = 0;
    uint256 private _uTradeCycles = 0;
    uint256 private _NSCycles = 0;
    uint256 private totalNS = 0;
    uint256 private movedNS = 0;
    uint256 private tax_rate = 0;
    uint256 private destroy_rate = 0;
    uint256 private constant MAX_SUPPLY = ~uint128(0);  // (2^128) - 1  
    uint256 private constant _MAX_RT_SIZE = 100000000 * _DECIMALFORMAT;
    uint256 private _gonsPerFragment;
    
    mapping(address => uint256) private _gonBalances;

    constructor () public {
        _kOwned[_msgSender()] = _kTotal;
        emit Transfer(address(0), _msgSender(), _uTotal);
    }

    event TransactionFailed(address indexed destination, uint index, bytes data);

    function name() public view returns (string memory) {
        return _NAME;
    }

    function symbol() public view returns (string memory) {
        return _SYMBOL;
    }

    function decimals() public view returns (uint8) {
        return _DECIMALS;
    }

    function totalSupply() public view override returns (uint256) {
        return _uTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        if (_isExcluded[account]) return _uOwned[account];
        return tokenFromReflection(_kOwned[account]);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "BEP20: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "BEP20: decreased allowance below zero"));
        return true;
    }

    function isExcluded(address account) public view returns (bool) {
        return _isExcluded[account];
    }

    function totalTaxes() public view returns (uint256) {
        return _uTaxTotal;
    }
    
    function totalBurn() public view returns (uint256) {
        return _uDestroyTotal;
    }

    function deliver(uint256 uAmount) public {
        address sender = _msgSender();
        require(!_isExcluded[sender], "Excluded addresses cannot call this function");
        (uint256 kAmount,,,,,) = _getValues(uAmount);
        _kOwned[sender] = _kOwned[sender].sub(kAmount);
        _kTotal = _kTotal.sub(kAmount);
        _uTaxTotal = _uTaxTotal.add(uAmount);
    }

    function reflectionFromToken(uint256 uAmount, bool deductTransfekTax) public view returns(uint256) {
        require(uAmount <= _uTotal, "Amount has to be lower than supply");
        if (!deductTransfekTax) {
            (uint256 kAmount,,,,,) = _getValues(uAmount);
            return kAmount;
        } else {
            (,uint256 kMoveAmount,,,,) = _getValues(uAmount);
            return kMoveAmount;
        }
    }

    function tokenFromReflection(uint256 kAmount) public view returns(uint256) {
        require(kAmount <= _kTotal, "Amount must be less than total reflections");
        uint256 presentRate =  _getRate();
        return kAmount.div(presentRate);
    }

    function excludeAccount(address account) external oOwner() {
        require(account != 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D, 'We can not exclude Uniswap router.');
        require(!_isExcluded[account], "Account is already excluded");
        if(_kOwned[account] > 0) {
            _uOwned[account] = tokenFromReflection(_kOwned[account]);
        }
        _isExcluded[account] = true;
        _excluded.push(account);
    }

    function includeAccount(address account) external oOwner() {
        require(_isExcluded[account], "Account is already excluded");
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_excluded[i] == account) {
                _excluded[i] = _excluded[_excluded.length - 1];
                _uOwned[account] = 0;
                _isExcluded[account] = false;
                _excluded.pop();
                break;
            }
        }
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "BEP20: approve from zero address");
        require(spender != address(0), "BEP20: approve to zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address sender, address recipient, uint256 amount) private {
        require(sender != address(0), "BEP20: transfer from zero address");
        require(recipient != address(0), "BEP20: transfer to zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
    
        if(sender != owner() && recipient != owner())
            require(amount <= _MAX_RT_SIZE, "Transfer amount exceeds the maxRTAmount.");

        if(destroy_rate >= 150){
        
        _uTradeCycles = _uTradeCycles.add(amount);

            if(_uTradeCycles >= (0 * _DECIMALFORMAT) && _uTradeCycles <= (1999999*_DECIMALFORMAT)){
                _beginDestroyTax(150);
            }   else if(_uTradeCycles >= (2000000 * _DECIMALFORMAT) && _uTradeCycles <= (4000000 * _DECIMALFORMAT)){
                _beginDestroyTax(250);
            }   else if(_uTradeCycles >= (4000000 * _DECIMALFORMAT) && _uTradeCycles <= (6000000 * _DECIMALFORMAT)){
                _beginDestroyTax(350);
            }   else if(_uTradeCycles >= (6000000 * _DECIMALFORMAT) && _uTradeCycles <= (8000000 * _DECIMALFORMAT)){
                _beginDestroyTax(450);
            } else if(_uTradeCycles >= (8000000 * _DECIMALFORMAT) && _uTradeCycles <= (10000000 * _DECIMALFORMAT)){
                _beginDestroyTax(550);
            }
            
        }

        if (_isExcluded[sender] && !_isExcluded[recipient]) {
            _transferFromExcluded(sender, recipient, amount);
        } else if (!_isExcluded[sender] && _isExcluded[recipient]) {
            _transferToExcluded(sender, recipient, amount);
        } else if (!_isExcluded[sender] && !_isExcluded[recipient]) {
            _transferStandard(sender, recipient, amount);
        } else if (_isExcluded[sender] && _isExcluded[recipient]) {
            _transferBothExcluded(sender, recipient, amount);
        } else {
            _transferStandard(sender, recipient, amount);
        }
    }

    function _transferStandard(address sender, address recipient, uint256 uAmount) private {
        uint256 presentRate =  _getRate();
        (uint256 kAmount, uint256 kMoveAmount, uint256 kTax, uint256 uMoveAmount, uint256 uTax, uint256 uDestroy) = _getValues(uAmount);
        uint256 yDestroy =  uDestroy.mul(presentRate);
        _kOwned[sender] = _kOwned[sender].sub(kAmount);
        _kOwned[recipient] = _kOwned[recipient].add(kMoveAmount);       
        _destroyFloat(kTax, yDestroy, uTax, uDestroy);
        emit Transfer(sender, recipient, uMoveAmount);
    }

    function _transferToExcluded(address sender, address recipient, uint256 uAmount) private {
        uint256 presentRate =  _getRate();
        (uint256 kAmount, uint256 kMoveAmount, uint256 kTax, uint256 uMoveAmount, uint256 uTax, uint256 uDestroy) = _getValues(uAmount);
        uint256 yDestroy =  uDestroy.mul(presentRate);
        _kOwned[sender] = _kOwned[sender].sub(kAmount);
        _uOwned[recipient] = _uOwned[recipient].add(uMoveAmount);
        _kOwned[recipient] = _kOwned[recipient].add(kMoveAmount);           
        _destroyFloat(kTax, yDestroy, uTax, uDestroy);
        emit Transfer(sender, recipient, uMoveAmount);
    }

    function _transferFromExcluded(address sender, address recipient, uint256 uAmount) private {
        uint256 presentRate =  _getRate();
        (uint256 kAmount, uint256 kMoveAmount, uint256 kTax, uint256 uMoveAmount, uint256 uTax, uint256 uDestroy) = _getValues(uAmount);
        uint256 yDestroy =  uDestroy.mul(presentRate);
        _uOwned[sender] = _uOwned[sender].sub(uAmount);
        _kOwned[sender] = _kOwned[sender].sub(kAmount);
        _kOwned[recipient] = _kOwned[recipient].add(kMoveAmount);   
        _destroyFloat(kTax, yDestroy, uTax, uDestroy);
        emit Transfer(sender, recipient, uMoveAmount);
    }

    function _transferBothExcluded(address sender, address recipient, uint256 uAmount) private {
        uint256 presentRate =  _getRate();
        (uint256 kAmount, uint256 kMoveAmount, uint256 kTax, uint256 uMoveAmount, uint256 uTax, uint256 uDestroy) = _getValues(uAmount);
        uint256 yDestroy =  uDestroy.mul(presentRate);
        _uOwned[sender] = _uOwned[sender].sub(uAmount);
        _kOwned[sender] = _kOwned[sender].sub(kAmount);
        _uOwned[recipient] = _uOwned[recipient].add(uMoveAmount);
        _kOwned[recipient] = _kOwned[recipient].add(kMoveAmount);        
        _destroyFloat(kTax, yDestroy, uTax, uDestroy);
        emit Transfer(sender, recipient, uMoveAmount);
    }

    function _destroyFloat(uint256 kTax, uint256 yDestroy, uint256 uTax, uint256 uDestroy) private {
        _kTotal = _kTotal.sub(kTax).sub(yDestroy);
        _uTaxTotal = _uTaxTotal.add(uTax);
        _uDestroyTotal = _uDestroyTotal.add(uDestroy);
        _uDestroyCycles = _uDestroyCycles.add(uDestroy);
        _uTotal = _uTotal.sub(uDestroy);

        if(_uDestroyCycles >= (350000 * _DECIMALFORMAT)){
                uint256 _qRebase = 70000 * _DECIMALFORMAT;
                _uDestroyCycles = _uDestroyCycles.sub((350000 * _DECIMALFORMAT));
                _uTradeCycles = 0;
                _beginDestroyTax(150);
                _rebase(_qRebase);
            } 
    }

    function _getValues(uint256 uAmount) private view returns (uint256, uint256, uint256, uint256, uint256, uint256) {
        (uint256 uMoveAmount, uint256 uTax, uint256 uDestroy) = _getUValues(uAmount, tax_rate, destroy_rate);
        uint256 presentRate =  _getRate();
        (uint256 kAmount, uint256 kMoveAmount, uint256 kTax) = _getKValues(uAmount, uTax, uDestroy, presentRate);
        return (kAmount, kMoveAmount, kTax, uMoveAmount, uTax, uDestroy);
    }

    function _getUValues(uint256 uAmount, uint256 taxRate, uint256 destroyTax) private pure returns (uint256, uint256, uint256) {
        uint256 uTax = ((uAmount.mul(taxRate)).div(_GRANULARITY)).div(100);
        uint256 uDestroy = ((uAmount.mul(destroyTax)).div(_GRANULARITY)).div(100);
        uint256 uMoveAmount = uAmount.sub(uTax).sub(uDestroy);
        return (uMoveAmount, uTax, uDestroy);
    }

    function _getKValues(uint256 uAmount, uint256 uTax, uint256 uDestroy, uint256 presentRate) private pure returns (uint256, uint256, uint256) {
        uint256 kAmount = uAmount.mul(presentRate);
        uint256 kTax = uTax.mul(presentRate);
        uint256 yDestroy = uDestroy.mul(presentRate);
        uint256 kMoveAmount = kAmount.sub(kTax).sub(yDestroy);
        return (kAmount, kMoveAmount, kTax);
    }

    function _getRate() private view returns(uint256) {
        (uint256 rSupply, uint256 uSupply) = _getCurrentSupply();
        return rSupply.div(uSupply);
    }

    function _getCurrentSupply() private view returns(uint256, uint256) {
        uint256 rSupply = _kTotal;
        uint256 uSupply = _uTotal;      
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_kOwned[_excluded[i]] > rSupply || _uOwned[_excluded[i]] > uSupply) return (_kTotal, _uTotal);
            rSupply = rSupply.sub(_kOwned[_excluded[i]]);
            uSupply = uSupply.sub(_uOwned[_excluded[i]]);
        }
        if (rSupply < _kTotal.div(_uTotal)) return (_kTotal, _uTotal);
        return (rSupply, uSupply);
    }
    

    function _beginDestroyTax(uint256 destroyTax) private {
        require(destroyTax >= 0 && destroyTax <= 550, "0-5.5%");
        destroy_rate = destroyTax;
    }
    
    function setTax(uint256 destroyTax) external oOwner() {
        require(destroyTax >= 0 && destroyTax <= 550, "0-5.5%");
        destroy_rate = destroyTax;
    }

    function _getDestroyTax() public view returns(uint256)  {
        return destroy_rate;
    }

    function _getMaxNSAmount() private view returns(uint256) {
        return _MAX_RT_SIZE;
    }

    function _getRotations() public view returns(uint256) {
        return _NSCycles;
    }

    function _getBurnRotations() public view returns(uint256) {
        return _uDestroyCycles;
    }

    function _getTradedRotations() public view returns(uint256) {
        return _uTradeCycles;
    }
    
    function _rebase(uint256 supplyDelta) internal {
        _NSCycles = _NSCycles.add(1);
        _uTotal = _uTotal.add(supplyDelta);

        if(_NSCycles > 251 || _uTotal <= 39557500 * _DECIMALFORMAT){
            _beginFinal();
        }
    }

    function _beginFinal() internal {
        _beginDestroyTax(0);
    }   
}