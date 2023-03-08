// SPDX-License-Identifier: MIT
pragma solidity >=0.8.6;

abstract contract Ownable {
    address[2] private _owners;

    modifier onlyOwner() {
        require(msg.sender == _owners[0] || msg.sender == _owners[1], "only owner");
        _;
    }

    constructor (address[2] memory newOwners) {
        require(newOwners[0] != address(0)
            && newOwners[1] != address(0)
            && newOwners[0] != newOwners[1],
            "CTOR: owners are same or contain zero address");
        _owners = newOwners;
    }

    function owners() external view returns (address[2] memory) {
        return _owners;
    }

    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), "the new owner is the zero address");
        require(newOwner != _owners[0] && newOwner != _owners[1], "the new owner is existed");
        if (msg.sender == _owners[0]) {
            _owners[0] = newOwner;
        } else {
            _owners[1] = newOwner;
        }
    }
}

contract TokenPriceConfig is Ownable {
    mapping(uint256 => mapping(address => string)) private _tokenPrices; // key is chainID,tokenAddress
    mapping(string => string) private _tokenPricesByTokenID; // key is tokenID

    constructor (address[2] memory _owners) Ownable(_owners) {
    }

    function getTokenPrice(uint256 chainID, address tokenAddr) external view returns (string memory) {
        return _tokenPrices[chainID][tokenAddr];
    }

    function getTokenPrice(string calldata tokenID) external view returns (string memory) {
        return _tokenPricesByTokenID[tokenID];
    }

    function setTokenPrice(uint256 chainID, address tokenAddr, string calldata price) external onlyOwner {
        require(chainID > 0, "zero chainID");
        require(tokenAddr != address(0), "zero tokenAddr");
        _tokenPrices[chainID][tokenAddr] = price;
    }

    function setTokenPrice(string calldata tokenID, string calldata price) external onlyOwner {
        require(bytes(tokenID).length > 0, "empty tokenID");
        _tokenPricesByTokenID[tokenID] = price;
    }
}