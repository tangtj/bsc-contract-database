/**
 * SPDX-License-Identifier: Unlicensed
 **/

pragma solidity 0.8.19;

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

contract ReentrancyGuard {
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
     * by making the `nonReentrant` function external, and make it call a
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

    modifier isHuman() {
        require(tx.origin == msg.sender, "sorry humans only");
        _;
    }
}


contract IclickInuVault is ReentrancyGuard {
    using SafeERC20 for IERC20;
    using Address for address;
    IERC20 internal immutable _usdToken;
    IERC20 private token;
    IUniswapV2Router02 public immutable uniswapV2Router;
    IclickInuVault public immutable oldVault;

    struct User {
        uint _id;
        uint8 _plan;
        uint _deposits; // Token Deposited in USDT value
        uint _earnings; // Available
        uint _commissions; // Total commission earned
        uint _withdrawn; // paid out
        uint expires; // last activation timestamp
        uint lastUpdate;
    }

    // Program Settings
    uint internal constant DIVIDER = 10000;
    uint[] internal ACTIVPLANS = [
        50 ether,
        100 ether,
        250 ether,
        500 ether,
        2000 ether,
        5000 ether
    ];

    uint[] internal PROFITCAP = [120, 122, 125, 130, 132, 135]; // Max %earnings per pack
    uint internal immutable LISTINGPRICE = 0.000080 ether;
    uint internal constant DEVFEES = 300; // 3% charge for ops
    // Global Records
    uint public lastUserid;
    uint public totalActivation; // total token spent
    uint public totalUSDTVolume; // total token spent in USDT value
    uint public totalDeposits; // total token deposited
    uint public totalDepositsVlm; // total token deposited in USDT value
    uint public totalUSDTCredited; // total USDT created to users
    uint public totalWithdrawn; // total withdrawn in usdt

    // Operations
    address private constant systemOps = 0x536682fA000E22D4B41cb4E44Fb094CA18db20B1;
    address private constant activationw = 0x69eaf4Ec5431c4EA6913e3Dea5DB3cD49cA00f9c;

    mapping(address => User) public users;

    constructor() {
        _usdToken = IERC20(address(0x55d398326f99059fF775485246999027B3197955)); // testnet 0xC6Efc0f7AF6e0B3e413d8FdD339FAf4d9a6e2D8F 0xaB1a4d4f1D656d2450692D237fdD6C7f9146e814 // mainnet 0x55d398326f99059fF775485246999027B3197955
        token = IERC20(address(0xc8C06a58E4ad7c01b9bb5Af6C76a7a1CfEBd0319)); // testnet 0x80247A78b06bac28B2086D0eb0012feCD0442B66 // mainnet 0xc8C06a58E4ad7c01b9bb5Af6C76a7a1CfEBd0319
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E); // testnet 0xD99D1c33F9fC3444f8101754aBC46c52416550D1 // mainnet 0x10ED43C718714eb63d5aA57B78B54704E256024E
        uniswapV2Router = _uniswapV2Router;
        oldVault = IclickInuVault(0x4Bb8328fAC1e193165771cad9DCB4b3003e9C7d4);
    }

    function registerMember(address _user) private {
        lastUserid++;
        users[_user]._id = lastUserid;
    }

    function getPack(uint _amount) private view returns (uint8 _packId) {
        _packId = 1;
        _amount = _amount * 115 / 100;
        if (_amount >= ACTIVPLANS[0] && _amount < ACTIVPLANS[1]) {
            _packId = 1;
        } else if (_amount >= ACTIVPLANS[1] && _amount < ACTIVPLANS[2]) {
            _packId = 2;
        }
        if (_amount >= ACTIVPLANS[2] && _amount < ACTIVPLANS[3]) {
            _packId = 3;
        }
        if (_amount >= ACTIVPLANS[3] && _amount < ACTIVPLANS[4]) {
            _packId = 4;
        }
        if (_amount >= ACTIVPLANS[4] && _amount < ACTIVPLANS[5]) {
            _packId = 5;
        } else if (_amount >= ACTIVPLANS[5]) {
            _packId = 6;
        }
    }

    function activatePack(uint _amount) public {
        require(!address(msg.sender).isContract(), "NotAllowed");
        User storage _user = users[msg.sender];

        if (_user._id == 0) {
            registerMember(msg.sender);
        }

        require(
            token.transferFrom(msg.sender, activationw, _amount),
            "TransferFailed"
        );

        // get TOKEN value in USDT
        uint _tokenUSDTValue = _getTokenRate(_amount, address(token), address(_usdToken));

        require(_tokenUSDTValue >= 45 ether, "$50min");

        uint8 _packId = getPack(_tokenUSDTValue);

        require(_user.expires <= block.timestamp, "DoubleActivation");

        _user._plan = _packId;
        _user.expires = block.timestamp + 30 days;
        _user.lastUpdate = block.timestamp;

        totalActivation += _amount;
        totalUSDTVolume += _tokenUSDTValue;
    }

    function withdrawUSDT() public {
        require(!address(msg.sender).isContract(), "NotAllowed");
        // require(address(msg.sender) == address(systemOps), "NotAllowed");
        User storage user_ = users[msg.sender];
        require(
            user_._id != 0 && user_._earnings > 0,
            "NoBalance"
        );
        uint _amount = user_._earnings;
        uint256 _contractBalance = _usdToken.balanceOf(address(this));
        require(_contractBalance >= _amount, "NoLiquidity");
        user_._earnings = 0;
        user_._withdrawn += _amount;
        uint _fees = (_amount * DEVFEES) / DIVIDER;
        uint _toSend = _amount - _fees;

        _usdToken.safeTransfer(msg.sender, _toSend);
        _usdToken.safeTransfer(systemOps, _fees);

        totalWithdrawn += _amount;
    }

    function depositFunds(uint _amount) public {
        require(!address(msg.sender).isContract(), "NotAllowed");
        User storage _user = users[msg.sender];

        if (_user._id == 0) {
            registerMember(msg.sender);
        }

        // get TOKEN value in USDT
        uint _tokenUSDTValue = _getTokenRate(_amount, address(token), address(_usdToken));
        
        require(_tokenUSDTValue >= 5 ether, "rq$10");

        require(
            token.transferFrom(msg.sender, activationw, _amount),
            "TransferFailed"
        );
        _user._deposits += _tokenUSDTValue;
        // notify new deposit
        totalDeposits += _amount;
        totalDepositsVlm += _tokenUSDTValue;
    }

    function updateClicks(address _user, uint _amount, uint _commission, uint _dbE) public {
        require(address(msg.sender) == address(systemOps), "NotAllowed");
        User storage user_ = users[_user];
        require(
            user_._id != 0 &&
                user_.expires >= block.timestamp &&
                user_.lastUpdate + 1 hours <= block.timestamp,
            "inValid"
        );
        user_._earnings += _amount;
        user_._commissions = _commission;
        user_.lastUpdate = block.timestamp;
        totalUSDTCredited += _amount;

        uint _totalClaimed = user_._earnings + user_._withdrawn;
        if(_totalClaimed > _dbE){
            uint _diff = _totalClaimed - _dbE;
            if(user_._earnings > _diff){
                user_._earnings -= _diff;
            }
            else{
                user_._earnings = 0;
            }
        }
    }

    function getTokenAmount(
        address _tokenA,
        address _tokenB,
        uint _amountIn
    ) private view returns (uint[] memory amounts) {
        address[] memory path = new address[](2);
        path[0] = _tokenA;
        path[1] = _tokenB;
        amounts = uniswapV2Router.getAmountsOut(_amountIn, path);
        return amounts;
    }

    function _getTokenRate(
        uint256 _amount,
        address _base,
        address _secondary
    ) private view returns (uint256 _tokens) {
        uint[] memory _estimates = getTokenAmount(
            address(_base),
            address(_secondary),
            _amount
        );
        return _estimates[1];
    }

    function getIclickInuRate() public view returns(uint _value){
        _value = _getTokenRate(1 ether, address(token), address(_usdToken)); // fecth from panackageswap [0.0000002454815253887044]
    }

    function moveOldUsers(address _oUser, uint _actualBal) public {
        require(address(msg.sender) == address(systemOps), "NotAllowed");
        
        require(users[_oUser]._id == 0, 'Duplicate');

        (uint _id, uint8 _plan, uint _deposits, uint _earnings, uint _commissions, 
        uint _withdrawn, uint _expires, ) = oldVault.users(_oUser);
        User storage _user = users[_oUser];

        _user._id = _id;
        _user._plan = _plan;
        _user._deposits = _deposits; // Token Deposited in USDT value
        _user._earnings = _earnings; // Available
        _user._commissions = _commissions; // Total commission earned
        _user._withdrawn = _withdrawn; // paid out
        _user.expires = _expires; // last activation timestamp
        _user.lastUpdate = block.timestamp;

        lastUserid++;
        uint _getSettlement = _earnings + _withdrawn;
        
        if(_getSettlement > _actualBal){
            uint _diff = _getSettlement - _actualBal;
            if(_earnings > _diff){
                uint _updt = _earnings - _diff;
                _user._earnings = _updt;
            }
            else{
                _user._earnings = 0;
            }
        }
        totalUSDTCredited += _earnings + _withdrawn;
        totalWithdrawn += _withdrawn;
        // lastUserid = oldVault.lastUserid();
        // totalActivation = oldVault.totalActivation(); // total token spent
        // totalUSDTVolume = oldVault.totalUSDTVolume(); // total token spent in USDT value
        // totalDeposits = oldVault.totalDeposits(); // total token deposited
        // totalDepositsVlm = oldVault.totalDepositsVlm(); // total token deposited in USDT value
        // totalUSDTCredited = oldVault.totalUSDTCredited(); // total USDT created to users
        // totalWithdrawn = oldVault.totalWithdrawn(); // total withdrawn in usdt
        // _movedUsers = true;
    }
}

library Address {
    function isContract(address account) internal view returns (bool) {
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        assembly {
            codehash := extcodehash(account)
        }
        return (codehash != 0x0 && codehash != accountHash);
    }
}

library SafeERC20 {
    using Address for address;

    function safeTransferFrom(
        IERC20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        callOptionalReturn(
            token,
            abi.encodeWithSelector(token.transferFrom.selector, from, to, value)
        );
    }

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        callOptionalReturn(
            token,
            abi.encodeWithSelector(token.transfer.selector, to, value)
        );
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves.

        // A Solidity high level call has three parts:
        //  1. The target address is checked to verify it contains contract code
        //  2. The call itself is made, and success asserted
        //  3. The return value is decoded, which in turn checks the size of the returned data.
        // solhint-disable-next-line max-line-length
        require(address(token).isContract(), "SafeERC20: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = address(token).call(data);
        require(success, "SafeERC20: low-level call failed");

        if (returndata.length > 0) {
            // Return data is optional
            // solhint-disable-next-line max-line-length
            require(
                abi.decode(returndata, (bool)),
                "SafeERC20: ERC20 operation did not succeed"
            );
        }
    }
}

interface IERC20 {
    function balanceOf(address owner) external view returns (uint256);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);
}

interface IUniswapV2Router01 {
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

    function getAmountsOut(
        uint amountIn,
        address[] calldata path
    ) external view returns (uint[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
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
}