//SPDX-License-Identifier: UNLICENSED

pragma solidity >=0.4.0;
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

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {

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

pragma solidity >=0.4.0;

interface IBEP20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the token decimals.
     */
    function decimals() external view returns (uint8);

    /**
     * @dev Returns the token symbol.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the token name.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the bep token owner.
     */
    function getOwner() external view returns (address);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address _owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    function getTaxPercent() external view returns (uint256);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

pragma solidity >=0.6.0;

library SafeBEP20 {
    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(
        IBEP20 token,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(
        IBEP20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function safeApprove(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        // solhint-disable-next-line max-line-length
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            'SafeBEP20: approve from non-zero to non-zero allowance'
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(
            value,
            'SafeBEP20: decreased allowance below zero'
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function _callOptionalReturn(IBEP20 token, bytes memory data) private {

        bytes memory returndata = address(token).functionCall(data, 'SafeBEP20: low-level call failed');
        if (returndata.length > 0) {
            // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), 'SafeBEP20: BEP20 operation did not succeed');
        }
    }
}


pragma solidity >=0.6.2;

library Address {

    function isContract(address account) internal view returns (bool) {

        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly {
            codehash := extcodehash(account)
        }
        return (codehash != accountHash && codehash != 0x0);
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, 'Address: insufficient balance');

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{value: amount}('');
        require(success, 'Address: unable to send value, recipient may have reverted');
    }

    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCall(target, data, 'Address: low-level call failed');
    }

    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return _functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, 'Address: low-level call with value failed');
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, 'Address: insufficient balance for call');
        return _functionCallWithValue(target, data, value, errorMessage);
    }

    function _functionCallWithValue(
        address target,
        bytes memory data,
        uint256 weiValue,
        string memory errorMessage
    ) private returns (bytes memory) {
        require(isContract(target), 'Address: call to non-contract');

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{value: weiValue}(data);
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



pragma solidity >=0.4.0;

contract Context {

    constructor() internal {}

    function _msgSender() internal view returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}



pragma solidity >=0.4.0;

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() internal {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), 'Ownable: caller is not the owner');
        _;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     */
    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0), 'Ownable: new owner is the zero address');
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}


pragma solidity >=0.5.16;

interface IPancakePair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
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

pragma solidity >=0.6.6;
interface IPancakeRouter{
    function getAmountsOut(uint amountIn, address[] memory path) external view returns (uint[] memory amounts);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
}

pragma solidity >=0.6.2;
pragma experimental ABIEncoderV2;

contract GPTB is Ownable {
    using SafeMath for uint256;
    using SafeBEP20 for IBEP20;

    IPancakeRouter public pancakeRouter;

    IBEP20 public mainToken;
    IBEP20 public usdtToken;

    event Deposit(address indexed user, uint256 amount,uint256 datetime,uint256 userId);
    event Withdraw(address indexed user, uint256 amount,uint256 datetime,uint256 userId);
    event Buy(address indexed user, uint256 amount,uint256 datetime,uint256 userId);
    event Claim(address indexed user, uint256 amount,uint256 datetime,uint256 userId);

    uint256 public price = 1*(10**6);
    uint256[] public bonuslevel = [1000,300,200];
    uint256 public percentBuy = 7000;
    address[] public path = [0x55d398326f99059fF775485246999027B3197955,0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c,0x489Db05480B76DC79B07Bc9DA963794c134170B7];

    struct UserInfo {
        uint256 balUSDT;
        uint256 balGPTB; 
    }

    mapping (address => UserInfo) public userInfo;
    mapping(address => address) public referrers;

    uint256 public totalusdt = 0;
    uint256 public totalgpt = 0;
    //test net
     //0x489Db05480B76DC79B07Bc9DA963794c134170B7
    //0x55d398326f99059fF775485246999027B3197955
    //0x10ED43C718714eb63d5aA57B78B54704E256024E
    constructor(IBEP20 _mainToken,IBEP20 _usdtToken,address _pancakeRouter) public {
        mainToken = IBEP20(_mainToken);
        usdtToken = IBEP20(_usdtToken);
        pancakeRouter = IPancakeRouter(_pancakeRouter);
    }

    function setMainToken(IBEP20 _mainToken) public onlyOwner {
        mainToken = IBEP20(_mainToken);
    }

    function setUsdtToken(IBEP20 _usdtToken) public onlyOwner{
        usdtToken = IBEP20(_usdtToken);
    }

    function setBonusLevel(uint256[] memory _bonuslevel) public onlyOwner{
        bonuslevel = _bonuslevel;
    }

    function getBonusLevel() public view  returns (uint256[] memory){
        return bonuslevel;
    }

    function setPercentBuy(uint256 _percentBuy) public onlyOwner{
        percentBuy = _percentBuy;
    }

    function getAmount(uint256 _amountin)  public view  returns (uint256) {
        //get from pancakeswap
        uint256 amountout = pancakeRouter.getAmountsOut(_amountin,path)[2]/100;
        //uint256 amountout = _amountin/price; 
        return amountout;
    }

    function setPrice(uint256 _price) public onlyOwner{
        price = _price;
    }

    function getPrice()  public view returns (uint256){
        return price;
    }

    function recordReferral(address _user, address _referrer) internal {
        if (_user != address(0)
            && _referrer != address(0)
            && _user != _referrer
            && referrers[_user] == address(0)
        ) {
            referrers[_user] = _referrer;
        }
    }

    function getReferrer(address _user) public  view returns (address) {
        return referrers[_user];
    }

    function deposit(uint256 _amount, address _referrer,uint256 _userId) public{
        //safe referral
        recordReferral(msg.sender,_referrer);
        UserInfo storage user = userInfo[msg.sender];
        usdtToken.safeTransferFrom(address(msg.sender),address(this),_amount);
        user.balUSDT = user.balUSDT + _amount;
        totalusdt = totalusdt + _amount;
        emit Deposit(msg.sender,_amount,block.timestamp,_userId);
    }

    function withdraw(uint256 _amount,uint256 _userId) public{
        //check balance
        UserInfo storage user = userInfo[msg.sender];
        require(user.balUSDT>=_amount,"BEP20: transfer amount exceeds balance");
        usdtToken.safeTransfer(msg.sender,_amount);
        user.balUSDT = user.balUSDT - _amount;
        totalusdt = totalusdt - _amount;
        emit Withdraw(msg.sender,_amount,block.timestamp,_userId);
    }

    function buy(uint256 _amount,uint256 _userId) public{
        //check balance
        UserInfo storage user = userInfo[msg.sender];
        require(user.balUSDT>=_amount,"BEP20: transfer amount exceeds balance");
        //deduct usdt
        user.balUSDT = user.balUSDT - _amount;
        totalusdt = totalusdt - _amount;
        //distribute to network
        address ref = getReferrer(msg.sender);
        
        //get price from pancake or swap
        uint256 amtGPTB = getAmount(_amount);
        for(uint16 i=0;i<3;i++){
            if(ref == address(0)){
                break;
            }
            UserInfo storage userRef = userInfo[ref];
            userRef.balUSDT = userRef.balUSDT + _amount*bonuslevel[i]/10000;
            totalusdt = totalusdt + _amount*bonuslevel[i]/10000;
            ref = getReferrer(ref);
        }
        //add balance
        user.balGPTB = user.balGPTB + amtGPTB*percentBuy/10000;
        totalgpt = totalgpt + amtGPTB*percentBuy/10000;
        emit Buy(msg.sender, _amount,block.timestamp,_userId);
    }

    function claim(uint256 _amount,uint256 _userId) public{
        UserInfo storage user = userInfo[msg.sender];
        require(user.balGPTB>=_amount,"BEP20: transfer amount exceeds balance");
        mainToken.safeTransfer(msg.sender,_amount);
        user.balGPTB = user.balGPTB - _amount;
        totalgpt = totalgpt - _amount;
        emit Claim(msg.sender,_amount,block.timestamp,_userId);
    }

    function tokenWithdraw(IBEP20 token,uint256 _amount,address _to) public onlyOwner {
        require(_amount < token.balanceOf(address(this)), 'not enough token');
        token.transfer(address(_to), _amount);
    }
    
}