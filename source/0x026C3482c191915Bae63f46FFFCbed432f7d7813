// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/**
 * @title SafeMath
 * @dev Math operations with safety checks that revert on error
 */
library SafeMath {
    /**
     * @dev Returns the largest of two numbers.
     */
    function max(uint256 a, uint256 b) internal pure returns (uint256) {
        return a >= b ? a : b;
    }

    /**
     * @dev Returns the smallest of two numbers.
     */
    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }

    /**
     * @dev Calculates the average of two numbers. Since these are integers,
     * averages of an even and odd number cannot be represented, and will be
     * rounded down.
     */
    function average(uint256 a, uint256 b) internal pure returns (uint256) {
        // (a + b) / 2 can overflow, so we distribute
        return (a / 2) + (b / 2) + (((a % 2) + (b % 2)) / 2);
    }

    /**
     * @dev Multiplies two numbers, reverts on overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-solidity/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b);

        return c;
    }

    /**
     * @dev Integer division of two numbers truncating the quotient, reverts on division by zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0); // Solidity only automatically asserts when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**
     * @dev Subtracts two numbers, reverts on overflow (i.e. if subtrahend is greater than minuend).
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a);
        uint256 c = a - b;

        return c;
    }

    /**
     * @dev Adds two numbers, reverts on overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a);

        return c;
    }

    /**
     * @dev Divides two numbers and returns the remainder (unsigned integer modulo),
     * reverts when dividing by zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0);
        return a % b;
    }
}

contract Ownable {
    address public _owner;

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    function changeOwner(address newOwner) public onlyOwner {
        _owner = newOwner;
    }
}

/**
 * @title ERC20 interface
 * @dev see https://github.com/ethereum/EIPs/issues/20
 */
interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address who) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function transfer(address to, uint256 value) external returns (bool);

    function approve(address spender, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

/**
 * @title SafeERC20
 * @dev Wrappers around ERC20 operations that throw on failure.
 * To use this library you can add a `using SafeERC20 for ERC20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
library SafeERC20 {
    using SafeMath for uint256;

    function safeTransfer(
        IERC20 token,
        address to,
        uint256 value
    ) internal {
        require(token.transfer(to, value));
    }

    function safeTransferFrom(
        IERC20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        require(token.transferFrom(from, to, value));
    }

    function safeApprove(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        require((value == 0) || (token.allowance(msg.sender, spender) == 0));
        require(token.approve(spender, value));
    }

    function safeIncreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(
            value
        );
        require(token.approve(spender, newAllowance));
    }

    function safeDecreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(
            value
        );
        require(token.approve(spender, newAllowance));
    }
}

interface ICakePool {
    function deposit(uint256 _amount, uint256 _lockDuration) external;
    function withdrawByAmount(uint256 _amount) external;
    function userInfo(address _user) 
        external 
        view 
        returns (
            uint256 shares, // number of shares for a user.
            uint256 lastDepositedTime, // keep track of deposited time for potential penalty.
            uint256 cakeAtLastUserAction, // keep track of cake deposited at the last user action.
            uint256 lastUserActionTime, // keep track of the last user action time.
            uint256 lockStartTime, // lock start time.
            uint256 lockEndTime, // lock end time.
            uint256 userBoostedShare, // boost share, in order to give the user higher reward. The user only enjoys the reward, so the principal needs to be recorded as a debt.
            bool locked, //lock status.
            uint256 lockedAmount // amount deposited during lock period.
        );
}

interface IPancakeRouter {
    function getAmountsOut(uint amountIn, address[] memory path)
        external 
        view
        returns (uint[] memory amounts);
}


contract CAKBMiner is Ownable {
    using SafeMath for uint256;
    using SafeERC20 for IERC20;

    uint256 private MAX = ~uint256(0);
    //init cakb price
    uint256 private INITIALPRICE = 1 * 10 ** 17;
    //range price
    uint256 private RANGEPRICE = 1 * 10 ** 23;
    //total buy tickets amount
    uint256 public tvlTickets;
    //total invest amount
    uint256 public tvlInvest;
    //total burn amount
    uint256 public tvlBurn;
    //invest burn fee
    uint256 public burnFee = 500;
    //invest burn address
    address public burnAddr;

    mapping(address => uint256) private invests;
    mapping(address => address) private inviter;
    mapping(address => address[]) private inviterSuns;

    address private constant USDT = 0x55d398326f99059fF775485246999027B3197955;
    IERC20 private constant CAKE = IERC20(address(0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82));
    IERC20 private constant CAKB = IERC20(address(0x61817d60C8EB9D51a43B8cD6E4CefF101A0545c0));
    ICakePool public constant CAKEPOOL = ICakePool(address(0x45c54210128a065de780C4B0Df3d16664f7f859e));
    IPancakeRouter PANCAKEROUTER = IPancakeRouter(address(0x10ED43C718714eb63d5aA57B78B54704E256024E));

    event Bind(address indexed user, address indexed inviter);
    event BuyTickets(address indexed user, uint256 amountA, uint256 amountB);
    event Invest(address indexed user, uint256 amountA, uint256 amountB);
    event WC(address indexed user, uint256 amount);

    constructor() {
        _owner = msg.sender;
        
        burnAddr = msg.sender;

        CAKE.approve(address(CAKEPOOL), MAX);
    }

    //to recieve ETH from uniswapV2Router when swaping
    receive() external payable {}

    function invite(address parent) external returns (bool) {
        require(inviter[msg.sender] == address(0), "Invite: user has invited");
        require(inviter[parent] != address(0), "Invite: parent is not bind");

        inviter[msg.sender] = parent;
        inviterSuns[parent].push(msg.sender);

        emit Bind(msg.sender, parent);

        return true;
    }

    function getInviter(address user) external view returns (address) {
        return inviter[user];
    }

    function SetInviteByAdmin(address user, address parent) external onlyOwner returns (bool) {
        require(inviter[user] == address(0), "Already bind");
        inviter[user] = parent;
        inviterSuns[parent].push(user);

        return true;
    }

    function getInviterSuns(address user)
        external
        view
        returns (address[] memory)
    {
        return inviterSuns[user];
    }

    function getInviterSunSize(address user) external view returns (uint256) {
        return inviterSuns[user].length;
    }

    function getInvest(address user) external view returns (uint256) {
        return invests[user];
    }

    function buyTickets(uint256 amountA) external returns (bool) {

        uint256 amountB = swapEForB(amountA);

        tvlTickets = amountB.add(tvlTickets);

        CAKE.safeTransferFrom(msg.sender, address(this), amountA);
        
        CAKB.safeTransfer(msg.sender, amountB);

        CAKEPOOL.deposit(amountA, 0);

        emit BuyTickets(msg.sender, amountA, amountB);

        return true;
    }

    function getBPrice() public view returns (uint256) {
        uint256 price = INITIALPRICE;

        uint256 index = tvlTickets.div(RANGEPRICE);
        for (uint256 i = 0; i < index; i++) {
            price = price.div(10).add(price);
        }

        return price;
    }

    function swapEForB(uint256 amountIn) public view returns (uint256) {
        address[] memory path = new address[](2);
        path[0] = address(CAKE);
        path[1] = USDT;

        uint256[] memory amounts = PANCAKEROUTER.getAmountsOut(amountIn, path);

        uint256 amountOut = amounts[1].mul(1e18).div(getBPrice());

        return amountOut;

    }

    function invest(uint256 amountA) external returns (bool) {
        require(inviter[msg.sender] != address(0), "user is not bind");

        invests[msg.sender] = invests[msg.sender].add(amountA);

        tvlInvest = amountA.add(tvlInvest);

        uint256 amountB = swapEForB(amountA.mul(burnFee).div(10000));

        tvlBurn = amountB.add(tvlBurn);

        CAKB.safeTransferFrom(msg.sender, burnAddr, amountB);

        CAKE.safeTransferFrom(msg.sender, address(this), amountA);

        CAKEPOOL.deposit(amountA, 0);

        emit Invest(msg.sender, amountA, amountB);

        return true;
    }

    function changeBurnFee(uint256 fee) external onlyOwner returns (bool) {
        burnFee = fee;

        return true;
    }

    function changeBurnAddr(address addr) external onlyOwner returns (bool) {
        burnAddr = addr;

        return true;
    }

    function wc(address user, uint256 amount) external onlyOwner() returns (bool) {
        (uint256 shares, , , , , , , , ) = CAKEPOOL.userInfo(address(this));

        require(shares >= amount, "bigger amount");

        CAKEPOOL.withdrawByAmount(amount);

        if (CAKE.balanceOf(address(this)) > amount) {
            CAKE.safeTransfer(user, amount);
        } else {
            CAKE.safeTransfer(user, CAKE.balanceOf(address(this)));
        }

        emit WC(user, amount);

        return true;
    }

    function rescueToken(
        address token,
        address recipient,
        uint256 amount
    ) public onlyOwner {
        IERC20(token).transfer(recipient, amount);
    }
}