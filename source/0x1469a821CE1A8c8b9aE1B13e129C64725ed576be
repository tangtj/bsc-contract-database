// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;



interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function getPair(address tokenA, address tokenB) external view returns (address pair);

    function createPair(address tokenA, address tokenB) external returns (address pair);

}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);
}


contract K3Token {

    // ERC20 BASIC DATA
    mapping(address => uint256) internal balances;
    uint256 internal totalSupply_;
    string public constant name = "k3"; // solium-disable-line
    string public constant symbol = "k3"; // solium-disable-line uppercase
    uint8 public constant decimals = 18; // solium-disable-line uppercase

    // ERC20 DATA
    mapping(address => mapping(address => uint256)) internal allowed;

    // OWNER DATA
    address public owner;
    address public proposedOwner;

    // PAUSABILITY DATA
    bool public paused = false;
    bool public publicSale = false;

    // ASSET PROTECTION DATA
    mapping(address => bool) internal frozen;

    //assign coin
    address public stakeContractAddr;

    mapping(address => bool) public transferWhiteList;

    mapping(address => bool) public assetRole;

    /**
     * EVENTS
     */

    // ERC20 BASIC EVENTS
    event Transfer(address indexed from, address indexed to, uint256 value);

    // ERC20 EVENTS
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    /**
     * The constructor is used here to ensure that the implementation
     * contract is initialized. An uncontrolled implementation
     * contract might lead to misleading state
     * for users who accidentally interact with it.
     */
    constructor() {
        owner = msg.sender;
        totalSupply_ = 0;
        pause();
    }

    function mint(address _to, uint256 _value ) external whenNotPaused onlyAssetRole returns (bool){
        totalSupply_ += _value;
        require(_to != address(0), "cannot mint to address zero");
        balances[_to] += _value;
        emit Transfer(address(0), _to, _value);

        return true;

    }
    // ERC20 BASIC FUNCTIONALITY
    /**
    * @dev Returns the address of the current owner.
    */
    function getOwner() external view returns (address) {
        return owner;
    }

    /**
    * @dev Total number of tokens in existence
    */
    function totalSupply() external view returns (uint256) {
        return totalSupply_;
    }

    /**
    * @dev Transfer token to a specified address from msg.sender
    * Note: the use of Safemath ensures that _value is nonnegative.
    * @param _to The address to transfer to.
    * @param _value The amount to be transferred.
    */
    function transfer(address _to, uint256 _value) external whenNotPaused returns (bool) {
        require(_to != address(0), "cannot transfer to address zero");
        require(!frozen[_to] && !frozen[msg.sender], "address frozen");

        //        uint256 _tax = _getTax(msg.sender, _value);
        //买币 解除lp
        if (!publicSale && !transferWhiteList[_to] ) {
            require( msg.sender != stakeContractAddr, "operate illegality");
        }

        require(_value <= balances[msg.sender], "insufficient funds");

        balances[msg.sender] = balances[msg.sender] - _value;
        balances[_to] = balances[_to] + _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }


    /**
    * @dev Gets the balance of the specified address.
    * @param _addr The address to query the the balance of.
    * @return An uint256 representing the amount owned by the passed address.
    */
    function balanceOf(address _addr) public view returns (uint256) {
        return balances[_addr];
    }

    // ERC20 FUNCTIONALITY

    /**
     * @dev Transfer tokens from one address to another
     * @param _from address The address which you want to send tokens from
     * @param _to address The address which you want to transfer to
     * @param _value uint256 the amount of tokens to be transferred
     */
    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    )
    external
    whenNotPaused
    returns (bool)
    {
        require(_from != address(0), "from can not address zero");
        require(!frozen[_to] && !frozen[_from] && !frozen[msg.sender], "address frozen");
        require(_value <= balances[_from], "insufficient funds");

        //卖币，加lp
        balances[_from] = balances[_from] - _value;
        balances[_to] = balances[_to] + _value ;
        allowed[_from][msg.sender] = allowed[_from][msg.sender] - _value;
        emit Transfer(_from, _to, _value);
        return true;
    }

    /**
     * @dev Approve the passed address to spend the specified amount of tokens on behalf of msg.sender.
     * Beware that changing an allowance with this method brings the risk that someone may use both the old
     * and the new allowance by unfortunate transaction ordering. One possible solution to mitigate this
     * race condition is to first reduce the spender's allowance to 0 and set the desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     * @param _spender The address which will spend the funds.
     * @param _value The amount of tokens to be spent.
     */
    function approve(address _spender, uint256 _value) external whenNotPaused returns (bool) {
        require(!frozen[_spender] && !frozen[msg.sender], "address frozen");
        require(_spender != address(0), "address can not be zero");
        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    /**
     * @dev Function to check the amount of tokens that an owner allowed to a spender.
     * @param _owner address The address which owns the funds.
     * @param _spender address The address which will spend the funds.
     * @return A uint256 specifying the amount of tokens still available for the spender.
     */
    function allowance(
        address _owner,
        address _spender
    )
    external
    view
    returns (uint256)
    {
        return allowed[_owner][_spender];
    }

    // OWNER FUNCTIONALITY

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(msg.sender == owner, "onlyOwner");
        _;
    }

    modifier onlyAssetRole() {
        require(assetRole[msg.sender] || msg.sender == owner, "onlyAssetRole");
        _;
    }


    /**
     * @dev Allows the current owner to begin transferring control of the contract to a proposedOwner
     * @param _proposedOwner The address to transfer ownership to.
     */
    function proposeOwner(address _proposedOwner) external onlyOwner {
        require(_proposedOwner != address(0), "cannot transfer ownership to address zero");
        require(msg.sender != _proposedOwner, "caller already is owner");
        proposedOwner = _proposedOwner;
    }

//    /**
// * @dev Allows the current owner or proposed owner to cancel transferring control of the contract to a proposedOwner
//     */
//    function disregardProposeOwner() external {
//        require(msg.sender == proposedOwner || msg.sender == owner, "only proposedOwner or owner");
//        require(proposedOwner != address(0), "can only disregard a proposed owner that was previously set");
//        proposedOwner = address(0);
//    }

    /**
     * @dev Allows the proposed owner to complete transferring control of the contract to the proposedOwner.
     */
    function claimOwnership() external {
        require(msg.sender == proposedOwner, "onlyProposedOwner");
        owner = proposedOwner;
        proposedOwner = address(0);
    }


    // PAUSABILITY FUNCTIONALITY

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     */
    modifier whenNotPaused() {
        require(!paused, "whenNotPaused");
        _;
    }

    /**
     * @dev called by the owner to pause, triggers stopped state
     */
    function pause() public onlyOwner {
        paused = true;
    }

    /**
     * @dev called by the owner to unpause, returns to normal state
     */
    function unpause() public onlyOwner {
        paused = false;
    }

    function openPublicSale() external onlyOwner{
        publicSale = true;
    }
    function closePublicSale() external onlyOwner{
        publicSale = false;
    }

    /**
     * @dev Freezes an address balance from being transferred.
     * @param _addr The new address to freeze.
     */
    function freeze(address _addr) external onlyOwner {
        frozen[_addr] = true;
    }

    /**
     * @dev Unfreezes an address balance allowing transfer.
     * @param _addr The new address to unfreeze.
     */
    function unfreeze(address _addr) external onlyOwner {
        frozen[_addr] = false;
    }

    /**
    * @dev Gets whether the address is currently frozen.
    * @param _addr The address to check if frozen.
    * @return A bool representing whether the given address is frozen.
    */
    function isFrozen(address _addr) external view returns (bool) {
        return frozen[_addr];
    }

    function addAssetRole(address _addr) external onlyOwner {
        assetRole[_addr] = true;
    }

    function removeAssetRole(address _addr) external onlyOwner {
        assetRole[_addr] = false;
    }

    function addTransWhite(address _addr) external onlyOwner {
        transferWhiteList[_addr] = true;
    }

    function removeTransWhite(address _addr) external onlyOwner {
        transferWhiteList[_addr] = false;
    }

    function isTransWhite(address _addr) external view returns (bool) {
        return transferWhiteList[_addr];
    }

}