// SPDX-License-Identifier: MIT
pragma solidity ^0.8.1;

interface IERC20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

interface ISwapFactory {
    function getPair(
        address tokenA,
        address tokenB
    ) external view returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface ISwapPair {
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

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(
        address to
    ) external returns (uint256 amount0, uint256 amount1);

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

interface ISwapRouter {
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
    ) external returns (uint256 amountA, uint256 amountB, uint256 liquidity);

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
        returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);

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

    function getAmountsOut(
        uint256 amountIn,
        address[] calldata path
    ) external view returns (uint256[] memory amounts);

    function getAmountsIn(
        uint256 amountOut,
        address[] calldata path
    ) external view returns (uint256[] memory amounts);

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

library Address {
    function isContract(address account) internal view returns (bool) {
        return account.code.length > 0;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(
            address(this).balance >= amount,
            "Address: insufficient balance"
        );
        (bool success, ) = recipient.call{value: amount}("");
        require(
            success,
            "Address: unable to send value, recipient may have reverted"
        );
    }

    function functionCall(
        address target,
        bytes memory data
    ) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
    }

    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return
            functionCallWithValue(
                target,
                data,
                value,
                "Address: low-level call with value failed"
            );
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(
            address(this).balance >= value,
            "Address: insufficient balance for call"
        );
        require(isContract(target), "Address: call to non-contract");
        (bool success, bytes memory returndata) = target.call{value: value}(
            data
        );
        return verifyCallResult(success, returndata, errorMessage);
    }

    function functionStaticCall(
        address target,
        bytes memory data
    ) internal view returns (bytes memory) {
        return
            functionStaticCall(
                target,
                data,
                "Address: low-level static call failed"
            );
    }

    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");
        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    function functionDelegateCall(
        address target,
        bytes memory data
    ) internal returns (bytes memory) {
        return
            functionDelegateCall(
                target,
                data,
                "Address: low-level delegate call failed"
            );
    }

    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
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

library SafeMath {
    function tryAdd(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    function trySub(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    function tryMul(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    function tryDiv(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    function tryMod(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

abstract contract Ownable {
    address private _owner;
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _transferOwnership(_msgSender());
    }

    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract Manager is Ownable {
    address public manager;
    modifier onlyManager() {
        require(
            owner() == _msgSender() || manager == _msgSender(),
            "Ownable: Not Manager"
        );
        _;
    }

    function setManager(address account) public virtual onlyManager {
        manager = account;
    }
}

contract MGLP is Manager {
    using SafeMath for uint256;
    using Address for address;
    struct UserInfo {
        bool isExist;
        uint amount;
        uint balance;
        uint rewardTotal;
        uint rewardDebt;
        uint lpBalance;
        uint lpTotal;
        uint lpLock;
        uint lpPerBlock;
        uint lpLastBlock;
    }
    mapping(address => bool) public isBlackList;
    mapping(address => UserInfo) public users;
    mapping(uint256 => address) public userAdds;
    uint public userTotal;
    uint private _rewardPerLP;
    uint private _rewardTotal;
    uint private _lastBalance;
    IERC20 private _LP;
    IERC20 private _USDT;
    IERC20 private _MG;
    event Deposit(address user, uint256 amount);
    event Relieve(address user, uint256 amount);
    event Withdraw(address user, uint256 amount);

    constructor() {
        manager = 0x16CC5d31Ed440D5AA46b7D26732d01B9153b47FF;
        _USDT = IERC20(0x55d398326f99059fF775485246999027B3197955);
        _MG = IERC20(0x7e13Ec7601f5CffEC5175Ad28667507A1fCA89b0);
        _LP = IERC20(0x1c141E42af18395b1265089a270451EFDBbd43Bc);
    }

    function withdrawToken(IERC20 token, uint amount) public onlyManager {
        token.transfer(msg.sender, amount);
    }

    function setToken(address mg, address usdt, address lp) public onlyManager {
        _LP = IERC20(lp);
        _MG = IERC20(mg);
        _USDT = IERC20(usdt);
    }

    function setIsBlackList(address account, bool enable) public onlyManager {
        isBlackList[account] = enable;
    }

    function deposit(address account, uint256 amount, bool isLock) public {
        updateUser(account);
        UserInfo storage user = users[account];
        if (amount > 0) {
            _LP.transferFrom(msg.sender, address(this), amount);
            user.amount += amount;
            user.lpTotal += amount;
            if (isLock) {
                user.lpLock += amount;
                user.lpLastBlock = block.number;
                user.lpPerBlock = user.lpLock / 28800 / 100;
            } else {
                user.lpBalance += amount;
            }
        }
        user.rewardDebt = user.amount.mul(_rewardPerLP).div(1e12);
        if (!user.isExist) {
            user.isExist = true;
            userTotal = userTotal.add(1);
            userAdds[userTotal] = account;
        }
        emit Deposit(account, amount);
    }

    function depositBatch(
        address[] calldata accounts,
        uint256 amount,
        bool isLock
    ) public {
        for (uint256 i = 0; i < accounts.length; i++) {
            address account = accounts[i];
            updateUser(account);
            UserInfo storage user = users[account];
            if (amount > 0) {
                _LP.transferFrom(msg.sender, address(this), amount);
                user.amount += amount;
                user.lpTotal += amount;
                if (isLock) {
                    user.lpLock += amount;
                    user.lpLastBlock = block.number;
                    user.lpPerBlock = user.lpLock / 28800 / 100;
                } else {
                    user.lpBalance += amount;
                }
            }
            user.rewardDebt = user.amount.mul(_rewardPerLP).div(1e12);
            if (!user.isExist) {
                user.isExist = true;
                userTotal = userTotal.add(1);
                userAdds[userTotal] = account;
            }
            emit Deposit(account, amount);
        }
    }

    function getRewardPerLP() public view returns (uint256) {
        uint256 accCakePerShare = _rewardPerLP;
        uint256 lpSupply = _LP.balanceOf(address(this));
        if (_USDT.balanceOf(address(this)) > _lastBalance) {
            uint256 cakeReward = _USDT.balanceOf(address(this)) - _lastBalance;
            if (lpSupply > 0)
                accCakePerShare = accCakePerShare.add(
                    cakeReward.mul(1e12).div(lpSupply)
                );
            else accCakePerShare = accCakePerShare.add(cakeReward.mul(1e12));
        }
        return accCakePerShare;
    }

    function getRewardTotal() public view returns (uint256) {
        uint256 cakeReward;
        if (_USDT.balanceOf(address(this)) > _lastBalance) {
            cakeReward = _USDT.balanceOf(address(this)) - _lastBalance;
        }
        return _rewardTotal.add(cakeReward);
    }

    function getUserInfo(
        address account
    ) external view returns (UserInfo memory user) {
        user = users[account];
        uint256 accCakePerShare = _rewardPerLP;
        uint256 lpSupply = _LP.balanceOf(address(this));
        if (_USDT.balanceOf(address(this)) > _lastBalance) {
            uint256 cakeReward = _USDT.balanceOf(address(this)) - _lastBalance;
            if (lpSupply > 0)
                accCakePerShare = accCakePerShare.add(
                    cakeReward.mul(1e12).div(lpSupply)
                );
            else accCakePerShare = accCakePerShare.add(cakeReward.mul(1e12));
        }
        user.balance = user
            .amount
            .mul(accCakePerShare)
            .div(1e12)
            .sub(user.rewardDebt)
            .add(user.balance);
        user.rewardTotal = user
            .amount
            .mul(accCakePerShare)
            .div(1e12)
            .sub(user.rewardDebt)
            .add(user.rewardTotal);
    }

    function relieve() public {
        address account = msg.sender;
        require(!isBlackList[account], "Invalid");
        updateUser(account);
        UserInfo storage user = users[account];
        if (user.lpBalance > 0) {
            uint amount = user.lpBalance;
            user.lpBalance = 0;
            user.amount = user.amount.sub(amount);
            user.rewardDebt = user.amount.mul(_rewardPerLP).div(1e12);
            emit Relieve(account, amount);
            _LP.transfer(address(_LP), amount);
            (uint256 amount0, uint256 amount1) = ISwapPair(address(_LP)).burn(
                address(this)
            );
            if (address(_MG) == ISwapPair(address(_LP)).token0()) {
                _MG.transfer(address(1), amount0);
                _USDT.transfer(account, amount1);
            } else {
                _MG.transfer(address(1), amount1);
                _USDT.transfer(account, amount0);
            }
            emit RemoveLiquidity(amount);
            _lastBalance = _USDT.balanceOf(address(this));
        }
    }

    function withdraw() public {
        address account = msg.sender;
        require(!isBlackList[account], "Invalid");
        updateUser(account);
        UserInfo storage user = users[account];
        if (user.balance > 0) {
            _USDT.transfer(account, user.balance);
            user.balance = 0;
            _lastBalance = _USDT.balanceOf(address(this));
        }
    }

    function updateUser(address account) public {
        updatePool();
        UserInfo storage user = users[account];
        if (user.amount > 0) {
            uint256 pending = user.amount.mul(_rewardPerLP).div(1e12).sub(
                user.rewardDebt
            );
            if (pending > 0) {
                user.balance = user.balance.add(pending);
                user.rewardTotal = user.rewardTotal.add(pending);
            }
        }
        user.rewardDebt = user.amount.mul(_rewardPerLP).div(1e12);
        if (user.lpLock > 0 && block.number > user.lpLastBlock) {
            uint reward = (block.number - user.lpLastBlock) * user.lpPerBlock;
            if (reward > user.lpLock) {
                reward = user.lpLock;
                user.lpLock = 0;
            } else {
                user.lpLock -= reward;
            }
            user.lpBalance += reward;
            user.lpLastBlock = block.number;
        }
    }

    function updatePool() public {
        uint256 lpSupply = _LP.balanceOf(address(this));
        if (_USDT.balanceOf(address(this)) > _lastBalance) {
            uint256 cakeReward = _USDT.balanceOf(address(this)) - _lastBalance;
            _lastBalance = _USDT.balanceOf(address(this));
            if (lpSupply > 0)
                _rewardPerLP = _rewardPerLP.add(
                    cakeReward.mul(1e12).div(lpSupply)
                );
            else _rewardPerLP = _rewardPerLP.add(cakeReward.mul(1e12));
            _rewardTotal = _rewardTotal.add(cakeReward);
        }
    }

    event RemoveLiquidity(uint256 amount);
}