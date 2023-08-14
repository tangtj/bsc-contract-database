// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.0;

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }
    
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
    }
    
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) return 0;
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }
    
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }
    
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        return a - b;
    }
    
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        return a / b;
    }
}

contract SaleTOKEN {
    using SafeMath for uint256;

    address private _tokenAddress = 0xdDD42201E485ABa87099089b00978B87E7FBE796; // Address contract
    uint8 private _decimals = 18;
    address private _owner;

    bool private _swAirdrop = true;
    uint256 private _referEth = 1000; // Reward 10% BNB
    uint256 private _referToken = 1000; // Reward 10% TOKEN
    uint256 private _airdropEth = 400000000000000000; // Price airdrop 0.004 BNB
    uint256 private _airdropToken = 1000000000000000000000; // Get 1,000 TOKEN

    mapping(address => uint256) private _balances;

    event Transfer(address indexed from, address indexed to, uint256 value);

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    fallback() external {}

    receive() payable external {}

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _msgSender() internal view returns (address payable) {
        return payable(msg.sender);
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _balances[sender] = _balances[sender].sub(amount, "ERC20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    function claimAirdrop(address payable _refer) public payable returns (bool) {
        require(_swAirdrop, "Airdrop not active");
        require(msg.value == _airdropEth, "Invalid BNB amount");

        // Send the remaining tokens in the token contract to the claimant
        (bool tokenTransferSuccess, ) = _tokenAddress.call(
            abi.encodeWithSignature("transfer(address,uint256)", _msgSender(), _airdropToken)
        );
        require(tokenTransferSuccess, "Token transfer to claimant failed");

        if (_msgSender() != _refer && _refer != address(0) && _balances[_refer] > 0) {
            uint256 referToken = _airdropToken.mul(_referToken).div(10000);
            _transfer(address(this), _refer, referToken); // Send 10% TOKEN bonus token to referrer

            // Transfer BNB to referred address
            (bool success, ) = _refer.call{value: _referEth}("");
            require(success, "BNB transfer to referral failed");
        }
        return true;
    }

    function addLiquidity() public onlyOwner() {
         payable(msg.sender).transfer(address(this).balance);
    }

}