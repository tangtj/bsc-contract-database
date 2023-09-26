// SPDX-License-Identifier: MIT
//Facilfi staking functions
pragma solidity ^0.8.4;


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

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}


// File @openzeppelin/contracts/access/Ownable.sol@v4.6.0

// OpenZeppelin Contracts v4.4.1 (access/Ownable.sol)

pragma solidity ^0.8.0;

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
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
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
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
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface Facilfi {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
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

interface pool {
    function trasferisciToken(uint256 amount) external returns (uint256);
}


contract Staking is Ownable {
    using SafeMath for uint256;

    pool public poolContract; // L'indirizzo del contratto di distribuzione
    uint256 public distributionInterval = 1 days; // Intervallo di 24 ore
    Facilfi public FacilfiContract;

    mapping(address => uint256) private _lastClaimTime;
    mapping(address => uint256) private _StakingTime;

    //Staking mappings
    //address[] internal stakeholders;
    mapping(address => uint256) private _stakedFacilfi; //stake
    mapping(address => uint256) private _accruedFacilfi; //Tracks reward

    //Staking pool
    uint256 private _stakedFacilfiPool; //T

    constructor(address _FacilfiContractAddress, address _poolContractAddress) {
        FacilfiContract = Facilfi(_FacilfiContractAddress);
        poolContract = pool(_poolContractAddress);
    }

    function setFacilfiContract(address _FacilfiContractAddress)
        external
        onlyOwner
    {
        FacilfiContract = Facilfi(_FacilfiContractAddress);
    }

    function setpoolContract(address _poolContract) external onlyOwner {
        poolContract = pool(_poolContract);
    }

    function setDistributionInterval(uint256 intervalInDays) public onlyOwner {
        distributionInterval = intervalInDays * 1 days;
    }

    function viewDistributionInterval() public view onlyOwner returns (uint256) {
        return distributionInterval;
    }

    function timeUntilNextDistribution() public view returns (uint256) {
        uint256 timeSinceLastClaim = block.timestamp - _lastClaimTime[msg.sender];
        uint256 timeSinceLastStaking = block.timestamp - _StakingTime[msg.sender];

        if (timeSinceLastStaking >= distributionInterval || timeSinceLastClaim >= distributionInterval) {
            return 0; // Il prossimo blocco di distribuzione è ora
        } else {
            return distributionInterval - timeSinceLastClaim;
        }
    }


    function _stakeFacilfi(uint256 stakeAmount) public {
        //Make sure the message sender has enought Facilfi
        require(
            FacilfiContract.balanceOf(msg.sender) >= stakeAmount,
            "Insufficient Facilfi balance"
        );

        //Find out how much Facilfi is staked
        uint256 initialFacilfiBalance = FacilfiContract.balanceOf(
            address(this)
        );

        //Transfer Facilfi from the sender
       FacilfiContract.transferFrom(msg.sender, address(this), stakeAmount);

        //Find out how much Facilfi was collected
        uint256 newFacilfiBalance = FacilfiContract.balanceOf(address(this));

        uint256 FacilfiReceived = newFacilfiBalance.sub(initialFacilfiBalance);

        _stakedFacilfi[msg.sender] = _stakedFacilfi[msg.sender].add(
            FacilfiReceived
        );
        _stakedFacilfiPool = _stakedFacilfiPool.add(FacilfiReceived);

        _StakingTime[msg.sender] = block.timestamp;
        _lastClaimTime[msg.sender] = block.timestamp;

    }


    function _claimAllFacilfi() public {
        require(_currentRewards(msg.sender) > 0, "Insufficient accrued Facilfi");

     //require(block.timestamp >= _StakingTime[msg.sender] + distributionInterval || block.timestamp >= _lastClaimTime[msg.sender] + distributionInterval);

            if (_stakedFacilfiPool > 0) {
                uint256 reward = _calculateReward(msg.sender);
                poolContract.trasferisciToken(reward);
                _accruedFacilfi[msg.sender] = _accruedFacilfi[msg.sender].add(reward);
                _stakedFacilfi[msg.sender] = _stakedFacilfi[msg.sender].add(reward);
                _stakedFacilfiPool = _stakedFacilfiPool.add(reward);
                _lastClaimTime[msg.sender] = block.timestamp; // Aggiorna l'ultimo momento di distribuzione
                _StakingTime[msg.sender] = block.timestamp;
            }
        }


    function _unstakeAll() public {
        require(
            _stakedFacilfi[msg.sender] > 0,
            "Message sender has no staked Facilfi"
        );
        // In entrambi i casi, tassa o non tassa, gli Facilfi staked vengono restituiti all'utente.
        FacilfiContract.transfer(msg.sender, _stakedFacilfi[msg.sender]);
        _stakedFacilfiPool = _stakedFacilfiPool.sub(_stakedFacilfi[msg.sender]);
        _stakedFacilfi[msg.sender] = _stakedFacilfi[msg.sender].sub(_stakedFacilfi[msg.sender]);
        _accruedFacilfi[msg.sender] = _accruedFacilfi[msg.sender].sub(_accruedFacilfi[msg.sender]);
         // Aggiorna l'ultimo momento di distribuzione
        _StakingTime[msg.sender] = 0;
        _lastClaimTime[msg.sender] = 0;
    }


    //Staking view funcitons
    function _addressStakedFacilfi(address addy) public view returns (uint256) {
        return _stakedFacilfi[addy];
    }

    function _totalStakedFacilfi() public view returns (uint256) {
        return _stakedFacilfiPool;
    }


    //Returns the percentage of the staked pool a new stake would have
    function _percentageOfStakePoolNewStake(uint256 amount)
        public
        view
        returns (uint256)
    {
        if (_stakedFacilfiPool == 0) return 10000;
        return (
            (amount.add(_stakedFacilfi[msg.sender])).mul(10**4).div(
                _stakedFacilfiPool.add(amount)
            )
        );
    }

    //return the percentage of the staked pool of an address with 2 decimal places
    function _percentageOfStakePools(address addy)
        public
        view
        returns (uint256)
    {
        return (_percentageOfStakePool(_stakedFacilfi[addy]));
    }

    //return the percentage of the staked pool some number of Facilfi is be with 2 decimals
    function _percentageOfStakePool(uint256 amount)
        public
        view
        returns (uint256)
    {
        if (_stakedFacilfiPool == 0) return 0;
        return (amount.mul(10**4).div(_stakedFacilfiPool));
    }


    function _calculateReward(address addy) internal view returns (uint256) {
        uint256 stakePercentage = _percentageOfStakePool(_stakedFacilfi[addy]);
        uint256 balancepool = FacilfiContract.balanceOf(address(poolContract));
        uint256 rewardAmount = (balancepool.mul(stakePercentage)).div(10000);
        return rewardAmount;
    }


    function _currentRewards(address addy) public view returns (uint256) {
        return _calculateReward(addy);
    }

    function earnedFacilfi(address addy) public view returns (uint256) {
        return _accruedFacilfi[addy];
    }

      //da togliere
    function emergencyWithdraw(uint _amount) external onlyOwner {
        FacilfiContract.transfer(msg.sender, _amount);
    }

    function emergencyWithdrawETH(uint _amount) external onlyOwner {
        payable(owner()).transfer(_amount);
    }

         //to recieve ETH from uniswapV2Router when swaping
    receive() external payable {}
         //to recieve ETH from uniswapV2Router when swaping
    fallback() external payable {}

    //far approvare da chi staking il contratto nel contratto token
    //-mettere in exclude fromfee questo e la pool, poi fare stake se ho deployato dopo questi rispetto al principale
}