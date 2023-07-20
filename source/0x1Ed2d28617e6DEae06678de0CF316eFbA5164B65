// SPDX-License-Identifier: Unlicensed

pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
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
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
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

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
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

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

contract Xiin is IERC20, Ownable {
    using SafeMath for uint256;

    string private _name = "XiiN";
    string private _symbol = "XiiN";
    uint8 private _decimals = 18;
    uint256 private _totalSupply = 77777 * 10 ** _decimals;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    mapping(address => bool) public isMarketPair;
    bool public permitSwapBuying = false;
    bool public permitSwapSelling = true;
    mapping(address => bool) public isExcludedFromFee;
    uint256 public totalTaxIfBuying = 0;
    uint256 public totalTaxIfSelling = 1;
    bool public enableFeeAllocateIfBuying = false;
    bool public enableFeeAllocateIfSelling = true;
    uint256 public limitToken0AmountIfBuying = 0;
    uint256 public limitToken0AmountIfSelling = 0;

    address public adminAddress = 0x641AA96fa332BE4549Bea6b400631B99Ec3Bcca7; //bsc mainnet
    address public devAddress = 0x641AA96fa332BE4549Bea6b400631B99Ec3Bcca7; //bsc mainnet
    // address public adminAddress = 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266; //local
    // address public devAddress = 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266; //local

    address public token0 = 0x55d398326f99059fF775485246999027B3197955; //bsc mainnet

    // address public token0 = 0x41717c1dCe909422E521707f5301C902bB07D9a8; //bsc testnet
    //address public usdt = 0x5FbDB2315678afecb367f032d93F642f64180aa3; //localhost

    constructor() {
        isExcludedFromFee[owner()] = true;
        isExcludedFromFee[adminAddress] = true;
        isExcludedFromFee[devAddress] = true;

        _balances[owner()] = 70000 * 10 ** _decimals;
        _balances[adminAddress] = 7777 * 10 ** _decimals;

        emit Transfer(address(0), owner(), 70000 * 10 ** _decimals);
        emit Transfer(address(0), adminAddress, 7777 * 10 ** _decimals);
    }

    function name() public view virtual returns (string memory) {
        return _name;
    }

    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(
        address account
    ) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function transfer(
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(
                amount,
                "ERC20: redi transfer amount exceeds allowance"
            )
        );
        return true;
    }

    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public virtual returns (bool) {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].add(addedValue)
        );
        return true;
    }

    function decreaseAllowance(
        address spender,
        uint256 subtractedValue
    ) public virtual returns (bool) {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
        return true;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        uint256 finalAmount = amount;

        if (isMarketPair[sender] && !isExcludedFromFee[recipient]) {
            require(permitSwapBuying, "Forbid");

            if (limitToken0AmountIfBuying > 0) {
                require(
                    IERC20(token0).balanceOf(sender) >
                        limitToken0AmountIfBuying,
                    "Forbid"
                );
            }

            if (totalTaxIfBuying > 0) {
                finalAmount = amount.sub(
                    amount.mul(totalTaxIfBuying).div(100),
                    "redi Insufficient amount"
                );
                if (finalAmount < amount) {
                    _feeAllocate(
                        sender,
                        recipient,
                        amount,
                        finalAmount,
                        enableFeeAllocateIfBuying
                    );
                }
            }
        }

        if (isMarketPair[recipient] && !isExcludedFromFee[sender]) {
            require(permitSwapSelling, "Forbid");

            if (limitToken0AmountIfSelling > 0) {
                require(
                    IERC20(token0).balanceOf(recipient) >
                        limitToken0AmountIfSelling,
                    "Forbid"
                );
            }

            if (totalTaxIfSelling > 0) {
                finalAmount = amount.sub(
                    amount.mul(totalTaxIfSelling).div(100),
                    "redi Insufficient amount"
                );
                if (finalAmount < amount) {
                    _feeAllocate(
                        sender,
                        recipient,
                        amount,
                        finalAmount,
                        enableFeeAllocateIfSelling
                    );
                }
            }
        }

        _balances[sender] = _balances[sender].sub(
            amount,
            "redi Insufficient Balance"
        );

        _balances[recipient] = _balances[recipient].add(finalAmount);

        emit Transfer(sender, recipient, finalAmount);
    }

    function _feeAllocate(
        address sender,
        address recipient,
        uint256 amount,
        uint256 finalAmount,
        bool enableFeeAllocate
    ) private {
        uint256 feeAmount = amount.sub(
            finalAmount,
            "redi Insufficient feeAmount"
        );
        if (enableFeeAllocate) {
            _balances[devAddress] += feeAmount;
            emit Transfer(sender, recipient, finalAmount);
        } else {
            _totalSupply = _totalSupply.sub(
                feeAmount,
                "redi Insufficient totalSupply"
            );
        }
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

    function setLimitToken0Amount(
        uint256 _limitToken0AmountIfBuying,
        uint256 _limitToken0AmountIfSelling
    ) external onlyOwner {
        limitToken0AmountIfBuying = _limitToken0AmountIfBuying;
        limitToken0AmountIfSelling = _limitToken0AmountIfSelling;
    }

    function setMarketPair(address account, bool newValue) public onlyOwner {
        isMarketPair[account] = newValue;
    }

    function setIsExcludedFromFee(
        address account,
        bool newValue
    ) public onlyOwner {
        isExcludedFromFee[account] = newValue;
    }

    function setTotalTax(
        uint256 _totalTaxIfBuying,
        uint256 _totalTaxIfSelling
    ) external onlyOwner {
        totalTaxIfBuying = _totalTaxIfBuying;
        totalTaxIfSelling = _totalTaxIfSelling;
    }

    function setPermitSwap(bool _buying, bool _selling) external onlyOwner {
        permitSwapBuying = _buying;
        permitSwapSelling = _selling;
    }

    function setEnableFeeAllocate(
        bool _buying,
        bool _selling
    ) external onlyOwner {
        enableFeeAllocateIfBuying = _buying;
        enableFeeAllocateIfSelling = _selling;
    }

    function setDevAddress(address _devAddress) external onlyOwner {
        require(_devAddress != address(0), "Redi::set: Zero address");
        devAddress = _devAddress;
    }

    function rescueTokens(
        address token,
        address to,
        uint256 amount
    ) public onlyOwner {
        IERC20(token).transfer(to, amount);
    }
}