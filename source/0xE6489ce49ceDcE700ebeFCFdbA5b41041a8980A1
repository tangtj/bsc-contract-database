// SPDX-License-Identifier: MIT

pragma solidity ^0.8.9;

library SafeMathInt {
    int256 private constant MIN_INT256 = int256(1) << 255;
    int256 private constant MAX_INT256 = ~(int256(1) << 255);

    function mul(int256 a, int256 b) internal pure returns (int256) {
        int256 c = a * b;

        require(c != MIN_INT256 || (a & MIN_INT256) != (b & MIN_INT256));
        require((b == 0) || (c / b == a), 'mul overflow');
        return c;
    }

    function div(int256 a, int256 b) internal pure returns (int256) {
        require(b != -1 || a != MIN_INT256);

        return a / b;
    }

    function sub(int256 a, int256 b) internal pure returns (int256) {
        int256 c = a - b;
        require((b >= 0 && c <= a) || (b < 0 && c > a),
            'sub overflow');
        return c;
    }

    function add(int256 a, int256 b) internal pure returns (int256) {
        int256 c = a + b;
        require((b >= 0 && c >= a) || (b < 0 && c < a),
            'add overflow');
        return c;
    }

    function abs(int256 a) internal pure returns (int256) {
        require(a != MIN_INT256,
            'abs overflow');
        return a < 0 ? -a : a;
    }

    function max(uint256 a, uint256 b) internal pure returns (uint256) {
        return a >= b ? a : b;
    }

    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }
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

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0,
            'parameter 2 can not be 0');
        return a % b;
    }

    function max(uint256 a, uint256 b) internal pure returns (uint256) {
        return a >= b ? a : b;
    }

    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }
}

interface IBEP20 {

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);

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

contract Ownable {
    address private _owner;

    event OwnershipRenounced(address indexed previousOwner);
    event TransferOwnerShip(address indexed previousOwner);

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _owner = msg.sender;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, 'Not owner');
        _;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipRenounced(_owner);
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        emit TransferOwnerShip(newOwner);
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0),
            'Owner can not be 0');
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract TokenFarm is Ownable {

    using SafeMath for uint256;
    using SafeMathInt for int256;

    IUniswapV2Router02 public immutable uniswapV2Router;

    struct UserInfo {
        uint256 amount;     // How many tokens the user has provided.
        uint256 stakingTime; // The time at which the user staked tokens.
        uint256 rewardClaimed;
    }

    struct PoolInfo {
        address tokenAddress;
        address rewardTokenAddress;
        uint256 maxPoolSize; 
        uint256 currentPoolSize;
        uint256 maxContribution;
        uint256 rewardAmount; 
        uint256 emergencyFees; // it is the fees in percentage, final fees is emergencyFees/1000
        uint256 lockDays;
        bool poolType; // true for public staking, false for whitelist staking
        bool poolActive;
        uint256 unstakeBurnFee; // it is the fees in percentage, final fees is unstakeBurnFee/1000
        uint256 unstakePoolFee; // it is the fees in percentage, final fees is unstakePoolFee/1000
        uint256 unstakeMarketingWalletFee; // it is the fees in percentage, final fees is unstakeMarketingWalletFee/1000
    }

    // Info of each pool.
    PoolInfo[] public poolInfo;
    bool lock_= false;

    uint256 public totalRewardsClaimed = 0;
    // Info of each user that stakes tokens.
    mapping (uint256 => mapping (address => UserInfo)) public userInfo;
    mapping (uint256 => mapping (address => bool)) public whitelistedAddress;
    mapping (address => bool) public isAuthorized;

    address public marketingWallet;

    event Deposit(address indexed user, uint256 indexed pid, uint256 amount);
    event Withdraw(address indexed user, uint256 indexed pid, uint256 amount);
    event EmergencyWithdraw(address indexed user, uint256 indexed pid, uint256 amount);


    constructor () {

        address currentRouter;
        marketingWallet = 0xb53ea262Ac8F62103Cde9EFcB0102A8333Ba5056;

        //Adding Variables for all the routers for easier deployment for our customers.
        if (block.chainid == 56) {
            currentRouter = 0x10ED43C718714eb63d5aA57B78B54704E256024E; // PCS Router
        } else if (block.chainid == 97) {
            currentRouter = 0xD99D1c33F9fC3444f8101754aBC46c52416550D1; // PCS Testnet
        } else if (block.chainid == 43114) {
            currentRouter = 0x60aE616a2155Ee3d9A68541Ba4544862310933d4; //Avax Mainnet
        } else if (block.chainid == 137) {
            currentRouter = 0xa5E0829CaCEd8fFDD4De3c43696c57F7D7A678ff; //Polygon Ropsten
        } else if (block.chainid == 250) {
            currentRouter = 0xF491e7B69E4244ad4002BC14e878a34207E38c29; //SpookySwap FTM
        } else if (block.chainid == 3) {
            currentRouter = 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D; //Ropsten
        } else if (block.chainid == 1 || block.chainid == 4) {
            currentRouter = 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D; //Mainnet
        } else {
            currentRouter = 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D; //Mainnet
            // revert();
        }

        //End of Router Variables.

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(currentRouter);

        uniswapV2Router = _uniswapV2Router;
        
    }


    modifier lock {
        require(!lock_, "Process is locked");
        lock_ = true;
        _;
        lock_ = false;
    }

    function poolLength() public view returns (uint256) {
        return poolInfo.length;
    }

    function addPool (address _tokenAddress, address _rewardTokenAddress, uint256 _maxPoolSize, uint256 _maxContribution, uint256 _emergencyFees, uint256 _lockDays, bool _poolType, bool _poolActive, uint256 _unstakeBurnFee, uint256 _unstakePoolFee, uint256 _unstakeMarketingWalletFee) public onlyOwner {
        poolInfo.push(PoolInfo({
            tokenAddress: _tokenAddress,
            rewardTokenAddress: _rewardTokenAddress,
            maxPoolSize: _maxPoolSize,
            currentPoolSize: 0,
            maxContribution: _maxContribution,
            rewardAmount: 0,
            emergencyFees: _emergencyFees,
            lockDays: _lockDays,
            poolType: _poolType,
            poolActive: _poolActive,
            unstakeBurnFee: _unstakeBurnFee,
            unstakePoolFee: _unstakePoolFee,
            unstakeMarketingWalletFee: _unstakeMarketingWalletFee
        }));
    }

    function updateMaxPoolSize (uint256 _pid, uint256 _maxPoolSize) public onlyOwner{
        require (_pid < poolLength(), "Invalid pool ID");
        require (_maxPoolSize >= poolInfo[_pid].currentPoolSize, "Cannot reduce the max size below the current pool size");
        poolInfo[_pid].maxPoolSize = _maxPoolSize;
    }

    function updateMaxContribution (uint256 _pid, uint256 _maxContribution) public onlyOwner{
        require (_pid < poolLength(), "Invalid pool ID");
        poolInfo[_pid].maxContribution = _maxContribution;
    }

    function setIsAuthorized ( address _address, bool _isAuthorized) public onlyOwner {
        isAuthorized[_address] = _isAuthorized;
    }

    function addRewards (uint256 _pid, uint256 _amount) public onlyOwner {
        require (_pid < poolLength(), "Invalid pool ID");

        address _tokenAddress = poolInfo[_pid].rewardTokenAddress;
        IBEP20 token = IBEP20 (_tokenAddress);
        bool success = token.transferFrom(msg.sender, address(this), _amount);
        require (success, "Transfer From failed. Please approve the token");
        
        poolInfo[_pid].rewardAmount += _amount;
    }

    function deposit (uint256 _pid, uint256 _amount) public {
        require (isAuthorized[msg.sender], "You are not authorized to add pool token data");
        require (_pid <= poolLength(), "Invalid pool ID");
        
        poolInfo[_pid-1].rewardAmount += _amount;
    }

    function updateEmergencyFees (uint256 _pid, uint256 _emergencyFees) public onlyOwner {
        require (_pid < poolLength(), "Invalid pool ID");
        if (poolInfo[_pid].currentPoolSize > 0){
            require (_emergencyFees <= poolInfo[_pid].emergencyFees, "You can't increase the emergency fees when people started staking");
        }
        poolInfo[_pid].emergencyFees = _emergencyFees;
    }

    function updateLockDays (uint256 _pid, uint256 _lockDays) public onlyOwner {
        require (_pid < poolLength(), "Invalid pool ID");
        require (poolInfo[_pid].currentPoolSize == 0, "Cannot change lock time after people started staking");
        poolInfo[_pid].lockDays = _lockDays;
    }

    // this function is to withdraw extra tokens locked in the contract.
    function withdrawLockedTokens (address _tokenAddress) external onlyOwner returns (bool) {
        IBEP20 token = IBEP20 (_tokenAddress);
        uint256 balance = token.balanceOf(address(this));

        bool success = token.transfer(msg.sender, balance);
        return success;
    }

    function updatePoolType (uint256 _pid, bool _poolType) public onlyOwner {
        require (_pid < poolLength(), "Invalid pool ID");
        poolInfo[_pid].poolType = _poolType;
    }

    function updatePoolActive (uint256 _pid, bool _poolActive) public onlyOwner {
        require (_pid < poolLength(), "Invalid pool ID");
        poolInfo[_pid].poolActive = _poolActive;
    }

    function updateUnstakeBurnFee (uint256 _pid, uint256 _unstakeBurnFee) public onlyOwner {
        require (_pid < poolLength(), "Invalid pool ID");
        poolInfo[_pid].unstakeBurnFee = _unstakeBurnFee;
    }

    function updateUnstakePoolFee (uint256 _pid, uint256 _unstakePoolFee) public onlyOwner {
        require (_pid < poolLength(), "Invalid pool ID");
        poolInfo[_pid].unstakePoolFee = _unstakePoolFee;
    }

    function updateUnstakeMarketingWalletFee (uint256 _pid, uint256 _unstakeMarketingWalletFee) public onlyOwner {
        require (_pid < poolLength(), "Invalid pool ID");
        poolInfo[_pid].unstakeMarketingWalletFee = _unstakeMarketingWalletFee;
    }

    function updateMarketingWallet (address _marketingWallet) public onlyOwner {
        marketingWallet = _marketingWallet;
    }

    function addWhitelist (uint256 _pid, address [] memory _whitelistAddresses) public onlyOwner {
        require (_pid < poolLength(), "Invalid pool ID");
        uint256 length = _whitelistAddresses.length;
        require (length<= 200, "Can add only 200 wl at a time");
        for (uint256 i = 0; i < length; i++){
            address _whitelistAddress = _whitelistAddresses[i];
            whitelistedAddress[_pid][_whitelistAddress] = true;
        }
    }

    function emergencyLock (bool _lock) public onlyOwner {
        lock_ = _lock;
    }

    function getUserLockTime (uint256 _pid, address _user) public view returns (uint256) {
        return (userInfo[_pid][_user].stakingTime).add((poolInfo[_pid].lockDays).mul(1 days));
    }

    function stakeTokens (uint256 _pid, uint256 _amount) public {
        require (_pid < poolLength(), "Invalid pool ID");
        require (poolInfo[_pid].poolActive, "Pool is not active");
        require (poolInfo[_pid].currentPoolSize.add(_amount) <= poolInfo[_pid].maxPoolSize, "Staking exceeds max pool size");
        require ((userInfo[_pid][msg.sender].amount).add(_amount) <= poolInfo[_pid].maxContribution , "Max Contribution exceeds");
        if (poolInfo[_pid].poolType == false){
            require (whitelistedAddress[_pid][msg.sender], "You are not whitelisted for this pool");
        }

        address _tokenAddress = poolInfo[_pid].tokenAddress;
        IBEP20 token = IBEP20 (_tokenAddress);
        bool success = token.transferFrom(msg.sender, address(this), _amount);
        require (success, "Transfer From failed. Please approve the token");

        poolInfo[_pid].currentPoolSize = (poolInfo[_pid].currentPoolSize).add(_amount);
        uint256 _stakingTime = block.timestamp; 
        _amount = _amount.add(userInfo[_pid][msg.sender].amount);
        uint256 _rewardClaimed = userInfo[_pid][msg.sender].rewardClaimed;
        userInfo[_pid][msg.sender] = UserInfo ({
            amount: _amount,
            stakingTime: _stakingTime,
            rewardClaimed: _rewardClaimed
        });
    }

    function claimableRewards (uint256 _pid, address _user) public view returns (uint256) {
        require (_pid < poolLength(), "Invalid pool ID");

        uint256 _refundValue = ((userInfo[_pid][_user].amount * poolInfo[_pid].rewardAmount ) / (poolInfo[_pid].currentPoolSize));
        return _refundValue;
    }

    function unstakeTokens (uint256 _pid, address _withdrawToken) public {
        require (_pid < poolLength(), "Invalid pool ID");
        require (userInfo[_pid][msg.sender].amount > 0 , "You don't have any staked tokens");
        require (userInfo[_pid][msg.sender].stakingTime > 0 , "You don't have any staked tokens");
        require (getUserLockTime(_pid, msg.sender) < block.timestamp , "Your maturity time is not reached. If you want you can do EmergencyWithdraw");
        
        address _tokenAddress = poolInfo[_pid].tokenAddress;
        IBEP20 token = IBEP20 (_tokenAddress);
        address _rewardTokenAddress = poolInfo[_pid].rewardTokenAddress;
        IBEP20 rewardToken = IBEP20 (_rewardTokenAddress);
        uint256 _amount = userInfo[_pid][msg.sender].amount;

        uint256 _refundValue = claimableRewards(_pid, msg.sender);
        userInfo[_pid][msg.sender].rewardClaimed = _refundValue;
        poolInfo[_pid].rewardAmount -= _refundValue;
        poolInfo[_pid].currentPoolSize = (poolInfo[_pid].currentPoolSize).sub(userInfo[_pid][msg.sender].amount);
        userInfo[_pid][msg.sender].amount = 0;

        // cut the respective fees and transfer the remaining amount
        uint256 _unstakeBurnFeeAmount = (poolInfo[_pid].unstakeBurnFee * _amount) / 1000;
        uint256 _unstakePoolFeeAmount = (poolInfo[_pid].unstakePoolFee * _amount) / 1000;
        uint256 _unstakeMarketingWalletFeeAmount = (poolInfo[_pid].unstakeMarketingWalletFee * _amount) / 1000;
        uint256 _fees = _unstakeBurnFeeAmount + _unstakePoolFeeAmount + _unstakeMarketingWalletFeeAmount;

        // burn the fees
        bool success = token.transfer(address(0xdead), _unstakeBurnFeeAmount);

        // transfer the fees to the marketing wallet
        success = token.transfer(marketingWallet, _unstakeMarketingWalletFeeAmount);
        

        bool success1 = token.transfer(msg.sender, _amount - _fees);

        // cases for token withdraw
        if (_withdrawToken == _rewardTokenAddress){
            bool success2 = rewardToken.transfer(msg.sender, _refundValue);
            require(success2 , "Transfer failed");

        } else if (_withdrawToken == uniswapV2Router.WETH()){

            // swap the reward token for eth and send to the user
            address[] memory path = new address[](2);
            path[0] = _rewardTokenAddress;
            path[1] = uniswapV2Router.WETH();
            rewardToken.approve(address(uniswapV2Router), _refundValue);
            uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                _refundValue,
                0, // accept any amount of ETH
                path,
                msg.sender,
                block.timestamp
            );

        } else {
                
            // swap the reward token for the withdraw token and send to the user
            address[] memory path = new address[](3);
            path[0] = _rewardTokenAddress;
            path[1] = uniswapV2Router.WETH();
            path[2] = _withdrawToken;
            rewardToken.approve(address(uniswapV2Router), _refundValue);
            uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                _refundValue,
                0, // accept any amount of ETH
                path,
                msg.sender,
                block.timestamp
            );
        }

        require(success1 , "Transfer failed");
    }

    function emergencyWithdraw (uint256 _pid) public {
        require (_pid < poolLength(), "Invalid pool ID");
        require (userInfo[_pid][msg.sender].amount > 0 , "You don't have any staked tokens");
        require (getUserLockTime(_pid, msg.sender) > block.timestamp , "Your maturity time is reached. You can unstake tokens and enjoy rewards");

        uint256 _emergencyFees = poolInfo[_pid].emergencyFees;

        uint256 _refundValue = (userInfo[_pid][msg.sender].amount)
            .sub((_emergencyFees)
            .mul(userInfo[_pid][msg.sender].amount)
            .div(1000));
        poolInfo[_pid].currentPoolSize = (poolInfo[_pid].currentPoolSize).sub(userInfo[_pid][msg.sender].amount);
        userInfo[_pid][msg.sender].amount = 0;

        address _tokenAddress = poolInfo[_pid].tokenAddress;
        IBEP20 token = IBEP20 (_tokenAddress);
        bool success = token.transfer(msg.sender, _refundValue);
        require (success, "Transfer failed");
    }

    // this function is to withdraw BNB sent to this address by mistake
    function withdrawEth () external onlyOwner returns (bool) {
        uint256 balance = address(this).balance;
        (bool success, ) = payable(msg.sender).call{
            value: balance
        }("");
        return success;
    }

    // this function is to withdraw BEP20 tokens sent to this address by mistake
    function withdrawBEP20 (address _tokenAddress) external onlyOwner returns (bool) {
        IBEP20 token = IBEP20 (_tokenAddress);
        uint256 balance = token.balanceOf(address(this));
        bool success = token.transfer(msg.sender, balance);
        return success;
    }

    // empty fallback to receive eth. This is required to enable swaping of tokens
    receive() external payable {
    }
}