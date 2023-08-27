// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

interface interfaceMaster {
  function myProxyNum(address user) external view returns (uint256);
  function batchActivate(address user, uint256 times) external;
  function batchBoost(address user, uint256 startIndex, uint256 endIndex) external;
  function batchWithdrawal(address user, uint256 startIndex, uint256 endIndex) external;
}
contract Fooyao_bulk {
    interfaceMaster private immutable fooyao = interfaceMaster(0xBD4E5aAe7071d44024f5d35B62635cd358d6b81e);

    /**
    * @dev 获取你的地址数
    */
    function myProxyNum(address theAddress) external view returns(uint256){
		return fooyao.myProxyNum(theAddress);
	}

    /**
    * @dev 批量activate
    * @param times 数量
    */
    function batchActivate(uint256 times) external{
		fooyao.batchActivate(msg.sender, times);
	}

    /**
    * @dev 批量boost
    * @param startIndex 起始序号,0开始
    * @param endIndex 结束序号,不包括
    */
    function batchBoost(uint256 startIndex, uint256 endIndex) external{
		fooyao.batchBoost(msg.sender, startIndex, endIndex);
	}

    /**
    * @dev 批量withdrawal
    * @param startIndex 起始序号,0开始
    * @param endIndex 结束序号,不包括
    */
    function batchWithdrawal(uint256 startIndex, uint256 endIndex) external{
		fooyao.batchWithdrawal(msg.sender, startIndex, endIndex);
	}

}