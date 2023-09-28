pragma solidity 0.5.16;

interface IBEP20 {
    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function getOwner() external view returns (address);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
    external
    returns (bool);

    function allowance(address _owner, address spender)
    external
    view
    returns (uint256);

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

contract Context {
    constructor() internal {}

    function _msgSender() internal view returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this;
        // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
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
        // Solidity only automatically asserts when dividing by 0
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

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() internal {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract ErrorReporter {
    event Failure(uint256 error);
    enum Error {
        NO_ERROR,
        UNAUTHORIZED,
        TOKEN_INSUFFICIENT_ALLOWANCE,
        TOKEN_INSUFFICIENT_BALANCE,
        CARD_SOLD_OUT,
        CARD_NOT_EXIST,
        LEVEL_IS_TOP,
        INSUFFICIENT_BALANCE,
        MINING,
        MIN_LIMIT,
        ALREADY_WITHDRAWAL,
        NOT_COMMISSION,
        CONTRACT_PAUSED
    }

    function fail(Error err) internal returns (uint256) {
        emit Failure(uint256(err));

        return uint256(err);
    }
}

contract RechargeSOK is Context, Ownable, ErrorReporter {
    using SafeMath for uint256;

    IBEP20 public usdtToken;

    address public rechargeAddress;

    event Recharge(address indexed sender, uint256 value, uint256 num);

    event Apply(address indexed owner, uint256 value, uint256 num);

    constructor() public {

    }


    function _recharge(address sender, uint256 amount, uint256 num) internal {
        require(sender != address(0), "BEP20: approve from the zero address");
        emit Recharge(sender, amount, num);
    }

    function _apply(address owner, uint256 amount, uint256 num) internal {
        require(owner != address(0), "BEP20: approve from the zero address");
        emit Apply(owner, amount, num);
    }


    function getOwner() external view returns (address) {
        return owner();
    }

    function setUsdtToken(address value) public onlyOwner returns (bool){
        usdtToken = IBEP20(value);
        return true;
    }

    function setRechargeAddress(address value) public onlyOwner returns (bool){
        rechargeAddress = value;
        return true;
    }

    function recharge(uint256 amount, uint256 num) public returns (uint256) {
        if (usdtToken.allowance(msg.sender, address(this)) < amount) {
            return fail(Error.TOKEN_INSUFFICIENT_ALLOWANCE);
        }
        if (usdtToken.balanceOf(msg.sender) < amount) {
            return fail(Error.TOKEN_INSUFFICIENT_BALANCE);
        }
        usdtToken.transferFrom(msg.sender, address(this), amount);
        usdtToken.transfer(rechargeAddress, amount);
        _recharge(msg.sender, amount, num);
        return uint(Error.NO_ERROR);
    }

    function applyAmount(uint256 amount, uint256 num) external returns (bool) {
        _apply(_msgSender(), amount, num);
        return true;
    }

}