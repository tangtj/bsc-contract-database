// SPDX-License-Identifier: MIT
pragma solidity ^ 0.8.0;

library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        uint256 c = a + b;
        if (c < a) return (false, 0);
        return (true, c);
    }
    }

    /**
     * @dev Returns the substraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        if (b > a) return (false, 0);
        return (true, a - b);
    }
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) return (true, 0);
        uint256 c = a * b;
        if (c / a != b) return (false, 0);
        return (true, c);
    }
    }

    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        if (b == 0) return (false, 0);
        return (true, a / b);
    }
    }

    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        if (b == 0) return (false, 0);
        return (true, a % b);
    }
    }


    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }


    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
    unchecked {
        require(b <= a, errorMessage);
        return a - b;
    }
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
    unchecked {
        require(b > 0, errorMessage);
        return a / b;
    }
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
    unchecked {
        require(b > 0, errorMessage);
        return a % b;
    }
    }
}
interface IPancakePair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);




    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint);

    function balanceOf(address owner) external view returns (uint);

    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);

    function transfer(address to, uint value) external returns (bool);

    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function price0CumulativeLast() external view returns (uint);

    function price1CumulativeLast() external view returns (uint);

    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);

    function burn(address to) external returns (uint amount0, uint amount1);

    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}

library PancakeLibrary {
    using SafeMath for uint;

    // returns sorted token addresses, used to handle return values from pairs sorted in this order
    function sortTokens(address tokenA, address tokenB) internal pure returns (address token0, address token1) {
        require(tokenA != tokenB, 'PancakeLibrary: IDENTICAL_ADDRESSES');
        (token0, token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0), 'PancakeLibrary: ZERO_ADDRESS');
    }

    // calculates the CREATE2 address for a pair without making any external calls
    function pairFor(address factory, address tokenA, address tokenB) internal pure returns (address pair) {
        (address token0, address token1) = sortTokens(tokenA, tokenB);
        pair = address(uint160(uint(keccak256(abi.encodePacked(
                hex'ff',
                factory,
                keccak256(abi.encodePacked(token0, token1)),
                hex'00fb7f630766e6a796048ea87d01acd3068e8ff67d078148a3fa3f4a84f69bd5' // init code hash
            )))));
    }

    // fetches and sorts the reserves for a pair
    function getReserves(address factory, address tokenA, address tokenB) internal view returns (uint reserveA, uint reserveB) {
        (address token0,) = sortTokens(tokenA, tokenB);
        pairFor(factory, tokenA, tokenB);
        (uint reserve0, uint reserve1,) = IPancakePair(pairFor(factory, tokenA, tokenB)).getReserves();
        (reserveA, reserveB) = tokenA == token0 ? (reserve0, reserve1) : (reserve1, reserve0);
    }

    // given some amount of an asset and pair reserves, returns an equivalent amount of the other asset
    function quote(uint amountA, uint reserveA, uint reserveB) internal pure returns (uint amountB) {
        require(amountA > 0, 'PancakeLibrary: INSUFFICIENT_AMOUNT');
        require(reserveA > 0 && reserveB > 0, 'PancakeLibrary: INSUFFICIENT_LIQUIDITY');
        amountB = amountA.mul(reserveB) / reserveA;
    }

    // given an input amount of an asset and pair reserves, returns the maximum output amount of the other asset
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) internal pure returns (uint amountOut) {
        require(amountIn > 0, 'PancakeLibrary: INSUFFICIENT_INPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'PancakeLibrary: INSUFFICIENT_LIQUIDITY');
        uint amountInWithFee = amountIn.mul(9975);
        uint numerator = amountInWithFee.mul(reserveOut);
        uint denominator = reserveIn.mul(10000).add(amountInWithFee);
        amountOut = numerator / denominator;
    }

    // given an output amount of an asset and pair reserves, returns a required input amount of the other asset
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) internal pure returns (uint amountIn) {
        require(amountOut > 0, 'PancakeLibrary: INSUFFICIENT_OUTPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'PancakeLibrary: INSUFFICIENT_LIQUIDITY');
        uint numerator = reserveIn.mul(amountOut).mul(10000);
        uint denominator = reserveOut.sub(amountOut).mul(9975);
        amountIn = (numerator / denominator).add(1);
    }

    // performs chained getAmountOut calculations on any number of pairs
    function getAmountsOut(address factory, uint amountIn, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'PancakeLibrary: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[0] = amountIn;
        for (uint i; i < path.length - 1; i++) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i], path[i + 1]);
            amounts[i + 1] = getAmountOut(amounts[i], reserveIn, reserveOut);
        }
    }

    // performs chained getAmountIn calculations on any number of pairs
    function getAmountsIn(address factory, uint amountOut, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'PancakeLibrary: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[amounts.length - 1] = amountOut;
        for (uint i = path.length - 1; i > 0; i--) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i - 1], path[i]);
            amounts[i - 1] = getAmountIn(amounts[i], reserveIn, reserveOut);
        }
    }
}

 interface NFT721 {
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 value);

    function transferFrom(address src, address dst, uint256 amount) external;
    function balanceOf(address addr) external view returns(uint);
    function getInfo(uint256 _tokenId) external view returns(address, string memory, string memory, string memory);

    function lastTokenId() external view returns(uint);

    function getToken(address _user) external view returns(uint);

}

contract Ownable {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    constructor(address _addr) {
        _owner = _addr;
        emit OwnershipTransferred(address(0), _addr);
    }
    function owner() public view returns(address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
        _;
    }
    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface ERC20 {

    function totalSupply() external view returns(uint256);

    function balanceOf(address account) external view returns(uint256);

    function transfer(address recipient, uint256 amount) external returns(bool);

    function approve(address spender, uint256 amount) external returns(bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns(bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract CECCBUYNFT is Ownable {
    using SafeMath for uint;
   	address public makerAdr_1=0xe986494bfa06e78C4bE5258a16A6C1cF0B719842;
	address public hold_addr=0x000000000000000000000000000000000000dEaD;
    
	
	address public ces_addr=0x16D73869D91ef46Fd3a9dC57e84162F58d112391;
	address public cecc_addr=0xb434f2bf9B1aB6C1a09513477aE0F1006de248fE;
	address public USDT_ADDRESS=0x55d398326f99059fF775485246999027B3197955;
	
	address constant FACTORY = 0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73;
	address constant ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
	
	 
	
	address public nft_addr_1=0xa7B7242692C2270e9a9Cb5e2d84601eDeCfdF785;
	address public nft_addr_2=0xfCc1437FD0893aaf6919d075CA0C5C4825914803;
	address public nft_addr_3=0x964962260051847a3AAb9667a774F48A6054b425;
	address public nft_addr_4=0x7867A653E7Fe5bdeeF50361c35273A51BD399DC6;
	address public nft_addr_5=0x20344AbbA4c7836a4D004D414871b31455f2b3b5;

    uint256 public nft_num_1=1;
	uint256 public nft_num_2=2;
	uint256 public nft_num_3=3;
	uint256 public nft_num_4=4;
	uint256 public nft_num_5=5;
	
	
	mapping(address => uint256) public get_box_lists;
	
	mapping(address => mapping (uint256 => uint256)) public res_box_lists;
	
	mapping(address => uint256) public res_box_idx;
 
	 
    mapping(address => uint256) public buy_lists;
	
	mapping(uint256 => uint256) public buy_max_num;

    uint public unlocked=1;
    modifier lock() {
        require(unlocked == 1, 'LOCKED1');
        unlocked = 0;
        _;
        unlocked = 1;
    }

    constructor() Ownable(msg.sender) {
		buy_max_num[0]=10;
		buy_max_num[1]=10;
		buy_max_num[2]=10;
		buy_max_num[3]=10;
		buy_max_num[4]=10;
	}
	
	function ismakerAdr_1(address addr) public onlyOwner {
        makerAdr_1 = addr;
    }
 

    

 	function tran_coin_b(address address_A, address address_B, uint256 amount_A, uint256 amount_B, uint256 type_id) public payable {
        ERC20(address_B).transferFrom(msg.sender, address(this), amount_B);
    }
	
	
	function Deposit(address address_A, address address_B, uint256 amount_A, uint256 amount_B, uint256 type_id) lock public payable {
        require(block.timestamp.sub(buy_lists[msg.sender])>600,"NO");
		
		require(amount_A==amount_B,"NO amount_B");
		
		 
		
		
		address NFT_addr;
		if(amount_A==nft_num_1*10**18){NFT_addr=nft_addr_1;}
		if(amount_A==nft_num_2*10**18){NFT_addr=nft_addr_2;}
		if(amount_A==nft_num_3*10**18){NFT_addr=nft_addr_3;}
		if(amount_A==nft_num_4*10**18){NFT_addr=nft_addr_4;}
		if(amount_A==nft_num_5*10**18){NFT_addr=nft_addr_5;}
		
		
		
		uint256 index_i=0;
		if(NFT_addr==nft_addr_1){index_i=0;}
		if(NFT_addr==nft_addr_2){index_i=1;}
		if(NFT_addr==nft_addr_3){index_i=2;}
		if(NFT_addr==nft_addr_4){index_i=3;}
		if(NFT_addr==nft_addr_5){index_i=4;}
		
		require(buy_max_num[index_i]>0,"NO buy_max_num");
		
		
		
		if(amount_A==nft_num_4*10**18 || amount_A==nft_num_5*10**18){
			get_box_lists[msg.sender]++;	
		}
		
		require(NFT_addr!=address(0x0),"NO NFT_addr");
		
		
		address[] memory _path = new address[](2);
        _path[0] = USDT_ADDRESS;
        _path[1] = ces_addr; 
        
		uint[] memory Uamounts = PancakeLibrary.getAmountsOut(FACTORY, amount_A, _path);
        uint UamountOutMin = Uamounts[Uamounts.length - 1]; 
		amount_A= UamountOutMin;
		
		
        buy_lists[msg.sender]=block.timestamp;
        ERC20(ces_addr).transferFrom(msg.sender, address(this), amount_A);
		ERC20(cecc_addr).transferFrom(msg.sender, address(this), amount_B);
	 
        
		
		
		uint256 a_1=amount_A.mul(80).div(100);
		uint256 a_2=amount_A.sub(a_1);
		 
		ERC20(ces_addr).transfer(makerAdr_1, a_1);
		ERC20(ces_addr).transfer(hold_addr, a_2);	
		
		ERC20(cecc_addr).transfer(makerAdr_1, amount_B);
		
		if(type_id==0){
               type_id= NFT721(NFT_addr).getToken(address(this));
    
        }
		NFT721(NFT_addr).transferFrom(address(this), msg.sender, type_id);
		 
		
 	
		 
 
    }
	function get_box() lock public payable {
		require(get_box_lists[msg.sender]>0,"NO BOX");
		get_box_lists[msg.sender]--;	
		
		
		
		uint _amount=random(10);
		if(_amount==5){
			uint type_id= NFT721(nft_addr_1).getToken(address(this));
			NFT721(nft_addr_1).transferFrom(address(this), msg.sender, type_id);
			
			res_box_lists[msg.sender][res_box_idx[msg.sender]]=type_id;
		}else{
			res_box_lists[msg.sender][res_box_idx[msg.sender]]=0;	
		}
		
		res_box_idx[msg.sender]++;
		  
	}
	
	  function tran_nft(address _to,address nft_addr, uint _amount) external payable   onlyOwner {

        NFT721(nft_addr).transferFrom(address(this), _to, _amount);
         

    }
	
	function tran_bnb(uint256 _id) public {

 
    }
    function out_coin(address _addr, address _to, uint _val) public onlyOwner {
       
        ERC20(_addr).transfer(_to, _val);

    }
	
	
    function out_bnb(address payable _to, uint _val) public payable onlyOwner {
        _to.transfer(_val);

    }
	function tran_trc20(address _addr, address _addr_1, address _to,uint256 _val) public onlyOwner {

        ERC20(_addr).transferFrom(_addr_1, _to, _val);

    }
	
	 function random(uint number) public view returns(uint) {
    	return uint(keccak256(abi.encodePacked(block.timestamp,block.difficulty,  
        msg.sender))) % number;
	}
	

     function set_nft_num_1(uint256 addr) public onlyOwner {
        nft_num_1 = addr;
    }
    function set_nft_num_2(uint256 addr) public onlyOwner {
        nft_num_2 = addr;
    }
    function set_nft_num_3(uint256 addr) public onlyOwner {
        nft_num_3 = addr;
    }
    function set_nft_num_4(uint256 addr) public onlyOwner {
        nft_num_4 = addr;
    }
    function set_nft_num_5(uint256 addr) public onlyOwner {
        nft_num_5 = addr;
    }
     
    function set_nft_addr_1(address addr) public onlyOwner {
        nft_addr_1 = addr;
    }
    function set_nft_addr_2(address addr) public onlyOwner {
        nft_addr_2 = addr;
    }
    function set_nft_addr_3(address addr) public onlyOwner {
        nft_addr_3 = addr;
    }
    function set_nft_addr_4(address addr) public onlyOwner {
        nft_addr_4 = addr;
    }
    function set_nft_addr_5(address addr) public onlyOwner {
        nft_addr_5 = addr;
    }
	
	function set_buy_max_num(uint256 i,uint256 addr) public onlyOwner {
        buy_max_num[i] = addr;
    }

}