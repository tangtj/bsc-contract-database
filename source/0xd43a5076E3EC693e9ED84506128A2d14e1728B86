/**
 *Submitted for verification at BscScan.com on 2023-08-24
*/

pragma solidity >=0.7.0 <0.9.0;

interface IERC20 {
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

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

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
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

contract PriceCalculator {
    uint public dayPrice = 0;
    address public admin ;
    uint public freeEventsCount = 0;
    uint public maxFreDays = 0;
    EventSpacesMarket public market;
    uint constant ONE = 10**18;

    constructor(address _admin, address _market){
        admin = _admin;
        market = EventSpacesMarket(_market);
    }

    /**
     * @dev setting promo params.
     * @param _freeEventsCount uint price in DOME for 1 day rent
     */
    function setParams(uint _freeEventsCount, uint _maxFreDays) public {
        require(
            msg.sender == admin,
            "event-spaces-market/not-admin"
        );
        freeEventsCount = _freeEventsCount;
        maxFreDays = _maxFreDays;
    }

    /**
     * @dev setting price per day.
     * @param _dayPrice uint price in DOME for 1 day rent
     */
    function setPricePerDay(uint _dayPrice) public {
        require(
            msg.sender == admin,
            "event-spaces-market/not-admin"
        );
        require(
            _dayPrice > ONE,
            "event-spaces-market/bad-price"
        );
        dayPrice = _dayPrice;
    }
    
     function changeAdmin(address newAdmin) public {
        require(
            msg.sender == admin || admin == address(0),
            "event-spaces-market/not-admin"
        );
        admin = newAdmin;
     }

     function getPrice(uint numberOfDays) public  view returns (uint){
         int newNUmberOfDays = (market.totalCounter()<freeEventsCount)?int(numberOfDays):int(numberOfDays)-int(maxFreDays);
         if(newNUmberOfDays<0)
            return 0;
         else
            return  uint(newNUmberOfDays) * dayPrice;
     }
}

/** 
 * @title Ballot
 * @dev Implements voting process along with vote delegation
 */
contract EventSpacesMarket {

    uint constant DAY = 24*3600;
    uint constant ONE = 10**18;

    struct EventSpace {
        uint64 from; //starting time from when event is active
        uint64 till; //finish time till when event is active
        address owner; 
        bytes32 passwordHash; 
    }

    struct EventSpaceData {
        uint32 from; //starting time from when event is active
        uint32 till; //finish time till when event is active
        address owner; 
    }

    struct UsersStats {
        uint domeBalance;
        uint allowance;
        uint amountNeeded;
    }

    mapping(uint /*day from Start */ => bytes32[]) public spacesForDay;
    mapping(address => bytes32[]) public  spacesOfUser;
    mapping(bytes32 => EventSpaceData) private addedSpaces;

    uint public start;
    address public priceCalculator;
    uint public totalCounter = 0;

    address public admin ;

    IERC20 private domeToken;

    /** 
     * @dev Create a new ballot to choose one of 'proposalNames'.
     * @param token names of proposals
     */
    constructor(IERC20 token, uint _start) {
        start = _start;
        admin = msg.sender;
        domeToken = token;
        priceCalculator = address(new PriceCalculator(msg.sender, address(this)));
    }

    /**
     * @dev allows Admin to withdraw earnings from sell
     */
    function withdrawTokens() public {
        require(
            msg.sender == admin,
            "event-spaces-market/not-admin"
        );
        IERC20(domeToken).transfer(msg.sender, IERC20(domeToken).balanceOf(address(this)));
    }

    /**
     * @dev allows user to buy EventSpace from specified day for specified number of days
     */
    function getCurrentDay() public view returns(uint){
        return (block.timestamp - start) / DAY + 1;
    }

    /**
     * @dev converts timestamp into spacesForDay index
     * @param startTime timestamp to convert
     */
    function getDay(uint startTime) public view returns(uint64){
        return uint64((startTime - start) / DAY) + 1;
    }
    
    /**
     * @dev allows user to buy EventSpace from specified day for specified number of days
     * @param _startTime UTC timestamp when event should start
     * @param numberOfDays how many days from now event starts
     */
     function buyEvent(uint _startTime, uint numberOfDays) public {
         UsersStats memory stats = getUsersStats(msg.sender, numberOfDays);
         require(domeToken.balanceOf(msg.sender) >= stats.amountNeeded, "event-spaces-market/not-enough-tokens");
         require(domeToken.allowance(msg.sender, address(this)) >= stats.amountNeeded, "event-spaces-market/incorrect-allowance");
         domeToken.transferFrom(msg.sender, address(this), stats.amountNeeded);
         uint today = getCurrentDay();
         uint startTime = _startTime;
         uint endTime = _startTime + numberOfDays * DAY;
         bytes32 hash = keccak256(abi.encodePacked(msg.sender, block.timestamp, spacesForDay[today].length));
         addedSpaces[hash] = EventSpaceData(uint32(startTime), uint32(endTime), msg.sender);
         spacesForDay[getDay(startTime)].push(hash);
         spacesOfUser[msg.sender].push(hash);
         emit SpaceData(hash, uint32(startTime), uint32(endTime), msg.sender, uint32(today));
         totalCounter+=1;
     }

     function editEventStart(bytes32 hash, uint _newStart) public {
         uint originalDay = getDay(_newStart);
         uint newDay = getDay(_newStart);
         EventSpaceData memory data = addedSpaces[hash];
         require(data.owner == msg.sender, "event-spaces-market/not-owner");
         uint32 newEnd = uint32(_newStart)+(data.till-data.from);
         data.from = uint32(_newStart);
         data.till = uint32(newEnd); 
         addedSpaces[hash] = data;
         uint indexToRemove = findEvIndex(spacesForDay[originalDay], hash);
         uint dayLen = spacesForDay[originalDay].length; 
         emit SpaceData(hash, data.from, data.till, msg.sender, uint32(newDay));
         spacesForDay[newDay].push(hash);
         spacesForDay[originalDay][indexToRemove] = spacesForDay[originalDay][dayLen-1];
         spacesForDay[originalDay].pop();
     }

     function changeAdmin(address newAdmin) public {
        require(
            msg.sender == admin,
            "event-spaces-market/not-admin"
        );
        admin = newAdmin;
     }

     function changePriceCalculator(address newCalculator) public {
        require(
            msg.sender == admin,
            "event-spaces-market/not-admin"
        );
        priceCalculator = newCalculator;
     }

     function findEvIndex(bytes32[] storage hashes, bytes32 hashToFind) private view returns(uint) {
         for(uint i=0;i<hashes.length;i++){
             if(hashes[i] == hashToFind){
                 return i;
             }
         }
         return uint(int(-1));
     }

     function changeEventOwner(address newOwner, uint eventIndex) public{
        require(spacesOfUser[msg.sender].length > eventIndex, "event-spaces-market/alid-index");
        bytes32 evHash = spacesOfUser[msg.sender][eventIndex];
        spacesOfUser[newOwner].push(evHash);
        spacesOfUser[msg.sender][eventIndex] = spacesOfUser[msg.sender][spacesOfUser[msg.sender].length-1];
        spacesOfUser[msg.sender].pop();
     }

    /**
     * @dev helper function, returns stats for a user
     * @param user, address of a user we want to get information about
     * @param numberOfDays number of days, user wants to rent specific EventSpace, used to calculate the price
     * @return 3 properties: balance of user, allowence set by user to the contract, price that user needs to pay for numberOfDays rent of an event space
     */
     function getUsersStats(address user, uint numberOfDays) public view returns(UsersStats memory){
         uint balance = domeToken.balanceOf(address(user));
         uint allowance = domeToken.allowance(user, address(this));
         uint amountNeeded = PriceCalculator(priceCalculator).getPrice(numberOfDays);
         return UsersStats(balance, allowance, amountNeeded);
     }

    /**
     * @dev helper function, returns array of EventSpaces of specific user that are future or ongoing
     * @param user, address of a user we want to get information about
     * return array of EventSpace records, each record described by from timestamp, till timestamp, owner, and eventId
     */
     function getUserNotEndedEvents(address user) public view returns (EventSpace[] memory data){  
         EventSpace[] memory initial = new EventSpace[](spacesOfUser[user].length);
         uint total = 0;
         for(uint i = 0; i< spacesOfUser[user].length; i++){
             bytes32 hash = spacesOfUser[user][i];
             EventSpaceData memory evData = addedSpaces[hash];
             if(evData.till > block.timestamp){
                initial[total] = EventSpace(evData.from, evData.till, evData.owner, hash);
                total++;
             }
         }
         data = new EventSpace[](total);
         for(uint j = 0; j< total; j++){
             data[j] = initial[j];
         }
     }

    /**
     * @dev THIS FUNCTION SHOULDN'T BE CALLED ON PRODUCTION OR CLIENT-SIDE, for testing purposes only
     * @param hash,  EventSpace.passwordHash of specific event
     * @param secret,  secret server-side hash allowing to transform hash into password
     * @return password - 12 digit hex code, user can use to enter event
     */
     function calculatePassword(bytes32 hash, bytes32 secret) public pure returns (bytes6 password) {
        password = bytes6(keccak256(abi.encode(hash, secret)));
        return password;
     }

    /**
    List of all spaces that should be currently active
    **/
     function getSpacesForToday() public view returns (EventSpace[] memory spaces){
        uint day = getCurrentDay();
        return getSpacesForDay(day);
     }

     function getSpacesForDay(uint256 _time) public view returns (EventSpace[] memory spaces){
        EventSpace[] memory initial = new EventSpace[](spacesForDay[_time].length);
        for(uint i = 0; i< initial.length; i++){
            EventSpaceData memory ev = addedSpaces[spacesForDay[_time][i]];
            initial[i] = EventSpace(ev.from, ev.till, ev.owner, spacesForDay[_time][i]);
        }
        return initial;
     }

     event SpaceData(bytes32 indexed hash, uint32 start, uint32 end, address indexed owner, uint32 indexed day);

}