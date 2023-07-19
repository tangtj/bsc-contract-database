/**
 *Submitted for verification at BscScan.com on 2023-07-17
*/

/**
 *Submitted for verification at Etherscan.io on 2023-06-19
*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    function decimals() external view returns (uint8);

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
    function allowance(address owner, address spender)
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
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

contract ICO {
    IERC20 public token;

    IERC20 public fiat;

    address payable owner;

    //ICO should have an enum with four rounds
    enum Stages {
        FF,
        PRIVATE1,
        PRIVATE2,
        PUBLIC
    }

    enum Planets {
        Mercury,
        Venus,
        Mars,
        Earth,
        Jupiter,
        Saturn,
        Uranus,
        Neptune
    }

    uint256 public endTime;

    uint8 public current_round = 0;
    uint8 public vestingPeriods = 18;
    uint8 public vestingPercent = 50;

    struct Buyer {
        address buyer;
        // uint256 amountFunded;
        // uint256 amountDue;
        // uint256 amountClaimed;
        //uint8 timesClaimed;
        Package[] packages;
        uint256 nextClaim;
    }

    struct Package {
        uint256 amountFunded;
        uint256 amountDue;
        uint256 amountClaimed;
        uint8 timesClaimed;
        uint8 round;
    }

    struct Referrer {
        address referrer;
        uint256 referralsMade;
        uint256 amountEarned;
    }

    mapping(Planets => uint256) public prices;
    mapping(Planets => uint256) public discounts;
    mapping(address => Buyer) public contributions;
    mapping(uint256 => address) public referrers;
    mapping(uint256 => address) public addressByRefCode;
    mapping(address => uint256) public refByAddress;
    mapping(address => Referrer) public referrals;

    uint256 public sold;
    uint256 public tokenPrice = .008 ether;

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the owner");
        _;
    }

    constructor(
        address _token,
        address _fiat,
        address payable _owner
    ) {
        token = IERC20(_token);
        fiat = IERC20(_fiat);
        owner = _owner;

        discounts[Planets.Mercury] = 0;
        discounts[Planets.Venus] = 0;
        discounts[Planets.Mars] = 0;
        discounts[Planets.Earth] = 3;
        discounts[Planets.Jupiter] = 12;
        discounts[Planets.Saturn] = 9;
        discounts[Planets.Uranus] = 7;
        discounts[Planets.Neptune] = 5;

        prices[Planets.Mercury] = 200;
        prices[Planets.Venus] = 1100;
        prices[Planets.Mars] = 500;
        prices[Planets.Earth] = 2300;
        prices[Planets.Jupiter] = 48000;
        prices[Planets.Saturn] = 23000;
        prices[Planets.Uranus] = 11000;
        prices[Planets.Neptune] = 5000;
    }

    //create random referral code
    function random(address _addr) private view returns (uint256) {
        return
            uint256(
                keccak256(
                    abi.encodePacked(block.prevrandao, block.timestamp, _addr)
                )
            ) % 10000000000;
    }

    function addReferralAddress(address _addr) public {
        uint256 _referralCode = random(_addr);
        referrers[_referralCode] = _addr;
        refByAddress[_addr] = _referralCode;
        addressByRefCode[_referralCode] = _addr;

        referrals[_addr] = Referrer(_addr, 0, 0);
    }

    function getAddrByRefCode(uint256 _code) public view returns (address) {
        return addressByRefCode[_code];
    }

    function getRefByAddress(address _addr) public view returns (uint256) {
        return refByAddress[_addr];
    }

    function getTotalRefRevenue(address _addr) public view returns (uint256) {
        return referrals[_addr].amountEarned;
    }

    //Added getclaim period function to let people know they can claim their next batch
    function getClaimPeriod(address _addr) public view returns (uint256) {
        return contributions[_addr].nextClaim;
    }

    function getAmountDue(address _addr) public view returns (uint256){
        uint256 amountDue = amountDuePackages(_addr);
        return amountDue;
    }

    function getAmountClaim(address _addr) public view returns (uint256){
        uint256 amountClaim = amountToClaimPackages(_addr);
        return amountClaim;
    }

    function getAmountClaimed(address _addr) public view returns (uint256){
        uint256 amountClaimed = amountClaimedPackages(_addr);
        return amountClaimed;
    }

    function getContributionsPackages(address _addr) public view returns(Package[] memory) {
        return contributions[_addr].packages;
    }

    function buyTokens(Planets _package, uint256 refCode) external {
       
        require(
            fiat.balanceOf(msg.sender) >=
                prices[_package] * 10**fiat.decimals(), 
            "Insufficient balance"
        );
        uint256 amountSent = prices[_package] * 10**fiat.decimals();
        uint256 convertedToken = (prices[_package] * 10**token.decimals()) /
            tokenPrice;
        uint256 amountReceived;
        uint256 tgeAmount;

        /*require(
            token.balanceOf(address(this)) >= amountReceived,
            "Insufficient balance of InfinityBee held in contract to complete order"
        );*/
        require(referrers[refCode] != msg.sender, "Can't refer yourself");
        if (referrers[refCode] != address(0)) {
            address refAddr = getAddrByRefCode(refCode);
            uint256 commission = (amountSent * 5) / 100;
            uint256 discount = (amountSent * 5)/ 100;

            amountSent = amountSent - commission - discount;
            referrals[refAddr].referralsMade++;
            referrals[refAddr].amountEarned += commission;

            fiat.transferFrom(msg.sender, refAddr, commission);
        }

        if (current_round > uint8(Stages.FF)) {
            //logic for timed crowdsale
            require(
                block.timestamp < endTime,
                "Current round has already ended!"
            );
        }

        if (current_round == uint8(Stages.FF)) {
            amountReceived = convertedToken;

            tgeAmount = (amountReceived * 10) / 100;

            contributions[msg.sender].buyer = msg.sender;

             Package memory pack = Package({
                amountFunded: amountSent / 10 ** fiat.decimals(),
                amountDue: amountReceived,
                amountClaimed: tgeAmount,
                timesClaimed: 0,
                round: current_round
            });
            contributions[msg.sender].packages.push(pack);
            
            contributions[msg.sender].nextClaim = block.timestamp + 2592000; //2592000;

            token.transfer(msg.sender, tgeAmount * 10 ** token.decimals());

            fiat.transferFrom(msg.sender, address(this), amountSent);
        } else {            

            if (_package < Planets.Earth) {
                //Packages under Earth do not receive a bonus, so amountReceived = convertedToken
                amountReceived = convertedToken; 

                contributions[msg.sender].buyer = msg.sender;

                Package memory pack = Package({
                    amountFunded: amountSent / 10 ** fiat.decimals(),
                    amountDue: amountReceived,
                    amountClaimed: amountReceived,
                    timesClaimed: vestingPeriods,
                    round: current_round
                });
                contributions[msg.sender].packages.push(pack);

                if (contributions[msg.sender].packages.length > 1)
                contributions[msg.sender].nextClaim = block.timestamp + 2592000; //2592000

                token.transfer(msg.sender, amountReceived * 10 ** token.decimals());

                fiat.transferFrom(msg.sender, address(this), amountSent);
            } else {
                amountReceived =
                    convertedToken +
                    ((convertedToken * discounts[_package]) / 100);
                tgeAmount = (amountReceived * 10) / 100;
                

                contributions[msg.sender].buyer = msg.sender;

                Package memory pack = Package({
                    amountFunded: amountSent / 10 ** fiat.decimals(),
                    amountDue: amountReceived,
                    amountClaimed: tgeAmount,
                    timesClaimed: 0,
                    round: current_round
                });
                contributions[msg.sender].packages.push(pack);

                contributions[msg.sender].nextClaim = block.timestamp + 2592000; //2592000;

                token.transfer(msg.sender, tgeAmount * 10 ** token.decimals());

                fiat.transferFrom(msg.sender, address(this), amountSent);
            }
        }

        sold += amountReceived;
    }

    function claim() external {
        require(
            msg.sender == contributions[msg.sender].buyer,
            "No contributions found!"
        );
        require(
            block.timestamp > contributions[msg.sender].nextClaim,
            "Not time for next vesting"
        );

        require(
            amountDuePackages(msg.sender) > 0,
            "No contributions found!"
        );

        require(
            remainderClaimPackages() > 0,
            "You are not due to collect anymore. You were likely fully vested upon purchase."
        );

        for (uint256 i = 0; i < contributions[msg.sender].packages.length; i++) {
            contributions[msg.sender].packages[i] = claimPackage(contributions[msg.sender].packages[i]);
        }

        if (remainderClaimPackages() > 0)
            contributions[msg.sender].nextClaim = block.timestamp + 2592000; //2592000;
    }

    function claimPackage(Package memory pak) private returns (Package memory){

        if (pak.timesClaimed >= vestingPeriods)
            return pak;

        if (pak.timesClaimed == (vestingPeriods - 1)) {
            uint256 remainder = pak.amountDue - pak.amountClaimed;
            require(
                token.balanceOf(address(this)) >= remainder,
                "Insufficient balance of InfinityBee held in contract to complete order"
            );

            token.transfer(msg.sender, remainder * 10 ** token.decimals());

            pak.amountClaimed += remainder;
            pak.timesClaimed++;
        } else if ((pak.amountDue - pak.amountClaimed) < (pak.amountDue * vestingPercent)/1000) {
            uint256 remainder = pak.amountDue - pak.amountClaimed;
             require(
                token.balanceOf(address(this)) >= remainder,
                "Insufficient balance of InfinityBee held in contract to complete order"
            );

            token.transfer(msg.sender, remainder * 10 ** token.decimals());

            pak.amountClaimed += remainder;
            pak.timesClaimed++;
        }  
        else {
            uint256 amountReceived = (pak.amountDue * vestingPercent) / 1000;
            require(
                token.balanceOf(address(this)) >= amountReceived * 10 ** token.decimals(),
                "Insufficient balance of InfinityBee held in contract to complete order"
            );

            token.transfer(msg.sender, amountReceived * 10 ** token.decimals());

            pak.amountClaimed += amountReceived;
            pak.timesClaimed++;
        }

        return pak;
    }

    function amountDuePackages(address _addr) private view returns (uint256){
        uint256 amountDue = 0;

        for (uint256 i = 0; i < contributions[_addr].packages.length; i++) {
            amountDue += contributions[_addr].packages[i].amountDue;
        }

        return amountDue;
    }

    function amountClaimedPackages(address _addr) private view returns (uint256){
        uint256 amountClaimed = 0;

        for (uint256 i = 0; i < contributions[_addr].packages.length; i++) {
            amountClaimed += contributions[_addr].packages[i].amountClaimed;
        }

        return amountClaimed;
    }

    function amountToClaimPackages(address _addr) private view returns (uint256){

        uint256 amountClaim = 0;

        for (uint256 i = 0; i < contributions[_addr].packages.length; i++) {
            
            Package memory pak = contributions[_addr].packages[i];

            if (pak.timesClaimed >= vestingPeriods)
                continue;

            if (pak.timesClaimed == (vestingPeriods - 1)) {
                amountClaim += pak.amountDue - pak.amountClaimed;

            } else if ((pak.amountDue - pak.amountClaimed) < (pak.amountDue * vestingPercent)/1000) {
                amountClaim += pak.amountDue - pak.amountClaimed;
            }  
            else {
                amountClaim += (pak.amountDue * vestingPercent) / 1000;
            }
        }

        return amountClaim;
    }

    function remainderClaimPackages() private view returns (uint256){
        uint256 remainder = 0;

        for (uint256 i = 0; i < contributions[msg.sender].packages.length; i++) {
            if (contributions[msg.sender].packages[i].timesClaimed < vestingPeriods)
                remainder += contributions[msg.sender].packages[i].amountDue - 
                    contributions[msg.sender].packages[i].amountClaimed;
        }

        return remainder;
    }

    function nextRound(uint256 _endTime) external onlyOwner {
        if (current_round >= 3) {
            revert("No more rounds!");
        } else {
            ++current_round;
            endTime = _endTime;
            if (current_round == 1) {
                tokenPrice = .01 ether;
            } else if (current_round == 2) {
                tokenPrice = .015 ether;
            } else {
                tokenPrice = .02 ether;
            }
        }
    }

    function getDecimals() public view returns (uint256, uint256) {
        return (token.decimals(), fiat.decimals());
    }

    function setVesting(uint8 _newPeriod, uint8 _newPercent) external onlyOwner
    {
        vestingPeriods = _newPeriod;
        vestingPercent = _newPercent;
    }

    function setTokenPrice(uint256 _newPrice) external onlyOwner {
        tokenPrice = _newPrice;
    }

    function transferOwnership(address payable _newOwner) external onlyOwner {
        owner = _newOwner;
    }

    function withdrawTokens() external onlyOwner {
        uint256 balance = token.balanceOf(address(this));
        token.transfer(msg.sender, balance);
    }

    function withdrawFiat() external onlyOwner {
        uint256 balance = fiat.balanceOf(address(this));
        fiat.transfer(msg.sender, balance);
    }

    function changeVesting(uint256 _newTime) external onlyOwner {
	    endTime = _newTime;
    }
}