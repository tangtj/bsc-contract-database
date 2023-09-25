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
    function trasferisciToken() external returns (uint256);
}


contract Staking is Ownable {
    using SafeMath for uint256;

    pool public poolContract; // L'indirizzo del contratto di distribuzione
    uint256 public lastDistributionTime; // dargli un giorno per fare unstake gratis
    uint256 public distributionInterval = 30 days; // Intervallo di 24 ore
    Facilfi public FacilfiContract;
    uint256 public maxStakePercentage = 50; // Percentuale massima di staking iniziale (può essere cambiata dall'owner)
    bool public isFirstDistribution = true;
    mapping(address => bool) public whitelistedContract;


    //Staking mappings
    //address[] internal stakeholders;
    mapping(address => uint256) private _stakedFacilfi; //stake
    mapping(address => uint256) private _accruedFacilfi; //Tracks reward
    mapping(address => uint256) private _stakeEntry; //S naught

    //Staking pool
    uint256 private _stakedFacilfiPool; //T
    uint256 private _totalAccruedFacilfi; //S
    uint256 private _actualAccruedFacilfi; //Facilfi that exists in the contract reward pool
    uint256 private constant FacilfiMagnitude = 2**128; //The magnitute of the entire Facilfi supply

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

    function timeUntilNextDistribution() public view returns (uint256) {
        uint256 timeSinceLastDistribution = block.timestamp -
            lastDistributionTime;

        if (timeSinceLastDistribution >= distributionInterval) {
            return 0; // Il prossimo blocco di distribuzione è ora
        } else {
            return distributionInterval - timeSinceLastDistribution;
        }
    }

    function getTokenBalance() public view returns (uint256) {
        return FacilfiContract.balanceOf(msg.sender);
    }

    function isNextclaim() public view returns (bool) {
        uint256 timeUntilNextclaim = timeUntilNextDistribution();

        if (timeUntilNextclaim < 2 days) {
            return true;
        } else {
            return false;
        }
    }

    function distribute() public {
        // Se è la prima distribuzione, o se è passato l'intervallo di distribuzione
        require(
            isFirstDistribution ||
                (block.timestamp >= lastDistributionTime + distributionInterval)
        );

        if (_stakedFacilfiPool > 0) {
            uint256 distributionAmount = poolContract.trasferisciToken();
            _distributeFacilfi(distributionAmount);
            lastDistributionTime = block.timestamp; // Aggiorna l'ultimo momento di distribuzione
            isFirstDistribution = false; // Imposta isFirstDistribution a false dopo la prima distribuzione
        }
    }

    function _stakeFacilfi(uint256 stakeAmount) public {
        //Make sure the message sender has enought Facilfi
        require(
            FacilfiContract.balanceOf(msg.sender) >= stakeAmount,
            "Insufficient Facilfi balance"
        );

        require(stakeAmount <= _maxLeftToStake());
        //Make sure someone isn't trying to stake more than 50% of their Facilfi
        //require(FacilfiContract.balanceOf(msg.sender).add(_stakedFacilfi[msg.sender]) <= (FacilfiContract.balanceOf(msg.sender).add(_stakedFacilfi[msg.sender]).add(stakeAmount)).div(2), "Staked Facilfi cannot be >50% of total Facilfi holdings");

        //Find out how much Facilfi is staked
        uint256 initialFacilfiBalance = FacilfiContract.balanceOf(
            address(this)
        );

        //Transfer Facilfi from the sender
       FacilfiContract.transferFrom(msg.sender, address(this), stakeAmount);

        //Find out how much Facilfi was collected
        uint256 newFacilfiBalance = FacilfiContract.balanceOf(address(this));

        uint256 FacilfiReceived = newFacilfiBalance.sub(initialFacilfiBalance);

        //Check if the sender already has Facilfi staked
        if (_stakedFacilfi[msg.sender] == 0) {
            //They have not
            //Set this as their stake entry point
            _stakeEntry[msg.sender] = _totalAccruedFacilfi;
        } else {
            //They have
            //Calculate and allocate their rewards from
            //Their original stake point
            //Reset stake point
            uint256 reward = _calculateReward(msg.sender);
            _accruedFacilfi[msg.sender] = _accruedFacilfi[msg.sender].add(
                reward
            );
            _stakeEntry[msg.sender] = _totalAccruedFacilfi;
        }

        _stakedFacilfi[msg.sender] = _stakedFacilfi[msg.sender].add(
            FacilfiReceived
        );
        _stakedFacilfiPool = _stakedFacilfiPool.add(FacilfiReceived);

        //Ensure that the message sender address is in the Facilfi earnings pool
        //(May not be necessary)
        //_accruedFacilfi[msg.sender] = _accruedFacilfi[msg.sender];
    }

    function _claimAllFacilfi() public {
        require(
            _currentRewards(msg.sender) > 0,
            "Insufficient accrued Facilfi"
        );

        //Calculate and allocate their rewards from
        //Their original stake point
        //Reset stake point
        uint256 reward = _calculateReward(msg.sender);
        if (!_slashWillOccur(msg.sender)) {
            _accruedFacilfi[msg.sender] = _accruedFacilfi[msg.sender].add(
                reward
            );
        } else {
            //User is trying to abuse the system by having >50% of their Facilfi staked
            //Redistribute their rewards from stake entry
            _distributeSlashedReward(reward);
        }
        _stakeEntry[msg.sender] = _totalAccruedFacilfi;

        if (_accruedFacilfi[msg.sender] > 0) {
            FacilfiContract.transferFrom(
                address(this),
                msg.sender,
                _accruedFacilfi[msg.sender]
            );
        }

        _actualAccruedFacilfi = _actualAccruedFacilfi.sub(
            _accruedFacilfi[msg.sender]
        );
        _accruedFacilfi[msg.sender] = _accruedFacilfi[msg.sender].sub(
            _accruedFacilfi[msg.sender]
        );
    }

    function _unstakeAll() public {
        require(block.timestamp >= lastDistributionTime);
        require(
            _stakedFacilfi[msg.sender] > 0,
            "Message sender has no staked Facilfi"
        );

        uint256 reward = _calculateReward(msg.sender);
        // In entrambi i casi, tassa o non tassa, gli Facilfi staked vengono restituiti all'utente.
        FacilfiContract.transfer(msg.sender, _stakedFacilfi[msg.sender]);
        _stakedFacilfiPool = _stakedFacilfiPool.sub(_stakedFacilfi[msg.sender]);
        _stakedFacilfi[msg.sender] = _stakedFacilfi[msg.sender].sub(
            _stakedFacilfi[msg.sender]
        );

        if (!_slashWillOccur(msg.sender)) {
            _accruedFacilfi[msg.sender] = _accruedFacilfi[msg.sender].add(
                reward
            );
        } else {
            // L'utente sta cercando di abusare del sistema
            // Ridistribuisci le loro ricompense dall'ingresso dello stake
            _distributeSlashedReward(reward);
        }
        _stakeEntry[msg.sender] = _totalAccruedFacilfi;
    }

    function _emergencyWithdraw() public {
        require(
            _stakedFacilfi[msg.sender] > 0,
            "Message sender has no staked Facilfi"
        );

        uint256 reward = _calculateReward(msg.sender);
        // In entrambi i casi, tassa o non tassa, gli Facilfi staked vengono restituiti all'utente.
        FacilfiContract.transfer(msg.sender, _stakedFacilfi[msg.sender]);
        _stakedFacilfiPool = _stakedFacilfiPool.sub(_stakedFacilfi[msg.sender]);
        _stakedFacilfi[msg.sender] = _stakedFacilfi[msg.sender].sub(
            _stakedFacilfi[msg.sender]
        );
        // L'utente sta cercando di abusare del sistema
        // Ridistribuisci le loro ricompense dall'ingresso dello stake
        _distributeSlashedReward(reward);

        _stakeEntry[msg.sender] = _totalAccruedFacilfi;
    }

    //Staking view funcitons
    function _addressStakedFacilfi(address addy) public view returns (uint256) {
        return _stakedFacilfi[addy];
    }

    function _totalStakedFacilfi() public view returns (uint256) {
        return _stakedFacilfiPool;
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
    function _percentageOfStakePool(address addy)
        public
        view
        returns (uint256)
    {
        return (_percentageOfStakePool(_stakedFacilfi[addy]));
    }

    function _distributeFacilfi(uint256 amount) private {
        if (_stakedFacilfiPool != 0) {
            //Multiplying by Facilfi Magnitude ensures that even if all Facilfi in existence was staked
            //1 Facilfi would still be > _stakedFacilfiPool
            _actualAccruedFacilfi = _actualAccruedFacilfi.add(amount);
            _totalAccruedFacilfi = _totalAccruedFacilfi.add(
                amount.mul(FacilfiMagnitude).div(_stakedFacilfiPool)
            );
        } else {
            return;
        }
    }

    function _distributeSlashedReward(uint256 amount) private {
        if (_stakedFacilfiPool != 0) {
            //Multiplying by Facilfi Magnitude ensures that even if all Facilfi in existence was staked
            //1 Facilfi would still be > _stakedFacilfiPool
            _totalAccruedFacilfi = _totalAccruedFacilfi.add(
                amount.mul(FacilfiMagnitude).div(_stakedFacilfiPool)
            );
            //Dont increase actual accrued Facilfi because this amount is being redistributed, not newly collected
        } else {
            return;
        }
    }

    function _slashWillOccur(address addy) public view returns (bool) {
        uint256 totalBalance = FacilfiContract.balanceOf(addy).add(
            _stakedFacilfi[addy]
        );
        return (_stakedFacilfi[addy] >
            totalBalance.mul(maxStakePercentage).div(100));
    }

    function _FacilfiNeededToAvoidSlash(address addy)
        public
        view
        returns (uint256)
    {
        uint256 totalBalance = FacilfiContract.balanceOf(addy).add(
            _stakedFacilfi[addy]
        );

        // User is compliant
        if (
            _stakedFacilfi[addy] <=
            totalBalance.mul(maxStakePercentage).div(100)
        ) return 0;

        // User is not compliant
        return (
            _stakedFacilfi[addy].sub(
                totalBalance.mul(maxStakePercentage).div(100)
            )
        );
    }

    function _calculateReward(address addy) public view returns (uint256) {
        return
            (
                _stakedFacilfi[addy].mul(
                    _totalAccruedFacilfi.sub(_stakeEntry[addy])
                )
            ).div(FacilfiMagnitude);
    }

    function changeMaxStakePercentage(uint256 newPercentage) public onlyOwner {
        require(newPercentage <= 100, "Percentage cannot exceed 100");
        maxStakePercentage = newPercentage; // Funzione per cambiare la percentuale massima di staking
    }

    function _maxStakeAmount() public view returns (uint256) {
        uint256 totalBalance = FacilfiContract.balanceOf(msg.sender).add(
            _stakedFacilfi[msg.sender]
        );
        return totalBalance.mul(maxStakePercentage).div(100);
    }

    function _maxLeftToStake() public view returns (uint256) {
        return _maxStakeAmount().sub(_stakedFacilfi[msg.sender]);
    }

    function _currentRewards(address addy) public view returns (uint256) {
        return _accruedFacilfi[msg.sender].add(_calculateReward(addy));
    }

    function actualAccruedFacilfi() public view returns (uint256) {
        return _actualAccruedFacilfi;
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
}