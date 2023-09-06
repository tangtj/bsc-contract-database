// File: @openzeppelin/contracts/token/ERC20/IERC20.sol

// SPDX-License-Identifier: UNLICENSED
// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

// File: portfolio_contract.sol


pragma solidity ^0.8.21;

contract Portfolio {

    struct ContractInfo {
        address contractAddress;
        string name;
    }

    mapping (uint256 => ContractInfo) public contracts;
    string public telegram;
    string public discord;
    uint256 public contractCount = 0;
    address public owner;

    constructor() payable {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Caller is not the owner");
        _;
    }

    function addContract(address _contractAddress, string memory _name) public onlyOwner {
        contracts[contractCount + 1] = ContractInfo(_contractAddress, _name);
        contractCount += 1;
    }

    function removeContract(uint256 _id) public onlyOwner {
        require(_id <= contractCount, "Contract not found");
        if(_id != contractCount){
            contracts[_id] = contracts[contractCount];
        }
        delete contracts[contractCount];
        contractCount -= 1;
    }

    function viewContracts(uint256 _id) public view returns (address, string memory) {
        return (contracts[_id].contractAddress, contracts[_id].name);
    }

    function viewAllContracts() public view returns (uint256[] memory, address[] memory, string[] memory) {
        uint256[] memory ids = new uint256[](contractCount);
        address[] memory addresses = new address[](contractCount);
        string[] memory names = new string[](contractCount);

        for (uint256 i = 0; i < contractCount; i++) {
            ids[i] = i + 1;
            addresses[i] = contracts[i + 1].contractAddress;
            names[i] = contracts[i + 1].name;
        }

        return (ids, addresses, names);
    }

    function updateTelegram(string memory _telegram) public onlyOwner {
        telegram = _telegram;
    }

    function updateDiscord(string memory _discord) public onlyOwner {
        discord = _discord;
    }

    function listTelegram() public view returns (string memory) {
        return telegram;
    }

    function listDiscord() public view returns (string memory) {
        return discord;
    }

    function donateBNB() public payable {}

    function donateToken(address _token, uint256 _amount) public {
        IERC20(_token).transferFrom(msg.sender, address(this), _amount);
    }

    function withdrawBNB(uint256 _amount, address payable _receiving) public onlyOwner {
        require(_amount <= address(this).balance, "Withdrawal amount exceeds balance");
        _receiving.transfer(_amount);
    }

    function withdrawToken(address _token, uint256 _amount, address _receiving) public onlyOwner {
        require(_amount <= IERC20(_token).balanceOf(address(this)), "Withdrawal amount exceeds balance");
        IERC20(_token).transfer(_receiving, _amount);
    }
}