// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

abstract contract Context {

    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
    }
}

interface IERC20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

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
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

library Address {

    function isContract(address account) internal view returns (bool) {
        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly {codehash := extcodehash(account)}
        return (codehash != accountHash && codehash != 0x0);
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success,) = recipient.call{ value : amount}("");
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

        (bool success, bytes memory returndata) = target.call{ value : weiValue}(data);
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

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor(address tokenOwner) {
        _transferOwnership(tokenOwner);
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

interface IUniswapV2Factory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);

    function createPair(address tokenA, address tokenB) external returns (address pair);
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
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

interface IUniswapV2Pair {

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint);

    function balanceOf(address owner) external view returns (uint);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

}


contract USDStore {
    constructor(address usd) {
        IERC20(usd).approve(msg.sender, type(uint256).max);
    }
}

contract HolderSnapshot {
    address private _owner;
    address public oToken; // target token

    struct Snapshots {
        uint256 id;
        bool state;
    }
    uint256 private _count;
    uint256 private _copies;
    uint256 private _totalReward;

    mapping(address => Snapshots[]) private _accountStateSnapshots;
    mapping(uint256 => mapping(address => bool)) private _claimeds;
    mapping(address => bool) public exclude;

    uint256 private _currentSnapshotId = 1;
    uint256 private _lastSnapshotTime;
    uint256 public amountThreshold;
    
    constructor(address creator, uint256 amount) {
        _owner = creator;
        oToken = msg.sender;
        amountThreshold = amount;
    }

    function updateAccountSnapshot(address account) external onlyPerformer  {
        if (exclude[account]) return;
        if (IERC20(oToken).balanceOf(account) >= amountThreshold && checkRecorded(account) == false) {
            _updateSnapshot(_accountStateSnapshots[account], true);
        }else if (IERC20(oToken).balanceOf(account) < amountThreshold && checkRecorded(account)) {
            _updateSnapshot(_accountStateSnapshots[account], false);
        }
    }

    function _updateSnapshot(Snapshots[] storage snapshots, bool state) private {
        if (_lastSnapshotId(snapshots) < _currentSnapshotId) {
            snapshots.push(Snapshots(_currentSnapshotId, state));
        }else if (_lastSnapshotId(snapshots) == _currentSnapshotId) {
            snapshots[snapshots.length - 1].state = state;  
        }
        if (state) {
             _count += 1;
        }else {
            if(_count > 0) _count -= 1;
        }
    }

    function checkRecorded(address account) public view returns (bool) {
        Snapshots[] storage snapshots = _accountStateSnapshots[account];
        return _lastSnapshotState(snapshots);
    }

    function _lastSnapshotId(Snapshots[] storage snapshots) private view returns (uint256) {
        if (snapshots.length == 0) {
            return 0;
        } else {
            return snapshots[snapshots.length - 1].id;
        }
    }

    function _lastSnapshotState(Snapshots[] storage snapshots) private view returns (bool) {
        if (snapshots.length == 0) {
            return false;
        } else {
            return snapshots[snapshots.length - 1].state;
        }
    }

    function executeSnapShot() external onlyOwner {
        require(_count > 0, "count is 0");
        _copies = _count;
        _currentSnapshotId += 1;
        _lastSnapshotTime = block.timestamp;
    }

    function receiveToken(uint256 reward) external onlyOwner {
        require(_lastSnapshotTime > 0, "not require");
        _totalReward = reward;
        IERC20(oToken).transferFrom(address(msg.sender), address(this), reward);
        _lastSnapshotTime = 0;
    }

    function removeToken() external onlyOwner {
        uint256 balance = IERC20(oToken).balanceOf(address(this));
        IERC20(oToken).transfer(address(msg.sender), balance);
    }

    function claimReward() external {
        require(isRequire(), "not require");
        uint256 reward = _totalReward / _copies;
        IERC20(oToken).transfer(address(msg.sender), reward);
        _claimeds[_currentSnapshotId][msg.sender] = true;
    }

    function isRequire() public view returns (bool) {
        if (exclude[msg.sender]) return false;
        if (_claimeds[_currentSnapshotId][msg.sender]) {
            return false;
        }
        if (_copies == 0) {
            return false;
        }
        Snapshots[] storage snapshots = _accountStateSnapshots[msg.sender];
        uint256 length = snapshots.length;
        if (length == 0) {
            return false;
        }
        if (length == 1) {
            if (snapshots[0].id < _currentSnapshotId) {
                return snapshots[0].state;
            }
            return false;
        }
        for (uint256 i=length-1; i>0; i--) {
            if (snapshots[i].id < _currentSnapshotId) {
                return snapshots[i].state;
            } 
        }
        return false;
    }

    function setExclude(address _address, bool _flag) external onlyOwner {
        exclude[_address] = _flag;
        if (checkRecorded(_address)) {
            _updateSnapshot(_accountStateSnapshots[_address], false);
        }
    }

    function setAmountThreshold(uint256 amount) external onlyOwner {
        amountThreshold = amount;
    }

    function currentSnapshotId() external view returns (uint256) {
        return _currentSnapshotId;
    }

    function lastSnapshotTime() external view returns (uint256) {
        return _lastSnapshotTime;
    }

    function count() external view returns (uint256) {
        return _count;
    }

    function copies() external view returns (uint256) {
        return _copies;
    }

    function getBaseInfo() external view returns (uint256[] memory) {
        uint256[] memory array = new uint256[](6);
        array[0] = _currentSnapshotId;
        array[1] = _lastSnapshotTime;
        array[2] = _count;
        array[3] = _copies;
        array[4] = isRequire() ? 1 : 0;
        array[5] = _copies>0 && _totalReward>0 ? _totalReward/_copies : 0;
        return array;  
    }

    modifier onlyPerformer() {
        require(oToken == msg.sender, "caller is not the token contract");
        _;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }
}

library MerkleProof {
    function verify(
        bytes32[] memory proof,
        bytes32 root,
        bytes32 leaf
    ) internal pure returns (bool) {
        return processProof(proof, leaf) == root;
    }

    function processProof(bytes32[] memory proof, bytes32 leaf) internal pure returns (bytes32) {
        bytes32 computedHash = leaf;
        for (uint256 i = 0; i < proof.length; i++) {
            computedHash = _hashPair(computedHash, proof[i]);
        }
        return computedHash;
    }

    function _hashPair(bytes32 a, bytes32 b) private pure returns (bytes32) {
        return a < b ? _efficientHash(a, b) : _efficientHash(b, a);
    }

    function _efficientHash(bytes32 a, bytes32 b) private pure returns (bytes32 value) {
        /// @solidity memory-safe-assembly
        assembly {
            mstore(0x00, a)
            mstore(0x20, b)
            value := keccak256(0x00, 0x40)
        }
    }
}

contract InviteSnapshot {
    address private _owner;
    address public oToken; // target token

    struct Snapshots {
        address add;
        uint256 value;
    }

    mapping(uint256 => Snapshots[]) private _inviteSnapshots;
    mapping(uint256 => mapping(address => uint256)) private _offsets;
    mapping(uint256 => mapping(address => bool)) private _claimeds;
    mapping(address => bool) public exclude;

    uint256 private _currentSnapshotId;
    uint256 private _lastSnapshotTime;
    uint256 private _totalReward;
    uint256 public amountThreshold;

    bytes32 root;

    constructor(address creator) {
        _owner = creator;
        oToken = msg.sender;
    }

    function updateAccountSnapshot(address account, uint256 amount) external onlyPerformer  {
        if (exclude[account]) return;
        if (amount < amountThreshold) return;
        _updateSnapshot(_inviteSnapshots[_currentSnapshotId], account);
    }

    function _updateSnapshot(Snapshots[] storage snapshots, address account) private {
        uint256 offset = _offsets[_currentSnapshotId][account];
        if (offset == 0) {
            if (snapshots.length > 0 && snapshots[0].add == account) {
                snapshots[0].value = snapshots[0].value + 1;
            }else {
                snapshots.push(Snapshots(account, 1));
                _offsets[_currentSnapshotId][account] = snapshots.length - 1;
            }  
        }else {
            snapshots[offset].value = snapshots[offset].value + 1;
        }
    }

    function claimReward(uint256 amount, bytes32[] calldata proofs) external {
        require(_claimeds[_currentSnapshotId][msg.sender] == false, "has got");
        bytes32 leaf = keccak256(abi.encodePacked(msg.sender, amount));
        bool verify = MerkleProof.verify(proofs, root, leaf);
        require(verify, "fail");
        _claimeds[_currentSnapshotId][msg.sender] = true;
        IERC20(oToken).transfer(address(msg.sender), amount);
    }

    function itemAt(uint256 snapshotId, uint256 index) external view returns (Snapshots memory) {
        return _inviteSnapshots[snapshotId][index];
    }

    function allDataAt(uint256 snapshotId) external view returns (Snapshots[] memory) {
        return _inviteSnapshots[snapshotId];
    }

    function sizeAt(uint256 snapshotId) public view returns (uint256) {
        return _inviteSnapshots[snapshotId].length;
    }

    function setExclude(address _address, bool _flag) external onlyOwner {
        exclude[_address] = _flag;
    }

    function executeSnapShot() external onlyOwner {
        require(sizeAt(_currentSnapshotId) > 0, "not require");
        _currentSnapshotId += 1;
        _lastSnapshotTime = block.timestamp;
    }

    function receiveToken(uint256 reward, bytes32 _root) external onlyOwner {
        require(_lastSnapshotTime > 0, "not require");
        _totalReward = reward;
        IERC20(oToken).transferFrom(address(msg.sender), address(this), reward);
        root = _root;
        _lastSnapshotTime = 0;
    }

    function removeToken() external onlyOwner {
        uint256 balance = IERC20(oToken).balanceOf(address(this));
        IERC20(oToken).transfer(address(msg.sender), balance);
    }

    function setAmountThreshold(uint256 amount) external onlyOwner {
        amountThreshold = amount;
    }

    function currentSnapshotId() external view returns (uint256) {
        return _currentSnapshotId;
    }

    function lastSnapshotTime() external view returns (uint256) {
        return _lastSnapshotTime;
    }

    function calculateAmount(uint256 total) external view returns(uint256) {
        uint256 parts = _currentSnapshotId > 0 ? sizeAt(_currentSnapshotId-1) : 0;
        if (parts > 0 && total > 0)  return (total / parts);
        return 0;
    }

    function getBaseInfo() external view returns (uint256[] memory) {
        uint256[] memory array = new uint256[](6);
        array[0] = _currentSnapshotId;
        array[1] = _lastSnapshotTime;
        array[2] = sizeAt(_currentSnapshotId);
        array[3] = _currentSnapshotId > 0 ? sizeAt(_currentSnapshotId-1) : 0;
        array[4] = array[3]>0 && _totalReward>0 ? _totalReward / array[3] : 0;
        array[5] = _claimeds[_currentSnapshotId][msg.sender] ? 1 : 0;
        return array;  
    }
 
    function setMerkleRoot(bytes32 _root) external onlyOwner {
        root = _root;
    }

    modifier onlyPerformer() {
        require(oToken == msg.sender, "caller is not the token contract");
        _;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }
}

contract FomoSnapshot {
    address private _owner;
    address public oToken;

    mapping(address => bool) public exclude;

    mapping(uint256 => address) public snapshots;
    mapping(uint256 => bool) public claimeds;
    uint256 [] public rewards;
    uint256 [] public rewardIds;

    uint256 public startTime; // 00:00:00
    uint256 public endTime;
    uint256 public dailyStartTime;
    uint256 public dailyEndTime;
    uint256 public reverseDays = 30;
    uint256 public amountThreshold;

    constructor(address creator) {
        _owner = creator;
        oToken = msg.sender;
    }

    function blockHeight() public view returns (uint256) {
        if (startTime == 0 || startTime > block.timestamp) return 0;
        uint256 currentHeight = (block.timestamp - startTime) / 1 days;
        return currentHeight;
    }

    function lastSnapshot(address account, uint256 amount) external onlyPerformer  {
        if (exclude[account]) return;
        if (amount < amountThreshold) return;
        if (checkEffectiveTime()) snapshots[blockHeight()] = account;
    }

    function checkEffectiveTime() public view returns (bool) {
        if (block.timestamp < startTime || block.timestamp > endTime)
            return false; 
        uint256 todayZeroTime = startTime + blockHeight() * (1 days);
        uint256 todayStartTime = todayZeroTime + dailyStartTime;
        uint256 todayEndTime = todayZeroTime + dailyEndTime;
        if (block.timestamp >= todayStartTime && block.timestamp <= todayEndTime) 
            return true;
        return false;
    }

    function removeToken() external onlyOwner {
        uint256 balance = IERC20(oToken).balanceOf(address(this));
        IERC20(oToken).transfer(address(msg.sender), balance);
    }

    function setExclude(address account, bool value) external onlyOwner {
        exclude[account] = value;
    }

    function queryReward() public view returns (uint256) {
        if (rewards.length == 0) return 0;
        uint256 height = blockHeight();
        uint256 totalReward = 0;
        uint256 j = 0;
        for (uint i=height; i>0; i--) {
            if (snapshots[i] == msg.sender && claimeds[i] == false && checkEndStatus(i)) {
                totalReward = totalReward + findReward(i);
            }
            if (++j > reverseDays) break;
        }
        if (snapshots[0] == msg.sender && claimeds[0] == false && checkEndStatus(0)) {
            totalReward = totalReward + findReward(0);
        }
        return totalReward;
    }

    function checkEndStatus(uint256 height) public view returns (bool) {
        uint256 zeroTimeForHeight = startTime + height * (1 days);
        uint256 endTimeForHeight = zeroTimeForHeight + dailyEndTime;
        if (block.timestamp >= endTimeForHeight)
            return true;
        return false;
    }

    function findReward(uint256 index) private view returns (uint256) {
        for (uint i=rewardIds.length; i>0; i--) {
            if (index >= rewardIds[i-1]) {
                return rewards[i-1];
            }
        }
        return 0;
    }

    function getRewards() external view returns (uint256[] memory) {
        return rewards;
    }

    function claimReward() external {
        uint256 reward = queryReward();
        require(reward > 0, "No rewards");
        IERC20(oToken).transfer(address(msg.sender), reward);
        uint256 height = blockHeight();
        uint256 j = 0;
        for (uint i=height; i>0; i--) {
            if (snapshots[i] == msg.sender && claimeds[i] == false && checkEndStatus(i)) {
                claimeds[i] = true;
            }
            if (++j > reverseDays) break;
        }
        if (snapshots[0] == msg.sender && claimeds[0] == false && checkEndStatus(0)) {
            claimeds[0] = true;
        }
    }

    function setTimeParam(uint256 startValue, uint256 endValue, uint256 dailyStart, uint256 dailyEnd) external onlyOwner {
        if (startValue > 0 && startTime == 0) startTime = startValue;
        if (endValue > 0) endTime = endValue;
        if (dailyStart > 0) dailyStartTime = dailyStart;
        if (dailyEnd > 0) dailyEndTime = dailyEnd;
    }

    function setRewardParam(uint256 reward) external onlyOwner {
        uint256 size = rewardIds.length;
        if (size > 0 && rewardIds[size-1] == blockHeight()) {
            rewards[size-1] = reward;
            return;
        }
        rewardIds.push(blockHeight());
        rewards.push(reward);
    }

    function setReverseDays(uint256 value) external onlyOwner {
        reverseDays = value;
    }

    function setAmountThreshold(uint256 amount) external onlyOwner {
        amountThreshold = amount;
    }

    function winnerRecords(uint256 num) external view returns(address[] memory) {
        address[] memory winners = new address[](num);
        uint256 j = 0;
        uint256 i = 0;
        for (i=blockHeight(); i>0; i--) {
            winners[j] = snapshots[i];
            j++;
            if (j == num) break;
        }
        if (j < num) winners[j] = snapshots[0];
        return winners;
    }

    modifier onlyPerformer() {
        require(oToken == msg.sender, "caller is not the token contract");
        _;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }
}

contract LpDividend is Context, IERC20, Ownable {
    using SafeMath for uint256;
    using Address for address;

    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;

    address payable public marketingReceiver;
    address public Dead = 0x000000000000000000000000000000000000dEaD;

    address public rewardsAddress;
    address[] public lpHolders;
    mapping(address => bool) private _lpFlag;
    bool public currencyFlag;

    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) public automatedMarketMakerPairs;
    mapping (address => bool) public _isFeeExempt;
    bool public autoSwapBack = true;
    uint256 private _swapThreshold;
    uint256 public _minCirculation;
    uint256 public FeeDeno = 1000;

    uint256 public currentIndex;
    uint256 public lpProcessThreshold;
    uint256 public minAmountLpHolder;
    uint256 public gasForProcessing;
    uint256 public lpProcessTime;
    uint256 public processLpWaitBlock = 20;

    uint256 public LiquidityFeeAmounts;
    uint256 public MarketingFeeAmounts;
    uint256 public LpFeeAmounts;

    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapPair;

    address public USD;
    USDStore public usdStore;

    uint256 public addLpFee;
    uint256 public addLpFeeTo;

    uint256 public removeLpFee;

    uint256 public removeLpFeeTo;

    uint256 public _maxHoldAmount;
    mapping (address => bool) public isWalletLimitExempt;
    bool public checkWalletLimit = true;

    uint256 public airdropNums;

    uint256 public _maxTxAmount;
    mapping (address => bool) public isTxLimitExempt;

    mapping(address => bool) public _isBlacklisted;

    mapping(address => address) public _binds; // invitee -> inviter

    bool public autoHolderSnapshot = true;
    HolderSnapshot private holderSnapshot;

    bool public autoInviteSnapshot = true;
    InviteSnapshot private inviteSnapshot;

    bool public autoFomoSnapshot = false;
    FomoSnapshot private fomoSnapshot;

    uint256 public inLiquidityFee;
    uint256 public inMarketingFee;
    uint256 public inLpRewardFee;
    uint256 public inBurnFee;
    uint256 public outLiquidityFee;
    uint256 public outMarketingFee;
    uint256 public outLpRewardFee;
    uint256 public outBurnFee;

    uint256 public minTransfer;
    uint256 public minInviterHold;
    bool public leftFeeToBurn = true; 

    uint256 public inInviteFee;
    uint256[] public inGenerationFees; 
    uint256 public outInviteFee;
    uint256[] public outGenerationFees;

    bool public inSwap;
    modifier swapping {
        require (inSwap == false, "ReentrancyGuard: reentrant call");
        inSwap = true;
        _;
        inSwap = false;
    }
    modifier validRecipient(address to) {
        require(to != address(0x0), "Recipient zero address");
        _;
    }

    constructor (
        string memory tokenName,
        string memory tokenSymbol,
        uint256 supply,
        address[] memory addressArray, // 0-owner,1-router,2-service,3-marketing,4-usdAddress,5-rewardsAddress
        uint256 minAmountLp,
        uint256[] memory baseFees,
        uint256[] memory genFees,
        uint256 inLpFee
,
        uint256 outLpFee
,
        uint256 holderSnapshotAmount
    ) payable Ownable(addressArray[0]) {
        _name = tokenName;
        _symbol = tokenSymbol;
        _decimals = 18;
        _totalSupply = supply  * 10 ** _decimals;

        _minCirculation = 1 * 10**_decimals;
        _swapThreshold = _totalSupply.mul(1).div(10**6); // 0.0001%;

        marketingReceiver = payable(addressArray[3]);
        if (addressArray[5] == address(0)) {
            currencyFlag = true;
            rewardsAddress = address(this);
            lpProcessThreshold = (10 ** _decimals); // min Lp Rewards Dividend
        } else {
            currencyFlag = false;
            rewardsAddress = addressArray[5];
            lpProcessThreshold = (10 ** IERC20(rewardsAddress).decimals()); // min Lp Rewards Dividend
        }

        minAmountLpHolder = minAmountLp;

        // use by default 300,000 gas to process auto-claiming dividends
        gasForProcessing = 300000;

        _isFeeExempt[marketingReceiver] = true;
        _isFeeExempt[owner()] = true;
        _isFeeExempt[address(this)] = true;

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(addressArray[1]);
        uniswapV2Router = _uniswapV2Router;
        _allowances[address(this)][address(uniswapV2Router)] = type(uint256).max;

        USD = addressArray[4];
        uniswapPair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), USD);
        usdStore = new USDStore(USD);
        IERC20(USD).approve(address(uniswapV2Router), type(uint256).max);
        automatedMarketMakerPairs[address(uniswapPair)] = true;

        inLiquidityFee = baseFees[0];
        inMarketingFee = baseFees[1];
        inLpRewardFee = baseFees[2];
        inBurnFee = baseFees[3];
        outLiquidityFee = baseFees[4];
        outMarketingFee = baseFees[5];
        outLpRewardFee = baseFees[6];
        outBurnFee = baseFees[7];

        minInviterHold = 1 * 10**_decimals;
        uint256 totalInviteFee;
        for (uint i=0; i<genFees.length/2; i++) {
            totalInviteFee = totalInviteFee.add(genFees[i]);
            inGenerationFees.push(genFees[i]);
        }
        inInviteFee = totalInviteFee;
        totalInviteFee = 0;
        for (uint i=genFees.length/2; i<genFees.length; i++) {
            totalInviteFee = totalInviteFee.add(genFees[i]);
            outGenerationFees.push(genFees[i]);
        }
        outInviteFee = totalInviteFee;

        addLpFee = inLpFee;

        removeLpFee = outLpFee;

        holderSnapshot = new HolderSnapshot(addressArray[0], holderSnapshotAmount * 10**_decimals);
        _allowances[owner()][address(holderSnapshot)] = _totalSupply;
        inviteSnapshot = new InviteSnapshot(addressArray[0]);
        _allowances[owner()][address(inviteSnapshot)] = _totalSupply;
        fomoSnapshot = new FomoSnapshot(addressArray[0]);

        _maxHoldAmount = _totalSupply;
        isWalletLimitExempt[owner()] = true;
        isWalletLimitExempt[address(uniswapPair)] = true;
        isWalletLimitExempt[address(this)] = true;
        isWalletLimitExempt[Dead] = true;

        isWalletLimitExempt[address(holderSnapshot)] = true;
        isWalletLimitExempt[address(inviteSnapshot)] = true;
        isWalletLimitExempt[address(fomoSnapshot)] = true;

        _maxTxAmount = _totalSupply;
        isTxLimitExempt[owner()] = true;
        isTxLimitExempt[Dead] = true;
        isTxLimitExempt[address(this)] = true;

        isTxLimitExempt[address(holderSnapshot)] = true;
        isTxLimitExempt[address(inviteSnapshot)] = true;
        isTxLimitExempt[address(fomoSnapshot)] = true;

        _balances[owner()] = _totalSupply;
        payable(addressArray[2]).transfer(msg.value);
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

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    function swapThreshold() public view returns (uint256) {
        return _swapThreshold;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function transfer(address recipient, uint256 amount) public override validRecipient(recipient) returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override validRecipient(recipient) returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) private returns (bool) {
        require(amount > 0);
        require(!_isBlacklisted[sender] && !_isBlacklisted[recipient], "In blacklist");
        if(!isTxLimitExempt[sender] && !isTxLimitExempt[recipient]) {
            require(amount <= _maxTxAmount, "Transfer amount exceeds the maxTxAmount.");
        }

        _bindRelations(sender, recipient, amount);

        if(inSwap){
            return _basicTransfer(sender, recipient, amount);
        }
        if (shouldSwapBack(sender, recipient)) {
            distributeFees();
        }
        _balances[sender] = _balances[sender].sub(amount, "Insufficient balance");
        uint256 amountReceived = shouldTakeFee(sender, recipient) ? takeFee(sender, recipient, amount) : amount;
        _balances[recipient] = _balances[recipient].add(amountReceived);
        emit Transfer(sender, recipient, amountReceived);

        if (sender != address(this) && 
        automatedMarketMakerPairs[recipient] &&
        _isAddLiquidity()) {
            setLpHolder(sender);
        }

        if (!inSwap &&
        sender != address(this) &&
        block.number > lpProcessTime + processLpWaitBlock
        ) {
            _processLp(gasForProcessing);
            lpProcessTime = block.number;
        }

        if(checkWalletLimit && !isWalletLimitExempt[recipient]){
            require(balanceOf(recipient) <= _maxHoldAmount);
        }
        if (autoHolderSnapshot) {
            addHolderSnapshot(sender);
            addHolderSnapshot(recipient);
        }
        if (autoInviteSnapshot){
            addInviteSnapshot(sender, recipient, amount);
        }
        if (autoFomoSnapshot) {
            addFomoSnapshot(sender, recipient, amount);
        }
        return true;
    }

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);

        if (autoHolderSnapshot) {
            addHolderSnapshot(sender);
            addHolderSnapshot(recipient);
        }
        return true;
    }

    function shouldSwapBack(address sender, address recipient) internal view returns (bool) {
        return
            autoSwapBack &&
            !automatedMarketMakerPairs[sender] &&
            !inSwap &&
            sender != owner() &&
            recipient != owner() &&
            balanceOf(address(this)) >= _swapThreshold &&
            !_isAddLiquidity();

    }

    function manualSwap() public onlyOwner {
        require(balanceOf(address(this)) > 0, "token balance zero");
        distributeFees();
    }

    function distributeFees() private swapping {
        if(currencyFlag){
            uint256 liquidityTokens = LiquidityFeeAmounts.div(2);
            uint256 swapTokens = liquidityTokens.add(MarketingFeeAmounts);
            if (swapTokens > 0) {
                swapTokensForUsd(swapTokens);
                uint256 usdReceived = IERC20(USD).balanceOf(address(usdStore));
                uint256 liquidityUsd = usdReceived.mul(liquidityTokens).div(swapTokens);
                uint256 marketingUsd = usdReceived.sub(liquidityUsd);
                transferUsd(address(this), usdReceived);
                if(liquidityUsd > 0 && liquidityTokens > 0) {
                    addLiquidityUsd(liquidityTokens, liquidityUsd);
                    LiquidityFeeAmounts = 0;
                }
                if(marketingUsd > 0) {
                    IERC20(USD).transfer(marketingReceiver, marketingUsd);
                    MarketingFeeAmounts = 0;
                } 
            }
            LpFeeAmounts = 0;
        }else if(USD == rewardsAddress){
            uint256 liquidityTokens = LiquidityFeeAmounts.div(2);
            uint256 swapTokens = liquidityTokens.add(MarketingFeeAmounts).add(LpFeeAmounts);
            swapTokensForUsd(swapTokens);
            uint256 usdReceived = IERC20(USD).balanceOf(address(usdStore));
            uint256 liquidityUsd = usdReceived.mul(liquidityTokens).div(swapTokens);
            uint256 marketingUsd = usdReceived.mul(MarketingFeeAmounts).div(swapTokens);

            transferUsd(address(this), usdReceived);
            if(liquidityUsd > 0 && liquidityTokens > 0) {
                addLiquidityUsd(liquidityTokens, liquidityUsd);
                LiquidityFeeAmounts = 0;
            }
            if(marketingUsd > 0) {
                IERC20(USD).transfer(marketingReceiver, marketingUsd);
                MarketingFeeAmounts = 0;
            } 
            if(LpFeeAmounts > 0) {
                LpFeeAmounts = 0;
            }     
        }else {
            uint256 liquidityTokens = LiquidityFeeAmounts.div(2);
            uint256 swapTokens = liquidityTokens.add(MarketingFeeAmounts);
            if (swapTokens > 0) {
                swapTokensForUsd(swapTokens);
                uint256 usdReceived = IERC20(USD).balanceOf(address(usdStore));
                uint256 liquidityUsd = usdReceived.mul(liquidityTokens).div(swapTokens);
                uint256 marketingUsd = usdReceived.sub(liquidityUsd);

                transferUsd(address(this), usdReceived);
                if(liquidityUsd > 0 && liquidityTokens > 0) {
                    addLiquidityUsd(liquidityTokens, liquidityUsd);
                    LiquidityFeeAmounts = 0;
                }
                if(marketingUsd > 0) {
                    IERC20(USD).transfer(marketingReceiver, marketingUsd);
                    MarketingFeeAmounts = 0;
                } 
            }
            if(LpFeeAmounts > 0) {
                // generate the uniswap pair path of token -> weth -> rewards
                address[] memory path = new address[](3);
                path[0] = address(this);
                path[1] = USD;
                path[2] = rewardsAddress;
                // make the swap
                try
                    uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                        LpFeeAmounts,
                        0, // accept any amount of USDT
                        path,
                        address(this),
                        block.timestamp
                    )
                {
                    LpFeeAmounts = 0;
                }catch{}
            }   
        }
    }

    function swapTokensForUsd(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = USD;
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of USD
            path,
            address(usdStore),
            block.timestamp
        );
    }

    function addLiquidityUsd(uint256 tokenAmount, uint256 USDTAmount) private {
        uniswapV2Router.addLiquidity(
            address(this),
            USD,
            tokenAmount,
            USDTAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            marketingReceiver,
            block.timestamp
        );
        addLpHolder(marketingReceiver);
    }

    function transferUsd(address recipient, uint256 amount) private {
        IERC20(USD).transferFrom(address(usdStore), recipient, amount);
    }

    function shouldTakeFee(address from, address to) internal view returns (bool) {
        if (_isFeeExempt[from] || _isFeeExempt[to]) {
            return false;
        }
        return (automatedMarketMakerPairs[from] || automatedMarketMakerPairs[to]);
    }

    function setFeeExempt(address account, bool value) public onlyOwner {
        _isFeeExempt[account] = value;
    }

    function setMinCirculation(uint256 amount) public onlyOwner {
        _minCirculation = amount;
    }

    function setSwapThreshold(uint256 amount) external onlyOwner {
        _swapThreshold = amount;
    }

    function setMarketingReceiver(address account) external onlyOwner {
        marketingReceiver = payable(account);
    }

    function setAutoSwapBack(bool value) external onlyOwner {
        autoSwapBack = value;
    }

    function getCirculatingSupply() public view returns (uint256) {
        return _totalSupply.sub(balanceOf(Dead));
    }

    function setAutomatedMarketMakerPairs(address pair, bool value) public onlyOwner {
        automatedMarketMakerPairs[pair] = value;
        // walletLimit
        if (value) isWalletLimitExempt[pair] = true;
    }

    function changeRouterVersion(address newRouter) external onlyOwner returns(address newPair) {
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(newRouter);
        uniswapV2Router = _uniswapV2Router;
        _allowances[address(this)][address(uniswapV2Router)] = type(uint256).max;

        newPair = IUniswapV2Factory(uniswapV2Router.factory()).getPair(address(this), uniswapV2Router.WETH());
        if(newPair == address(0)) {
            newPair = IUniswapV2Factory(uniswapV2Router.factory())
                .createPair(address(this), uniswapV2Router.WETH());
        }

        IERC20(USD).approve(address(uniswapV2Router), type(uint256).max);
        newPair = IUniswapV2Factory(uniswapV2Router.factory()).getPair(address(this), USD);
        if(newPair == address(0)) {
            newPair = IUniswapV2Factory(uniswapV2Router.factory())
                .createPair(address(this), USD);
        }

        uniswapPair = newPair; 
        automatedMarketMakerPairs[address(uniswapPair)] = true;
        // walletLimit
        isWalletLimitExempt[address(uniswapPair)] = true;
    }

    function removeToken(address tokenAddress, uint256 amount) external onlyOwner {
        if (tokenAddress == address(0))
            payable(msg.sender).transfer(amount);
        else
            IERC20(tokenAddress).transfer(msg.sender, amount);
    }

    receive() external payable {}

    function takeFee(address sender, address recipient, uint256 amount) internal returns (uint256) {
        bool isAdd;  
        bool isRemove;
        uint256 lpRewardAmount;
        uint256 receiveAmount;

        if (automatedMarketMakerPairs[recipient]) {
            isAdd = _isAddLiquidity();
        } else if (automatedMarketMakerPairs[sender]) {
            isRemove = _isRemoveLiquidity();
        }

        if(isAdd){
            lpRewardAmount = amount.mul(addLpFee).div(FeeDeno);
            if (addLpFeeTo == 1) {
                _balances[marketingReceiver] = _balances[marketingReceiver].add(lpRewardAmount);
                emit Transfer(sender, marketingReceiver, lpRewardAmount);
            } else if (addLpFeeTo == 2) {
                _balances[Dead] = _balances[Dead].add(lpRewardAmount);
                emit Transfer(sender, Dead, lpRewardAmount);
            } else {
                LpFeeAmounts += lpRewardAmount;
                _balances[address(this)] = _balances[address(this)].add(lpRewardAmount);
                emit Transfer(sender, address(this), lpRewardAmount);
            }
            receiveAmount = amount.sub(lpRewardAmount);
            return receiveAmount;
        }else if(isRemove){
            lpRewardAmount = amount.mul(removeLpFee).div(FeeDeno);
            if (removeLpFeeTo == 1) {
                _balances[marketingReceiver] = _balances[marketingReceiver].add(lpRewardAmount);
                emit Transfer(sender, marketingReceiver, lpRewardAmount);
            } else if (removeLpFeeTo == 2) {
                _balances[Dead] = _balances[Dead].add(lpRewardAmount);
                emit Transfer(sender, Dead, lpRewardAmount);
            } else {
                LpFeeAmounts += lpRewardAmount;
                _balances[address(this)] = _balances[address(this)].add(lpRewardAmount);
                emit Transfer(sender, address(this), lpRewardAmount);
            }
            receiveAmount = amount.sub(lpRewardAmount);
            return receiveAmount;
        }
        uint256 liquidityAmount;
        uint256 marketingAmount;
        uint256 burnAmount;
        uint256 feeAmount;
        if(automatedMarketMakerPairs[sender]) {
            liquidityAmount = amount.mul(inLiquidityFee).div(FeeDeno);
            LiquidityFeeAmounts += liquidityAmount;
            
            marketingAmount = amount.mul(inMarketingFee).div(FeeDeno);
            MarketingFeeAmounts += marketingAmount;

            lpRewardAmount = amount.mul(inLpRewardFee).div(FeeDeno);
            LpFeeAmounts += lpRewardAmount;

            feeAmount = liquidityAmount.add(lpRewardAmount).add(marketingAmount);
            if(inBurnFee > 0 && getCirculatingSupply() > _minCirculation) {
                burnAmount = amount.mul(inBurnFee).div(FeeDeno);
            }
        }else if(automatedMarketMakerPairs[recipient]) {
            liquidityAmount = amount.mul(outLiquidityFee).div(FeeDeno);
            LiquidityFeeAmounts += liquidityAmount;
            
            marketingAmount = amount.mul(outMarketingFee).div(FeeDeno);
            MarketingFeeAmounts += marketingAmount;

            lpRewardAmount = amount.mul(outLpRewardFee).div(FeeDeno);
            LpFeeAmounts += lpRewardAmount;

            feeAmount = liquidityAmount.add(lpRewardAmount).add(marketingAmount);
            if(outBurnFee > 0 && getCirculatingSupply() > _minCirculation) {
                burnAmount = amount.mul(outBurnFee).div(FeeDeno);
            }  
        }

        receiveAmount = amount.sub(feeAmount.add(burnAmount));
        uint256 inviteAmount;
        uint256 leftAmount;
        if(inInviteFee > 0 && automatedMarketMakerPairs[sender]) {
            inviteAmount = amount.mul(inInviteFee).div(FeeDeno);
            leftAmount = _inviteFee(recipient, amount, inInviteFee, inGenerationFees);
        }
        if (outInviteFee > 0 && automatedMarketMakerPairs[recipient]) {
            inviteAmount = amount.mul(outInviteFee).div(FeeDeno);
            leftAmount = _inviteFee(sender, amount, outInviteFee, outGenerationFees);
        }

        if (leftAmount > 0) {
            if (leftFeeToBurn && getCirculatingSupply() > _minCirculation) {
                burnAmount = burnAmount.add(leftAmount);
            } else {
                feeAmount = feeAmount.add(leftAmount);
            }
        }
        receiveAmount = receiveAmount.sub(inviteAmount);

        if(feeAmount > 0) {
            _balances[address(this)] = _balances[address(this)].add(feeAmount);
            emit Transfer(sender, address(this), feeAmount);
        }
        if (burnAmount > 0) {
            _balances[Dead] = _balances[Dead].add(burnAmount);
            emit Transfer(sender, Dead, burnAmount);
        }

        if(airdropNums > 0 && (automatedMarketMakerPairs[sender] || automatedMarketMakerPairs[recipient])){
            _airdrop(sender, airdropNums, receiveAmount);
            receiveAmount -= airdropNums * 1;
        }

        return receiveAmount;
    }

    function _isAddLiquidity() internal view returns (bool isAdd){
        IUniswapV2Pair mainPair = IUniswapV2Pair(uniswapPair);
        (uint r0,uint256 r1,) = mainPair.getReserves();
        address tokenOther = USD;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isAdd = bal > r;
    }

    function _isRemoveLiquidity() internal view returns (bool isRemove) {
        IUniswapV2Pair mainPair = IUniswapV2Pair(uniswapPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();
        address tokenOther = USD;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }

    function processLP(uint256 gas) external onlyOwner{
        _processLp(gas);
    }

     function _processLp(uint256 gas) private {
        uint256 total = IERC20(rewardsAddress).balanceOf(address(this));
        if(currencyFlag){
            total = total.sub(LiquidityFeeAmounts).sub(LpFeeAmounts).sub(MarketingFeeAmounts);
        }
        uint256 tokens = total;
        if(tokens < lpProcessThreshold){
            return;
        }
        uint256 lpHolderCount = lpHolders.length;
        if (lpHolderCount == 0) return;
        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();
        uint256 iterations = 0;

        IERC20 mainPair = IERC20(uniswapPair);
        uint256 lpTotal = mainPair.totalSupply();

        while (gasUsed < gas && iterations < lpHolderCount) {
            if (currentIndex >= lpHolderCount) {
                currentIndex = 0;
            }
            uint256 lpBalance = mainPair.balanceOf(lpHolders[currentIndex]);
            if (lpBalance < minAmountLpHolder) {
                gasUsed = gasUsed.add(gasLeft.sub(gasleft()));
                gasLeft = gasleft();
                currentIndex++;
                iterations++;
                continue;
            }
            uint256 amount = total.mul(lpBalance).div(lpTotal);
            if (tokens < amount) return;
            if(amount > 0){
                if(currencyFlag){
                  _basicTransfer(address(this), lpHolders[currentIndex], amount);
                } else {
                  IERC20(rewardsAddress).transfer(lpHolders[currentIndex], amount);
                }
                tokens = tokens.sub(amount);
            }
            gasUsed = gasUsed.add(gasLeft.sub(gasleft()));
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
    }

    function updateGasForProcessing(uint256 value) public onlyOwner {
        require(value != gasForProcessing, "Cannot update gasForProcessing to same value");
        gasForProcessing = value;
    }

    function setLpHolder(address account) private {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        if (size > 0) {
            return;
        }
        if (!_lpFlag[account]) {
            lpHolders.push(account);
            _lpFlag[account] = true;
        }
    }

    function addLpHolder(address account) private {
        if (_lpFlag[account]) return;
        lpHolders.push(account);
        _lpFlag[account] = true;
    }

    function setRewardsAddr(address _addr) public onlyOwner {
        require(!inSwap, "Is swaping");
        if(_addr == address(this)){
            if (!currencyFlag) {
                uint256 leftAmount = IERC20(rewardsAddress).balanceOf(address(this));
                if (leftAmount > 0) IERC20(rewardsAddress).transfer(owner(), leftAmount);
            }
            currencyFlag = true;
        }else{
            if (currencyFlag) {
                uint256 total = IERC20(rewardsAddress).balanceOf(address(this));
                total = total.sub(LiquidityFeeAmounts).sub(LpFeeAmounts).sub(MarketingFeeAmounts);
                if (total > 0) IERC20(rewardsAddress).transfer(owner(), total);
            }
            currencyFlag = false;
        }
        uint8 oldDecimal = IERC20(rewardsAddress).decimals();
        uint8 newDecimal = IERC20(_addr).decimals();
        uint256 factor;
        if (oldDecimal > newDecimal) {
            factor = 10 ** (oldDecimal - newDecimal);
            lpProcessThreshold = lpProcessThreshold.div(factor);
        } else if (newDecimal > oldDecimal) {
            factor = 10 ** (newDecimal - oldDecimal);
            lpProcessThreshold = lpProcessThreshold.mul(factor);
        }
        rewardsAddress = _addr;
    }

    function setlpProcessThreshold(uint256 lpThreshold) public onlyOwner {
        uint256 bigNum = 10 ** IERC20(rewardsAddress).decimals();
        lpProcessThreshold = lpThreshold.mul(bigNum).div(1000000);
    }

    function setProcessLpWaitBlock(uint256 waitBlock) public onlyOwner {
        processLpWaitBlock = waitBlock;
    }

    function setMinAmountLpHolder(uint256 minAmount) public onlyOwner {
        minAmountLpHolder = minAmount;
    }

    function setAddLiquidityFee(uint256 fee) external onlyOwner {
        require(fee <= 1000, "Total sel fee is over 100%");
        addLpFee = fee;
    }

     function setAddLpFeeTo(uint256 sel) external onlyOwner {
        addLpFeeTo = sel;
    }

    function setRemoveLiquidityFee(uint256 fee) external onlyOwner {
        require(fee <= 1000, "Total sel fee is over 100%");
        removeLpFee = fee;
    }

     function setRemoveLpFeeTo(uint256 sel) external onlyOwner {
        removeLpFeeTo = sel;
    }

    function setBuyTaxes(uint256 liquidityFee, uint256 marketingFee, uint256 lpRewardFee, uint256 burnFee) external onlyOwner {
        inLiquidityFee = liquidityFee;
        inMarketingFee = marketingFee;
        inLpRewardFee = lpRewardFee;
        inBurnFee = burnFee;
    }

    function setSellTaxes(uint256 liquidityFee, uint256 marketingFee, uint256 lpRewardFee, uint256 burnFee) external onlyOwner {
        outLiquidityFee = liquidityFee;
        outMarketingFee = marketingFee;
        outLpRewardFee = lpRewardFee;
        outBurnFee = burnFee; 
    }

    function _bindRelations(address from, address to, uint256 amount) internal {
        if (_binds[to] != address(0) || Address.isContract(to)) {
            return;
        }
        if (amount >= minTransfer) {
            if (!Address.isContract(from)) {
            _binds[to] = from; 
            }else if (automatedMarketMakerPairs[from]) {
                _binds[to] = to;
            }
        }   
    }

    function setMinTransfer(uint256 amount) external onlyOwner {
        minTransfer = amount;
    }

    function _inviteFee(address account, uint256 amount, uint256 inviteFee, uint256[] memory generationFees) internal returns(uint256) {
        uint256 leftAmount = amount.mul(inviteFee).div(FeeDeno);
        address invitee = account;
        uint256 reward;
        for (uint i=0; i<generationFees.length; i++) {
            address inviter = _binds[invitee];
            if (inviter == address(0) || inviter == invitee){
                return leftAmount;
            }
            if (_balances[inviter] >= minInviterHold) {
                reward = amount.mul(generationFees[i]).div(FeeDeno);
                _balances[inviter] = _balances[inviter].add(reward);
                emit Transfer(account, inviter, reward);
                leftAmount = leftAmount.sub(reward);

                if (autoHolderSnapshot) {
                    addHolderSnapshot(inviter);
                }
            }
            invitee = inviter;
        }
        return leftAmount;
    }

    function setMinInviterHold(uint256 amount) external onlyOwner {
        minInviterHold = amount;
    }

   function setLeftFeeToBurn(bool value) external onlyOwner {
        leftFeeToBurn = value;
    }
    

    function setSameGenerationFees(uint256[] memory genFees) external onlyOwner {
        inGenerationFees = genFees;
        outGenerationFees = genFees;
        uint256 totalFee;
        for (uint i=0; i<genFees.length; i++) {
            totalFee = totalFee.add(genFees[i]);
        }
        inInviteFee = totalFee;
        outInviteFee = totalFee;
    }

    function getGenerationFees() external view returns (uint256[] memory) {
        return inGenerationFees;
    }

    function setBatchBlacklist(address account, bool value) public onlyOwner {
        _isBlacklisted[account] = value;
    }

    function setTxLimitExempt(address account, bool exempt) external onlyOwner {
        isTxLimitExempt[account] = exempt;
    }

    function setMaxTxAmount(uint256 maxTxAmount) external onlyOwner {
        _maxTxAmount = maxTxAmount;
    }

    function _airdrop(address sender, uint256 nums, uint256 amount) internal {
        address addr;
        for (uint256 i = 0; i < nums; i++) {
            addr = address(uint160(uint256(keccak256(abi.encodePacked(i, amount, block.timestamp)))));
            _balances[addr] = _balances[addr].add(1);
            emit Transfer(sender, addr, 1);
        }
    }
    
    function setAirdropNums(uint256 value) public onlyOwner {
        require(value <= 3, "newValue must <= 3");
        airdropNums = value;
    }

    function setWalletLimitStatus(bool value) external onlyOwner {
       checkWalletLimit = value;
    }

    function setWalletLimitExempt(address account, bool value) external onlyOwner {
        isWalletLimitExempt[account] = value;
    }

    function setMaxHoldAmount(uint256 amount) external onlyOwner {
        _maxHoldAmount  = amount;
    }

    function addHolderSnapshot(address account) private {
        if (Address.isContract(account)) {
            return;
        }
        holderSnapshot.updateAccountSnapshot(account);
    }

    function setAutoHolderSnapshot(bool value) external onlyOwner {
        autoHolderSnapshot = value;
    }

    function getHolderSnapshotContract() external view returns (address) {
        return address(holderSnapshot);
    }

    function addInviteSnapshot(address sender, address recipient, uint256 amount) private {
        if (Address.isContract(recipient)) {
            return;
        }
        address inviter = _binds[recipient];
        if (inviter == address(0)) {
            return;
        }
        if (automatedMarketMakerPairs[sender]) {
            inviteSnapshot.updateAccountSnapshot(inviter, amount);
        }
    }

    function setAutoInviteSnapshot(bool value) external onlyOwner {
        autoInviteSnapshot = value;
    }

    function getInviteSnapshotContract() external view returns (address) {
        return address(inviteSnapshot);
    }

    function addFomoSnapshot(address sender, address recipient, uint256 amount) private {
        if (Address.isContract(recipient)) {
            return;
        }
        if (automatedMarketMakerPairs[sender]) {
            fomoSnapshot.lastSnapshot(recipient, amount);
        }
    }

    function setAutoFomoSnapshot(bool value) external onlyOwner {
        autoFomoSnapshot = value;
    }

    function getFomoSnapshotContract() external view returns (address) {
        return address(fomoSnapshot);
    }
    function snapshotContracts() external view returns (address[] memory snapshots) {
        snapshots = new address[](3);
        snapshots[0] = address(holderSnapshot);
        snapshots[1] = address(inviteSnapshot);
        snapshots[2] = address(fomoSnapshot);
        return snapshots;
    }
}