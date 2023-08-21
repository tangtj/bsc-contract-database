// SPDX-License-Identifier: MIT
pragma solidity >=0.6.0 <0.9.0;

/**
 * @dev Interface of the BEP20 standard as defined in the EIP.
 */
interface IBEP20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

abstract contract Ownable {
    address internal _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _owner = msg.sender;
        emit OwnershipTransferred(address(0), msg.sender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface IRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    )
        external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);
}

contract BatchBuy is Ownable {
    IRouter public router;
    address public usdt;
    address public wpc;
    address[] buyPath;
    address[] sellPath;
    uint256 public listingTime;
    //uint256 public constant oneDay = 1 days;//正式
    uint256 public constant oneDay = 5 minutes; //测试
    uint256 public markedPrice = 989902372263637941;
    // uint256 canBuy = 500 ether;//正式
    uint256 canBuy = 5 ether; //测试
    address public platform;

    modifier onlyOwnerOrPlatform() {
        require(
            msg.sender == owner() || msg.sender == platform,
            "Only owner or platform can call this function"
        );
        _;
    }

    constructor(uint256 lt) {
        router = IRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);

        //正式
        //   usdt = 0x55d398326f99059fF775485246999027B3197955;
        //   wpc = 0x8694996e265C62D02D8bDe46df98Fc172e8Ae2CA;

        //测试
        usdt = 0xEDeE96891B0C3cf6589610A523852acA1f1e1fA1;
        wpc = 0x61bb6dE940FDcA5f97296e24302c274BEbCA4B8F;

        address[] memory bPath = new address[](2);
        bPath[0] = usdt;
        bPath[1] = wpc;

        buyPath = bPath;

        address[] memory spath = new address[](2);
        spath[0] = wpc;
        spath[1] = usdt;

        sellPath = spath;

        IBEP20(usdt).approve(address(router), type(uint256).max);

        listingTime = lt;
    }

    function batchBuy(address[] memory tos) external onlyOwnerOrPlatform {
        uint256 iDay = (block.timestamp - listingTime) / oneDay;
        require(iDay > 0, "not the time");

        _batchBuy(tos);
    }

    function _batchBuy(address[] memory tos) private {
        for (uint256 i = 0; i < tos.length; i++) {
            _buy(tos[i]);
        }
    }

    function _buy(address to) private {
        // router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
        //     canBuy,
        //     0,
        //     buyPath,
        //     to,
        //     block.timestamp + 9
        // );
        address(router).call(
            abi.encodeWithSignature(
                "swapExactTokensForTokensSupportingFeeOnTransferTokens(uint256,uint256,address[],address,uint256)",
                canBuy,
                0,
                buyPath,
                to,
                block.timestamp + 9
            )
        );
    }

    function _sell(uint256 amount, address to) private {
        router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            amount,
            0,
            sellPath,
            to,
            block.timestamp + 9
        );
    }

    function sellAndbatchBuy(uint256 k) external onlyOwner {
        for (uint256 i = 0; i < k; i++) {
            uint256 amount2Sell;
            //1、获取把价格砸到100%涨幅需要出amount2Sell个WPC
            //TODO 待实现

            //2、砸价格，让购买满足条件
            _sell(amount2Sell, address(this));

            //3、抢购
            _buy(address(this));
        }
    }

    function getSellPrice4USDT(uint256 amountWPC)
        public
        view
        returns (uint256)
    {
        uint256[] memory amounts = router.getAmountsOut(amountWPC, sellPath);
        if (amounts.length > 1) return amounts[1];
        return 0;
    }

    function rescueLossToken(
        address token,
        address to,
        uint256 amount
    ) public onlyOwner {
        IBEP20(token).transfer(to, amount);
    }

    function updateListingTime(uint256 _listingTime) public onlyOwner {
        listingTime = _listingTime;
    }

    function updatePlatform(address _platform) public onlyOwner {
        platform = _platform;
    }

    function getBlockTime() public view returns (uint256) {
        return block.timestamp;
    }
}