//SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.19;


interface tfIERC20 {

	function totalSupply() external view returns (uint);

	function balanceOf(address account) external view returns (uint);

	function transfer(address recipient, uint amount) external returns (bool);

	function allowance(address owner, address spender) external view returns (uint);

	function approve(address spender, uint amount) external returns (bool);

	function transferFrom(address sender, address recipient, uint amount) external returns (bool);

}

library tfAddress {

	function isContract(address account) internal view returns (bool) {

		uint size;
		assembly {
			size := extcodesize(account)
		}
		return size > 0;
	}

	function sendValue(address payable recipient, uint amount) internal {
		require(address(this).balance >= amount, "Address: insufficient balance");
		(bool success, ) = recipient.call{value: amount}("");
		require(success, "Address: unable to send value, recipient may have reverted");
	}

	function functionCall(address target, bytes memory data) internal returns (bytes memory) {
		return functionCall(target, data, "Address: low-level call failed");
	}

	function functionCall(
		address target,
		bytes memory data,
		string memory errorMessage
	) internal returns (bytes memory) {
		return functionCallWithValue(target, data, 0, errorMessage);
	}

	function functionCallWithValue(
		address target,
		bytes memory data,
		uint value
	) internal returns (bytes memory) {
		return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
	}

	function functionCallWithValue(
		address target,
		bytes memory data,
		uint value,
		string memory errorMessage
	) internal returns (bytes memory) {
		require(address(this).balance >= value, "Address: insufficient balance for call");
		require(isContract(target), "Address: call to non-contract");
		(bool success, bytes memory returndata) = target.call{value: value}(data);
		return verifyCallResult(success, returndata, errorMessage);
	}

	function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
		return functionStaticCall(target, data, "Address: low-level static call failed");
	}

	function functionStaticCall(
		address target,
		bytes memory data,
		string memory errorMessage
	) internal view returns (bytes memory) {
		require(isContract(target), "Address: static call to non-contract");
		(bool success, bytes memory returndata) = target.staticcall(data);
		return verifyCallResult(success, returndata, errorMessage);
	}

	function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
		return functionDelegateCall(target, data, "Address: low-level delegate call failed");
	}

	function functionDelegateCall(
		address target,
		bytes memory data,
		string memory errorMessage
	) internal returns (bytes memory) {
		require(isContract(target), "Address: delegate call to non-contract");
		(bool success, bytes memory returndata) = target.delegatecall(data);
		return verifyCallResult(success, returndata, errorMessage);
	}

	function verifyCallResult(
		bool success,
		bytes memory returndata,
		string memory errorMessage
	) internal pure returns (bytes memory) {
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

library tfSafeERC20 {

	using tfAddress for address;

	function safeTransfer(
		tfIERC20 token,
		address to,
		uint value
	) internal {
		_callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
	}

	function safeTransferFrom(
		tfIERC20 token,
		address from,
		address to,
		uint value
	) internal {
		_callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
	}

	function safeApprove(
		tfIERC20 token,
		address spender,
		uint value
	) internal {
		require(
			(value == 0) || (token.allowance(address(this), spender) == 0),
			"SafeERC20: approve from non-zero to non-zero allowance"
		);
		_callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
	}

	function safeIncreaseAllowance(
		tfIERC20 token,
		address spender,
		uint value
	) internal {
		uint newAllowance = token.allowance(address(this), spender) + value;
		_callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
	}

	function safeDecreaseAllowance(
		tfIERC20 token,
		address spender,
		uint value
	) internal {
		unchecked {
			uint oldAllowance = token.allowance(address(this), spender);
			require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
			uint newAllowance = oldAllowance - value;
			_callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
		}
	}

	function _callOptionalReturn(tfIERC20 token, bytes memory data) private {
		bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
		if (returndata.length > 0) {
			require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
		}
	}
}


interface ITrueFundInsurance {
	function fundProject(uint _amountRequested) external returns(uint _amountSent);
}

contract TRUEFUND_INSURANCE {

	receive() external payable { }

	modifier OnlyProject() { require(msg.sender == PROJECT, 'Only Project' ); _; }

	using tfSafeERC20 for tfIERC20;

	tfIERC20 public	TOKEN;
	address public	PROJECT;

	constructor(address _tokenAddr) {

		TOKEN = tfIERC20(_tokenAddr);
		PROJECT = msg.sender;
	}

	function fundProject(uint _amountRequested) external OnlyProject returns(uint _amountFunded) {

		uint balance = TOKEN.balanceOf( address(this) );
		_amountFunded = _amountRequested > balance ? balance : _amountRequested;
		TOKEN.safeTransfer(PROJECT, _amountFunded);
	}

}


contract TRUEFUND_V2 {

	receive() external payable { }

	using tfSafeERC20 for tfIERC20;

	modifier OnlyOwner() { require( msg.sender == OWNER, 'OnlyOwner' ); _; }


	address public					OWNER;
	tfIERC20 public					TOKEN;
	uint public						LAUNCHED;
	uint16 public					DAILY_ROI =				25;							// 2.5% fixed
	uint public						EARNINGS_CUTOFF =		3 days;
	uint public						MINIMAL_INVEST =		1 ether;
	uint16 constant					REF_LVLS =				3;
	uint16[REF_LVLS] public			REF_PERCENTAGES =		[30, 20, 10];				// 3-2-1 %
	uint16[3] public				TOPREF_BONUS =			[500, 250, 100];			// 50-25,10 % for 1st, 2nd, 3rd places
	uint16 public					FEE_INVEST =			100;						// 10% [updates to 0-10 %]

	address payable public			INSURANCE;
	uint public						AGG_INVESTED;
	uint16 constant					AVG_BALANCE_PERIOD =	7;

	uint16 public					INS_PER_INVEST =		100;						// 10% [updates to 0-10 %]
	uint16 public					INS_PER_TRIGGER =		250;

	uint public						LOTTERY_ROUND;
	uint16 public					LOTTERY_USRCNT =		100;						// 100 users max [updates to 10-inf]
	uint public						LOTTERY_PRICE =			2500000000000000000;		// 2.5 USDT [updates to 1-inf]
	uint16 public					LOTTERY_PERCENT =		800;						// 80% winner prize [updates to 50-100 %]

	uint public						HT_INVESTED;
	uint public						HT_REFREWARD;
	uint public						HT_EARNED;
	uint public						HT_COMPOUNDED;
	uint public						HT_WITHDRAWN;
	uint public						HT_LOTTERY;
	uint public						HT_INS_SENT;
	uint public						HT_INS_RECV;


	event event_invest(address user, uint amount);
	event event_setUpline(address user, address upline);
	event event_lotteryWinner(uint lotteryRound, uint ticketsCount, uint bank, uint prizeAmount, uint winnerTicket, address winnerUser);
	event event_leaderPayout(address leader, uint TOP, uint wk_refReward, uint bonus);

	enum Flags { NEWCOMER, INVEST, COMPOUND, REFPAYOUT, WITHDRAW, TOP_DEPOSIT, LOTTERY_BUY, LOTTERY_WINNER, REFBONUS_PAID }


	struct Investment {
		address	user;
		address referrer;
		uint	amount;
	}

	struct History {
		Flags	flag;
		uint	time;
		uint	amt1;
		uint	amt2;
		uint	amt3;
		address	addr1;
		address	addr2;
	}

	struct User {
		address						upline;
		mapping(uint => address[])	refUsers;
		uint						invested;
		uint						refReward;
		uint						ltrReward;
		uint						earnReward;
		uint						checkpoint;
		uint						ht_invested;
		uint						ht_refReward;
		mapping(uint => uint)		wk_refReward;
		uint						ht_ltrReward;
		uint						ht_earned;
		uint						ht_compounded;
		uint						ht_withdrawn;
		History[]					history;
	}

	struct Deposit {
		address	user;
		uint	amount;
	}

	struct AvgBalance {
		uint	count;
		uint	amount;
	}


	struct LotteryUser {
		address	addr;
		uint	ticketsCount;
	}

	struct Lottery {
		uint						bank;
		uint						ticketsCount;
		uint						winnerTicket;
		address						winnerUser;
		LotteryUser[]				users;
		mapping(address => uint)	usersTickets;
	}

	struct LotteryResult {
		uint	finished;
		address	winner;
		uint	bank;
		uint	prize;
	}

	Investment[] public						INVESTMENTS;
	mapping(address => User) public			USERS;
	Deposit[10] public						TOPDEPOSITS;
	mapping(uint => AvgBalance) public		AVG_BALANCE;
	History[] public						HISTORY;
	mapping(uint => Lottery) public			LOTTERY;
	mapping(uint => LotteryResult) public	LOTTERY_RESULTS;
	mapping(uint => Deposit[10]) public		TOPREFERRERS;
	mapping(uint => uint) public			WK_REFREWARD;
	uint public								WEEK_INDEX;



	/*
		constructor
	*/

	constructor(address _tokenAddr) {

		OWNER = msg.sender;
		TOKEN = tfIERC20(_tokenAddr);
		INSURANCE = payable( new TRUEFUND_INSURANCE(_tokenAddr) );
	}


	/*
		public functions
	*/

	function invest(uint _amount, address _upline) external {

		require( _amount >= MINIMAL_INVEST, 'Insufficient amount');
		require( (LAUNCHED > 0) || (msg.sender == OWNER), 'Please wait for the official launch');

		if( LAUNCHED == 0 ) {
			LAUNCHED = block.timestamp;
		}

		User storage user = USERS[msg.sender];

		if(!_isUser(msg.sender)) {
			_userHistory(user, Flags.NEWCOMER, 0, 0, 0, address(0), address(0) );
		}

		TOKEN.safeTransferFrom( msg.sender, address(this), _amount );

		TOKEN.safeTransfer(OWNER, _per(_amount, FEE_INVEST) );

		_setUpline(msg.sender, _upline);

		_refPayout(msg.sender, _amount);

		_encashEarnings(msg.sender);

		_compoundRewards(msg.sender);

		unchecked {
			user.invested += _amount;
			AGG_INVESTED += _amount;

			user.ht_invested += _amount;
			HT_INVESTED += _amount;
		}

		INVESTMENTS.push(
			Investment({
				user: msg.sender,
				referrer: user.upline,
				amount: _amount
			})
		);

		_insuranceTrigger(0);

		_insuranceFill( _per(_amount, INS_PER_INVEST) );

		_trackBalance();

		_rateDeposit(msg.sender, _amount);


		_userHistory(user, Flags.INVEST, _amount, user.invested, AGG_INVESTED, address(0), address(0) );

		_globalHistory(Flags.INVEST, _amount, user.invested, AGG_INVESTED, msg.sender, address(0) );

		emit event_invest(msg.sender, _amount);
	}


	function compound() external {

		require( _isUser(msg.sender), 'Invalid user');

		_encashEarnings(msg.sender);

		_compoundRewards(msg.sender);

		_trackBalance();

	}


	function withdraw() external {

		require( _isUser(msg.sender), 'Invalid user');

		User storage user = USERS[msg.sender];

		_encashEarnings(msg.sender);

		(uint refReward, uint ltrReward, uint earnReward, uint totalRewards) = _drainRewards(msg.sender);

		_insuranceTrigger(totalRewards);

		TOKEN.safeTransfer(msg.sender, totalRewards);

		unchecked {
			user.ht_withdrawn += totalRewards;
			HT_WITHDRAWN += totalRewards;
		}

		_trackBalance();

		_userHistory(user, Flags.WITHDRAW, refReward, ltrReward, earnReward, address(0), address(0) );

	}


	function lotteryBuy(uint _ticketsCount) external {

		require( _isUser(msg.sender), 'Invalid user');

		User storage user = USERS[msg.sender];

		Lottery storage lottery = LOTTERY[LOTTERY_ROUND];

		require( lottery.users.length < LOTTERY_USRCNT, 'Max users count reached' );

		uint amount = _ticketsCount * LOTTERY_PRICE;

		TOKEN.safeTransferFrom( msg.sender, address(this), amount );

		lottery.users.push(
			LotteryUser({
				addr:			msg.sender,
				ticketsCount:	_ticketsCount
			})
		);
		unchecked {
			lottery.usersTickets[msg.sender] += _ticketsCount;
			lottery.ticketsCount += _ticketsCount;
			lottery.bank += amount;
		}

		_userHistory(user, Flags.LOTTERY_BUY, _ticketsCount, amount, 0, address(0), address(0) );
		_globalHistory(Flags.LOTTERY_BUY, _ticketsCount, lottery.ticketsCount, 0, msg.sender, address(0) );

	}


	function leaderBonusPayouts() external {

		require( _weekEndsIn() == 0, 'Wait for the end of this week');

		Deposit[10] storage TOP = TOPREFERRERS[WEEK_INDEX];

		//top1/2/3 receive their bonus
		for(uint i; i < TOPREF_BONUS.length; i++) {

			Deposit storage rank = TOP[9-i];
			if(rank.amount == 0) break;

			User storage user = USERS[ rank.user ];
			uint refReward = user.wk_refReward[WEEK_INDEX];
			uint bonus = _per( refReward, TOPREF_BONUS[i] );
			user.refReward += bonus;

			_userHistory(user, Flags.REFBONUS_PAID, bonus, 0, 0, address(0), address(0) );
			_globalHistory(Flags.REFBONUS_PAID, bonus, refReward, 0, rank.user, address(0) );
			emit event_leaderPayout(rank.user, i, refReward, bonus);
		}

		WEEK_INDEX++;
	}


	struct LotteryInfo {
		uint	bank;
		uint	ticketsCount;
		uint	usersCount;
		uint	winnerTicket;
		address	winnerUser;
		uint	maxUsers;
		uint	percent;
		uint	price;
		uint	prize;
	}

	struct DataContract {
		uint		time;
		uint		launched;
		uint		balance;
		uint		minDeposit;

		uint		agg_invested;
		uint		ht_invested;
		uint		ht_refReward;
		uint		ht_earned;
		uint		ht_withdrawn;
		uint		ht_lottery;

		uint		insBalance;
		uint		ht_insSentAgg;
		uint		ht_insRecvAgg;
		uint		avgBalance;

		uint		weekIndex;
		uint		wk_refReward;
		uint		weekEndsIn;

		Deposit[10]			topDeposits;
		Deposit[10]			topReferrers;
		History[20]			history;
		LotteryInfo			lotteryPrev;
		LotteryInfo			lotteryCurr;
		LotteryResult[20]	lotteryResults;
	}

	struct DataUser {
		uint	balance;
		uint	allowance;
	}

	function contractInfo(address _user) public view returns(DataContract memory _dataContract, DataUser memory _dataUser) {

		History[20] memory _HISTORY;
		for(uint i = HISTORY.length;  i > 0;  i--) {
			uint x = HISTORY.length - i;
			if(x >= _HISTORY.length) break;
			_HISTORY[x] = HISTORY[i-1];
		}

		LotteryResult[20] memory _LOTTERY_RESULTS;
		for(uint i = LOTTERY_ROUND;  i > 0;  i--) {
			uint x = LOTTERY_ROUND - i;
			if(x >= _LOTTERY_RESULTS.length) break;
			_LOTTERY_RESULTS[x] = LOTTERY_RESULTS[i-1];
		}

		Lottery storage lp = LOTTERY[LOTTERY_ROUND > 0 ? LOTTERY_ROUND - 1 : 0];
		Lottery storage lc = LOTTERY[LOTTERY_ROUND];

		_dataContract = DataContract({
			time:			block.timestamp,
			launched:		LAUNCHED,
			balance:		TOKEN.balanceOf(address(this)),
			minDeposit:		MINIMAL_INVEST,

			agg_invested:	AGG_INVESTED,
			ht_invested:	HT_INVESTED,
			ht_refReward:	HT_REFREWARD,
			ht_earned:		HT_EARNED,
			ht_withdrawn:	HT_WITHDRAWN,
			ht_lottery:		HT_LOTTERY,

			insBalance:		TOKEN.balanceOf(INSURANCE),
			ht_insSentAgg:	HT_INS_SENT,
			ht_insRecvAgg:	HT_INS_RECV,
			avgBalance:		_avgBalance(),

			weekIndex:		WEEK_INDEX,
			wk_refReward:	WK_REFREWARD[WEEK_INDEX],
			weekEndsIn:		_weekEndsIn(),

			topDeposits:	TOPDEPOSITS,
			topReferrers:	TOPREFERRERS[WEEK_INDEX],

			history:		_HISTORY,
			lotteryPrev:	LotteryInfo({ bank: lp.bank, ticketsCount: lp.ticketsCount, usersCount: lp.users.length, winnerTicket: lp.winnerTicket, winnerUser: lp.winnerUser, maxUsers: 0,              percent: 0,               price: 0,             prize: 0 }),
			lotteryCurr:	LotteryInfo({ bank: lc.bank, ticketsCount: lc.ticketsCount, usersCount: lc.users.length, winnerTicket: lc.winnerTicket, winnerUser: lc.winnerUser, maxUsers: LOTTERY_USRCNT, percent: LOTTERY_PERCENT, price: LOTTERY_PRICE, prize: _per(lc.bank, LOTTERY_PERCENT) }),
			lotteryResults:	_LOTTERY_RESULTS
		});

		_dataUser = DataUser({
			balance:	TOKEN.balanceOf(_user),
			allowance:	TOKEN.allowance(_user, address(this))
		});
	}


	struct DataUserEx {
		uint			time;
		uint			balance;
		uint			allowance;
		bool			isUser;
		address			upline;
		uint			checkpoint;
		uint			invested;
		uint			refReward;
		uint			ltrReward;
		uint			earnReward;
		uint			amtFull;
		uint			amtEarn;
		uint			amtDaily;
		uint			beforeCutoff;
		uint[REF_LVLS]	refCounts;
		uint			ht_invested;
		uint			ht_refReward;
		uint			wk_refReward;
		uint			wk_percentage;
		uint			ht_ltrReward;
		uint			ht_earned;
		uint			ht_compounded;
		uint			ht_withdrawn;
		uint			lotteryTickets;
	}

	function userInfo(address _user) public view returns(DataUserEx memory _dataUserEx, History[10] memory _history) {

		(/*timeFull*/, uint timeEarn, uint amtFull, uint amtEarn, uint amtDaily) = _calcEarnings(_user);

		User storage user = USERS[_user];

		uint[REF_LVLS] memory refCounts;
		for(uint16 i; i < REF_LVLS; i++)
			refCounts[i] = user.refUsers[i].length;

		_dataUserEx = DataUserEx({
			time:				block.timestamp,
			balance:			TOKEN.balanceOf(_user),
			allowance:			TOKEN.allowance(_user, address(this)),
			isUser:				_isUser(_user),
			upline:				user.upline,
			checkpoint:			user.checkpoint,
			invested:			user.invested,
			refReward:			user.refReward,
			ltrReward:			user.ltrReward,
			earnReward:			user.earnReward,
			amtFull:			amtFull,
			amtEarn:			amtEarn,
			amtDaily:			amtDaily,
			beforeCutoff:		timeEarn < EARNINGS_CUTOFF ? EARNINGS_CUTOFF - timeEarn : 0,
			refCounts:			refCounts,
			ht_invested:		user.ht_invested,
			ht_refReward:		user.ht_refReward,
			wk_refReward:		user.wk_refReward[WEEK_INDEX],
			wk_percentage:		WK_REFREWARD[WEEK_INDEX] > 0 ? user.wk_refReward[WEEK_INDEX]*100/WK_REFREWARD[WEEK_INDEX] : 0,
			ht_ltrReward:		user.ht_ltrReward,
			ht_earned:			user.ht_earned,
			ht_compounded:		user.ht_compounded,
			ht_withdrawn:		user.ht_withdrawn,
			lotteryTickets:		LOTTERY[LOTTERY_ROUND].usersTickets[_user]
		});

		(,,_history) = userHistory(_user, 0);

	}


	function userHistory(address _user, uint _offset) public view returns(bool _canPrev, bool _canNext, History[10] memory _history) {

		_canPrev = _offset > 0;
		uint LEN = USERS[_user].history.length;
		uint len = _history.length;

		if(LEN < _offset+1) return (false, _canPrev, _history);

		uint start = LEN - _offset - 1;
		uint cnt = _min(len, start+1);
		_canNext = (start+1 > len);
		for(uint i; i < cnt; i++)
			_history[i] = USERS[_user].history[start-i];
	}


	struct RefUser {
		address			addr;
		address			upline;
		uint			invested;
		uint			refReward;
		uint[REF_LVLS]	refCounts;
	}
	function userReferrals(address _user, uint _level) public view returns(RefUser[] memory) {

		User storage user = USERS[_user];

		uint length = user.refUsers[_level].length;

		RefUser[] memory referrals = new RefUser[]( length );

		for(uint i; i < length; i++) {

			address addr = user.refUsers[_level][i];

			User storage ref = USERS[addr];

			uint[REF_LVLS] memory refCounts;
			for(uint16 r; r < REF_LVLS; r++)
				refCounts[r] = ref.refUsers[r].length;

			referrals[i] = RefUser({
								addr:		addr,
								upline:		ref.upline,
								invested:	ref.ht_invested,
								refReward:	ref.ht_refReward,
								refCounts:	refCounts
							});
		}

		return referrals;
	}


	/*
		admin functions
	*/


	function updateFees(uint16 _FEE_INVEST, uint16 _INS_PER_INVEST) public OnlyOwner {

		require( _FEE_INVEST >= 0 && _FEE_INVEST <= 100, 'Incorrect FEE_INVEST' );
		require( _INS_PER_INVEST >= 0 && _INS_PER_INVEST <= 100, 'Incorrect INS_PER_INVEST' );

		FEE_INVEST = _FEE_INVEST;
		INS_PER_INVEST = _INS_PER_INVEST;
	}


	// Force the Insurance contract to fund the Main contract, no USDT will be sent outside
	function pullInsurance(uint _amount) public OnlyOwner {

		_insurancePull( _amount );
	}


	// Lottery: choose a winner / start a new lottery
	function lotteryFinish(bool _updateParams, uint _bonusFunds, uint16 _LOTTERY_USRCNT, uint _LOTTERY_PRICE, uint16 _LOTTERY_PERCENT) public OnlyOwner {

		Lottery storage lottery = LOTTERY[LOTTERY_ROUND];

		require( lottery.ticketsCount > 0 , 'No tickets');

		uint prizeAmount = _per(lottery.bank, LOTTERY_PERCENT);
		lottery.winnerTicket = _random(1, lottery.ticketsCount);

		uint length = lottery.users.length;
		uint tCnt;
		for(uint i; i < length; i++) {
			unchecked {
				tCnt += lottery.users[i].ticketsCount;
			}
			if(tCnt >= lottery.winnerTicket) {
				lottery.winnerUser = lottery.users[i].addr;
				break;
			}
		}

		_insuranceFill( lottery.bank - prizeAmount );

		HT_INVESTED += lottery.bank - prizeAmount;

		User storage user = USERS[lottery.winnerUser];
		
		user.ltrReward += prizeAmount;

		HT_LOTTERY += prizeAmount;

		LOTTERY_RESULTS[LOTTERY_ROUND] = LotteryResult({ finished: block.timestamp, winner: lottery.winnerUser, bank: lottery.bank, prize: prizeAmount });

		_userHistory(user, Flags.LOTTERY_WINNER, prizeAmount, 0, 0, address(0), address(0) );
		_globalHistory(Flags.LOTTERY_WINNER, prizeAmount, 0, 0, lottery.winnerUser, address(0) );

		emit event_lotteryWinner(LOTTERY_ROUND, lottery.ticketsCount, lottery.bank, prizeAmount, lottery.winnerTicket, lottery.winnerUser);


		//starting a new Lottery

		LOTTERY_ROUND++;

		//update params, if specified. Does not affect the current Lottery
		if(_updateParams) {
			require( _LOTTERY_USRCNT >= 10, 'Incorrect LOTTERY_USRCNT' );
			require( _LOTTERY_PRICE >= 1 ether, 'Incorrect LOTTERY_PRICE' );
			require( _LOTTERY_PERCENT >= 500 && _LOTTERY_PERCENT <= 1000, 'Incorrect LOTTERY_PERCENT' );

			LOTTERY_USRCNT = _LOTTERY_USRCNT;
			LOTTERY_PRICE = _LOTTERY_PRICE;
			LOTTERY_PERCENT = _LOTTERY_PERCENT;
		}

		//add Bonus funds to Lottery if specified - take it from Owner's wallet
		if(_bonusFunds > 0) {
			TOKEN.safeTransferFrom( msg.sender, address(this), _bonusFunds );
			LOTTERY[LOTTERY_ROUND].bank += _bonusFunds;
		}

	}


	// Interface for Moonarch rankings: accumulated earnings&rewards amount
	function moonArch(address _user) public view returns(uint) {

		User storage user = USERS[_user];
		(,,,uint amtEarn,) = _calcEarnings(_user);
		return amtEarn + user.refReward + user.ltrReward + user.earnReward;
	}


	/*
		private functions
	*/


	function _min(uint num1, uint num2) private pure returns(uint) {

		return num1 < num2 ? num1 : num2;
	}


	function _max(uint num1, uint num2) private pure returns(uint) {

		return num1 > num2 ? num1 : num2;
	}

	function _random(uint _minV, uint _maxV) private view returns(uint) {
		return _minV + (uint(keccak256(abi.encodePacked(block.timestamp,address(this).balance,HT_EARNED))) % _maxV);
	}


	function _per(uint _amount, uint _percent) private pure returns(uint) {

		return (_amount * _percent) / 1000;
	}


	function _isUser(address _user) private view returns(bool) {

		return (USERS[_user].checkpoint > 0);
	}


	function _weekEndsIn() private view returns(uint) {

		return LAUNCHED + (WEEK_INDEX+1)*604800 > block.timestamp ? LAUNCHED + (WEEK_INDEX+1)*604800 - block.timestamp : 0;
	}


	function _globalHistory(Flags _flag, uint _amt1, uint _amt2, uint _amt3, address _addr1, address _addr2) private {

		HISTORY.push(
			History({
				flag:	_flag,
				time:	block.timestamp,
				amt1:	_amt1,
				amt2:	_amt2,
				amt3:	_amt3,
				addr1:	_addr1,
				addr2:	_addr2
			})
		);
	}


	function _userHistory(User storage _user, Flags _flag, uint _amt1, uint _amt2, uint _amt3, address _addr1, address _addr2) private {

		_user.history.push(
			History({
				flag:	_flag,
				time:	block.timestamp,
				amt1:	_amt1,
				amt2:	_amt2,
				amt3:	_amt3,
				addr1:	_addr1,
				addr2:	_addr2
			})
		);
	}


	function _setUpline(address _user, address _upline) private {

		if( USERS[_user].upline != address(0) ) return;
		if( !_isUser(_upline) ) return;
		if( _user == _upline ) return;

		USERS[_user].upline = _upline;
		emit event_setUpline(_user, _upline);

		for(uint16 i; i < REF_LVLS; i++) {
			USERS[_upline].refUsers[i].push(_user);
			_upline = USERS[_upline].upline;
			if(_upline == address(0)) break;
		}
	}


	function _refPayout(address _user, uint _amount) private {

		address upline = USERS[_user].upline;

		for(uint16 i; i < REF_LVLS; i++) {

			if(upline == address(0)) break;

			_mintReferralReward(_user, upline, _per(_amount, REF_PERCENTAGES[i]) );

			upline = USERS[upline].upline;
		}
	}


	function _mintReferralReward(address _source, address _referrer, uint _reward) private {

		uint isNewUser = _isUser(_source) ? 0 : 1;

		User storage referrer = USERS[_referrer];

		//referral payouts
		unchecked {
			referrer.refReward += _reward;
			referrer.ht_refReward += _reward;
			HT_REFREWARD += _reward;
		}

		_userHistory(referrer, Flags.REFPAYOUT, _reward, isNewUser, 0, _source, address(0) );
		if(isNewUser != 1) return;

		//increase wk_refReward + recalculate ranking [on Newbie deposits only]
		uint weekIdx = _getWeekIdx();
		unchecked {
			referrer.wk_refReward[weekIdx] += _reward;
			WK_REFREWARD[weekIdx] += _reward;
		}
		_rateReferrer(_referrer, referrer.wk_refReward[weekIdx], weekIdx);
	}


	function _rateDeposit(address _user, uint _amount) private {

		uint length = TOPDEPOSITS.length;
		bool move; uint moveIdx;
		for(uint i; i < length; i++) {
			if(TOPDEPOSITS[i].amount == _amount) return;
			if(TOPDEPOSITS[i].amount < _amount) {
				if(!move) move = true;
				moveIdx = i;
			}
		}
		if(!move) return;

		for(uint i; i < moveIdx; i++)
			TOPDEPOSITS[i] = TOPDEPOSITS[i+1];

		TOPDEPOSITS[moveIdx] = Deposit({ user: _user, amount: _amount });

		_globalHistory(Flags.TOP_DEPOSIT, _amount, 0, 0, _user, address(0) );

	}


	function _rateReferrer(address _user, uint _amount, uint _weekIdx) private {

		Deposit[10] storage TOP = TOPREFERRERS[_weekIdx];

		uint length = TOP.length;
		bool move; uint moveIdx; uint selfIdx;
		for(uint i; i < length; i++) {
			if(TOP[i].user == _user) {
				selfIdx = i;
			}
			if(TOP[i].amount < _amount) {
				if(!move) move = true;
				moveIdx = i;
			}
		}
		if(!move) return;

		for(uint i=selfIdx; i < moveIdx; i++)
			TOP[i] = TOP[i+1];

		TOP[moveIdx] = Deposit({ user: _user, amount: _amount });
	}


	function _calcEarnings(address _user) private view returns(uint _timeFull, uint _timeEarn, uint _amtFull, uint _amtEarn, uint _daily) {

		User storage user = USERS[_user];

		_timeFull = user.checkpoint > 0 ? block.timestamp - user.checkpoint : 0;
		_timeEarn = _min( _timeFull , EARNINGS_CUTOFF );

		_amtFull = (user.invested * _timeFull * DAILY_ROI) / 86400000;
		_amtEarn = (user.invested * _timeEarn * DAILY_ROI) / 86400000;
		_daily = (user.invested * DAILY_ROI) / 1000;
	}


	function _encashEarnings(address _user) private {

		User storage user = USERS[_user];

		(/*uint timeFull*/, /*uint timeEarn*/, /*uint amtFull*/, uint amtEarn, /*daily*/) = _calcEarnings(_user);

		unchecked {
			user.checkpoint = block.timestamp;
			user.earnReward += amtEarn;
			user.ht_earned += amtEarn;
			HT_EARNED += amtEarn;
		}
	}


	function _drainRewards(address _user) private returns(uint _refReward, uint _ltrReward, uint _earnReward, uint _totalRewards) {

		User storage user = USERS[_user];

		unchecked {
		(_refReward, _ltrReward, _earnReward) = (user.refReward, user.ltrReward, user.earnReward);
		_totalRewards = _refReward + _ltrReward + _earnReward;
		(user.refReward, user.ltrReward, user.earnReward) = (0,0,0);
		}
	}


	function _compoundRewards(address _user) private {

		User storage user = USERS[_user];

		(uint refReward, uint ltrReward, uint earnReward, uint totalRewards) = _drainRewards(_user);

		if(totalRewards==0) return;

		unchecked {
			user.invested += totalRewards;
			AGG_INVESTED += totalRewards;

			user.ht_compounded += totalRewards;
			HT_COMPOUNDED += totalRewards;
		}

		_userHistory(user, Flags.COMPOUND, refReward, ltrReward, earnReward, address(0), address(0) );
	}


	function _getDayIdx() private view returns(uint) {

		return block.timestamp / 86400;
	}


	function _getWeekIdx() private view returns(uint) {

		return (block.timestamp - LAUNCHED) / 604800;
	}


	function _trackBalance() private {

		AvgBalance storage avg = AVG_BALANCE[_getDayIdx()];
		unchecked {
			avg.amount += TOKEN.balanceOf(address(this));
			avg.count++;
		}
	}


	function _avgBalance() private view returns(uint) {

		uint idx = _getDayIdx();
		uint count;
		uint amount;
		for(uint i = idx - AVG_BALANCE_PERIOD; i < idx; i++) {
			AvgBalance storage avg = AVG_BALANCE[i];
			if(avg.count == 0) continue;
			amount += avg.amount / avg.count;
			count++;
		}

		return count == 0 ? 0 : amount / count;
	}


	function _insuranceFill(uint _amount) private {

		TOKEN.safeTransfer( INSURANCE, _amount );

		HT_INS_SENT += _amount;
	}


	function _insurancePull(uint _amountRequested) private {

		uint amountFunded = ITrueFundInsurance(INSURANCE).fundProject( _amountRequested );

		HT_INS_RECV += amountFunded;
	}


	function _insuranceTrigger(uint _amount) private returns(bool) {

		uint balance = TOKEN.balanceOf(address(this));

		if(balance == 0) {

			_insurancePull( TOKEN.balanceOf(INSURANCE) );
			return true;
		}

		if(_amount * 1000 / balance >= INS_PER_TRIGGER) {

			_insurancePull( _amount );
			return true;
		}

		uint avg = _avgBalance();
		if(avg > 0) {
			if(balance * 1000 / avg <= 1000-INS_PER_TRIGGER) {

				_insurancePull( avg - balance );
				return true;
			}
		}

		return false;
	}


}