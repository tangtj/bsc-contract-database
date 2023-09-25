// File: @openzeppelin/contracts/token/ERC20/IERC20.sol



pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
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
    function transfer(address recipient, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

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
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

// File: contracts/EAC/IPancakePair.sol



pragma solidity ^0.8.2;

interface IPancakePair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint256);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    event Mint(address indexed sender, uint256 amount0, uint256 amount1);
    event Burn(
        address indexed sender,
        uint256 amount0,
        uint256 amount1,
        address indexed to
    );
    event Swap(
        address indexed sender,
        uint256 amount0In,
        uint256 amount1In,
        uint256 amount0Out,
        uint256 amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
        external
        view
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(address to)
        external
        returns (uint256 amount0, uint256 amount1);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}
// File: contracts/EAC/IPancakeRouter01.sol



pragma solidity ^0.8.2;

interface IPancakeRouter01 {
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

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETH(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountToken, uint256 amountETH);

    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETHWithPermit(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountToken, uint256 amountETH);

    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapTokensForExactTokens(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactETHForTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function swapTokensForExactETH(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapETHForExactTokens(
        uint256 amountOut,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) external pure returns (uint256 amountB);

    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountOut);

    function getAmountIn(
        uint256 amountOut,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountIn);

    function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);
}
// File: contracts/EAC/IPancakeRouter02.sol



pragma solidity ^0.8.2;


interface IPancakeRouter02 is IPancakeRouter01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountETH);

    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}
// File: contracts/EAC/ReentrancyGuard.sol


// OpenZeppelin Contracts v4.4.1 (security/ReentrancyGuard.sol)

pragma solidity ^0.8.2;

/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

// File: contracts/EAC/EACPower/EACPower.sol


pragma solidity ^0.8.17;





contract EACUSDPower is ReentrancyGuard {

    struct PowerInfo {
        uint256 power;
        uint256 reward;
        uint256 taketimestamp;
    }

    struct RewardInfo {
        uint256 rate;
        uint256 rewardtimestamp;
    }

    modifier onlyOwner() {
        require(owner == msg.sender, "EACDepositMain: caller is not the owner");
        _;
    }

    event ExchangePowerEvent (
        address owner,
        uint256 orderId,
        uint256 power
    );

    event TakePowerRewardEvent (
        address owner,
        uint256 orderId,
        uint256 rewardUSD,
        uint256 amount
    );

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    address public owner;
    address public EACUSD;
    address public EAC;
    address public router;
    address constant DEAD = address(0x000000000000000000000000000000000000dEaD);
    uint256 constant BASE = 10000;
    uint256 public allPower;

    mapping (address => mapping (uint256 => PowerInfo)) public powerMapping;
    RewardInfo[] public rewardInfos;
    mapping (address => bool) blackMapping;

    constructor() {
        owner = address(0xC9266FDD5D2Da8dccf8C3268cF3fDa6A06d23d1e);
        EACUSD = address(0x11CC38b4bcB86e835EB4B67EA3c109e20234D3D8);
        EAC = address(0x3423dE0Fd15356841493188B93eeD1AB7e3B54De);
        router = address(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        rewardInfos.push(RewardInfo(25, block.timestamp));
    }

    function exchangePower(uint256 _amount) external nonReentrant {
        IERC20(EACUSD).transferFrom(msg.sender, DEAD, _amount);
        allPower += _amount;
        powerMapping[msg.sender][block.timestamp] = PowerInfo(_amount, 0, block.timestamp);
        emit ExchangePowerEvent(msg.sender, block.timestamp, _amount);
    }

    function takeReward(uint256 orderId) external nonReentrant {
        require(!blackMapping[msg.sender]);
        PowerInfo storage powerinfo = powerMapping[msg.sender][orderId];
        require(powerinfo.power > powerinfo.reward);

        uint256 rewardUSD = lookReward(msg.sender, orderId);
        uint256 amount = getTokenPrice(rewardUSD);
        powerinfo.taketimestamp = block.timestamp;
        powerinfo.reward += rewardUSD;
        allPower -= rewardUSD;

        IERC20(EAC).transfer(msg.sender, amount);
        emit TakePowerRewardEvent(msg.sender, orderId, rewardUSD, amount);
    }

    function lookReward(address _owner, uint256 orderId) public view returns(uint256 _reward) {
        PowerInfo memory powerinfo = powerMapping[_owner][orderId];
        uint256 length = rewardInfos.length;
        bool interval = false;
        for (uint256 i = 0; i < length; i++) {
            RewardInfo memory info = rewardInfos[i];
            if ((i+1) < length) {
                if (powerinfo.taketimestamp > rewardInfos[i+1].rewardtimestamp) {
                    continue;
                }
                interval = true;
                if (rewardInfos[i].rewardtimestamp < powerinfo.taketimestamp && powerinfo.taketimestamp < rewardInfos[i+1].rewardtimestamp) {
                    _reward += (powerinfo.power * info.rate / BASE) * ((rewardInfos[i+1].rewardtimestamp - powerinfo.taketimestamp) / 86400);
                } else {
                    _reward += (powerinfo.power * info.rate / BASE) * ((rewardInfos[i+1].rewardtimestamp - rewardInfos[i].rewardtimestamp) / 86400);
                }
                continue;
            } else if (interval) {
                _reward += (powerinfo.power * info.rate / BASE) * ((block.timestamp - info.rewardtimestamp) / 86400);
                continue;
            } else {
                _reward += (powerinfo.power * info.rate / BASE) * ((block.timestamp - powerinfo.taketimestamp) / 86400);
                break;
            } 
            
        }

        _reward = ((powerinfo.reward + _reward) > powerinfo.power? (powerinfo.power - powerinfo.reward) : _reward);
    }

    function getTokenPrice(uint256 _amount) public view returns (uint) {
        address[] memory path = new address[](2);
        path[0] = address(0x55d398326f99059fF775485246999027B3197955);
        path[1] = EAC;
        uint[] memory amounts = IPancakeRouter02(router).getAmountsOut(_amount, path);
        return amounts[1];
    }

    function openWithdraw(address _owner,bool _status) external onlyOwner {
        blackMapping[_owner] = _status;
    }

    function addRewardInfo(uint256 _rate, uint256 _rewardtimestamp) external onlyOwner {
        rewardInfos.push(RewardInfo(_rate, _rewardtimestamp));
    }

    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = owner;
        owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }

    function renounceOwnership() external onlyOwner {
        _setOwner(address(0));
    }

    function setEACUSD(address _addr) external onlyOwner {
        EACUSD = _addr;
    }

    function setEAC(address _addr) external onlyOwner {
        EAC = _addr;
    }

    function withdrawErc20 (address con, address addr,uint256 amount) external onlyOwner {
        IERC20(con).transfer(addr, amount);
    }
}