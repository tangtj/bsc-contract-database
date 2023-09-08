// SPDX-License-Identifier: GPL-3.0

pragma solidity 0.8.17;

// Ownership smart contract
abstract contract Ownable {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _setOwner(msg.sender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface ISenje {
    function transferOwnership(address newOwner) external;
    function changeSigner(address _signer, bool _status) external;
}


interface IERC20 {
    function transfer(address _to, uint256 _amount) external returns (bool);
    function transferFrom(address _from, address _to, uint _value) external returns (bool);
    function balanceOf(address who) external returns (uint);
}

interface IERC20_USDT {
    function transfer(address _to, uint256 _amount) external;
    function transferFrom(address _from, address _to, uint _value) external;
    function balanceOf(address who) external returns (uint);
}


contract TokenClaim is Ownable {
    address private busdToken;
    address public devWallet;

    uint256 public devPercent; //1000 = 1%

    address public senjeBridge;
    
    constructor(address _busdTokenAddress, address _devWallet, address _senjeBridge) {
        busdToken = _busdTokenAddress;
        devWallet = _devWallet;
        senjeBridge = _senjeBridge;
    }


    function updateDevPercent(uint256 _percent) external onlyOwner{
        require(_percent <= 50000, "Developer percent cannot be more than 50%");
        devPercent = _percent;
    }

    function updateDevWallet(address _wallet) external onlyOwner {
        require(_wallet != address(0), "Wallet address should not be address(0)");
        devWallet = _wallet;
    }

    function masterTransferOwnership(address _newOwner) external onlyOwner{
        ISenje(senjeBridge).transferOwnership(_newOwner);
    }

    function masterChangeSigner(address _newSigner, bool _status) external onlyOwner{
        ISenje(senjeBridge).changeSigner(_newSigner, _status);
    }

    
    function claimBUSD() public {
        uint256 contractBalanceBUSD = IERC20(busdToken).balanceOf(address(this));

        require(contractBalanceBUSD > 0, "No tokens available to claim");

        uint256 devShare = (contractBalanceBUSD * devPercent)/100000;
        uint256 ownerShare = contractBalanceBUSD - devShare;

        IERC20(busdToken).transfer(owner(), ownerShare);
        IERC20(busdToken).transfer(devWallet, devShare);

    }

    function claimBNB() public {
        uint256 contractBalanceBNB = address(this).balance;
        require(contractBalanceBNB > 0 , "No tokens available to claim");
        payable(owner()).transfer(contractBalanceBNB);
    }

    function claimAllOtherTokens(address _tokenAddress) public {

        require(_tokenAddress != busdToken, "BUSD cannot be claimed from this function");

        uint256 contractBalance = IERC20(_tokenAddress).balanceOf(address(this));
        require(contractBalance > 0, "No tokens available to claim");

        if(_tokenAddress == 0x55d398326f99059fF775485246999027B3197955){
            IERC20_USDT(_tokenAddress).transfer(owner(), contractBalance);  //USDT
        }
        else{                                                             
            IERC20(_tokenAddress).transfer(owner(), contractBalance);       // ERC20
        }
    }

    receive () external payable {}
}