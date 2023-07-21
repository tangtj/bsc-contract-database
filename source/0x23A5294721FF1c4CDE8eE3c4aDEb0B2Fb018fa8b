/**
 *Submitted for verification at BscScan.com on 2023-07-17
*/

/**
 *Submitted for verification at BscScan.com on 2023-06-04
*/

/**
 *Submitted for verification at BscScan.com on 2023-06-03
*/

/**
 *Submitted for verification at BscScan.com on 2023-06-01
*/

/**
 *Submitted for verification at BscScan.com on 2023-05-31
*/

/**
 *Submitted for verification at BscScan.com on 2023-03-11
*/

// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)
pragma solidity ^0.8.0;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

// File: @openzeppelin/contracts/access/Ownable.sol


// OpenZeppelin Contracts (last updated v4.7.0) (access/Ownable.sol)

pragma solidity ^0.8.0;


/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}






pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint);

    function balanceOf(address account) external view returns (uint);

    function transfer(address recipient, uint amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

interface StakingRewards {
    function balanceOfs(address account) external view returns (uint);
}


contract Team is Ownable {
    IERC20 public immutable usdtToken;//USDT
    IERC20 public immutable hoToken;//HO
    StakingRewards public  stake;
    mapping(address => uint) public balanceStake;
    bool public pasue;
    mapping(address => address) public relation;
    mapping(address => uint) public balanceOf;
    mapping(address => uint) public below1;
    mapping(address => uint) public below2;
    mapping(address => uint) public below3;
    mapping(address => bool) public white;
    mapping(address => bool) public captain;
	uint256 public limits;

    address public feer;
    uint256 public fees;


	event relations(address low,address high);
    modifier onlyWhite() {
        require(white[msg.sender] == true, "not authorized");
        _;
    }
    constructor() {
        usdtToken = IERC20(0x55d398326f99059fF775485246999027B3197955);
        hoToken = IERC20(0x868f0DfAD219C8C93d34b2414cf6F663FBCCe7FF);//ho address
        pasue = true;
        feer = msg.sender;
        limits = 1 * 10**18;
        fees = 200;
    }

    function setFeer(address _adr) external onlyOwner{
        feer = _adr;
    }
    function setFees(uint256 _num) external onlyOwner{
        require(_num < 1000,"must is < 1000");
        fees = _num;
    }
    function setLimit(uint256 _num) external onlyOwner{
        limits = _num;
    }

   function setPasue(uint256 _pasue) external onlyOwner{
        if(_pasue > 0){
            pasue = true;
        }else{
            pasue = false;
        }
    }

	function setBalanceStake(address[] calldata adr,uint256[] calldata num) external onlyWhite{

        for(uint i=0;i<adr.length;i++){
            balanceStake[adr[i]] +=num[i];
        }



    }

    function withdrawHo() external{
        uint num = balanceStake[msg.sender];
        balanceStake[msg.sender] = 0;
        if(num > 0){
            if(fees > 0){
                uint num2 = num * fees /1000 ;
                uint num1 = num-num2;
                hoToken.transfer(msg.sender,num1);
                hoToken.transfer(feer,num2);
            }else{
                hoToken.transfer(msg.sender,num);
            }
      
        }
    }

    function setStake(address _adr) external onlyOwner{
        stake = StakingRewards(_adr);
    }

    function bulid(address superior) external{

        require(captain[msg.sender] != true,"captain not below");
        require(relation[msg.sender] == address(0),"you have captain");

        require( captain[superior] == true || relation[superior] != address(0),"superior not be to");

        relation[msg.sender] = superior;

        below1[superior] += 1;

        if(relation[superior] != address(0)){
            below2[relation[superior]] += 1;
        }

        if(relation[relation[superior]] != address(0)){
            below3[relation[relation[superior]]] += 1;
        }

        emit relations(msg.sender,superior);
    }

    function setbulid(address[] memory _h,address[] memory _l) external onlyOwner{
        for(uint i=0;i<_h.length;i++){
            relation[_l[i]] = _h[i];  
            below1[_h[i]] += 1;

            if(relation[_h[i]] != address(0)){
                below2[relation[_h[i]]] += 1;
            }

            if(relation[relation[_h[i]]] != address(0)){
                below3[relation[relation[_h[i]]]] += 1;
            }   
        }
    }

    function setWhit(address[] memory _adr) external onlyOwner{
        for(uint i=0;i<_adr.length;i++){
            white[_adr[i]] = true;
        }

    }


    function setCaptain(address[] memory _adr) external onlyOwner{
        for(uint i=0;i<_adr.length;i++){
            captain[_adr[i]] = true;
        }

    }


    function setBalance(address[] memory _adr,uint256[] memory _num) external onlyWhite{
         for(uint i=0;i<_adr.length;i++){
            balanceOf[_adr[i]] += _num[i];
         }
        




    }

    function withdraw() external{
        uint num = balanceOf[msg.sender];
        balanceOf[msg.sender] = 0;
		
		if(num > 0){
            uint num2 = num * fees /1000 ;
            uint num1 = num-num2;
            usdtToken.transfer(msg.sender,num1);
            usdtToken.transfer(feer,num2);
        }
		
    }


    function isCan(address _adr) public view virtual  returns (bool) {
        return captain[_adr] || relation[_adr] != address(0);
    }


}