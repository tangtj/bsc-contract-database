// SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.0;

contract namomo21 {
    address public owner;

    // 이벤트 정의
    event OwnerSet(address indexed _owner);
    event FunctionCalled(string indexed functionName, uint256 indexed value);
    event RandomIndexCalculated(uint256 indexed value);
    event RandomIndexPicked(uint256 indexed index);

    constructor() {
        owner = msg.sender;
        emit OwnerSet(owner);  // Owner 설정 로그
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");  // 오류 메시지 추가
        _;
    }

    function pickRandomIndex(uint256 _length) external onlyOwner returns (uint256) {
        emit FunctionCalled("pickRandomIndex", _length);  // 함수 호출 로그

        require(_length > 0, "Length should be greater than 0");  // 오류 메시지 추가
        uint256 randomIndex = uint256(keccak256(abi.encodePacked(block.timestamp, msg.sender))) % _length;

        emit RandomIndexCalculated(randomIndex);  // 계산된 랜덤 인덱스 로그
        emit RandomIndexPicked(randomIndex);  // 랜덤 인덱스 선택 이벤트 로그

        return randomIndex;
    }
}