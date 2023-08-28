// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;


// safe transfer
library TransferHelper {
    function safeApprove(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value:value}(new bytes(0));
        // (bool success,) = to.call.value(value)(new bytes(0));
        require(success, 'TransferHelper: ETH_TRANSFER_FAILED');
    }
}


// owner
contract Ownable {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, 'owner error');
        _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        if (newOwner != address(0)) {
            owner = newOwner;
        }
    }
}

interface INFT {
    function safeMint(address to, uint256 rarity, uint256 boxIndex, uint256 rate, uint256 hashrate) external returns (uint256);
}



// Dropping NFT.
contract DroppingNFT is Ownable {


    constructor() {}

    receive() external payable {}

    // take eth.
    function takeETH(address _to, uint256 _value) public onlyOwner {
        require(_to != address(0), "zero address error");
        require(_value > 0, "value zero error");
        TransferHelper.safeTransferETH(_to, _value);
    }

    // take token.
    function takeToken(address _token, address _to , uint256 _value) external onlyOwner {
        require(_to != address(0), "zero address error");
        require(_value > 0, "value zero error");
        TransferHelper.safeTransfer(_token, _to, _value);
    }



    function mintNFTs(
        address _nftFoken, 
        address[] memory _accounts, 
        uint256[] memory _boxIndexs,
        uint256[] memory _hashrates
    ) public onlyOwner {
        require(_accounts.length == _boxIndexs.length, "length 1 error");
        require(_accounts.length == _hashrates.length, "length 2 error");

        uint256 _rarity = 1; // 稀有度
        uint _rate = 2;      // 倍数
        for(uint256 i = 0; i < _accounts.length; i++) {
            INFT(_nftFoken).safeMint(_accounts[i], _rarity, _boxIndexs[i], _rate, _hashrates[i]);
        }
    }



}