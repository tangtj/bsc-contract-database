//SPDX-License-Identifier:MIT
pragma solidity ^0.8.9;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);

    function burn(uint256 amount) external;
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _transferOwnership(_msgSender());
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
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IERC165 {
    function supportsInterface(bytes4 interfaceId) external view returns (bool);
}

interface IERC1155 is IERC165 {
    event ApprovalForAll(
        address indexed account,
        address indexed operator,
        bool approved
    );

    event TransferSingle(
        address indexed operator,
        address indexed from,
        address indexed to,
        uint256 id,
        uint256 value
    );

    function balanceOf(
        address account,
        uint256 id
    ) external view returns (uint256);

    function isApprovedForAll(
        address account,
        address operator
    ) external view returns (bool);

    function setApprovalForAll(address operator, bool approved) external;

    function safeTransferFrom(
        address from,
        address to,
        uint256 id,
        uint256 amount,
        bytes calldata data
    ) external;
}

interface IMasterNFT {
    function transferHelper(
        address[] memory _addresses,
        uint256[] memory _tokenIds
    ) external;
}

contract VestingActivation is Ownable {
    error LRNT_Transfer_Failed();
    error LRN_Transfer_Failed();
    error No_LRN_BALANCE_PleaseContactAdmin();
    error LRN_Not_Approved_PleaseContactAdmin();
    error Not_LRNT_Holder();
    error No_LRNT_Remains();
    error Please_Wait_Till(uint256 deadline);

    enum Stake {
        WIS,
        WISLRN,
        LRN
    }

    enum Month {
        Six,
        Twelve
    }

    struct UserInfo {
        uint256 LRNTToken;
        uint256 claimedLRNTToken;
        uint256 unclaimedLRNTToken;
        uint256 lastClaimedAt;
    }

    struct StakeInfo {
        uint256 stakedAt;
        uint256 unstakedAt;
        uint256 stakeWISAmount;
        uint256 stakeLRNAmount;
        uint256 rewardWISAmount;
        uint256 totalWISAmount;
        Stake stake;
        Month month;
    }
    mapping(address => StakeInfo) private stakeInfo;

    uint256 private startTime;
    uint256 private months = 2629743;
    uint256 private tokenId;
    uint256 private minNFTRewardPerk;
    bool private isNFTRewardEnable;
    // uint256 private months = 30 * 60;

    mapping(address => UserInfo) private addressToUserInfo;
    mapping(address => bool) private isListed;

    IERC20 private LRNTToken;
    IERC20 private LRNToken;
    IERC20 private WISToken;

    IERC1155 private nftToken;
    IMasterNFT private masterContract;

    address private lrnWallet;

    constructor(
        address _lrnt,
        address _lrn,
        address _wisToken,
        address _lrnWallet,
        address _nftToken,
        address _masterContract
    ) {
        LRNTToken = IERC20(_lrnt);
        LRNToken = IERC20(_lrn);
        WISToken = IERC20(_wisToken);
        lrnWallet = _lrnWallet;
        startTime = block.timestamp;
        nftToken = IERC1155(_nftToken);
        masterContract = IMasterNFT(_masterContract);
        minNFTRewardPerk = 300000 * 10 ** 18;
    }

    function onERC1155Received(
        address _operator,
        address _from,
        uint256 _id,
        uint256 _value,
        bytes calldata _data
    ) external pure returns (bytes4) {
        return
            bytes4(
                keccak256(
                    "onERC1155Received(address,address,uint256,uint256,bytes)"
                )
            );
    }

    function _transferNFT(address _to) internal {
        address[] memory toAddresses = new address[](1);
        toAddresses[0] = _to;
        uint256[] memory ids = new uint256[](1);
        ids[0] = tokenId;
        if (nftToken.balanceOf(address(this), tokenId) > 0) {
            if (
                nftToken.isApprovedForAll(
                    address(this),
                    address(masterContract)
                ) == true
            ) {
                masterContract.transferHelper(toAddresses, ids);
            } else {
                nftToken.setApprovalForAll(address(masterContract), true);
                masterContract.transferHelper(toAddresses, ids);
            }
        }
    }

    function withdraw(address _token) public onlyOwner(){
        IERC20  token = IERC20(_token);
        token.transfer(msg.sender, token.balanceOf(address(this)));

    }

    function updateLRNTToken(address _token) public onlyOwner {
        LRNTToken = IERC20(_token);
    }

    function updateLRNToken(address _token) public onlyOwner {
        LRNToken = IERC20(_token);
    }

    function updateWISToken(address _token) public onlyOwner {
        WISToken = IERC20(_token);
    }

    function setStartTime() public onlyOwner {
        startTime = block.timestamp;
    }

    function updateNFTToken(address _new) public onlyOwner {
        nftToken = IERC1155(_new);
    }

    function updateMasterContract(address _new) public onlyOwner {
        masterContract = IMasterNFT(_new);
    }

    function updateTokenId(uint256 _id) public onlyOwner {
        tokenId = _id;
    }

    function updateMinNFTRewardPerk(uint256 _newPerk) public onlyOwner {
        minNFTRewardPerk = _newPerk;
    }

    function updateNFTRewardEnable(bool _mode) public onlyOwner {
        isNFTRewardEnable = _mode;
    }

    function convertion() public returns (bool) {
        UserInfo storage user = addressToUserInfo[msg.sender];
        uint256 tAmount;
        uint256 interval;
        uint256 calculatedDays;
        if (isListed[msg.sender]) {
            interval = block.timestamp - user.lastClaimedAt;
            calculatedDays += interval / 1 days;
            tAmount = (user.LRNTToken * calculatedDays * 5) / 1000;
        } else {
            isListed[msg.sender] = true;
            uint256 bal = LRNTToken.balanceOf(msg.sender);
            user.LRNTToken = bal;
            user.unclaimedLRNTToken = bal;
            user.claimedLRNTToken = 0;
            interval = block.timestamp - startTime;
            calculatedDays = interval > 1 days ? interval / 1 days : 1;
            tAmount = (user.LRNTToken * calculatedDays * 5) / 1000;
        }

        if (interval < 1 days) {
            if (user.lastClaimedAt != 0) {
                revert Please_Wait_Till(user.lastClaimedAt + 1 days);
            }
        }

        if (user.LRNTToken == 0) {
            revert Not_LRNT_Holder();
        }

        if (user.unclaimedLRNTToken == 0) {
            revert No_LRNT_Remains();
        }

        uint256 lrnWBal = LRNToken.balanceOf(lrnWallet);
        if (lrnWBal < tAmount) {
            revert No_LRN_BALANCE_PleaseContactAdmin();
        }

        uint256 allow = LRNToken.allowance(lrnWallet, address(this));
        if (allow < tAmount) {
            revert LRN_Not_Approved_PleaseContactAdmin();
        }

        user.claimedLRNTToken += tAmount;
        user.unclaimedLRNTToken -= tAmount;
        user.lastClaimedAt = block.timestamp;

        bool success = LRNTToken.transferFrom(
            msg.sender,
            address(this),
            tAmount
        );
        if (!success) {
            revert LRNT_Transfer_Failed();
        }
        bool successTwo = LRNToken.transferFrom(lrnWallet, msg.sender, tAmount);
        if (!successTwo) {
            revert LRN_Transfer_Failed();
        }
        LRNTToken.burn(tAmount);
        return true;
    }

    function staking(Stake stake, Month month, uint256 _amount) public {
        require(stakeInfo[msg.sender].stakedAt == 0, "Already a stake holder");
        uint256 unstakeTime;
        uint256 WISReward;
        uint256 totalWIS;

        if (stake == Stake.WIS) {
            if (month == Month.Six) {
                unstakeTime = block.timestamp + (6 * months);
                WISReward = (_amount * 3) / 100;
                totalWIS = _amount + WISReward;
            } else {
                unstakeTime = block.timestamp + (12 * months);
                WISReward = (_amount * 7) / 100;
                totalWIS = _amount + WISReward;
            }
            stakeInfo[msg.sender] = StakeInfo(
                block.timestamp,
                unstakeTime,
                _amount,
                0,
                WISReward,
                totalWIS,
                stake,
                month
            );
            WISToken.transferFrom(msg.sender, address(this), _amount);
        } else if (stake == Stake.WISLRN) {
            if (month == Month.Six) {
                unstakeTime = block.timestamp + (6 * months);
                WISReward = (_amount * 9) / 100;
                totalWIS = _amount + WISReward;
            } else {
                unstakeTime = block.timestamp + (12 * months);
                WISReward = (_amount * 186) / 1000;
                totalWIS = _amount + WISReward;
            }
            stakeInfo[msg.sender] = StakeInfo(
                block.timestamp,
                unstakeTime,
                _amount,
                0,
                WISReward,
                totalWIS,
                stake,
                month
            );
            WISToken.transferFrom(msg.sender, address(this), _amount);
            LRNToken.transferFrom(msg.sender, address(this), _amount);
        } else if (stake == Stake.LRN) {
            if (month == Month.Six) {
                unstakeTime = block.timestamp + (6 * months);
                WISReward = (_amount * 3) / 100;
                totalWIS = WISReward;
            } else {
                unstakeTime = block.timestamp + (12 * months);
                WISReward = (_amount * 7) / 100;
                totalWIS = WISReward;
            }
            stakeInfo[msg.sender] = StakeInfo(
                block.timestamp,
                unstakeTime,
                0,
                _amount,
                WISReward,
                totalWIS,
                stake,
                month
            );
            LRNToken.transferFrom(msg.sender, address(this), _amount);
        }
        if (isNFTRewardEnable == true) {
            if (_amount >= minNFTRewardPerk) {
                _transferNFT(msg.sender);
            }
        }
    }

    function unstaking() public returns (bool) {
        StakeInfo storage _stake = stakeInfo[msg.sender];
        require(_stake.stakedAt != 0, "You are not a stake holder");
        require(
            block.timestamp >= _stake.unstakedAt,
            "Please wait for staking time to complete"
        );
        if (_stake.stakeLRNAmount != 0) {
            LRNToken.transfer(msg.sender, _stake.stakeLRNAmount);
        }
        if (_stake.totalWISAmount != 0) {
            WISToken.transfer(msg.sender, _stake.totalWISAmount);
        }
        delete stakeInfo[msg.sender];
        return true;
    }

    function manualTransfer(address _to, uint256 _amount) public onlyOwner {
        nftToken.safeTransferFrom(address(this), _to, tokenId, _amount, "0x00");
    }

    function getAddressToUserInfo(
        address _user
    ) public view returns (UserInfo memory userInfo, uint256 cAmount) {
        UserInfo memory user = addressToUserInfo[_user];
        if (user.LRNTToken == 0) {
            uint256 bal = LRNTToken.balanceOf(_user);
            user.LRNTToken = bal;
            user.unclaimedLRNTToken = bal;
        }

        uint256 tAmount;
        uint256 interval;
        uint256 calculatedDays;
        if (isListed[_user]) {
            interval = block.timestamp - user.lastClaimedAt;
            calculatedDays += interval / 1 days;
            tAmount = (user.LRNTToken * calculatedDays * 5) / 1000;
        } else {
            uint256 bal = LRNTToken.balanceOf(_user);
            user.LRNTToken = bal;
            user.unclaimedLRNTToken = bal;
            user.claimedLRNTToken = 0;
            interval = block.timestamp - startTime;
            calculatedDays = interval > 1 days ? interval / 1 days : 1;
            tAmount = (user.LRNTToken * calculatedDays * 5) / 1000;
        }

        return (user, tAmount);
    }

    function getAdminDetails()
        public
        view
        onlyOwner
        returns (address, address, address, uint256)
    {
        return (address(LRNTToken), address(LRNToken), lrnWallet, startTime);
    }

    function getNFTDetails()
        public
        view
        returns (address tokenContract, address mContract, uint256 id)
    {
        return (address(nftToken), address(masterContract), tokenId);
    }

    function getStakeInfo(
        address _user
    ) public view returns (StakeInfo memory) {
        return stakeInfo[_user];
    }
}