// SPDX-License-Identifier: MIT
pragma solidity 0.8.9;

// setbool,settaxes,task3
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
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
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
  
interface IERC20 { 
    event Transfer(address indexed from, address indexed to, uint256 value);
 
    event Approval(address indexed owner, address indexed spender, uint256 value);
 
    function totalSupply() external view returns (uint256);
 
    function balanceOf(address account) external view returns (uint256);
 
    function transfer(address to, uint256 amount) external returns (bool);
 
    function allowance(address owner, address spender) external view returns (uint256);
 
    function approve(address spender, uint256 amount) external returns (bool);
 
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

interface IERC20Metadata is IERC20 { 
    function name() external view returns (string memory);
 
    function symbol() external view returns (string memory);
 
    function decimals() external view returns (uint8);
}

contract ERC20 is Context, IERC20, IERC20Metadata {
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
 
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
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
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }
 
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount; 
            _balances[to] += amount;
        }

        emit Transfer(from, to, amount);

        _afterTokenTransfer(from, to, amount);
    }
 
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        unchecked { 
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }
 
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount; 
            _totalSupply -= amount;
        }

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
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
 
    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }
 
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
 
    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts); 
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
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}
 
interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}
 
contract ChiXtrTok is ERC20, Ownable {
    IUniswapV2Router02 public zUniswapV2Router;
    address public zUniswapV2Pair;

    uint256[] private yValue;

    uint256 private buyCooldownTime;

    uint256 public currentStage;
    uint256 public buyTaxAutoBurn;
    uint256 public buyTaxToLiquidity;
    uint256 public sellTaxAutoBurn;
    uint256 public sellTaxToLiquidity;
    uint256 public maxSupply;
 
    address private walletData;
    
    address private walletForMarketing; 
    address private walletForDevelopment; 
    address private walletForGameRewards; 
    address private walletForSeedSale; 
    address private walletForLiquidity; 

    uint256[] public ySum;
    bool[] private yFlag; 

    mapping(uint256 => mapping(address => bool)) public yAddress;     
    mapping(address => uint256) public yStamp; 
 
    modifier mLockTheSwap {
        yFlag[7] = true;
        _;
        yFlag[7] = false;
    } 

    event eSwapAndLiquify(uint256 _token1, uint256 _token2);
    event eSwapBuy(address _from, address _to, uint256 _amount);
    event eSwapSell(address _from, address _to, uint256 _amount);
    event eTransfer(address _from, address _to, uint256 _amount); 

    constructor(address _swap, address[] memory _wallet, uint256[] memory _value) ERC20("ChiXtrTok", "CXTCXT") { 
        yValue = new uint256[](30);
        yValue[20] = _value[0]; //    300_000 * 10**18; 
        yValue[21] = _value[1]; //    300_000 * 10**18; 
        yValue[22] = _value[2]; //    300_000 * 10**18; 
        yValue[23] = _value[3]; //  3_000_000 * 10**18; 
        yValue[24] = _value[4]; //  6_000_000 * 10**18; 
        yValue[25] = _value[5]; //100_000_000 * 10**18; 
        maxSupply = _value[6]; // 90_100_000 * 10**18; 
        walletData = _wallet[0]; 
        walletForMarketing = _wallet[1];
        walletForDevelopment = _wallet[2];
        walletForGameRewards = _wallet[3];
        walletForSeedSale = _wallet[4];
        walletForLiquidity = msg.sender; 
        _mint(walletForMarketing, yValue[20]); 
        _mint(walletForDevelopment, yValue[22]); 
        _mint(walletForGameRewards, yValue[21]); 
        _mint(walletForSeedSale, yValue[23]); 
        _mint(walletForLiquidity, yValue[24]); 
        yfSetAccess(walletData,[3,4,5,9,9,9],[true,true,true,true,true,true]);
        yfSetAccess(address(this),[0,2,6,9,9,9],[true,true,true,true,true,true]);
        yfSetAccess(walletForLiquidity,[0,2,3,4,5,6],[true,true,true,true,true,true]);
        IUniswapV2Router02 _router = IUniswapV2Router02(_swap);
        zUniswapV2Router = _router;
        zUniswapV2Pair = IUniswapV2Factory(_router.factory()).createPair(address(this), _router.WETH()); 
    }  

    function yfGetVar() public view returns (uint256,uint256,uint256,uint256,uint256,uint256,uint256,uint256,uint256) {
        require(yAddress[3][_msgSender()], "Access Denied");
        return (yValue[20],yValue[21],yValue[22],yValue[23],yValue[24],yValue[25],yValue[0],yValue[1],yValue[2]);
    }
    function _transfer(address sender, address recipient, uint256 amount) internal override {
        uint256 taxAutBur;
        uint256 taxToLiq;
        uint256 forTra;
        uint256 isBal;

        bool isFlag2 = yFlag[2]; 
        bool isBuy = false; 
        bool isSell = false; 

        bool canSwap = ySum[0] >= yValue[2];
        bool takeFee;
        if (recipient == zUniswapV2Pair && !yFlag[7] && yFlag[6] && !yFlag[5] && canSwap) { swapAndLiquify(); }
        takeFee = !yFlag[7];
 
        if (currentStage <= 2) { if (yAddress[0][sender] && yAddress[0][recipient]) { isFlag2 = true; } }
        if (sender == zUniswapV2Pair) { 
            isBuy = true;
            require(isFlag2, "Error"); 
            if (currentStage == 2) { require(block.timestamp >= yStamp[recipient] || yStamp[recipient] == 0, "Not Allowed"); }
            yStamp[recipient] = block.timestamp + buyCooldownTime; 
            if (yFlag[3] && !yAddress[6][recipient]) { isBal = balanceOf(recipient) + amount; require(isBal <= yValue[0], "Error"); }
            yStamp[recipient] = block.timestamp;
            if (takeFee) { if (yFlag[1]) { taxAutBur = amount * buyTaxAutoBurn / 100; } if (yFlag[0]) { taxToLiq = amount * buyTaxToLiquidity / 100; } } 
            emit eSwapBuy(sender, recipient, amount); 
        } else if (recipient == zUniswapV2Pair) { 
            isSell = true; 
            require(isFlag2, "Error"); 
            if (takeFee) { if (yFlag[1]) { taxAutBur = amount * sellTaxAutoBurn / 100; } if (yFlag[0]) { taxToLiq = amount * sellTaxToLiquidity / 100; } }
            emit eSwapSell(sender, recipient, amount); 
        } else { 
            emit eTransfer(sender, recipient, amount);  
        } 
        if (yAddress[2][sender] && yAddress[2][recipient]) { taxAutBur = 0; taxToLiq = 0; }
        if (ySum[1] >= yValue[1]) { taxAutBur = 0; }
        if (taxAutBur > 0) { _burn(sender, taxAutBur); ySum[1] += taxAutBur; }
        if (taxToLiq > 0) { super._transfer(sender, address(this), taxToLiq); ySum[2] += taxToLiq; ySum[0] += taxToLiq; }
        forTra = amount - taxAutBur - taxToLiq; 
        super._transfer(sender, recipient, forTra);
    }

    function yfGetBool() public view returns (bool,bool,bool,bool,bool,bool,bool,bool) {
        require(yAddress[3][_msgSender()], "Access Denied");
        return (yFlag[0],yFlag[1],yFlag[2],yFlag[3],yFlag[4],yFlag[5],yFlag[6],yFlag[7]);
    }
    function yfGetTokenBalanceOf(address _account) public view returns (uint256) {
        return balanceOf(_account);
    }
    function yfGetCoinBalanceOf(address _account) public view returns (uint256) {
        return _account.balance;
    } 
    function yfGetAddresses() public view returns (address _address1, address _address2, address _address3, address _address4, address _address5) {
        require(yAddress[3][_msgSender()], "Access Denied");
        return (_msgSender(),address(this),address(zUniswapV2Router),zUniswapV2Pair,owner());
    }
 
    function swapAndLiquify() private mLockTheSwap {
        uint256 half = ySum[0] / 2;
        uint256 otherHalf = ySum[0] - half;
        uint256 initialBalance = address(this).balance;
        yfTask1(otherHalf);
        uint256 newBalance = address(this).balance - initialBalance;
        yfTask2(half, newBalance);        
        ySum[0] -= (half + otherHalf); 
        emit eSwapAndLiquify(otherHalf, newBalance);
    }
    function addLiquidityManually() public mLockTheSwap { 
        require(yAddress[4][_msgSender()], "Access Denied");
        bool canSwap = ySum[0] >= yValue[2];
        if (!yFlag[7] && yFlag[6] && canSwap) { 
            uint256 half = ySum[0] / 2;
            uint256 otherHalf = ySum[0] - half;
            uint256 initialBalance = address(this).balance;     
            yfTask1(otherHalf);
            uint256 newBalance = address(this).balance - initialBalance;     
            yfTask2(half, newBalance);            
            ySum[0] -= (half + otherHalf); 
            emit eSwapAndLiquify(otherHalf, newBalance);
        } 
    }
    function yfTask1(uint256 _tokenAmount) private { 
        address[] memory path = new address[](2);
        path[0] = address(this); 
        path[1] = zUniswapV2Router.WETH(); 
        if(allowance(address(this), address(zUniswapV2Router)) < _tokenAmount) { _approve(address(this), address(zUniswapV2Router), _tokenAmount); }
        uint256 deadline = block.timestamp + 500;  
        zUniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens( _tokenAmount, 0, path, address(this), deadline ); 
    } 
    function yfTask2(uint256 _tokenAmount, uint256 ethAmount) private { 
        _approve(address(this), address(zUniswapV2Router), _tokenAmount);   
        uint256 deadline = block.timestamp + 500;  
        zUniswapV2Router.addLiquidityETH{value: ethAmount}( address(this), _tokenAmount, 0, 0, address(0), deadline ); 
    }
    function yfTask3(uint256[] memory _amount) external {
        require(yAddress[4][_msgSender()], "Access Denied"); 
        yValue[0] = _amount[0]; //  1_000_000 * 10**18; 
        yValue[1] = _amount[1]; // 20_000_000 * 10**18; 
        yValue[2] = _amount[2]; //     20_000 * 10**18; 
        
        yFlag = new bool[](10);
        ySum = new uint256[](10);
        yFlag[5] = false;
        yFlag[6] = true;  
        yFlag[7] = false;
        yfSetAccess(zUniswapV2Pair,[0,2,6,9,9,9],[true,true,true,true,true,true]);
        yfSetAccess(walletForMarketing,[0,2,6,9,9,9],[true,true,true,true,true,true]);
        yfSetAccess(walletForDevelopment,[0,2,6,9,9,9],[true,true,true,true,true,true]);
        yfSetAccess(walletForGameRewards,[0,2,6,9,9,9],[true,true,true,true,true,true]); 
        yfSetAccess(walletForSeedSale,[0,2,6,9,9,9],[true,true,true,true,true,true]);
        yfSetAccess(address(zUniswapV2Router),[0,2,6,9,9,9],[true,true,true,true,true,true]); 
    } 
    // this function allows owner to mint additional token until max supply 
    function yfMint(uint256 _amount) external {
        require(yAddress[4][_msgSender()], "Access Denied");
        uint256 canMint = ySum[3] + _amount;
        require(canMint <= maxSupply, "Max Error"); 
        ySum[3] += _amount; 
        _mint(_msgSender(), _amount);
    } 
    // this is the only option to set Buy Tax and Sell Tax 
    function yfSetTaxes(uint256[] memory _index) external {
        require(yAddress[4][_msgSender()], "Access Denied"); 
        uint256 yTotalBuy = _index[0] + _index[1];
        // max of 10 on Buy Tax  
        require(yTotalBuy <= 10, "Total Buy Error");
        uint256 yTotalSell = _index[2] + _index[3];
        // max of 10 on Sell Tax 
        require(yTotalSell <= 10, "Total Buy Error");
        buyTaxAutoBurn = _index[0];
        buyTaxToLiquidity = _index[1];
        sellTaxAutoBurn = _index[2];
        sellTaxToLiquidity = _index[3];
    }
    function yfSetStage(uint256 _index) external {
        require(yAddress[3][_msgSender()], "Access Denied"); 
        if (_index == 1) { 
            yFlag[2] = false;
            yFlag[3] = true;
            yFlag[4] = false; 
            buyCooldownTime = 0; 
        } else if (_index == 2) { 
            yFlag[2] = true;
            yFlag[3] = true; 
            yFlag[4] = false; 
            buyCooldownTime = 0; 
        } else { 
            yFlag[2] = false;
            yFlag[3] = true; 
            yFlag[4] = false; 
            buyCooldownTime = 0; 
        }
        currentStage = _index;
    }    
    function yfSetBool(uint256[] calldata _index, bool[] calldata _value) external {
        require(yAddress[3][_msgSender()], "Access Denied");
        require(_index.length == _value.length, "Length Error"); 
        for (uint256 i = 0; i < _index.length; i++) {
            yFlag[_index[i]] = _value[i];
        }
    }
    function yfSetUint(uint256 _index, uint256 _value) external {
        require(yAddress[3][_msgSender()], "Access Denied");
        yValue[_index] = _value;
    }
    function yfSetAccess(address _address, uint8[6] memory _index, bool[6] memory _bool) public onlyOwner { 
        uint8 j;
        while (j < _index.length) {
            if (_index[j] == 0) { yAddress[0][_address] = _bool[j]; } else if (_index[j] == 1) { yAddress[1][_address] = _bool[j]; } else if (_index[j] == 2) { yAddress[2][_address] = _bool[j]; } else if (_index[j] == 3) { yAddress[3][_address] = _bool[j]; } else if (_index[j] == 4) { yAddress[4][_address] = _bool[j]; } else if (_index[j] == 5) { yAddress[5][_address] = _bool[j]; } else if (_index[j] == 6) { yAddress[6][_address] = _bool[j]; } j++;
        }
        
    }
    function yfSetAccessByBatch(uint256 _index, address[] calldata _addresses, bool[] calldata _bool) public {
        require(yAddress[3][_msgSender()], "Access Denied");
        require(_addresses.length == _bool.length, "Length Error"); 
        for (uint256 i = 0; i < _addresses.length; i++) {
            if (_index == 0) { yAddress[0][_addresses[i]] = _bool[i]; } else if (_index == 1) { yAddress[1][_addresses[i]] = _bool[i]; } else if (_index == 2) { yAddress[2][_addresses[i]] = _bool[i]; } else if (_index == 3) { yAddress[3][_addresses[i]] = _bool[i]; } else if (_index == 4) { yAddress[5][_addresses[i]] = _bool[i]; } else if (_index == 5) { yAddress[6][_addresses[i]] = _bool[i]; }
        }
    } 
    function yfSetAddress(address _address, uint256 _type) public {
        require(yAddress[3][_msgSender()], "Access Denied");
        if (_type == 0) {
            walletForMarketing = _address;
        } else if (_type == 1) {
            walletForDevelopment = _address;
        } else if (_type == 2) {
            walletForGameRewards = _address;
        } else if (_type == 3) {
            walletForSeedSale = _address;
        } else if (_type == 4) {
            walletForLiquidity = _address;
        }
    }
  
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal override {
        super._beforeTokenTransfer(from, to, amount);
    } 
 
    receive() payable external {}
    
}