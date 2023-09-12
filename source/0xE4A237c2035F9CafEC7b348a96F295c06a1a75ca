// SPDX-License-Identifier: MIT
pragma solidity 0.8.0;

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
        require(b > 0, errorMessage);
        uint256 c = a / b;

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0);
        return a % b;
    }
}

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

contract Flex {

    using SafeMath for uint256;

    address private _owner;

    struct Share {
        uint256 amount;
        uint256 totalExcluded;
        uint256 totalRealised;
    }
    //Mainnet
    IERC20 BUSD = IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56); 
    //Testnet 
    //IERC20 BUSD = IERC20(0x78867BbEeF44f2326bF8DDd1941a4439382EF2A7);

    mapping (address => uint256) shareholderClaims;
    mapping (address => Share) public shares;

    uint256 public totalShares;
    uint256 public totalDividends;
    uint256 public totalDistributed;
    uint256 public dividendsPerShare;

    uint256 public dividendsPerShareAccuracyFactor = 10 ** 36;
    uint256 public minimumTokenToReward = 2000_000 * 10 ** 18;
    mapping (address => bool) public excludeFromMarket;

    mapping (address => bool) public whitelisted;
    bool public paused;

    modifier onlyWhitelisted() {
        require(whitelisted[msg.sender],"Error: Caller must be whitelisted!!");
        _;
    }

    modifier onlyOwner() {
        require(msg.sender == _owner,"Error: Caller must be Owner!!");
        _;
    }

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _owner = msg.sender;
        whitelisted[address(this)] = true;
        whitelisted[address(0x27647154b54e95066BAE190cBf50B8CBE98BFc51)] = true; //This is the address we are deploying from (arbitrary)
    }

    function setShare(address shareholder, uint256 amount) public onlyWhitelisted {
        if(excludeFromMarket[shareholder]) return;
        uint _selector = amount >= minimumTokenToReward ? amount : 0;
        totalShares = totalShares.sub(shares[shareholder].amount).add(_selector);
        shares[shareholder].amount = _selector;
        shares[shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);
    }

    function deposit(uint256 amount) external onlyWhitelisted {
        BUSD.transferFrom(msg.sender, address(this), amount);
        totalDividends = totalDividends.add(amount);
        dividendsPerShare = dividendsPerShare.add(dividendsPerShareAccuracyFactor.mul(amount).div(totalShares));
    }

    function distributeDividend(address shareholder) internal {
        if(shares[shareholder].amount == 0){ return; }

        uint256 amount = getUnpaidEarnings(shareholder);
        if(amount > 0){
            totalDistributed = totalDistributed.add(amount);
            BUSD.transfer(shareholder, amount);
            shareholderClaims[shareholder] = block.timestamp;
            shares[shareholder].totalRealised = shares[shareholder].totalRealised.add(amount);
            shares[shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);
        }
    }

    function getUnpaidEarnings(address shareholder) public view returns (uint256) {
        if(shares[shareholder].amount == 0){ return 0; }

        uint256 shareholderTotalDividends = getCumulativeDividends(shares[shareholder].amount);
        uint256 shareholderTotalExcluded = shares[shareholder].totalExcluded;

        if(shareholderTotalDividends <= shareholderTotalExcluded){ return 0; }

        return shareholderTotalDividends.sub(shareholderTotalExcluded);
    }

    function getCumulativeDividends(uint256 share) internal view returns (uint256) {
        return share.mul(dividendsPerShare).div(dividendsPerShareAccuracyFactor);
    }

    function claimDividend() external {
        require(!paused,"Error: Market is Closed!!");
        distributeDividend(msg.sender);
    }

    function setMinToken(uint256 _limit) external onlyOwner {
        minimumTokenToReward = _limit;
    }

    function setExcludeFromMarket(address _holder, bool _status) external onlyOwner {
        require(excludeFromMarket[_holder] != _status,"Error: Record Not Changed!");
        excludeFromMarket[_holder] = _status;
        if(_status) {
            setShare(_holder,0);
        }
    }

    function setWhitelisted(address _adr,bool _status) external onlyOwner {
        require(whitelisted[_adr] != _status,"Error: Record Not Changed!");
        whitelisted[_adr] = _status;
    }

    function setToken(address _newtoken) external onlyOwner {
        BUSD = IERC20(_newtoken);
    }

    function rescueToken(address _rtoken, uint256 _amount) external onlyOwner {
        IERC20(_rtoken).transfer(msg.sender, _amount);
    }

    function rescueFunds() external onlyOwner {
        (bool os,) = payable(msg.sender).call{value: address(this).balance}("");
        require(os);
    }

    function setPauser(bool _status) external onlyOwner {
        paused = _status;
    }

    function owner() public view returns (address) {
        return _owner;
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