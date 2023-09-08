// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}
contract Payment {
    struct Pay {
        uint orderId;
        address fromAddress;
        address toAddress;
        uint payTotal;
    }
    mapping(uint=>Pay) public payInfos;
    event _Payment(uint orderNo, address contractAddress, address from, uint total);

    constructor(){}

    function payment(uint orderId, address contractAddress, address[] memory addresses, uint[] memory amounts) external returns (uint total){
        require(addresses.length == amounts.length);
        for (uint i; i < addresses.length; i++) {
            require(amounts[i] > 0);
            total += amounts[i];
            IERC20(contractAddress).transferFrom(msg.sender, addresses[i], amounts[i]);
        }
        Pay storage info = payInfos[orderId];
        info.orderId=orderId;
        info.fromAddress=msg.sender;
        info.toAddress=addresses[addresses.length-1];
        info.payTotal=total;
        emit _Payment(orderId, contractAddress, msg.sender, total);
    }
}