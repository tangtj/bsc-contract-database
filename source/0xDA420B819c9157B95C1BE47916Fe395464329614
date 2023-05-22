// SPDX-License-Identifier: MIT

pragma solidity ^0.8.10;

library Address {
    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {size := extcodesize(account)}
        return size > 0;
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

abstract contract Ownable is Context {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    constructor () {
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
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
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
        return c;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}



library SafeERC20 {
    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }
    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }
    function callOptionalReturn(IERC20 token, bytes memory data) private {
        require(address(token).isContract(), "SafeERC20: call to non-contract");
        (bool success, bytes memory returndata) = address(token).call(data);
        require(success, "SafeERC20: low-level call failed");
        if (returndata.length > 0) {
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

contract ReentrancyGuard {
    uint256 private _guardCounter;
    constructor () {
        _guardCounter = 1;
    }
    modifier nonReentrant() {
        _guardCounter += 1;
        uint256 localCounter = _guardCounter;
        _;
        require(localCounter == _guardCounter, "ReentrancyGuard: reentrant call");
    }
}

library Math {
    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }
}

// Inheritancea
interface IStakingRewards {
    // Views
    function getRewardForDuration() external view returns (uint256);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);

    // Mutative
    function stake(uint256 num) external;
    function exit() external;

    // EVENTS
    event Exit(address indexed user);
    event RewardAdded(uint256 reward);
    event Staked(address indexed user, uint256 amount);
}

interface IFBX_NFT_Token {
    function safeTransferFrom(address from_, address to_, uint256 tokenId_) external;
    function getHashrateByTokenId(uint256 tokenId_) external view returns(uint256);
    function feedFBXOnlyPrice() external view returns (uint256);
    function tokenOfOwnerByIndex(address owner, uint256 index) external view returns (uint256);
    function balanceOf(address owner) external view returns (uint256);
}


interface IERC721Receiver {
    function onERC721Received(address operator,address from,uint256 tokenId,bytes calldata data) view external returns (bytes4);
}

contract ForthBoxNFT_StakingRewards is IStakingRewards, Ownable, ReentrancyGuard,IERC721Receiver {
    using SafeMath for uint256;
    using SafeERC20 for IERC20;
    using Address for address;

    string private _name = "ForthBoxFighter DeFi";
    string private _symbol = "NFT DeFi";

    IERC20 public stakingToken;
    IFBX_NFT_Token public rewardsTokenNFT;

    uint256 public rewardsDuration = 30 days;
    uint256 public lastUpdateTime;

    struct sStakeProperty {
        uint256 _Num;
        uint256 _UpdataTime;
    }

    mapping(address => sStakeProperty) private _stakePropertys;
    mapping(address => uint256) private _balances;

    uint256 private _totalSupply;
    uint256 public stakePrice = 3000e18;
    uint256 public totalNum = 1000;
    uint256 public maxStakeNumPerAdress = 1;
    uint256 public alreadyStakeNum = 0;
    bool public start = false;   

    mapping (address => bool) private _Is_WhiteContractArr;
    address[] private _WhiteContractArr;

    constructor() {
    }

    /* ========== VIEWS ========== */
    function totalSupply() external view returns (uint256) {
        return _totalSupply;
    }
    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }
    function stakeTime(address account) external view returns (uint256) {
        return _stakePropertys[account]._UpdataTime;
    }
    function getRewardForDuration() external view returns (uint256) {
        return totalNum;
    }
    function stakeNum(address account) external view returns (uint256) {
        return _stakePropertys[account]._Num;
    }
    function onERC721Received(address,address,uint256,bytes memory) public pure returns (bytes4) {
        return this.onERC721Received.selector;
    }

    function isWhiteContract(address account) public view returns (bool) {
        if(!account.isContract()) return true;
        return _Is_WhiteContractArr[account];
    }
    function getWhiteAccountNum() public view returns (uint256){
        return _WhiteContractArr.length;
    }
    function getWhiteAccountIth(uint256 ith) public view returns (address WhiteAddress){
        require(ith <_WhiteContractArr.length, "Defi: not in White Adress");
        return _WhiteContractArr[ith];
    }
    function getParameters(address account) public view returns (uint256[] memory){
        uint256[] memory paraList = new uint256[](uint256(7));
        paraList[0]= alreadyStakeNum;
        paraList[1]= totalNum;
        paraList[2]= _balances[account];
        paraList[3]= _totalSupply;
        paraList[4]= stakePrice;
        paraList[5]= _stakePropertys[account]._Num;
        paraList[6]= _stakePropertys[account]._UpdataTime;
        return paraList;
    }

    //---write---//
    function stake(uint256 num) external nonReentrant {
        require(num > 0, "Defi:Cannot stake 0");
        require(start, "Defi:not start");
        require(alreadyStakeNum.add(num) <= totalNum, "Defi:stake num exceed max number");
        require(_stakePropertys[_msgSender()]._Num.add(num) <= maxStakeNumPerAdress, "Defi:stake num exceed max number");
        require(isWhiteContract(_msgSender()), "Defi: Contract not in white list!");

        uint256 amount = stakePrice.mul(num);
        uint256 beforeValue = stakingToken.balanceOf(address(this));
        stakingToken.safeTransferFrom(msg.sender, address(this), amount);
        uint256 afterValue = stakingToken.balanceOf(address(this));

        _balances[msg.sender] = _balances[msg.sender].add(afterValue.sub(beforeValue));
        _totalSupply = _totalSupply.add(afterValue.sub(beforeValue));

        _stakePropertys[_msgSender()]._Num = _stakePropertys[_msgSender()]._Num.add(num);
        _stakePropertys[_msgSender()]._UpdataTime = block.timestamp;
        alreadyStakeNum = alreadyStakeNum.add(num);

        for(uint256 i=0; i<num; ++i) {
            rewardsTokenNFT.safeTransferFrom(address(this),_msgSender(), 
                rewardsTokenNFT.tokenOfOwnerByIndex(address(this),0)
            );
        }
        emit Staked(msg.sender, amount);
    }
   //取出代币和奖励
    function exit() external nonReentrant {
        require(block.timestamp >= _stakePropertys[_msgSender()]._UpdataTime + rewardsDuration,"Defi: can only exit exceed rewards Duration!");
        require(_balances[msg.sender] > 0, "Defi: not stake");
        require(start, "Defi:not start");
        require(isWhiteContract(_msgSender()), "Defi: Contract not in white list!");
   
        uint256 stakeAmount = _balances[msg.sender];
        _totalSupply = _totalSupply.sub(_balances[msg.sender] );
        _balances[msg.sender] = 0;
        _stakePropertys[_msgSender()]._Num=0;
        if(stakeAmount>0) stakingToken.safeTransfer(msg.sender, stakeAmount);
        emit Exit(_msgSender());
    }

    //---write onlyOwner---//
    function setTokens(address _rewardNFTsToken,address _stakingToken,uint256 _rewardsDuration) external onlyOwner {
        rewardsTokenNFT = IFBX_NFT_Token(_rewardNFTsToken);
        stakingToken = IERC20(_stakingToken);
        rewardsDuration = _rewardsDuration;
    }

    function notifyRewardAmount(uint256 tStakePrice,uint256 tTotalNum,uint256 tMaxStakeNumPerAdress,bool tStart) external onlyOwner{
        stakePrice = tStakePrice;
        totalNum = tTotalNum;
        maxStakeNumPerAdress = tMaxStakeNumPerAdress;
        start = tStart;
        require(totalNum.sub(alreadyStakeNum) <= rewardsTokenNFT.balanceOf(address(this)), "Defi: Provided total NFT Num too high");
        
        lastUpdateTime = block.timestamp;
        emit RewardAdded(totalNum);
    }
    function addWhiteAccount(address account) external onlyOwner{
        require(!_Is_WhiteContractArr[account], "Defi:Account is already White list");
        require(account.isContract(), "Defi: not Contract Adress");
        _Is_WhiteContractArr[account] = true;
        _WhiteContractArr.push(account);
    }
    function removeWhiteAccount(address account) external onlyOwner{
        require(_Is_WhiteContractArr[account], "Defi:Account is already out White list");
        for (uint256 i = 0; i < _WhiteContractArr.length; i++){
            if (_WhiteContractArr[i] == account){
                _WhiteContractArr[i] = _WhiteContractArr[_WhiteContractArr.length - 1];
                _WhiteContractArr.pop();
                _Is_WhiteContractArr[account] = false;
                break;
            }
        }
    }
}