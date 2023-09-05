// SPDX-License-Identifier: MIT
pragma solidity 0.8.9;

interface IBEP20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the token decimals.
     */
    function decimals() external view returns (uint8);

    /**
     * @dev Returns the token symbol.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the token name.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the bep token owner.
     */
    function getOwner() external view returns (address);

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
    function allowance(address _owner, address spender)
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
     * @dev Moves `amount` tokens from message sender to the null address(0x0) and reduce
     * and reduce the total amount of token by `amount`
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function burn(uint256 amount) external returns (bool);

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
contract Ownable {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );


    /**
     * @dev Initializes the contract owner.
     */
    constructor(address masterWallet) {
        _owner = masterWallet;
        emit OwnershipTransferred(address(0), masterWallet);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     */
    function _transferOwnership(address newOwner) internal {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

}

contract DepositLogic is Ownable{
    uint8 private lockDayNum;
    uint256 private totalDeposit;
    uint256 private currentDeposit;
    mapping (address => DepositLog[]) userDeposits;
    uint8 interest;
    event DepositTransferred(
        address indexed,
        uint256 value
    );
    constructor(uint8 _days, uint8 _interest, address _masterWallet) Ownable(_masterWallet){
        lockDayNum = _days;
        interest = _interest;
    }
    struct DepositLog{
        uint256 releaseTime;
        uint256 depositAmount;
        uint256 interestAmount;
        bool released;
    }
    function setLockDayNum(uint8 _days) external onlyOwner returns(bool){
        lockDayNum = _days;
        return true;
    }
    function getInterest() public view returns(uint8){
        return interest;
    }
    function setInterest(uint8 _interest) external onlyOwner returns(bool){
        interest = _interest;
        return true;
    }
    function getTotalDeposit() public view returns(uint256){
        return totalDeposit;
    }

    function getCurrentDeposit() public view returns(uint256){
        return currentDeposit;
    }

    function _addCurrentDeposit(uint256 _amount) internal returns(bool){
        currentDeposit += _amount;
        totalDeposit += _amount;
        return true;
    }
    function _subCurrentDeposit(uint256 _amount) internal returns(bool){
        currentDeposit -= _amount;
        return true;
    }

    function getLockDayNum() public view returns(uint8){
        return lockDayNum;
    }
    function _calcTime() internal view returns(uint256){
        // return getLockDayNum() * 24 * 60 * 60;
        return  getLockDayNum() * 60;
    }

    function _deposit(address _address, uint256 _value)internal returns(bool){
        DepositLog[] storage userLog = userDeposits[_address];
        uint256 releaseTime = _calcTime() + getBlockTimestamp();
        uint256 interestAmount = _value * interest/100;

        DepositLog memory curLog = DepositLog({
            releaseTime : releaseTime,
            depositAmount : _value,
            interestAmount : interestAmount,
            released : false
        });
        userLog.push(curLog);
        _addCurrentDeposit(_value);
        emit DepositTransferred(_address, _value);
        return true;
    }
    
    function getBlockTimestamp() public view returns(uint256){
        return block.timestamp;
    }

    function getCurrentReleaseAmountForAddress(address _address) public view returns(uint256, uint256){
        DepositLog[] memory userLog = userDeposits[_address];
        uint256 totalCapital = 0;
        uint256 totalInterest = 0;
        for(uint256 i=0; i<userLog.length; i++){
            DepositLog memory log = userLog[i];
            if(!log.released){
                if(getBlockTimestamp() >= log.releaseTime){
                    totalCapital += log.depositAmount;
                    totalInterest += log.interestAmount;
                }else{
                    break;
                }
            }
        }
        return (totalCapital, totalInterest);
    }

    function _release(address _address) internal returns(uint256, uint256){
        DepositLog[] storage userLog = userDeposits[_address];
        uint256 releaseDepositAmount = 0;
        uint256 releaseInterestAmount = 0;
        for(uint256 i=0; i<userLog.length; i++){
            DepositLog storage log = userLog[i];
            if(!log.released){
                if(getBlockTimestamp() >= log.releaseTime){
                    releaseDepositAmount += log.depositAmount;
                    releaseInterestAmount += log.interestAmount;
                    log.released = true;
                }else{
                    break;
                }
            }
        }
        _subCurrentDeposit(releaseDepositAmount);
        return (releaseDepositAmount, releaseInterestAmount);
    }
    

    function getAllLogsBaseOnAddress(address _address) public view returns(DepositLog[] memory){
        DepositLog[] memory userLog = userDeposits[_address];
        return userLog;
    }


}

contract StakingCC is DepositLogic{
    IBEP20 private curToken;
    IBEP20 private exchangeToken;
    uint256 private ratio;
    bool private paused; 
    constructor(address _masterWallet, address _curToken, address _exchangeToken, uint256 _ratio, uint8 _lockDayNum, uint8 _interest) DepositLogic(_lockDayNum, _interest, _masterWallet){
        curToken = IBEP20(_curToken);
        exchangeToken = IBEP20(_exchangeToken);
        ratio = _ratio;
        paused = false;
    }
    function getPaused() public view returns(bool){
        return paused;
    }
    function startExchange() public onlyOwner returns(bool){
        paused = false;
        return true;
    }
    function stopExchange() public onlyOwner returns(bool){
        paused = true;
        return true;
    }
    modifier onlyStarted(){
        require(!paused, "Exchange: The exchange have not started.");
        _;
    }

    function getCurToken() public view returns(IBEP20){
        return curToken;
    }
    function setCurToken(address _newCurToken) public onlyOwner returns(bool){
        curToken = IBEP20(_newCurToken);
        return true;
    } 
    function getExchangetoken() public view returns(IBEP20){
        return exchangeToken;
    }
    function setSwapToken(address _newSwapToken) public onlyOwner returns(bool){
        exchangeToken = IBEP20(_newSwapToken);
        return true;
    }
    function getRatio() public view returns(uint256){
        return ratio;
    }

    function setSwapRatioOwner(uint256 _ratio) public onlyOwner returns(bool){
        require(_ratio>0,"Swap: Swap ratio cannot be lower than 0");
        ratio = _ratio;
        return true;
    }


    function getAllowance(address _address) public view returns(uint256){
        return curToken.allowance(_address, address(this));
    }
    function getExchangeTokenBal() external view returns(uint256){
        return exchangeToken.balanceOf(address(this));
    }
    function getCurTokenBal() external view returns(uint256){
        return curToken.balanceOf(address(this));
    }

    function stake(uint256 _amount) public onlyStarted returns(bool){
        require(getAllowance(msg.sender) >= _amount, "Exchange: Amount exceeds allowance");
        curToken.transferFrom(msg.sender, address(this), _amount);
        uint256 amountAfterRatio = _amount * ratio / 100;
        _deposit(msg.sender, amountAfterRatio);
        return true;
    }

    function release() public onlyStarted returns(bool){
        (uint256 releaseDepositAmount, uint256 releaseInterestAmount) = _release(msg.sender);
        require(releaseDepositAmount >0,"Release: Release amount is 0");
        curToken.transfer(msg.sender, releaseDepositAmount);
        exchangeToken.transfer(msg.sender, releaseInterestAmount);
        return true;
    }


    function withdrawExchangeToken(address _address, uint256 _amount) public onlyOwner returns(bool){
        exchangeToken.transfer(_address, _amount);
        return true;
    }

    function withdrawCurToken(address _address, uint256 _amount) public onlyOwner returns(bool){
        curToken.transfer(_address, _amount);
        return true;
    }
}