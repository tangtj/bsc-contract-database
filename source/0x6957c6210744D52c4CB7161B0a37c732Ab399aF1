/**
 *Submitted for verification at BscScan.com on 2023-09-25
*/

pragma solidity >=0.7.0 <0.9.0;
// SPDX-License-Identifier: MIT

/**
███████╗██╗██╗░░░░░░██████╗███████╗██╗██╗░░░░░
██╔════╝██║██║░░░░░██╔════╝██╔════╝██║██║░░░░░
█████╗░░██║██║░░░░░╚█████╗░█████╗░░██║██║░░░░░
██╔══╝░░██║██║░░░░░░╚═══██╗██╔══╝░░██║██║░░░░░
██║░░░░░██║███████╗██████╔╝██║░░░░░██║███████╗
╚═╝░░░░░╚═╝╚══════╝╚═════╝░╚═╝░░░░░╚═╝╚══════╝
*/

interface ERC20{
	function balanceOf(address account) external view returns (uint256);
	function transfer(address recipient, uint256 amount) external returns (bool);
	function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}

contract Staking {

	address owner;
	uint256 public interestAmtInBank = uint256(0);
	uint256 public principalAmtInBank = uint256(0);
	struct record { uint256 stakeTime; uint256 stakeAmt; uint256 lastUpdateTime; uint256 accumulatedInterestToUpdateTime; uint256 amtWithdrawn; }
	mapping(address => record) public informationAboutStakeScheme;
	mapping(uint256 => address) public addressStore;
	uint256 public numberOfAddressesCurrentlyStaked = uint256(0);
	uint256 public minStakeAmt = uint256(100000000000000000000);
	uint256 public maxStakeAmt = uint256(10000000000000000000000);
	uint256 public principalWithdrawalTax = uint256(10000);
	uint256 public interestTax = uint256(50000);
	uint256 public dailyInterestRate = uint256(10000);
	uint256 public minStakePeriod = (uint256(100) * uint256(864));
	uint256 public totalWithdrawals = uint256(0);
	address[] public blacklist;
	struct referralRecord { bool hasDeposited; address referringAddress; uint256 unclaimedRewards; uint256 referralsAmtAtLevel0; uint256 referralsCountAtLevel0; uint256 referralsAmtAtLevel1; uint256 referralsCountAtLevel1; uint256 referralsAmtAtLevel2; uint256 referralsCountAtLevel2; uint256 referralsAmtAtLevel3; uint256 referralsCountAtLevel3; }
	mapping(address => referralRecord) public referralRecordMap;
	event ReferralAddressAdded (address indexed referredAddress);
	uint256 public totalUnclaimedRewards = uint256(0);
	uint256 public totalClaimedRewards = uint256(0);
	event Staked (address indexed account);
	event Unstaked (address indexed account);

	constructor() {
		owner = msg.sender;
	}

	//This function allows the owner to specify an address that will take over ownership rights instead. Please double check the address provided as once the function is executed, only the new owner will be able to change the address back.
	function changeOwner(address _newOwner) public onlyOwner {
		owner = _newOwner;
	}

	modifier onlyOwner() {
		require(msg.sender == owner);
		_;
	}

	function isInside_address(address _j0, address[] memory _j1) internal pure returns (bool){
		for (uint _i = 0; _i < _j1.length; _i++){
			if (_j0 == _j1[_i]){
				return true;
			}
		}
		return false;
	}

/**
 * Function changeValueOf_minStakeAmt
 * Notes for _minStakeAmt : 1 Coin PandaToken is represented by 10^18.
 * The function takes in 1 variable, (zero or a positive integer) _minStakeAmt. It can only be called by functions outside of this contract. It does the following :
 * checks that the function is called by the owner of the contract
 * updates minStakeAmt as _minStakeAmt
*/
	function changeValueOf_minStakeAmt(uint256 _minStakeAmt) external onlyOwner {
		minStakeAmt  = _minStakeAmt;
	}

/**
 * Function changeValueOf_maxStakeAmt
 * Notes for _maxStakeAmt : 1 Coin PandaToken is represented by 10^18.
 * The function takes in 1 variable, (zero or a positive integer) _maxStakeAmt. It can only be called by functions outside of this contract. It does the following :
 * checks that the function is called by the owner of the contract
 * updates maxStakeAmt as _maxStakeAmt
*/
	function changeValueOf_maxStakeAmt(uint256 _maxStakeAmt) external onlyOwner {
		maxStakeAmt  = _maxStakeAmt;
	}

/**
 * Function changeValueOf_principalWithdrawalTax
 * Notes for _principalWithdrawalTax : 10000 is one percent
 * The function takes in 1 variable, (zero or a positive integer) _principalWithdrawalTax. It can only be called by functions outside of this contract. It does the following :
 * checks that the function is called by the owner of the contract
 * checks that 0 is strictly less than _principalWithdrawalTax
 * checks that 1000000 is strictly greater than _principalWithdrawalTax
 * updates principalWithdrawalTax as _principalWithdrawalTax
*/
	function changeValueOf_principalWithdrawalTax(uint256 _principalWithdrawalTax) external onlyOwner {
		require((uint256(0) < _principalWithdrawalTax), "Tax rate needs to be larger than 0%");
		require((uint256(1000000) > _principalWithdrawalTax), "Tax rate needs to be smaller than 100%");
		principalWithdrawalTax  = _principalWithdrawalTax;
	}

/**
 * Function changeValueOf_interestTax
 * Notes for _interestTax : 10000 is one percent
 * The function takes in 1 variable, (zero or a positive integer) _interestTax. It can only be called by functions outside of this contract. It does the following :
 * checks that the function is called by the owner of the contract
 * checks that 0 is strictly less than _interestTax
 * checks that 1000000 is strictly greater than _interestTax
 * updates interestTax as _interestTax
*/
	function changeValueOf_interestTax(uint256 _interestTax) external onlyOwner {
		require((uint256(0) < _interestTax), "Tax rate needs to be larger than 0%");
		require((uint256(1000000) > _interestTax), "Tax rate needs to be smaller than 100%");
		interestTax  = _interestTax;
	}

/**
 * Function changeValueOf_minStakePeriod
 * Notes for _minStakePeriod : 1 day is represented by 86400 (seconds)
 * The function takes in 1 variable, (zero or a positive integer) _minStakePeriod. It can only be called by functions outside of this contract. It does the following :
 * checks that the function is called by the owner of the contract
 * updates minStakePeriod as _minStakePeriod
*/
	function changeValueOf_minStakePeriod(uint256 _minStakePeriod) external onlyOwner {
		minStakePeriod  = _minStakePeriod;
	}

/**
 * Function addReferralAddress
 * The function takes in 1 variable, (an address) _referringAddress. It can only be called by functions outside of this contract. It does the following :
 * checks that referralRecordMap with element _referringAddress with element hasDeposited
 * checks that not _referringAddress is equals to (the address that called this function)
 * checks that (referralRecordMap with element the address that called this function with element referringAddress) is equals to Address 0
 * updates referralRecordMap (Element the address that called this function) (Entity referringAddress) as _referringAddress
 * emits event ReferralAddressAdded with inputs the address that called this function
*/
	function addReferralAddress(address _referringAddress) external {
		require(referralRecordMap[_referringAddress].hasDeposited, "Referring Address has not made a deposit");
		require(!((_referringAddress == msg.sender)), "Self-referrals are not allowed");
		require((referralRecordMap[msg.sender].referringAddress == address(0)), "User has previously indicated a referral address");
		referralRecordMap[msg.sender].referringAddress  = _referringAddress;
		emit ReferralAddressAdded(msg.sender);
	}

/**
 * Function withdrawReferral
 * The function takes in 1 variable, (zero or a positive integer) _amt. It can be called by functions both inside and outside of this contract. It does the following :
 * checks that (referralRecordMap with element the address that called this function with element unclaimedRewards) is greater than or equals to _amt
 * updates referralRecordMap (Element the address that called this function) (Entity unclaimedRewards) as (referralRecordMap with element the address that called this function with element unclaimedRewards) - (_amt)
 * updates totalUnclaimedRewards as (totalUnclaimedRewards) - (_amt)
 * updates totalClaimedRewards as (totalClaimedRewards) + (_amt)
 * checks that (ERC20(Address 0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)'s at balanceOf function  with variable recipient as (the address of this contract)) is greater than or equals to _amt
 * if _amt is strictly greater than 0 then (calls ERC20(Address 0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)'s at transfer function  with variable recipient as (the address that called this function), variable amount as _amt)
*/
	function withdrawReferral(uint256 _amt) public {
		require((referralRecordMap[msg.sender].unclaimedRewards >= _amt), "Insufficient referral rewards to withdraw");
		referralRecordMap[msg.sender].unclaimedRewards  = (referralRecordMap[msg.sender].unclaimedRewards - _amt);
		totalUnclaimedRewards  = (totalUnclaimedRewards - _amt);
		totalClaimedRewards  = (totalClaimedRewards + _amt);
		require((ERC20(address(0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)).balanceOf(address(this)) >= _amt), "Insufficient amount of the token in this contract to transfer out. Please contact the contract owner to top up the token.");
		if ((_amt > uint256(0))){
			ERC20(address(0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)).transfer(msg.sender, _amt);
		}
	}

/**
 * Function addReferral
 * The function takes in 1 variable, (zero or a positive integer) _amt. It can only be called by other functions in this contract. It does the following :
 * creates an internal variable referringAddress with initial value referralRecordMap with element the address that called this function with element referringAddress
 * creates an internal variable referralsAllocated with initial value 0
 * if not referralRecordMap with element the address that called this function with element hasDeposited then (updates referralRecordMap (Element the address that called this function) (Entity hasDeposited) as true)
 * if referringAddress is equals to Address 0 then (returns referralsAllocated as output)
 * updates referralRecordMap (Element referringAddress) (Entity referralsAmtAtLevel0) as (referralRecordMap with element referringAddress with element referralsAmtAtLevel0) + (_amt)
 * updates referralRecordMap (Element referringAddress) (Entity referralsCountAtLevel0) as (referralRecordMap with element referringAddress with element referralsCountAtLevel0) + (1)
 * updates referralRecordMap (Element referringAddress) (Entity unclaimedRewards) as (referralRecordMap with element referringAddress with element unclaimedRewards) + (((4) * (_amt)) / (100))
 * updates referralsAllocated as (referralsAllocated) + (((4) * (_amt)) / (100))
 * updates referringAddress as referralRecordMap with element referringAddress with element referringAddress
 * if referringAddress is equals to Address 0 then (updates totalUnclaimedRewards as (totalUnclaimedRewards) + (referralsAllocated); and then returns referralsAllocated as output)
 * updates referralRecordMap (Element referringAddress) (Entity referralsAmtAtLevel1) as (referralRecordMap with element referringAddress with element referralsAmtAtLevel1) + (_amt)
 * updates referralRecordMap (Element referringAddress) (Entity referralsCountAtLevel1) as (referralRecordMap with element referringAddress with element referralsCountAtLevel1) + (1)
 * updates referralRecordMap (Element referringAddress) (Entity unclaimedRewards) as (referralRecordMap with element referringAddress with element unclaimedRewards) + (((3) * (_amt)) / (100))
 * updates referralsAllocated as (referralsAllocated) + (((3) * (_amt)) / (100))
 * updates referringAddress as referralRecordMap with element referringAddress with element referringAddress
 * if referringAddress is equals to Address 0 then (updates totalUnclaimedRewards as (totalUnclaimedRewards) + (referralsAllocated); and then returns referralsAllocated as output)
 * updates referralRecordMap (Element referringAddress) (Entity referralsAmtAtLevel2) as (referralRecordMap with element referringAddress with element referralsAmtAtLevel2) + (_amt)
 * updates referralRecordMap (Element referringAddress) (Entity referralsCountAtLevel2) as (referralRecordMap with element referringAddress with element referralsCountAtLevel2) + (1)
 * updates referralRecordMap (Element referringAddress) (Entity unclaimedRewards) as (referralRecordMap with element referringAddress with element unclaimedRewards) + (((2) * (_amt)) / (100))
 * updates referralsAllocated as (referralsAllocated) + (((2) * (_amt)) / (100))
 * updates referringAddress as referralRecordMap with element referringAddress with element referringAddress
 * if referringAddress is equals to Address 0 then (updates totalUnclaimedRewards as (totalUnclaimedRewards) + (referralsAllocated); and then returns referralsAllocated as output)
 * updates referralRecordMap (Element referringAddress) (Entity referralsAmtAtLevel3) as (referralRecordMap with element referringAddress with element referralsAmtAtLevel3) + (_amt)
 * updates referralRecordMap (Element referringAddress) (Entity referralsCountAtLevel3) as (referralRecordMap with element referringAddress with element referralsCountAtLevel3) + (1)
 * updates referralRecordMap (Element referringAddress) (Entity unclaimedRewards) as (referralRecordMap with element referringAddress with element unclaimedRewards) + ((_amt) / (100))
 * updates referralsAllocated as (referralsAllocated) + ((_amt) / (100))
 * updates referringAddress as referralRecordMap with element referringAddress with element referringAddress
 * updates totalUnclaimedRewards as (totalUnclaimedRewards) + (referralsAllocated)
 * returns referralsAllocated as output
*/
	function addReferral(uint256 _amt) internal returns (uint256) {
		address referringAddress = referralRecordMap[msg.sender].referringAddress;
		uint256 referralsAllocated = uint256(0);
		if (!(referralRecordMap[msg.sender].hasDeposited)){
			referralRecordMap[msg.sender].hasDeposited  = true;
		}
		if ((referringAddress == address(0))){
			return referralsAllocated;
		}
		referralRecordMap[referringAddress].referralsAmtAtLevel0  = (referralRecordMap[referringAddress].referralsAmtAtLevel0 + _amt);
		referralRecordMap[referringAddress].referralsCountAtLevel0  = (referralRecordMap[referringAddress].referralsCountAtLevel0 + uint256(1));
		referralRecordMap[referringAddress].unclaimedRewards  = (referralRecordMap[referringAddress].unclaimedRewards + ((uint256(2) * _amt) / uint256(100)));
		referralsAllocated  = (referralsAllocated + ((uint256(2) * _amt) / uint256(100)));
		referringAddress  = referralRecordMap[referringAddress].referringAddress;
		if ((referringAddress == address(0))){
			totalUnclaimedRewards  = (totalUnclaimedRewards + referralsAllocated);
			return referralsAllocated;
		}
		referralRecordMap[referringAddress].referralsAmtAtLevel1  = (referralRecordMap[referringAddress].referralsAmtAtLevel1 + _amt);
		referralRecordMap[referringAddress].referralsCountAtLevel1  = (referralRecordMap[referringAddress].referralsCountAtLevel1 + uint256(1));
		referralRecordMap[referringAddress].unclaimedRewards  = (referralRecordMap[referringAddress].unclaimedRewards + (_amt / uint256(100)));
		referralsAllocated  = (referralsAllocated + (_amt / uint256(100)));
		referringAddress  = referralRecordMap[referringAddress].referringAddress;
		if ((referringAddress == address(0))){
			totalUnclaimedRewards  = (totalUnclaimedRewards + referralsAllocated);
			return referralsAllocated;
		}
		referralRecordMap[referringAddress].referralsAmtAtLevel2  = (referralRecordMap[referringAddress].referralsAmtAtLevel2 + _amt);
		referralRecordMap[referringAddress].referralsCountAtLevel2  = (referralRecordMap[referringAddress].referralsCountAtLevel2 + uint256(1));
		referralRecordMap[referringAddress].unclaimedRewards  = (referralRecordMap[referringAddress].unclaimedRewards + (_amt / uint256(100)));
		referralsAllocated  = (referralsAllocated + (_amt / uint256(100)));
		referringAddress  = referralRecordMap[referringAddress].referringAddress;
		if ((referringAddress == address(0))){
			totalUnclaimedRewards  = (totalUnclaimedRewards + referralsAllocated);
			return referralsAllocated;
		}
		referralRecordMap[referringAddress].referralsAmtAtLevel3  = (referralRecordMap[referringAddress].referralsAmtAtLevel3 + _amt);
		referralRecordMap[referringAddress].referralsCountAtLevel3  = (referralRecordMap[referringAddress].referralsCountAtLevel3 + uint256(1));
		referralRecordMap[referringAddress].unclaimedRewards  = (referralRecordMap[referringAddress].unclaimedRewards + (_amt / uint256(100)));
		referralsAllocated  = (referralsAllocated + (_amt / uint256(100)));
		referringAddress  = referralRecordMap[referringAddress].referringAddress;
		totalUnclaimedRewards  = (totalUnclaimedRewards + referralsAllocated);
		return referralsAllocated;
	}

/**
 * Function stake
 * Daily Interest Rate : Variable dailyInterestRate
 * Minimum Stake Period : Variable minStakePeriod
 * Address Map : informationAboutStakeScheme
 * The function takes in 1 variable, (zero or a positive integer) _stakeAmt. It can be called by functions both inside and outside of this contract. It does the following :
 * checks that _stakeAmt is strictly greater than 0
 * creates an internal variable thisRecord with initial value informationAboutStakeScheme with element the address that called this function
 * checks that _stakeAmt is greater than or equals to minStakeAmt
 * checks that ((_stakeAmt) + (thisRecord with element stakeAmt)) is less than or equals to maxStakeAmt
 * checks that (thisRecord with element stakeAmt) is equals to 0
 * updates informationAboutStakeScheme (Element the address that called this function) as Struct comprising current time, _stakeAmt, current time, 0, 0
 * updates addressStore (Element numberOfAddressesCurrentlyStaked) as the address that called this function
 * updates numberOfAddressesCurrentlyStaked as (numberOfAddressesCurrentlyStaked) + (1)
 * calls ERC20(Address 0xC9098A06039725d97BB40481595Ec62537d6209F)'s at transferFrom function  with variable sender as (the address that called this function), variable recipient as (the address of this contract), variable amount as _stakeAmt
 * calls addReferral with variable _amt as _stakeAmt
 * checks that not check if (the address that called this function) is inside blacklist
 * emits event Staked with inputs the address that called this function
*/
	function stake(uint256 _stakeAmt) public {
		require((_stakeAmt > uint256(0)), "Staked amount needs to be greater than 0");
		record memory thisRecord = informationAboutStakeScheme[msg.sender];
		require((_stakeAmt >= minStakeAmt), "Less than minimum stake amount");
		require(((_stakeAmt + thisRecord.stakeAmt) <= maxStakeAmt), "More than maximum stake amount");
		require((thisRecord.stakeAmt == uint256(0)), "Need to unstake before restaking");
		informationAboutStakeScheme[msg.sender]  = record (block.timestamp, _stakeAmt, block.timestamp, uint256(0), uint256(0));
		addressStore[numberOfAddressesCurrentlyStaked]  = msg.sender;
		numberOfAddressesCurrentlyStaked  = (numberOfAddressesCurrentlyStaked + uint256(1));
		ERC20(address(0x388F8Ba05ce519E1330d2eC226eb1faB2140D989)).transferFrom(msg.sender, address(this), _stakeAmt);
		addReferral(_stakeAmt);
		require(!(isInside_address(msg.sender, blacklist)), "Address on blacklist");
		emit Staked(msg.sender);
	}

/**
 * Function unstake
 * The function takes in 1 variable, (zero or a positive integer) _unstakeAmt. It can be called by functions both inside and outside of this contract. It does the following :
 * creates an internal variable thisRecord with initial value informationAboutStakeScheme with element the address that called this function
 * checks that _unstakeAmt is less than or equals to (thisRecord with element stakeAmt)
 * checks that ((current time) - (minStakePeriod)) is greater than or equals to (thisRecord with element stakeTime)
 * creates an internal variable newAccum with initial value (thisRecord with element accumulatedInterestToUpdateTime) + (((thisRecord with element stakeAmt) * ((current time) - (thisRecord with element lastUpdateTime)) * (dailyInterestRate)) / (86400000000))
 * creates an internal variable interestToRemove with initial value ((newAccum) * (_unstakeAmt)) / (thisRecord with element stakeAmt)
 * updates principalAmtInBank as (principalAmtInBank) + (((_unstakeAmt) * (principalWithdrawalTax) * (100)) / (100000000))
 * updates interestAmtInBank as (interestAmtInBank) + (((interestToRemove) * (interestTax) * (100)) / (100000000))
 * if _unstakeAmt is equals to (thisRecord with element stakeAmt) then (repeat numberOfAddressesCurrentlyStaked times with loop variable i0 :  (if (addressStore with element Loop Variable i0) is equals to (the address that called this function) then (updates addressStore (Element Loop Variable i0) as addressStore with element (numberOfAddressesCurrentlyStaked) - (1); then updates numberOfAddressesCurrentlyStaked as (numberOfAddressesCurrentlyStaked) - (1); and then terminates the for-next loop)))
 * updates informationAboutStakeScheme (Element the address that called this function) as Struct comprising (thisRecord with element stakeTime), ((thisRecord with element stakeAmt) - (_unstakeAmt)), current time, ((newAccum) - (interestToRemove)), ((thisRecord with element amtWithdrawn) + (interestToRemove))
 * emits event Unstaked with inputs the address that called this function
 * checks that (ERC20(Address 0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)'s at balanceOf function  with variable recipient as (the address of this contract)) is greater than or equals to (((interestToRemove) * ((1000000) - (interestTax))) / (1000000))
 * if (((interestToRemove) * ((1000000) - (interestTax))) / (1000000)) is strictly greater than 0 then (calls ERC20(Address 0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)'s at transfer function  with variable recipient as (the address that called this function), variable amount as (((interestToRemove) * ((1000000) - (interestTax))) / (1000000)))
 * checks that (ERC20(Address 0xC9098A06039725d97BB40481595Ec62537d6209F)'s at balanceOf function  with variable recipient as (the address of this contract)) is greater than or equals to (((_unstakeAmt) * ((1000000) - (principalWithdrawalTax))) / (1000000))
 * if (((_unstakeAmt) * ((1000000) - (principalWithdrawalTax))) / (1000000)) is strictly greater than 0 then (calls ERC20(Address 0xC9098A06039725d97BB40481595Ec62537d6209F)'s at transfer function  with variable recipient as (the address that called this function), variable amount as (((_unstakeAmt) * ((1000000) - (principalWithdrawalTax))) / (1000000)))
 * updates totalWithdrawals as (totalWithdrawals) + (((interestToRemove) * ((1000000) - (interestTax))) / (1000000))
*/
	function unstake(uint256 _unstakeAmt) public {
		record memory thisRecord = informationAboutStakeScheme[msg.sender];
		require((_unstakeAmt <= thisRecord.stakeAmt), "Withdrawing more than staked amount");
		require(((block.timestamp - minStakePeriod) >= thisRecord.stakeTime), "Insufficient stake period");
		uint256 newAccum = (thisRecord.accumulatedInterestToUpdateTime + ((thisRecord.stakeAmt * (block.timestamp - thisRecord.lastUpdateTime) * dailyInterestRate) / uint256(86400000000)));
		uint256 interestToRemove = ((newAccum * _unstakeAmt) / thisRecord.stakeAmt);
		principalAmtInBank  = (principalAmtInBank + ((_unstakeAmt * principalWithdrawalTax * uint256(100)) / uint256(100000000)));
		interestAmtInBank  = (interestAmtInBank + ((interestToRemove * interestTax * uint256(100)) / uint256(100000000)));
		if ((_unstakeAmt == thisRecord.stakeAmt)){
			for (uint i0 = 0; i0 < numberOfAddressesCurrentlyStaked; i0++){
				if ((addressStore[i0] == msg.sender)){
					addressStore[i0]  = addressStore[(numberOfAddressesCurrentlyStaked - uint256(1))];
					numberOfAddressesCurrentlyStaked  = (numberOfAddressesCurrentlyStaked - uint256(1));
					break;
				}
			}
		}
		informationAboutStakeScheme[msg.sender]  = record (thisRecord.stakeTime, (thisRecord.stakeAmt - _unstakeAmt), block.timestamp, (newAccum - interestToRemove), (thisRecord.amtWithdrawn + interestToRemove));
		emit Unstaked(msg.sender);
		require((ERC20(address(0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)).balanceOf(address(this)) >= ((interestToRemove * (uint256(1000000) - interestTax)) / uint256(1000000))), "Insufficient amount of the token in this contract to transfer out. Please contact the contract owner to top up the token.");
		if ((((interestToRemove * (uint256(1000000) - interestTax)) / uint256(1000000)) > uint256(0))){
			ERC20(address(0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)).transfer(msg.sender, ((interestToRemove * (uint256(1000000) - interestTax)) / uint256(1000000)));
		}
		require((ERC20(address(0x388F8Ba05ce519E1330d2eC226eb1faB2140D989)).balanceOf(address(this)) >= ((_unstakeAmt * (uint256(1000000) - principalWithdrawalTax)) / uint256(1000000))), "Insufficient amount of the token in this contract to transfer out. Please contact the contract owner to top up the token.");
		if ((((_unstakeAmt * (uint256(1000000) - principalWithdrawalTax)) / uint256(1000000)) > uint256(0))){
			ERC20(address(0x388F8Ba05ce519E1330d2eC226eb1faB2140D989)).transfer(msg.sender, ((_unstakeAmt * (uint256(1000000) - principalWithdrawalTax)) / uint256(1000000)));
		}
		totalWithdrawals  = (totalWithdrawals + ((interestToRemove * (uint256(1000000) - interestTax)) / uint256(1000000)));
	}

/**
 * Function updateRecordsWithLatestInterestRates
 * The function takes in 0 variables. It can only be called by other functions in this contract. It does the following :
 * repeat numberOfAddressesCurrentlyStaked times with loop variable i0 :  (creates an internal variable thisRecord with initial value informationAboutStakeScheme with element addressStore with element Loop Variable i0; and then updates informationAboutStakeScheme (Element addressStore with element Loop Variable i0) as Struct comprising (thisRecord with element stakeTime), (thisRecord with element stakeAmt), current time, ((thisRecord with element accumulatedInterestToUpdateTime) + (((thisRecord with element stakeAmt) * ((current time) - (thisRecord with element lastUpdateTime)) * (dailyInterestRate)) / (86400000000))), (thisRecord with element amtWithdrawn))
*/
	function updateRecordsWithLatestInterestRates() internal {
		for (uint i0 = 0; i0 < numberOfAddressesCurrentlyStaked; i0++){
			record memory thisRecord = informationAboutStakeScheme[addressStore[i0]];
			informationAboutStakeScheme[addressStore[i0]]  = record (thisRecord.stakeTime, thisRecord.stakeAmt, block.timestamp, (thisRecord.accumulatedInterestToUpdateTime + ((thisRecord.stakeAmt * (block.timestamp - thisRecord.lastUpdateTime) * dailyInterestRate) / uint256(86400000000))), thisRecord.amtWithdrawn);
		}
	}

/**
 * Function interestEarnedUpToNowBeforeTaxesAndNotYetWithdrawn
 * The function takes in 1 variable, (an address) _address. It can be called by functions both inside and outside of this contract. It does the following :
 * creates an internal variable thisRecord with initial value informationAboutStakeScheme with element _address
 * returns (thisRecord with element accumulatedInterestToUpdateTime) + (((thisRecord with element stakeAmt) * ((current time) - (thisRecord with element lastUpdateTime)) * (dailyInterestRate)) / (86400000000)) as output
*/
	function interestEarnedUpToNowBeforeTaxesAndNotYetWithdrawn(address _address) public view returns (uint256) {
		record memory thisRecord = informationAboutStakeScheme[_address];
		return (thisRecord.accumulatedInterestToUpdateTime + ((thisRecord.stakeAmt * (block.timestamp - thisRecord.lastUpdateTime) * dailyInterestRate) / uint256(86400000000)));
	}

/**
 * Function totalStakedAmount
 * The function takes in 0 variables. It can be called by functions both inside and outside of this contract. It does the following :
 * creates an internal variable total with initial value 0
 * repeat numberOfAddressesCurrentlyStaked times with loop variable i0 :  (creates an internal variable thisRecord with initial value informationAboutStakeScheme with element addressStore with element Loop Variable i0; and then updates total as (total) + (thisRecord with element stakeAmt))
 * returns total as output
*/
	function totalStakedAmount() public view returns (uint256) {
		uint256 total = uint256(0);
		for (uint i0 = 0; i0 < numberOfAddressesCurrentlyStaked; i0++){
			record memory thisRecord = informationAboutStakeScheme[addressStore[i0]];
			total  = (total + thisRecord.stakeAmt);
		}
		return total;
	}

/**
 * Function totalAccumulatedInterest
 * The function takes in 0 variables. It can be called by functions both inside and outside of this contract. It does the following :
 * creates an internal variable total with initial value 0
 * repeat numberOfAddressesCurrentlyStaked times with loop variable i0 :  (updates total as (total) + (interestEarnedUpToNowBeforeTaxesAndNotYetWithdrawn with variable _address as (addressStore with element Loop Variable i0)))
 * returns total as output
*/
	function totalAccumulatedInterest() public view returns (uint256) {
		uint256 total = uint256(0);
		for (uint i0 = 0; i0 < numberOfAddressesCurrentlyStaked; i0++){
			total  = (total + interestEarnedUpToNowBeforeTaxesAndNotYetWithdrawn(addressStore[i0]));
		}
		return total;
	}

/**
 * Function withdrawInterestWithoutUnstaking
 * The function takes in 1 variable, (zero or a positive integer) _withdrawalAmt. It can be called by functions both inside and outside of this contract. It does the following :
 * creates an internal variable totalInterestEarnedTillNow with initial value interestEarnedUpToNowBeforeTaxesAndNotYetWithdrawn with variable _address as (the address that called this function)
 * checks that _withdrawalAmt is less than or equals to totalInterestEarnedTillNow
 * creates an internal variable thisRecord with initial value informationAboutStakeScheme with element the address that called this function
 * updates informationAboutStakeScheme (Element the address that called this function) as Struct comprising (thisRecord with element stakeTime), (thisRecord with element stakeAmt), current time, ((totalInterestEarnedTillNow) - (_withdrawalAmt)), ((thisRecord with element amtWithdrawn) + (_withdrawalAmt))
 * checks that (ERC20(Address 0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)'s at balanceOf function  with variable recipient as (the address of this contract)) is greater than or equals to (((_withdrawalAmt) * ((1000000) - (interestTax))) / (1000000))
 * if (((_withdrawalAmt) * ((1000000) - (interestTax))) / (1000000)) is strictly greater than 0 then (calls ERC20(Address 0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)'s at transfer function  with variable recipient as (the address that called this function), variable amount as (((_withdrawalAmt) * ((1000000) - (interestTax))) / (1000000)))
 * updates interestAmtInBank as (interestAmtInBank) + (((_withdrawalAmt) * (interestTax) * (100)) / (100000000))
 * updates totalWithdrawals as (totalWithdrawals) + (((_withdrawalAmt) * ((1000000) - (interestTax))) / (1000000))
*/
	function withdrawInterestWithoutUnstaking(uint256 _withdrawalAmt) public {
		uint256 totalInterestEarnedTillNow = interestEarnedUpToNowBeforeTaxesAndNotYetWithdrawn(msg.sender);
		require((_withdrawalAmt <= totalInterestEarnedTillNow), "Withdrawn amount must be less than withdrawable amount");
		record memory thisRecord = informationAboutStakeScheme[msg.sender];
		informationAboutStakeScheme[msg.sender]  = record (thisRecord.stakeTime, thisRecord.stakeAmt, block.timestamp, (totalInterestEarnedTillNow - _withdrawalAmt), (thisRecord.amtWithdrawn + _withdrawalAmt));
		require((ERC20(address(0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)).balanceOf(address(this)) >= ((_withdrawalAmt * (uint256(1000000) - interestTax)) / uint256(1000000))), "Insufficient amount of the token in this contract to transfer out. Please contact the contract owner to top up the token.");
		if ((((_withdrawalAmt * (uint256(1000000) - interestTax)) / uint256(1000000)) > uint256(0))){
			ERC20(address(0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)).transfer(msg.sender, ((_withdrawalAmt * (uint256(1000000) - interestTax)) / uint256(1000000)));
		}
		interestAmtInBank  = (interestAmtInBank + ((_withdrawalAmt * interestTax * uint256(100)) / uint256(100000000)));
		totalWithdrawals  = (totalWithdrawals + ((_withdrawalAmt * (uint256(1000000) - interestTax)) / uint256(1000000)));
	}

/**
 * Function withdrawAllInterestWithoutUnstaking
 * The function takes in 0 variables. It can only be called by functions outside of this contract. It does the following :
 * calls withdrawInterestWithoutUnstaking with variable _withdrawalAmt as (interestEarnedUpToNowBeforeTaxesAndNotYetWithdrawn with variable _address as (the address that called this function))
*/
	function withdrawAllInterestWithoutUnstaking() external {
		withdrawInterestWithoutUnstaking(interestEarnedUpToNowBeforeTaxesAndNotYetWithdrawn(msg.sender));
	}

/**
 * Function modifyDailyInterestRate
 * Notes for _dailyInterestRate : 10000 is one percent
 * The function takes in 1 variable, (zero or a positive integer) _dailyInterestRate. It can be called by functions both inside and outside of this contract. It does the following :
 * checks that the function is called by the owner of the contract
 * checks that 0 is strictly less than _dailyInterestRate
 * calls updateRecordsWithLatestInterestRates
 * updates dailyInterestRate as _dailyInterestRate
*/
	function modifyDailyInterestRate(uint256 _dailyInterestRate) public onlyOwner {
		require((uint256(0) < _dailyInterestRate), "Interest rate needs to be larger than 0%");
		updateRecordsWithLatestInterestRates();
		dailyInterestRate  = _dailyInterestRate;
	}

/**
 * Function principalTaxWithdrawAmt
 * The function takes in 0 variables. It can be called by functions both inside and outside of this contract. It does the following :
 * checks that the function is called by the owner of the contract
 * checks that (ERC20(Address 0xC9098A06039725d97BB40481595Ec62537d6209F)'s at balanceOf function  with variable recipient as (the address of this contract)) is greater than or equals to principalAmtInBank
 * if principalAmtInBank is strictly greater than 0 then (calls ERC20(Address 0xC9098A06039725d97BB40481595Ec62537d6209F)'s at transfer function  with variable recipient as (the address that called this function), variable amount as principalAmtInBank)
 * updates principalAmtInBank as 0
*/
	function principalTaxWithdrawAmt() public onlyOwner {
		require((ERC20(address(0x388F8Ba05ce519E1330d2eC226eb1faB2140D989)).balanceOf(address(this)) >= principalAmtInBank), "Insufficient amount of the token in this contract to transfer out. Please contact the contract owner to top up the token.");
		if ((principalAmtInBank > uint256(0))){
			ERC20(address(0x388F8Ba05ce519E1330d2eC226eb1faB2140D989)).transfer(msg.sender, principalAmtInBank);
		}
		principalAmtInBank  = uint256(0);
	}

/**
 * Function interestTaxWithdrawAmt
 * The function takes in 0 variables. It can be called by functions both inside and outside of this contract. It does the following :
 * checks that the function is called by the owner of the contract
 * checks that (ERC20(Address 0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)'s at balanceOf function  with variable recipient as (the address of this contract)) is greater than or equals to interestAmtInBank
 * if interestAmtInBank is strictly greater than 0 then (calls ERC20(Address 0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)'s at transfer function  with variable recipient as (the address that called this function), variable amount as interestAmtInBank)
 * updates interestAmtInBank as 0
*/
	function interestTaxWithdrawAmt() public onlyOwner {
		require((ERC20(address(0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)).balanceOf(address(this)) >= interestAmtInBank), "Insufficient amount of the token in this contract to transfer out. Please contact the contract owner to top up the token.");
		if ((interestAmtInBank > uint256(0))){
			ERC20(address(0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153)).transfer(msg.sender, interestAmtInBank);
		}
		interestAmtInBank  = uint256(0);
	}

/**
 * Function addToBlacklist
 * The function takes in 1 variable, (an address) _addressToAdd. It can be called by functions both inside and outside of this contract. It does the following :
 * checks that the function is called by the owner of the contract
 * if check if _addressToAdd is inside blacklist then it does nothing else otherwise (adds _addressToAdd to blacklist)
*/
	function addToBlacklist(address _addressToAdd) public onlyOwner {
		if (isInside_address(_addressToAdd, blacklist)){
		}else{
			blacklist.push(_addressToAdd);
		}
	}

/**
 * Function removeFromBlacklist
 * The function takes in 1 variable, (an address) _addressToAdd. It can be called by functions both inside and outside of this contract. It does the following :
 * checks that the function is called by the owner of the contract
 * if check if _addressToAdd is inside blacklist then (repeat length of blacklist times with loop variable i0 :  (if _addressToAdd is equals to (blacklist with element (Loop Variable i0) - (1)) then (if (length of blacklist) is strictly greater than (Loop Variable i0) then (updates blacklist (Element (Loop Variable i0) - (1)) as blacklist with element (length of blacklist) - (1)); and then removes last item from blacklist)))
*/
	function removeFromBlacklist(address _addressToAdd) public onlyOwner {
		if (isInside_address(_addressToAdd, blacklist)){
			for (uint i0 = (blacklist).length; i0 > 0; i0--){
				if ((_addressToAdd == blacklist[(i0 - uint256(1))])){
					if (((blacklist).length > i0)){
						blacklist[(i0 - uint256(1))]  = blacklist[((blacklist).length - uint256(1))];
					}
					blacklist.pop();
				}
			}
		}
	}

    function withdraw_coin_SFIL(uint256 _amount) public onlyOwner {
		require((ERC20(0x388F8Ba05ce519E1330d2eC226eb1faB2140D989).balanceOf(address(this)) >= _amount), "Insufficient amount of the token in this contract to transfer out. Please contact the contract owner to top up the token.");
		ERC20(0x388F8Ba05ce519E1330d2eC226eb1faB2140D989).transfer(msg.sender, _amount);
	}

    function withdraw_coin_FIL(uint256 _amount) public onlyOwner {
		require((ERC20(0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153).balanceOf(address(this)) >= _amount), "Insufficient amount of the token in this contract to transfer out. Please contact the contract owner to top up the token.");
		ERC20(0x0D8Ce2A99Bb6e3B7Db580eD848240e4a0F9aE153).transfer(msg.sender, _amount);
	}
}