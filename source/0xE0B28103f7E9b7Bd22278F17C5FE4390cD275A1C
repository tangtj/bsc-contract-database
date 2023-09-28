/**
 *Submitted for verification at BscScan.com on 2022-02-20
*/

/**
 *Submitted for verification at BscScan.com on 2022-02-08
*/

/**
 *Submitted for verification at BscScan.com on 2022-01-20
*/

// SPDX-License-Identifier: Unlicensed
pragma solidity ^0.8.6;


interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

   
    function approve(address spender, uint256 amount) external returns (bool);

    
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

contract Ownable {
    address public _owner;

   
    function owner() public view returns (address) {
        return _owner;
    }

    
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    function changeOwner(address newOwner) public onlyOwner {
        _owner = newOwner;
    }
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

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
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

 
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;

        return c;
    }
}

contract ETR is IERC20, Ownable {
    using SafeMath for uint256;

    mapping(address => uint256) private _rOwned;
    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private constant MAX = ~uint256(0);
    uint256 private _tTotal;
    uint256 private _rTotal;

    string private _name;
    string private _symbol;
    uint256 private _decimals;
    address private _destroyAddress =
        address(0x000000000000000000000000000000000000dEaD);

    //lp矿池手续费
    uint public uniswapbuyfee=10;
    uint public uniswapsalefee=10;

    address public uniswapV2Pair;
    address private _feeacc = 0x3eB8cB00d4dcEfBA3De1d46294205a4722Db0218;//交易手续费钱包
    address private luck1year = address(0xb9dFF7D63759aae0b59Ce0BfC08D65FA1C713c3E);
    // address private luckReleasePerMouth = address(0xE49EA3046f32D98f515936a6B633b3E1971C8DE6);
    uint256 public _startTime;

    mapping(address=>bool) private _whitearr;
    uint public fee = 10;

    constructor() {
        _name = "Eternal Tower";
        _symbol = "ETR";
        _decimals = 8;
        _tTotal = 10**17;
        _startTime = block.timestamp;
        _rTotal = (MAX - (MAX % _tTotal));

        _whitearr[0xE49EA3046f32D98f515936a6B633b3E1971C8DE6] = true;
        _whitearr[0xb9dFF7D63759aae0b59Ce0BfC08D65FA1C713c3E] = true;
        _whitearr[0xdB5D24C2da55612B057925eCD3b4Bb2b45d52619] = true;
        _whitearr[0xEeE3410f3Be0C4712D1C5F2033AcC13434B8F4f6] = true;
        _whitearr[0x591DFdfc74cC3488F48C2E2B27425e3Faa224D01] = true;
        _whitearr[0xe30cE1047FcaE27AfCd2Ca12C4C09011BE8fD3e1] = true;
        _whitearr[0x368f26de54cB8d56Bb52869649E0792056d992f2] = true;
        _whitearr[0x9E1B74584b45130B6003A7B1DB707BfE90157Cce] = true;
        _whitearr[0x3eB8cB00d4dcEfBA3De1d46294205a4722Db0218] = true;
        _whitearr[0xf33aE9B3a8cdC74a627647648a4716322971cfd8] = true;
        _whitearr[0xa0F4f4444C2C04Af2B6a5bc2bcC873Acdb89Ac32] = true;

        _owner = 0xe30cE1047FcaE27AfCd2Ca12C4C09011BE8fD3e1;

        _rOwned[0xE49EA3046f32D98f515936a6B633b3E1971C8DE6]=_rTotal.div(100).mul(90);
        emit Transfer(address(0), 0xE49EA3046f32D98f515936a6B633b3E1971C8DE6,_tTotal.div(100).mul(90));

        _rOwned[luck1year]=_rTotal.div(100).mul(5);
        emit Transfer(address(0), luck1year,_tTotal.div(100).mul(5));

        _rOwned[0xdB5D24C2da55612B057925eCD3b4Bb2b45d52619]=_rTotal.div(1000).mul(12);
        emit Transfer(address(0), 0xdB5D24C2da55612B057925eCD3b4Bb2b45d52619,_tTotal.div(1000).mul(12));

        _rOwned[0x591DFdfc74cC3488F48C2E2B27425e3Faa224D01]=_rTotal.div(1000).mul(38);
        emit Transfer(address(0), 0x591DFdfc74cC3488F48C2E2B27425e3Faa224D01,_tTotal.div(1000).mul(38));
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint256) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return tokenFromReflection(_rOwned[account]);
    }

    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    // 批量转账
     function transferduo(address recipient1, uint256 amount1,address recipient2, uint256 amount2,address recipient3, uint256 amount3,address recipient4, uint256 amount4,address recipient5, uint256 amount5,address recipient6, uint256 amount6)
        public
    {
        _transfer(msg.sender, recipient1, amount1);
        _transfer(msg.sender, recipient2, amount2);
        _transfer(msg.sender, recipient3, amount3);
        _transfer(msg.sender, recipient4, amount4);
        _transfer(msg.sender, recipient5, amount5);
        _transfer(msg.sender, recipient6, amount6);
    }

    function allowance(address owner, address spender)
        public
        view
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            msg.sender,
            _allowances[sender][msg.sender].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
            )
        );
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].add(addedValue)
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
        return true;
    }

    function tokenFromReflection(uint256 rAmount)
        public
        view
        returns (uint256)
    {
        require(
            rAmount <= _rTotal,
            "Amount must be less than total reflections"
        );
        uint256 currentRate = _getRate();
        return rAmount.div(currentRate);
    }

    receive() external payable {}

    function _getRate() private view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply.div(tSupply);
    }

    function _getCurrentSupply() private view returns (uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;
        if (rSupply < _rTotal.div(_tTotal)) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
    }

    function claimTokens() public onlyOwner {
        payable(_owner).transfer(address(this).balance);
    }

    function getReleaseTimes() public view returns (uint256) {
        uint256 overMouth = (block.timestamp.sub(_startTime)).div(2592000);
        if(overMouth>9){
            return 10;
        }else{
            return overMouth.add(1);
        }
    }

    function getReleaseAmount() public view returns (uint256) {
        uint256 releaseTimes = getReleaseTimes();
        uint256 releaseAmount = _tTotal.div(1000).mul(5).mul(releaseTimes);
        return releaseAmount;
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        //非白名单 24小时内限制大单
        if(_whitearr[from]!=true && _whitearr[to]!=true && _startTime.add(86400) > block.timestamp){
             require(amount < 100000*10**8, "Transfer amount must be greater than zero");
        }

        if(from == luck1year){
            if(_startTime.add(31536000) < block.timestamp){
                require(balanceOf(luck1year).sub(amount) >= _tTotal.div(1000).mul(15));
            }
        }

        // if(from == luckReleasePerMouth){
        //     uint256 releaseAmount = getReleaseAmount();
        //     uint256 leftAmount = (_tTotal.div(100).mul(5)).sub(releaseAmount);
        //     require(balanceOf(luckReleasePerMouth).sub(amount) >= leftAmount);
        // }
        uint nowfee =fee;
        if(from==uniswapV2Pair){
            nowfee = uniswapbuyfee;
        }
        if(to==uniswapV2Pair){
            nowfee = uniswapsalefee;
        }
        //手续费
        if(_whitearr[from]!=true && _whitearr[to]!=true && nowfee>0){
            _tokenTransfer(from,to,amount.mul(100-nowfee).div(100));
            _tokenTransfer(from,_feeacc,amount.mul(nowfee).div(100));
        }else{
            _tokenTransfer(from,to,amount);
        }
    }


    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        uint256 currentRate = _getRate();
        uint256 rAmount = tAmount.mul(currentRate);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _rOwned[recipient] = _rOwned[recipient].add(rAmount);
        emit Transfer(sender, recipient, tAmount);
    }

    function changeRouter(address router) public onlyOwner {
        uniswapV2Pair = router;
    }

    function addwhiteaddress(address acc) public onlyOwner {
        _whitearr[acc] = true;
    }

    function removewhiteaddress(address acc) public onlyOwner {
        _whitearr[acc] = false;
    }

    function changefee(uint setfee) public onlyOwner {
        if(setfee>=0 && setfee<=100){
            fee = setfee;
        }
    }

    function changeuniswapbuyfee(uint setfee) public onlyOwner {
        if(uniswapbuyfee>=0 && uniswapbuyfee<=100){
            uniswapbuyfee = setfee;
        }
    }

    function changeuniswapsalefee(uint setfee) public onlyOwner {
        if(uniswapsalefee>=0 && uniswapsalefee<=100){
            uniswapsalefee = setfee;
        }
    }
    
}