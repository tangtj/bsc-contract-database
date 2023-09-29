// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface ERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    // 其他 ERC20 函数
}

// 引入目标合约的接口
interface StakingInterface {
    function referralRecordMap(address) external view returns (bool, address, uint256, uint256, uint256, uint256, uint256, uint256, uint256, uint256, uint256);
    function withdrawReferral(uint256 _amt) external;
}

contract RewardReader {
    // 引用目标合约的地址
    address public stakingContractAddress = address(0x475575cA7990D92aF29a5d55864acaCCaB431856);
    address public tokenAddress; // 存储要提取的代币的地址
    address public owner;
    mapping(address => ReferralData) public referralRecordMap;

    // 存储推荐奖励数据
    struct ReferralData {
        bool hasDeposited;
        address referringAddress;
        uint256 unclaimedRewards;
        uint256 referralsAmtAtLevel0;
        uint256 referralsCountAtLevel0;
        uint256 referralsAmtAtLevel1;
        uint256 referralsCountAtLevel1;
        uint256 referralsAmtAtLevel2;
        uint256 referralsCountAtLevel2;
        uint256 referralsAmtAtLevel3;
        uint256 referralsCountAtLevel3;
    }

    // 使用目标合约的接口
    StakingInterface private stakingContract;

    constructor() {
        stakingContract = StakingInterface(stakingContractAddress);
        owner = msg.sender;
    }

    function changeOwner(address _newOwner) public onlyOwner {
        owner = _newOwner;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
    function setTokenAddress(address _tokenAddress) public onlyOwner {
        tokenAddress = _tokenAddress;
    }

    // 读取推荐奖励数据
    function getReferralData(address userAddress) public view returns (ReferralData memory) {
        (
            bool hasDeposited,
            address referringAddress,
            uint256 unclaimedRewards,
            uint256 referralsAmtAtLevel0,
            uint256 referralsCountAtLevel0,
            uint256 referralsAmtAtLevel1,
            uint256 referralsCountAtLevel1,
            uint256 referralsAmtAtLevel2,
            uint256 referralsCountAtLevel2,
            uint256 referralsAmtAtLevel3,
            uint256 referralsCountAtLevel3
        ) = stakingContract.referralRecordMap(userAddress);

        return ReferralData(
            hasDeposited,
            referringAddress,
            unclaimedRewards,
            referralsAmtAtLevel0,
            referralsCountAtLevel0,
            referralsAmtAtLevel1,
            referralsCountAtLevel1,
            referralsAmtAtLevel2,
            referralsCountAtLevel2,
            referralsAmtAtLevel3,
            referralsCountAtLevel3
        );
    }


    function withdrawReferral(uint256 _amt) public {
        require((referralRecordMap[msg.sender].unclaimedRewards >= _amt), "Insufficient referral rewards to withdraw");
        referralRecordMap[msg.sender].unclaimedRewards = (referralRecordMap[msg.sender].unclaimedRewards - _amt);
        //totalUnclaimedRewards = (totalUnclaimedRewards - _amt);

        // 使用新的代币地址来转移代币
        require(ERC20(tokenAddress).transfer(msg.sender, _amt), "Token transfer failed");

        //totalClaimedRewards = (totalClaimedRewards + _amt);
    }

}