// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b);
        return c;
    }
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0);
        uint256 c = a / b;
        return c;
    }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a);
        uint256 c = a - b;
        return c;
    }
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a);
        return c;
    }
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0);
        return a % b;
    }
}
interface IERC20 {
    function transfer(address to, uint256 value) external returns (bool);
    function approve(address spender, uint256 value) external returns (bool);
    function _approve(address owner, address spender, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function totalSupply() external view returns (uint256);
    function balanceOf(address who) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}
abstract contract ReentrancyGuard {

    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() 
    {   _status = _NOT_ENTERED;     }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
        _;
        _status = _NOT_ENTERED;
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
interface IPancakeRouter01 {
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
}

contract battlemoney is Ownable, ReentrancyGuard{

    using SafeMath for uint256;

    IPancakeRouter01 public Router;
    IERC20 public AGC;
    IERC20 public USDT;

    uint256 public miniDeposit = 10; 

    constructor()
    {
       AGC = IERC20(0x6e3fd1DEA627226998dA6e9e0C7EF95F417d6c35);
       USDT = IERC20(0x55d398326f99059fF775485246999027B3197955);
       Router = IPancakeRouter01(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    }
    
    function getPrice(uint256 packageamount) 
    public 
    view 
    returns(uint256, uint256)
    {
        require(packageamount.mod(miniDeposit) == 0 && packageamount >= miniDeposit, "mod err");
        // no. of tokens in one dollar 
        address[] memory path0 = new address[](2);
        path0[0] = address(USDT);
        path0[1] = address(AGC);
        uint256[] memory amounts0 = Router.getAmountsOut(1 ether,path0);

        return (amounts0[1].mul(packageamount), packageamount.mul(1 ether));
    }

    function buyWithUSDT(uint256 packageamount)
    public 
    nonReentrant
    {
        require(msg.sender == tx.origin," External Error ");
        (, uint256 token1) = getPrice(packageamount);
        require(USDT.balanceOf(msg.sender) >= token1,"Insufficient user USDT balance!");
        require(USDT.transferFrom(_msgSender(),address(this),token1)," Approve USDT First ");
    }

    function buyWithAGC(uint256 packageamount)
    public 
    nonReentrant
    {
        require(msg.sender == tx.origin," External Error ");
        (uint256 token0,) = getPrice(packageamount);
        require(AGC.balanceOf(msg.sender) >= token0,"Insufficient user AGC balance!");
        require(AGC.transferFrom(_msgSender(),address(this),token0)," Approve AGC First ");
    }

    function UpdateROUTER(IPancakeRouter01 _Router)
    external
    onlyOwner
    {      Router = _Router;        }

    function emergencyWithdrawToken()
    external 
    onlyOwner
    {   require(AGC.transfer(owner(),(AGC.balanceOf(address(this)))),"transfer failed!");   }

    function emergencyWithdrawUSDTToken()
    external
    onlyOwner
    {   require(USDT.transfer(owner(),(USDT.balanceOf(address(this)))),"transfer failed!");   }

    function WithdrawToken(address _tokenAddress)
    external
    onlyOwner
    {   IERC20(_tokenAddress).transfer(owner(),(IERC20(_tokenAddress).balanceOf(address(this))));   }

    function updateMiniDeposit(uint256 _miniAmount) 
    external 
    onlyOwner
    { miniDeposit = _miniAmount; }
}