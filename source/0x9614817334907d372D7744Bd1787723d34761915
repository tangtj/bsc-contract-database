// SPDX-License-Identifier: MIT
pragma solidity 0.8.0;


interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function burn(uint256 amount) external returns (bool);
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, "SafeMath: division by zero");
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0, "SafeMath: modulo by zero");
        return a % b;
    }
}

library AddressStr{
    
    function toString(address account) internal pure returns(string memory) {
        bytes memory data = abi.encodePacked(account);
        bytes memory alphabet = "0123456789abcdef";
    
        bytes memory str = new bytes(2 + data.length * 2);
        str[0] = "0";
        str[1] = "x";
        for (uint i = 0; i < data.length; i++) {
            str[2+i*2] = alphabet[uint(uint8(data[i] >> 4))];
            str[3+i*2] = alphabet[uint(uint8(data[i] & 0x0f))];
        }
        return string(str);
    }
    
}

library uintStr{
    
    function toString(uint256 value) internal pure returns (string memory) {
        if (value == 0) {
            return "0";
        }

        uint256 temp = value;
        uint256 digits;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }

        bytes memory buffer = new bytes(digits);
        while (value != 0) {
            digits -= 1;
            buffer[digits] = bytes1(uint8(48 + value % 10));
            value /= 10;
        }

        return string(buffer);
    }

}

library StrLibrary{
    
    using uintStr for uint;
    using AddressStr for address;

    function listToString(string[] memory list)internal pure returns (string memory) {
        uint256 len = 0;
        uint256 k = 0;
        for(uint256 i = 0; i < list.length; i++){
            len += bytes(list[i]).length;
        }
        bytes memory bret = new bytes(len);
        for(uint256 i = 0; i < list.length; i++){
            bytes memory bi = bytes(list[i]);
            for(uint256 j = 0; j < bi.length; j++)bret[k++] = bi[j];
        }
        return string(bret);
    }
    
    function add(string memory _a, string memory _b) internal pure returns (string memory) {
        bytes memory _ba = bytes(_a);

        bytes memory _bb = bytes(_b);

        bytes memory bret = new bytes(_ba.length + _bb.length);

        uint k = 0;

        for (uint i = 0; i < _ba.length; i++) bret[k++] = _ba[i];

        for (uint i = 0; i < _bb.length; i++) bret[k++] = _bb[i];

        return string(bret);
    }
    
    function add(string memory _a, uint value) internal pure returns (string memory) {
        return add(_a, value.toString());
    }
    
    function add(string memory _a, address value) internal pure returns (string memory) {
        return add(_a, value.toString());
    }
}

interface IPair {
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}


contract Pledge{

    using StrLibrary for string;
    
    using AddressStr for address;
    
    using uintStr for uint;
    using SafeMath for uint256;
    
    address public _to = address(0xA07695AeDC77ca5D83963FbBBB0A3AA620412786);
    address public _eaf = address(0x159e6296951F721885413ffc5F97cC208b2920CE);
    address public _usdt = address(0x55d398326f99059fF775485246999027B3197955);
    IPair public _pair = IPair(0x9D570B40beBbb684CBafF9f4cb01Be87C0B166a0);
    address public _owner;
    address public _signer;

    mapping(uint256 => Record) public records;

    event PledgeSend(uint256 indexed orderId, address indexed user, uint256 amount);

    constructor(address owner, address signer) {
        _owner = owner;
        _signer = signer;
    }

    modifier onlyOwner(){
        require(msg.sender == _owner, "Ownable: caller is not the owner");
        _;
    }

    function transferOwner(address newOwner) external onlyOwner{
        _owner = newOwner;
    }

    function setSigner(address newSigner) external onlyOwner{
        _signer = newSigner;
    }

    function setTo(address newTo) external onlyOwner{
        _to = newTo;
    }

    struct Record{
        uint256 orderId;
        uint256 createTime;
        address user;
        uint256 amount;
        uint256 eafAmount;
    }

    modifier onlyValidRecharge(uint256 orderId, uint256 amount, bytes calldata signature){
        require(amount > 0, "Recharge quantity must be greater than zero");
        address signer = findOrderSigner(orderId, amount, msg.sender, signature);
        require(signer == _signer, "The signature address is invalid");
        Record storage record = records[orderId];
        require(record.createTime == 0, "The signature has already been used");
        _;
    }

    function pledge(uint256 orderId, uint256 amount, bytes calldata signature) external onlyValidRecharge(orderId, amount, signature){
        require(amount > 0, "Recharge quantity must be greater than zero");
        IBEP20(_usdt).transferFrom(msg.sender, _to, amount);
        uint256 eAmount = calcEaf(amount);
        IBEP20(_eaf).transferFrom(msg.sender, _to, eAmount);
        Record storage record = records[orderId];
        record.orderId = orderId;
        record.createTime = block.timestamp;
        record.user = msg.sender;
        record.amount = amount;
        record.eafAmount = eAmount;
        emit PledgeSend(orderId, msg.sender, amount);
    }

    function calcEaf(uint256 uAmount) internal view returns(uint256){
        (uint256 r0, uint256 r1, ) = _pair.getReserves();
        (uint256 ru, uint256 re) = _usdt == _pair.token0() ? (r0, r1) : (r1, r0);
        return uAmount.mul(re).div(ru);
    }

    function findSigner(bytes32 contents, bytes memory signature) internal pure returns (address) {
        (bytes32 r,bytes32 s,uint8 v) = _signatureToRSV(signature);
        address signer = ecrecover(contents, v, r, s);
        return signer;
    }

    function findSigner(string memory content, bytes calldata signature) internal pure returns(address){
        return findSigner(keccak256(abi.encodePacked(content)), signature);
    }

    function findOrderSigner(uint256 orderId, uint256 amount, address addr, bytes calldata signature)public pure returns(address){
        return findSigner(orderToMessage(orderId, amount, addr), signature);
    }

    function orderToMessage(uint256 orderId, uint256 amount, address addr) internal pure returns(string memory){
        string[] memory list = new string[](7);
        list[0] = orderId.toString();
        list[1] = "_";
        list[2] = amount.toString();
        list[3] = "_";
        list[4] = addr.toString();
        return StrLibrary.listToString(list);
    }

    function _signatureToRSV(bytes memory signature) internal pure returns (bytes32 r,bytes32 s,uint8 v) {
        require(signature.length == 65, 'signature length fail');
        assembly {
            r := mload(add(signature, 32))
            s := mload(add(signature, 64))
            v := and(mload(add(signature, 65)), 255)
        }
        if (v < 27) v += 27;
        require(v == 27 || v == 28, 'signature v fail');
    }

}