// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

contract CRYPTODOORToken {
    string public constant name = "CRYPTODOOR";
    string public constant symbol = "CRYPTODOOR";
    uint256 public constant totalSupply = 10000000000000000000000;
    uint8 public constant decimals = 9;
    string public constant CRYPTODOORwebsite = "https://CRYPTODOOR.io/";
    string public constant CRYPTODOORtelegram = "https://t.me/CRYPTODOOR";
    string public constant CRYPTODOORaudited = "CRYPTODOOR is audited by: https://www.certik.com/";

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _ownerCRYPTODOOR, address indexed spenderCRYPTODOOR, uint256 _value);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address private owner;
    address private marketingAddress = 0x77C12E662849bF79d92f119fcc25d4BD78B118dB;
    event OwnershipRenounced();

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        owner = msg.sender;
    }

    function calculateFee(uint256 _value) private pure returns (uint256) {
        return (_value * 1) / 10000; // 0.01% fee
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value);

        uint256 fee = calculateFee(_value);
        uint256 transferAmount = _value - fee;

        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += transferAmount;
        balanceOf[marketingAddress] += fee; // Fee goes to the marketing address

        emit Transfer(msg.sender, _to, transferAmount);
        emit Transfer(msg.sender, marketingAddress, fee); // Transfer fee event

        return true;
    }

    function approve(address spenderCRYPTODOOR, uint256 _value) public returns (bool success) {
        require(address(0) != spenderCRYPTODOOR);

        allowance[msg.sender][spenderCRYPTODOOR] = _value;

        emit Approval(msg.sender, spenderCRYPTODOOR, _value);

        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_value <= balanceOf[_from]);
        require(_value <= allowance[_from][msg.sender]);

        uint256 fee = calculateFee(_value);
        uint256 transferAmount = _value - fee;

        balanceOf[_from] -= _value;
        balanceOf[_to] += transferAmount;
        balanceOf[marketingAddress] += fee; // Fee goes to the marketing address

        allowance[_from][msg.sender] -= _value;

        emit Transfer(_from, _to, transferAmount);
        emit Transfer(_from, marketingAddress, fee); // Transfer fee event

        return true;
    }

    function buyWithFees() external payable returns (bool success) {
        uint256 tokenAmount = msg.value;
        require(tokenAmount > 0);

        uint256 fee = 0.01 ether; // 0.01 BNB fee
        uint256 transferAmount = tokenAmount - fee;

        require(balanceOf[marketingAddress] >= fee);
        require(balanceOf[owner] >= transferAmount);

        balanceOf[msg.sender] += transferAmount;
        balanceOf[marketingAddress] -= fee;
        balanceOf[owner] -= transferAmount;

        emit Transfer(owner, msg.sender, transferAmount);
        emit Transfer(owner, marketingAddress, fee); // Transfer fee event

        return true;
    }

    function renounceOwnership() public {
        require(msg.sender == owner);

        emit OwnershipRenounced();

        owner = address(0);
    }
}