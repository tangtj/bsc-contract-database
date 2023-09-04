// SPDX-License-Identifier: MIT
pragma solidity 0.8.6;

    abstract contract BP20 {
    function totalSupply() public virtual view returns (uint);
    function balanceOf(address tokenOwner) public virtual view returns (uint balance);
    function allowance(address tokenOwner, address spender) public virtual view returns (uint remaining);
    function transfer(address to, uint tokens) public virtual returns (bool success);
    function approve(address spender, uint tokens) public virtual returns (bool success);
    function transferFrom(address from, address to, uint tokens) public virtual returns (bool success);

    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}

abstract contract Contexta {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}


contract Math {
 function tryAdd(uint bd1, uint bd2) internal pure returns (bool, uint) {
        unchecked {
            uint bd3 = bd1 + bd2;
            if (bd3 < bd1) return (false, 0);
            return (true, bd3);
        }
    }

 
    function trySub(uint bd1, uint bd2) internal pure returns (bool, uint) {
        unchecked {
            if (bd2 > bd1) return (false, 0);
            return (true, bd1 - bd2);
        }
    }

   
    function tryMul(uint bd1, uint bd2) internal pure returns (bool, uint) {
        unchecked {
            // Gas optimization: this is cheaper than requiring 'bd1' not being zero, but the
            // benefit is lost if 'bd2' is also tested.
            // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
            if (bd1 == 0) return (true, 0);
            uint bd3 = bd1 * bd2;
            if (bd3 / bd1 != bd2) return (false, 0);
            return (true, bd3);
        }
    }


    function tryDiv(uint bd1, uint bd2) internal pure returns (bool, uint) {
        unchecked {
            if (bd2 == 0) return (false, 0);
            return (true, bd1 / bd2);
        }
    }


    function tryMod(uint bd1, uint bd2) internal pure returns (bool, uint) {
        unchecked {
            if (bd2 == 0) return (false, 0);
            return (true, bd1 % bd2);
        }
    }

  
    function add(uint bd1, uint bd2) internal pure returns (uint) {
        return bd1 + bd2;
    }

   
    function sub(uint bd1, uint bd2) internal pure returns (uint bd3) {
        require(bd2 <= bd1);
        bd3 = bd1 - bd2;
    }


    function mul(uint bd1, uint bd2) internal pure returns (uint) {
        return bd1 * bd2;
    }

 
    function div(uint bd1, uint bd2) internal pure returns (uint) {
        return bd1 / bd2;
    }


    function mod(uint bd1, uint bd2) internal pure returns (uint) {
        return bd1 % bd2;
    }


    function sub(uint bd1, uint bd2, string memory errorMessage) internal pure returns (uint bd3) {
        unchecked {
            require(bd2 <= bd1, errorMessage);
            bd3 = bd1 - bd2;
        }
    }


    function div(uint bd1, uint bd2, string memory errorMessage) internal pure returns (uint) {
        unchecked {
            require(bd2 > 0, errorMessage);
            return bd1 / bd2;
        }
    }

    function mod(uint bd1, uint bd2, string memory errorMessage) internal pure returns (uint) {
        unchecked {
            require(bd2 > 0, errorMessage);
            return bd1 % bd2;
        }
    }
   
}

contract Ownable is Contexta {
    address private _owner;
    address private _previousOwner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor ()  {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }


    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

}

contract ArticFinance is BP20, Contexta , Math, Ownable {
    string public name =  "Artic";
    string public symbol =  "ARTIC";
    uint8 public decimals = 9;
    uint public _totalSupply = 1*10**11 * 10**9;

    mapping(address => uint) balances;
    mapping(address => mapping(address => uint)) allowed;

    constructor() {
        balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }
    

    function totalSupply() public override view returns (uint) {
        return _totalSupply - balances[address(0)];
    }

    function balanceOf(address tokenOwner) public override view returns (uint balance) {
        return balances[tokenOwner];
    }

    function allowance(address tokenOwner, address spender) public override view returns (uint remaining) {
        return allowed[tokenOwner][spender];
    }
    
    function _transfer(address sender, address recipient, uint256 amount) internal virtual {
        require(sender != address(0), "BEAC20: transfer from the zero address");
        require(recipient != address(0), "BEAC20: transfer to the zero address");

        _beforeTokenTransfer(sender, recipient, amount);

        uint256 senderBalance = balances[sender];
        require(senderBalance >= amount, "BEAC20: transfer amount exceeds balance");
        unchecked {
            balances[sender] = senderBalance - amount;
        }
        balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }

    function transfer(address to, uint tokens) public override returns (bool success) {
        _transfer(msg.sender, to, tokens);
        emit Transfer(msg.sender, to, tokens);
        return true;
    }

    function approve(address spender, uint tokens) public override returns (bool success) {
        allowed[msg.sender][spender] = tokens;
        emit Approval(msg.sender, spender, tokens);
        return true;
    }

    function transferFrom(address from, address to, uint tokens) public override returns (bool success) {
        allowed[from][msg.sender] = sub(allowed[from][msg.sender], tokens);
        _transfer(from, to, tokens);
        emit Transfer(from, to, tokens);
        return true;
    }
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual { }


}