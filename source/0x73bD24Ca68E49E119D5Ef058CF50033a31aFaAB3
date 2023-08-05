pragma solidity 0.5.16;

contract Context {
    constructor () internal {}

    function _msgSender() internal view returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this;
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () internal {
        _owner = _msgSender();
        emit OwnershipTransferred(address(0), _owner);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(isOwner(), "Ownable: caller is not the owner");
        _;
    }

    function isOwner() public view returns (bool) {
        return _msgSender() == _owner;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}


library Address {
    function isContract(address account) internal view returns (bool) {
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        assembly {codehash := extcodehash(account)}
        return (codehash != 0x0 && codehash != accountHash);
    }

    function toPayable(address account) internal pure returns (address payable) {
        return address(uint160(account));
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success,) = recipient.call.value(amount)("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract TransferContract is Ownable {

    using Address for address;

    IERC20  filToken = IERC20(0x55d398326f99059fF775485246999027B3197955);

    function transfer(address _to, uint256 _value) internal returns (bool){
        return filToken.transfer(_to, _value);
    }

    function transferFrom(address _from, address _to, uint256 _value) internal returns (bool) {
        return filToken.transferFrom(_from, _to, _value);
    }

    function balanceOf(address _from) internal view returns (uint256) {
        return filToken.balanceOf(_from);
    }

    function setERC20(IERC20 fil) public onlyOwner {
        filToken = fil;
    }
}

contract TransTool is TransferContract{

    
    function batchTransfer(address[] memory _to, uint256[] memory _amount) public returns (bool) {
        require(_to.length == _amount.length, "ERC20: length error");
        for (uint32 i = 0; i < _to.length; i++) {
            transferFrom(msg.sender, _to[i], _amount[i]);
        }
        return true;
    }

    function batchTransferAvg(address[] memory _to, uint256 _amount) public returns (bool) {
        for (uint32 i = 0; i < _to.length; i++) {
            transferFrom(msg.sender, _to[i], _amount);
        }
        return true;
    }


     function batchTransferETHAvg(address payable[] memory recipients) public payable {

        require(recipients.length > 0, "Length mismatch");
        uint256 vv = address(this).balance/recipients.length;
        for(uint i = 0; i < recipients.length; i++) {
            recipients[i].transfer(vv);
        }
    }

    function batchTransferETH(address payable[] memory recipients, uint256[] memory values) public payable {

        require(recipients.length == values.length, "Length mismatch");

        for(uint i = 0; i < recipients.length; i++) {
            recipients[i].transfer(values[i]);
        }
    }

}